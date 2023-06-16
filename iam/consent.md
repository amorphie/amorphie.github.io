# Consent

When accessing the scope for the first time, obtaining consent is a common practice to ensure compliance with privacy and data protection regulations, as well as to respect individuals' rights and preferences. This is an important step in establishing a lawful and ethical approach to handling personal data.

Obtaining consent serves as evidence that individuals are aware of and have agreed to the processing of their personal data within the defined scope. It demonstrates a commitment to transparency, giving individuals the opportunity to make informed decisions about how their data is used.

it is essential to include specific information about the scope, client, and user to ensure clarity and transparency. Including these details helps individuals understand the context and purpose of the data processing, enabling them to make informed decisions about granting consent. Here's an explanation of each element:

- Scope: The scope refers to the specific context or project for which the consent is being obtained. It outlines the boundaries, objectives, and activities related to the processing of personal data. Clearly defining the scope helps individuals understand the purpose and extent of their data's usage.
- Client: The client refers to the organization or entity responsible for initiating or managing the data processing activities within the defined scope. It is important to identify the client as it establishes accountability and allows individuals to know who is requesting their consent.
- User: The user refers to the individual whose personal data is being processed within the defined scope. Providing information about the user ensures that individuals understand that the consent request pertains to their specific data and enables them to make decisions based on their own preferences.

## Requirements

- [ ] Consent flow is composition of pages in sso frontend and ui flow is pushed from worklow
- [ ] Consent flow is started after grant flow when first access to scope.
- [ ] Contracts: Consent definition must know the document sets to get consent. Consent instance must know signed documents and versions. If consent require new version of any document, after grant flow must require to sign new version of document. 
- [ ] NameValueCollection: Consent can include some special dataset like account list for open banking. Custom datasets are stored and shared as JSON. 


**NEED CONSENT VALIDATION FRAMEWORK**

## Definition Model

```json
{
  "id": "1664d72c-1408-4307-973c-bd74e3d8a185",
  "name": "open-banking-consent",
  "scope-tags": [
    "retail-customer",
    "corporate-customer"
  ],
  "clients": [
    "b567133c-3536-48ae-b677-89725242e248"
  ],
  "role-assigment": "auto | user-select | admin"
}
```

## Instance Model

```json
{
  "id": "a664d72c-1408-4307-973c-bd74e3d8a185",
  "consent": "1664d72c-1408-4307-973c-bd74e3d8a185",
  "user": "g664d72c-1408-4307-973c-bd74e3d8a185",
  "client": "c664d72c-1408-4307-973c-bd74e3d8a185",
  "role": "admin",
  "access-token-duration": "5m",
  "refresh-token-duration": "180d",
  "data": [
    {
      "documents": "...escaped json data..."
    },
    {
      "accounts": "[ \"195f2ab8-8d4f-40c8-b4f9-3fac0f254c49\",  \"ddac1206-0413-4dc3-8ddd-e982fac8b472\",  \"a2ae3bb4-4024-4627-bd87-ac63893dd8e3\",  \"949602de-b893-4d07-a662-cc005a1c99b6\",  \"7947834b-96a7-48e1-9034-546aa2db0f9e\"]"
    }
  ],
  "state": "active"
}
```

!> All data will be transmitted as header to upstream for every request.

```html
X-User-ID :  g664d72c-1408-4307-973c-bd74e3d8a185
X-User-Reference :  38653069009
X-Scope-ID :  a664d72c-1408-4307-973c-bd74e3d8a185
X-Scope-Reference :  38653069009
X-Client-ID :  a664d72c-1408-4307-973c-bd74e3d8a185
X-Client-Name :  BKM
X-Client-Variant :  Akbank
X-Consent-Data-Documents : "...json..."
X-Consent-Data-Accounts : "[ \"195f2ab8-8d4f-40c8-b4f9-3fac0f254c49\",  \"ddac1206-0413-4dc3-8ddd-e982fac8b472\",  \"a2ae3bb4-4024-4627-bd87-ac63893dd8e3\",  \"949602de-b893-4d07-a662-cc005a1c99b6\",  \"7947834b-96a7-48e1-9034-546aa2db0f9e\"]"
```
