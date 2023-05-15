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
      "type": "pin",
      "algorithm": "Argon2"
    }
  ]
}
```

## Passwords

User can have password for multiple different platforms. The password of type "*password*" is used by default for the password grant type. Different grant types are given for verification with other passwords.


| Amorphie Grant Type    | Firstname | Lastname | Reference   | Tags                    | State  |
| ----- | --------- | -------- | ----------- | ----------------------- | ------ |
| Selda | Selda     | Melda    | 58562768098 | loan-partner-user       | Active |
| Uğur  | Uğur      | Karataş  | 38552069008 | retail-customer, staff  | Active |
| Doğan | Doğan     | Karataş  | 28552069008 | corporate-customer-user | Active |





## Devices

## Security Questions

## Anti Phissing Picture
