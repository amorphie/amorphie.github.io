# Definition
In OAuth 2.0, Client represents an application or service that wants to access protected resources on behalf of an end-user. The client is an essential component in the OAuth 2.0 framework and plays a crucial role in the authorization process.

The purpose of the client in OAuth 2.0 is to initiate the authorization process and obtain authorization from the resource owner (end-user) to access their protected resources. The client can be a web application, a mobile application, a server-side application, or any other software component that needs to access protected resources.

The client is typically registered with the authorization server, and during the authorization process, it interacts with both the authorization server and the resource server. Here's a high-level overview of the OAuth 2.0 flow involving the client:

1. Registration: The client registers itself with the authorization server. This registration involves providing information about the client, such as its client ID, client secret (in some cases), redirect URIs, and supported grant types.

2. Authorization Request: The client initiates the authorization process by redirecting the resource owner to the authorization server. This request includes the client ID, requested scope, and redirect URI.

3. User Consent: The resource owner is presented with an authorization prompt, where they can review the requested permissions (scope) and grant or deny access to the client.

4. Authorization Grant: If the resource owner grants access, the authorization server issues an authorization grant to the client, typically in the form of an authorization code or an access token.

5. Token Request: The client exchanges the authorization grant for an access token by making a token request to the authorization server. This request typically includes the client credentials (client ID and client secret) and the authorization grant.

6. Access Protected Resources: The client uses the obtained access token to authenticate itself and access the protected resources from the resource server on behalf of the resource owner.

