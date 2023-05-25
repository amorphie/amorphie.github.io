# Coding Standart

## Entity
In the Amorphie project, database entities must derive from the EntityBase class.
The EntityBase class contains the following fields.

```
[Key]
public Guid Id { get; set; }
public DateTime CreatedAt { get; set; }
public Guid CreatedBy { get; set; }
public Guid? CreatedByBehalfOf { get; set; }
public DateTime ModifiedAt { get; set; }
public Guid ModifiedBy { get; set; }
public Guid? ModifiedByBehalfOf { get; set; }
```
A DTO class can also be defined for each entity, but there is no requirement. 
If DTO and database Entity are the same, database entity can be used instead of DTO. 
Standard CRUD operations in all APIs in the project work on these entities and DTO classes.

## Validation
In addition, a validation class must be written for data validation in upsert operations specific to each database entity. [Fluentvalidation](https://docs.fluentvalidation.net/en/latest/index.html) is used for validation. As an example, you can see a validation class below;
```
public sealed class DocumentTagValidator : AbstractValidator<DocumentTag>
    {
        public DocumentTagValidator()
        {
            // RuleFor(x => x.Name).NotNull();
            // RuleFor(x => x.Name).MinimumLength(10);
        }
    }
```
## Context
DBContexts used in the project must derive from BBTDbContext. With BBTDbContext, it is aimed to automate the processes done in all projects. Currently, only those that are mandatory in the entity are filled automatically.
```
    public abstract class BBTDbContext : DbContext
    {
        internal IBBTIdentity bbtIdentity;
        public BBTDbContext(DbContextOptions options, IBBTIdentity _bbtIdentity) : base(options)
        {
            bbtIdentity = _bbtIdentity;
        }
        public override async Task<int> SaveChangesAsync(CancellationToken cancellationToken = new CancellationToken())
        {
            foreach (var entry in ChangeTracker.Entries<EntityBase>().ToList())
            {
                switch (entry.State)
                {
                    case EntityState.Added:
                        entry.Entity.CreatedAt = DateTime.UtcNow;
                        entry.Entity.CreatedBy = bbtIdentity.UserId.Value;
                        entry.Entity.CreatedByBehalfOf = bbtIdentity.BehalfOfId.Value;

                        entry.Entity.ModifiedAt = entry.Entity.CreatedAt;
                        entry.Entity.ModifiedBy = entry.Entity.CreatedBy;
                        entry.Entity.ModifiedByBehalfOf = entry.Entity.CreatedByBehalfOf;
                        break;
                    case EntityState.Modified:
                        entry.Entity.ModifiedAt = DateTime.UtcNow;
                        entry.Entity.ModifiedBy = bbtIdentity.UserId.Value;
                        entry.Entity.ModifiedByBehalfOf = bbtIdentity.BehalfOfId.Value;
                        break;
                }
            }
            var result = await base.SaveChangesAsync(cancellationToken);
            return result;
        }
    }
```
## Security
IBBTIdentity interface has been added in order to get the information of users who operate on the entity in the system. It has been used as a mock since oauth development continues.

## RestApi
In order for all APIs to be standard in the project, BaseBBTRouteRepository must be derived. The following methods are created automatically in APIs derived from this class.

/{apiname}/{id} -> (GET) Returns entity with id <br />
/{apiname}/?page={page}&pageSize={pageSize} ->.(GET) Returns all entities according to page and pagesize <br />
/{apiname}/ -> (POST) It performs the insert or update operation according to the DTO model posted. <br />
/{apiname}/{id} -> (DELETE) Deletes entity with id. <br />

```
public abstract class BaseBBTRouteRepository<
        TDTOModel,
        TDBModel,
        TValidator,
        TDbContext,
        TRepository> : BaseRoute
        where TDTOModel : class, new()
        where TDBModel : EntityBase
        where TValidator : AbstractValidator<TDBModel>
        where TDbContext : DbContext
        where TRepository : IBBTRepository<TDBModel, TDbContext>
    {
        protected BaseBBTRouteRepository(WebApplication app) : base(app)
        {
        }

        public abstract string[]? PropertyCheckList { get; }

        public override void AddRoutes(RouteGroupBuilder routeGroupBuilder)
        {
            routeGroupBuilder.MapGet("/{id}", Get)
            .Produces<TDTOModel>(StatusCodes.Status200OK)
            .Produces(StatusCodes.Status404NotFound);

            routeGroupBuilder.MapGet("/", GetAll)
            .Produces<TDTOModel[]>(StatusCodes.Status200OK)
            .Produces(StatusCodes.Status204NoContent);

            routeGroupBuilder.MapPost("/", Upsert)
            .Produces<TDTOModel>(StatusCodes.Status201Created)
            .Produces(StatusCodes.Status204NoContent);

            routeGroupBuilder.MapDelete("/{id}", Delete)
            .Produces<TDTOModel>(StatusCodes.Status200OK)
            .Produces(StatusCodes.Status404NotFound);
        }

        protected virtual async ValueTask<IResult> Get(
            [FromServices] TRepository repository,
            [FromRoute(Name = "id")] Guid id)
        {
            return await repository.GetById(id)
                is TDBModel model
                    ? TypedResults.Ok(model)
                    : TypedResults.NotFound();
        }

        protected virtual async ValueTask<IResult> GetAll(
            [FromServices] TRepository repository,
            [FromQuery][Range(0, 100)] int page,
            [FromQuery][Range(5, 100)] int pageSize)
        {
            IList<TDBModel> resultList = await repository.GetAll(page, pageSize).ToListAsync();

            return (resultList != null && resultList.Count() > 0)
                 ? Results.Ok(resultList)
                 : Results.NoContent();
        }

        protected virtual async ValueTask<IResult> Upsert(
            [FromServices] IMapper mapper,
            [FromServices] TValidator validator,
            [FromServices] TRepository repository,
            [FromBody] TDTOModel data)
        {
            var dbModelData = mapper.Map<TDBModel>(data);


            FluentValidation.Results.ValidationResult validationResult = await validator.ValidateAsync(dbModelData);

            if (!validationResult.IsValid)
            {
                return Results.ValidationProblem(validationResult.ToDictionary());
            }

            bool IsChange = false;
            TDBModel? dataFromDB = await repository.GetById(dbModelData.Id);

            if (dataFromDB != null)
            {
                if (PropertyCheckList != null)
                {
                    object? dbValue;
                    object? dtoValue;

                    foreach (string property in PropertyCheckList)
                    {
                        dbValue = typeof(TDBModel).GetProperties().First(p => p.Name.Equals(property)).GetValue(dataFromDB);
                        dtoValue = typeof(TDTOModel).GetProperties().First(p => p.Name.Equals(property)).GetValue(data);

                        if (dbValue != null && !dbValue.Equals(dtoValue))
                        {
                            typeof(TDBModel).GetProperties().First(p => p.Name.Equals(property)).SetValue(dataFromDB, dtoValue);
                            IsChange = true;
                        }
                    }
                }

                if (IsChange)
                    await repository.SaveChangesAsync();

                return Results.NoContent();
            }
            else
            {
                await repository.Insert(dbModelData);

                return Results.Created($"/{dbModelData.Id}", dbModelData);
            }
        }

        protected virtual async ValueTask<IResult> Delete(
            [FromServices] TRepository repository,
            [FromRoute(Name = "id")] Guid id)
        {
            if (await repository.GetById(id) is TDBModel model)
            {
                await repository.Delete(model);
                return Results.Ok(model);
            }

            return Results.NotFound();
        }
    }
```

To automatically register the rest apis in Program.cs app.AddRoutes(); should be added

