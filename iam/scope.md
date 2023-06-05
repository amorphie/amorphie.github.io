# Definition
In OAuth 2.0, a scope is a mechanism used to define and limit the permissions and access rights granted to a client application when accessing protected resources on behalf of a resource owner (user).

The purpose of scopes in OAuth 2.0 is to ensure that clients have explicit permission from the resource owner to access specific resources or perform certain actions. Scopes allow for fine-grained control over the access privileges granted to client applications, promoting security and protecting user data.

Scopes are defined by the resource server and are specific to the particular API or service being accessed. They represent a set of permissions or actions that a client application may request during the authorization process. Scopes can vary depending on the API or service and can be defined in a granular manner to provide precise control over the requested access.

During the OAuth 2.0 authorization process, the client application includes the requested scope(s) in the authorization request to the authorization server. The resource owner is then presented with the requested scope(s) and can choose to grant or deny the client's access based on the permissions being requested.

Examples of scopes in OAuth 2.0 could include:
* read: Grants read-only access to specific resources.
* write: Grants write access to specific resources, allowing the client to modify or create new data.
* profile: Grants access to the user's profile information.
* email: Grants access to the user's email address.
* offline_access: Grants the ability to access protected resources even when the resource owner is not present or authenticated.

By utilizing scopes, OAuth 2.0 allows for flexible and controlled access to protected resources, ensuring that client applications only have the necessary permissions required for their intended functionality and that resource owners have control over the access privileges granted to client applications.

# Scope

Scope is a way to limit an app’s access to a user’s data. Scope is a way to limit what an application can do within the context of what a user can do.

For example, in the banking sector, we can accept all your individual and corporate customers and partner companies as a scope. Thus, users can make transactions with a single login for both their own customer accounts and the accounts of the companies they work for. For example, bank employees can make transactions both on their own accounts and on behalf of the customers of the branch they work for.

## Model

```json
[
  {
    "id": "1324d72c-1408-4307-973c-bd74e3d8a185",
    "reference": 32452343,
    "name": "John De",
    "tags": [
      "retail-customer"
    ],
    "role-group": "retail",
    "consent": {
      "flows": [
        {
          "client": "mobile",
          "workflow": "retail-user-consent"
        },
        {
          "client": "psd2-gateway",
          "workflow": "retail-user-psd2-gateway-consent"
        },
        {
          "client": "client-secret",
          "workflow": "retail-user-api-key-consent"
        }
      ],
      "inviter-roles": [
        "admin"
      ]
    }
  },
  {
    "id": "3234d72c-1408-4307-973c-bd74e3d8a185",
    "reference": 11123213,
    "name": "Microsoft",
    "tags": [
      "corporate-customer"
    ],
    "role-group": "big-corporate",
    "consent": {
      "flows": [
        {
          "client": "default",
          "workflow": "corporate-user-consent"
        },
        {
          "client": "client-secret",
          "workflow": "corporate-user-api-key-consent"
        }
      ],
      "inviter-roles": [
        "admin",
        "ceo",
        "cto"
      ]
    }
  },
  {
    "id": "5555d72c-1408-4307-973c-bd74e3d8a185",
    "reference": 51123213,
    "name": "BestBuy",
    "tags": [
      "loan-partner"
    ],
    "role-group": "partner",
    "consent": {
      "flows": [
        {
          "client": "default",
          "workflow": "partner-user-consent"
        }
      ],
      "inviter-roles": [
        "admin"
      ]
    }
  }
]
```

## Highlights

### Role Group
Role group selection for scope refers to the process of defining and assigning roles within a project or organization based on the specific scope of work. It involves creating a flexible structure where roles can be tailored to meet the requirements and objectives of a particular project or initiative.

The purpose of role group selection for scope is to ensure that individuals are assigned roles that align with their skills, expertise, and responsibilities within the defined scope of work. By customizing roles based on the specific project requirements, organizations can optimize team performance and enhance overall project success.


### Consent
When accessing the scope for the first time, obtaining consent is a common practice to ensure compliance with privacy and data protection regulations, as well as to respect individuals' rights and preferences. Consent is obtained from the individuals whose data or information is being accessed or processed within the defined scope. This process is typically known as obtaining "informed consent."

Obtaining consent for accessing the scope can be facilitated through two different approaches, as you described:

* **Owner's Consent**: If the user is the owner of the scope, with matching references indicating their ownership (*by checking is user reference equal to scope reference*), consent can be automatically granted after their initial login. This means that when the owner logs in for the first time, their ownership of the scope is verified, and consent is implied or automatically granted based on this verification. This approach assumes that the owner has the authority to grant consent on behalf of themselves and potentially other users within the scope. 

* **Invitation-Based Consent**: In this approach, the owner of the scope invites specific users to access the scope's resources. The invitation includes information about the scope, its purpose, and any relevant details regarding data access or processing. When users receive the invitation, they have the option to review the details and provide their explicit consent to access the scope. This ensures that users have an opportunity to make an informed decision before granting consent. Invitation must include at least  user reference value, and role;

    * User Reference Value: The invitation should include a unique user reference value or identifier that associates the invitation with the specific user being invited. This reference value helps in accurately identifying and linking the user's consent with their profile or account. It could be a username, email address, employee ID, or any other unique identifier that is used to identify users within the system.
    
    * Role: The invitation should clearly specify the role or roles that the user is being invited to fulfill within the scope. The role defines the responsibilities, permissions, and access levels associated with the user's involvement in the project or organization. By explicitly stating the role in the invitation, users can understand their expected contributions and the scope of their access.