By acting as an intermediary between the resource owner and the resource server, the client enables secure and controlled access to protected resources without requiring the resource owner to share their credentials directly with the client. This separation of concerns improves security and allows users to control the access privileges granted to client applications.

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
  "allowed-grant-types": ["authorization_code", "password", "refresh_token"],
  "allowed-scope-tags": ["retail-customer", "corporate-customer", "staff"],
  "login-url": "https://acme.com/oauth/login",
  "return-uri": "https://acme.com/oauth/authorized",
  "logout-uri": "https://acme.com/oauth/logout",
  "client-secret": "a1510240a9a747dc86067cfdfe83121ba1510240a9a747dc86067cfdfe83121b",
  "pkce": "must",
  "jws": {
    "mode": "must",
    "header": "X-JWS-Signature",
    "secret": "22769d4d21e345ff8de5211f959eba51",
    "algorithm": "HS384"
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
      "claims": ["user.reference"]
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

#### Highlights

##### Allowed Scopes

Allowed scopes for the client, defined as allowed scope tags.
If left blank, the client can request access to all scopes.

Tags on scopes allow scopes to be grouped

```json
  "allowed-scope-tags": [
    "retail-customer",
    "corporate-customer",
    "staff"
  ],
```

##### URL Definitions

- **Login URL** is client login URI. In some cases, Amorphie can redirect to client application's login page.
- **Return URI** is endpoint of client. After a user logged Amorphie SSO Application, user is redirected to return URL with authorization code.
- **Logout URI** is endpoint of client. If the user is logged out or is logged out by someone else (such as for a fraudulent reason), the client is notified by calling the logout uri.

All URIs are optional for client definitions.

```json
  "login-url": "https://acme.com/oauth/login",
  "return-uri": "https://acme.com/oauth/authorized",
  "logout-uri": "https://acme.com/oauth/logout",
```

##### PKCE [Proof Key for Code Exchange]
This attribute specifies whether Proof Key for Code Exchange (PKCE) is required for authorization code grant type flows. "must" indicates that PKCE is mandatory for this client.
(<https://oauth.net/2/pkce/#:~:text=RFC%207636%3A%20Proof%20Key%20for%20Code%20Exchange&text=PKCE%20(RFC%207636)%20is%20an,secret%20or%20other%20client%20authentication>) configuration only two values are allowed. If **must** is configured, Client must request authorization with the code challenge hashed as SHA256. If left blank or **optional**, validation can be used optionally.

##### JWS
[JSON Web Signature](https://www.rfc-editor.org/rfc/rfc7515) validation can be configured for client. If client has to supply signature for every request **mode** must be set to **must**. If **mode** is empty or is set to **optional** signature validation is only occur when signature supplied.

Signature should supplied as http header.

```json
"jws": {
  "mode":"must",
  "header": "X-JWS-Signature",
  "secret": "22769d4d21e345ff8de5211f959eba51",
  "algorithm": "HS384"
}
```
1. "mode": "must"
This attribute specifies the mode of JWS usage. In this case, "must" indicates that JWS is mandatory for the client.

2. "header": "X-JWS-Signature"
This attribute represents the header name where the JWS signature will be included. In this case, it is set to "X-JWS-Signature".

3. "secret": "22769d4d21e345ff8de5211f959eba51"
This attribute contains the secret key used for JWS signing and verification. The secret key is a shared secret between the client and the server. It is used to sign the data and validate the integrity of the message.

4. "algorithm": "HS384"
This attribute specifies the algorithm used for JWS signature generation and verification. In this case, "HS384" refers to HMAC-SHA384, which is a symmetric cryptographic algorithm using a secret key for signing and verification.

Commonly used JWS signature generation algorithms:

1. HMAC-SHA256 (HS256): This algorithm uses HMAC-SHA256 for symmetric key-based signing and verification.
2. HMAC-SHA384 (HS384): This algorithm uses HMAC-SHA384 for symmetric key-based signing and verification.
3. HMAC-SHA512 (HS512): This algorithm uses HMAC-SHA512 for symmetric key-based signing and verification.
4. RSA-SHA256 (RS256): This algorithm utilizes RSA asymmetric key pairs for signing and verification, using SHA-256 as the hash function.
5. RSA-SHA384 (RS384): This algorithm uses RSA asymmetric key pairs for signing and verification, with SHA-384 as the hash function.
6. RSA-SHA512 (RS512): This algorithm uses RSA asymmetric key pairs for signing and verification, using SHA-512 as the hash function.
7. ECDSA-SHA256 (ES256): This algorithm employs the Elliptic Curve Digital Signature Algorithm (ECDSA) with the NIST P-256 curve for signing and verification, using SHA-256 as the hash function.
8. ECDSA-SHA384 (ES384): This algorithm uses ECDSA with the NIST P-384 curve for signing and verification, with SHA-384 as the hash function.
9. ECDSA-SHA512 (ES512): This algorithm employs ECDSA with the NIST P-521 curve for signing and verification, using SHA-512 as the hash function.
10. RSA-PSS-SHA256 (PS256): RSA Probabilistic Signature Scheme (RSA-PSS) with SHA-256 as the hash function is used for signing and verification.
11. RSA-PSS-SHA384 (PS384): RSA-PSS with SHA-384 as the hash function is used for signing and verification.
12. RSA-PSS-SHA512 (PS512): RSA-PSS with SHA-512 as the hash function is used for signing and verification.

## Flows
This attribute defines the different flows or scenarios in which the client will interact with the authorization server.
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

- Only one must be defined per client.
- Uses **flow** token type.
- Token duration can be configured per flow.

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

- It is optional.
- Only one can be defined for each client.
- Uses **flow** token type.
- Token duration can be configured per flow.


**Highlights**

- It is optional.
- Only one can be defined for each client.
- Uses **access** token type. Only authenticated users can interact with consent flow

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

- It is optional.
- More than one can be defined for each customer.
- Uses **access** or **flow** token type.
- If **flow** token used duration can be configured.

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
}
```

**Highlights**

- A duration definition should be made for each token.
- For claim, user and related, scope, consent and client records and the fields in these records and associated tag fields can be used.
- If variants and altering token durations feature are allowed for the client user can alter token durations per variant.

_Client with variants sample configuration_

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
