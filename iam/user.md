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
      "question": "What is your first pet name?"
    }
  ],
  "security-image": "65fe44ee-e34c-4318-a7d8-7ae1380733be",
  "passwords": [
    {
      "type": "password",
      "algorithm": "Argon"
    },
    {
      "type": "mobile-pin",
      "algorithm": "Bcrypt"
    },
    {
      "type": "mobile-pin",
      "algorithm": "Argon2"
    }
  ],
  "devices": [
    {
      "type": "mobile",
      "device": "Apple IPhone 13 Pro",
      "id": "33ae0360-0d79-4a44-8b24-a40e72f53c65",
      "token": "33ae0360-0d79-4a44-8b24-a40e72f53c65",
      "registrar-client": "excalibur-mobile"
    },
    {
      "type": "IP",
      "device": "SP_ERP_PROD",
      "id": "192.165.123.123",
      "mac": "00:25:96:FF:FE:12:34:56",
      "registrar-client": "client-secret/erp"
    }
  ]
}
```

## Passwords

User can have password for multiple different platforms. The password of type "*password*" is used by default for the password grant type. Different grant types are given for verification with other passwords.

!> Only first-party use of these grant types is allowed for user authentications. Which means only **/authorize** endpoint can use these grant types.



| Grant Type          | Full Name Of GrantType                   |
| ------------------- | ---------------------------------------- |
| password            | password                                 |
| pin                 | io.amorphie.identity.oauth2.grant.pin    |
| puk                 | io.amorphie.identity.oauth2.grant.puk    |
| mobile              | io.amorphie.identity.oauth2.grant.mobile |
| web                 | io.amorphie.identity.oauth2.grant.web    |


## Devices

User registered devices can used to authantication factor. For a device to be considered a factor, its status must be active.

Device **Type** can be **mobile** and **IP**. *For now :)*

### Mobile Device
For mobile devices device id and token id is has to be set to use as factor.

!>Please consider  [OAuth 2.0 for Native Apps](https://www.rfc-editor.org/rfc/rfc8252) RFC before implementing mobile authantication mechanism. Also you can read [OAuth 2 Best Practices For Native Apps](https://auth0.com/blog/oauth-2-best-practices-for-native-apps) for more information

### IP Based Device

For IP-based restrictions, the IP information of the device to be accessed and the mac address of the device are defined together.

## Security Questions

## Anti Phishing Picture
