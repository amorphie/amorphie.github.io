# Roles & Permissions

Roles and permissions allows you to control authorization on your resources.

Roles Group allows you to group roles.  And also Role Group is assigned to scope  for limit to available roles. This means that different scopes can manage with different sets of roles.

## Role Group Model

```json
[
  {
    "id": "1324d72c-1408-4307-973c-bd74e3d8a185",
    "name": "customer",
    "roles": [
      "product-read",
      "self-basket-create",
      "self-basket-checkout",
      "self-order-tracking"
    ]
  },
  {
    "id": "1324d72c-1408-4307-973c-bd74e3d8a185",
    "name": "merchant",
    "roles": [
      "product-read",
      "product-modify-merchant",   
      "self-basket-create",
      "self-basket-checkout",
      "self-order-tracking",
      "merchant-order-tracking"
    ]
  }
]
```

## Role  Model

```json
[
  {
    "id": "1324d72c-1408-4307-973c-bd74e3d8a185",
    "name": "self-order-tracking",
    "resources": [
      {
        "url": "ecommerce/order/{order-id}",
        "method": "GET",
        "permissions": [
          "ecommerce/security/order/ownership?order={{$request.query.order-id}}&customer={{$token.user.reference}}"
        ]
      }
    ]
  },
  {
    "id": "1324d72c-1408-4307-973c-bd74e3d8a185",
    "name": "merchant-order-tracking",
    "resources": [
      {
        "url": "ecommerce/order/{order-id}",
        "method": "GET",
        "permissions": [
          "ecommerce/security/order/ownership?order={{$request.query.order-id}}&merchant={{$token.scope.reference}}"
        ]
      }
    ]
  }
]
``` 

#### Highlights

##### Resources

In Amorphie every resource refers to API endpoint. For authorization functionalty all APIs must registered to api gateway and Amorphie authorization plugin must be activated on api gateway.

?> Amorphie has authorization plugin for APISIX.

##### Permissions

Permissions is a request-based access control mechanism. Generally, the user's privileges are controlled by the parameters of the request(header, query, body) and token (user, scope, client, consent) data.

!> Permissions must be HTTP GET URL. And must return 200 Ok as response code for privileged access.

Simple templating is used to access request and token data during querying.

Samples

```js  
{{ $token.user.tags.customer.province }} // Accesses the province contained in the customer tag data of the customer who owns the token.

{{ $request.body.id }} // Accesses id from request body data

{{ $request.query.reference }} // Accesses reference from request query params
```