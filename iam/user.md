# User

Users are people who can log into your system. They can act on their own behalf or on behalf of the institutions they represent. They can have specific roles and can be authorized according to their role.


## Model
```json
{
  "id": "46fe44ee-e34c-4318-a7d8-7ae1380733b1",
  "reference": "AA 01 23 44 B",
  "firstname": "John",
  "lastname": "Dee",
  "email": "john.dee@example.com",
  "phone": {
    "country-code": 90,
    "prefix": 530,
    "number": 1527608
  },
  "state": "Active",
  "base-state": "Active",
  "tags": [
    "customer",
    "staff"
  ],
  "security-questions": [
    {
      "id": "65fe44ee-e34c-4318-a7d8-7ae1380733be",
      "question": "What is your first pet name?",
      "salt": "A1d96203838544378a2f091f3a1036de8",
      "answer": "A528071485c1a43804dc796cf98a8609e",
      "algorithm": "Argon2"
    },
    {
      "id": "66fe44ee-e34c-4318-a7d8-7ae1380733be",
      "question": "Your first teacher name?",
      "salt": "B1d96203838544378a2f091f3a1036de8",
      "answer": "B528071485c1a43804dc796cf98a8609e",
      "algorithm": "Argon2"
    }
  ],
  "security-image": "15fe44ee-e34c-4318-a7d8-7ae1380733be",
  "passwords": [
    {
      "type": "password",
      "algorithm": "Argon",
      "salt": "1d96203838544378a2f091f3a1036de8",
      "value": "528071485c1a43804dc796cf98a8609e"
    },
    {
      "type": "mobile-pin",
      "algorithm": "Bcrypt",
      "salt": "1d96203838544378a2f091f3a1036de8",
      "value": "528071485c1a43804dc796cf98a8609e"
    },
    {
      "type": "mobile-pin",
      "algorithm": "Argon2",
      "salt": "2d96203838544378a2f091f3a1036de8",
      "value": "328071485c1a43804dc796cf98a8609e"
    }
  ],
  "devices": [
    {
      "type": "mobile",
      "device": "Apple IPhone 13 Pro",
      "id": "33ae0360-0d79-4a44-8b24-a40e72f53c65",
      "token": "53ae0360-0d79-4a44-8b24-a40e72f53c65",
      "registrar-client": "excalibur-mobile"
    },
    {
      "id": "23ae0360-0d79-4a44-8b24-a40e72f53c65",
      "type": "IP",
      "device": "ERP PROD",
      "ip": "192.165.123.123",
      "registrar-client": "client-secret/erp"
    }
  ]
}
```

## Passwords
The user can have many passwords for multiple different platforms. The password of type "*password*" is used by default for the password grant type. Different grant types are given for verification with other passwords.

The security algorithm and salt value of each password definition can be defined. Thus, it allows the passwords in different systems to be migrated.

!> Only first-party use of these grant types is allowed for user authentications. Which means only **/authorize** endpoint can use these grant types.

| Grant Type          | Full Name Of GrantType                   |
| ------------------- | ---------------------------------------- |
| password            | password                                 |
| pin                 | io.amorphie.identity.oauth2.grant.pin    |
| puk                 | io.amorphie.identity.oauth2.grant.puk    |
| mobile              | io.amorphie.identity.oauth2.grant.mobile |
| web                 | io.amorphie.identity.oauth2.grant.web    |


## Devices

User registered devices can used to authantication or authorization factor. For a device to be considered a factor, its status must be active.

Device **Type** can be **mobile** and **IP**. *For now :)*

### Mobile Device
For mobile devices device id and token id is has to be set to use as authantication factor.

!>Please consider  [OAuth 2.0 for Native Apps](https://www.rfc-editor.org/rfc/rfc8252) RFC before implementing mobile authantication mechanism. Also you can read [OAuth 2 Best Practices For Native Apps](https://auth0.com/blog/oauth-2-best-practices-for-native-apps) for more information

### IP Based Device
For IP-based restrictions, the IP information of the device to be accessed is defined.

This definition can be used as additional authorization factor for api calls. This authentication check is done for every API call at the gateway level.

## Security Questions
Security questions are similar to passwords but are used for the password recovery flow.

As with the password, the security algorithm and salt value of the answer to each security question are kept.
## Anti Phishing Picture
Each user chooses an anti-phishing image when signing up to help prevent phishing attacks in future sessions. Of course the user can update the pishing image