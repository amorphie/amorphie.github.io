# Client

Clients are entities that can request Amorphie to authenticate a user. Most often, clients are applications and services that want to use Amorphie to secure clients and provide a single sign-on solution.

There is the concept of **variant** for compatibility with 3rd party gateways (like PSD2 Gateway) and API keys. You can assume variants as sub client definition. It does not need to be predefined like the client. A variant can crate automatically on first access.

## Model
```json
{
  "id": "b567133c-3536-48ae-b677-89725242e248",
  "name": "Web Client",
  "flows": [
    {
      "type": "register",
      "workflow": "register-mobile-user",
      "token-duration": "5m"
    },
    {
      "type": "login",
      "workflow": "login-mobile-user",
      "token-duration": "5m"
    },
    {
      "type": "consent",
      "workflow": "mobile-user-consent"
    },
    {
      "type": "support",
      "workflow": "reset-password",
      "token": "flow",
      "token-duration": "5m"
    },
    {
      "type": "support",
      "token": "access",
      "workflow": "change-mobile-number"
    }
  ],
  "allowed-scope-tags": [
    "retail-customer",
    "corporate-customer",
    "staff"
  ],
  "login-url": "https://acme.com/oauth/login",
  "return-url": "https://acme.com/oauth/authorized",
  "post-logout-url": "https://acme.com/oauth/logout",
  "client-secret": "a1510240a9a747dc86067cfdfe83121ba1510240a9a747dc86067cfdfe83121b",
  "pkce": "must",
  "jws": {
    "header": "X-JWS-Signature",
    "secret": "22769d4d21e345ff8de5211f959eba51",
    "algirthm": "HS384"
  },
  "idempotency": {
    "mode": "alert",
    "header": "X-Request-ID"
  },
  "variant": {
    "mode": "none"
  },
  "session": {
    "header": "X-Group-ID"
  },
  "location": {
    "header": "PSU-GEO-Location"
  },
  "tokens": [
    {
      "type": "access",
      "duration": "5m",
      "claims": [
        "user.reference"
      ]
    },
    {
      "type": "identity",
      "duration": "5m",
      "claims": [
        "user.reference",
        "user.mobile-phone",
        "user.tags.customer.email",
        "user.tags.customer.campaign- permission",
        "scope.tags.corporate-customer.tax-no"
      ]
    },
    {
      "type": "refresh",
      "duration": "15m",
      "claims": []
    }
  ]
}
```

## Flows
Grant and other flows are developed with Amorphie Workflow Engine. Workflow Engine allows to create multi factor, multi page authentication flows easily.

Available flow types are;

#### Flow Type: `login`
Client uses this grant flow for authorization.


```json
  {
      "type": "login",
      "workflow": "login-mobile-user",
      "token-duration": "5m"
  }
```

**Highlights**
* Only one must be defined per client.
* Uses **flow** token type.
* Token duration can be configured per flow.

#### Flow Type: `register`
Client uses this grant flow for registration process.  

```json
 {
      "type": "register",
      "workflow": "register-mobile-user",
      "token-duration": "5m"
 }
```

**Highlights**
* It is optional.
* Only one can be defined for each client. 
* Uses **flow** token type.
* Token duration can be configured per flow.

#### Flow Type: `consent`
If user try to access to requested scope first time, consent flow triggered after login successful completion of login flow.
```json
  {
    "type": "consent",
    "workflow": "mobile-user-consent",
  }
```
**Highlights**
* It is optional.
* Only one can be defined for each client. 
* Uses **access** token type. Only authenticated users can interact with consent flow


#### Flow Type: `support`
Workflows such as resetting password, changing mobile number, unpairing mobile phone are considered support flows

```json
  {
    "type": "support",
    "workflow": "reset-password",
    "token": "flow",
    "token-duration": "5m"
  },
  {
    "type": "support",
    "token": "access",
    "workflow": "change-mobile-number"
  }
```
**Highlights**
* It is optional.
* More than one can be defined for each customer.
* Uses **access** or **flow** token type.
* If **flow** token used duration can be configured.

## Tokens

Tokens are RFC 7519 compliant JSON Web Token (JWT). Amorphie uses only JWT bearer token for authorization.


```json
 {
      "type": "identity",
      "duration": "5m",
      "claims": [
        "user.reference",
        "user.mobile-phone",
        "user.tags.customer.email",
        "user.tags.customer.campaign- permission",
        "scope.tags.corporate-customer.tax-no"
      ]
    },
```
**Highlights**
* A duration definition should be made for each token.
* For claim, user and related, scope, consent and client records and the fields in these records and associated tag fields can be used.
* If variants and altering token durations feature are allowed for the client user can alter token durations per variant.

*Client with variants sample configuration*

```json
{
  ...
  "name": "PSD2 Gateway",
  ...
  "variant": {
    "mode": "allow-all",
    "header": "X-TPP-Code",
    "alter-duration": true
  },
  ...
  "tokens": [
    {
      "variant": "Burgan Bank",
      "type": "access",
      "duration": "30m",
      "claims": [
        "user.reference"
      ]
    },
    {
      "variant": "Ak Bank",
      "type": "access",
      "duration": "20m",
      "claims": [
        "user.reference"
      ]
    }
  ]
}
```

#### Token Type: `access`
Access token is a JWT that represents the authorization granted to the client by the resource owner. The access token is issued by an authorization server and is used to access protected resources on behalf of the resource owner.


#### Token Type: `refresh`
Refresh token is used to obtain a new access token when the current access token expires. The refresh token is issued by the authorization server and is used to obtain a new access token without requiring the user to re-authenticate. The refresh token is usually a long-lived token that can be used to obtain new access tokens for a certain period of time.

#### Token Type: `flow`
Flow token is similar to access token with a major difference. Flow tokens are issued to a flow without requiring authorization. And usage is for only issued flow.

#### Token Type: `identity`
An identity token is a type of token that is used to represent the identity of a user. It contains claims about the user such as their name, email address, and other attributes. Identity tokens are used in authentication scenarios where the identity of the user needs to be verified.
