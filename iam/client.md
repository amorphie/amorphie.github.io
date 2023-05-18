# Client

Clients are entities that can request Amorphie to authenticate a user. Most often, clients are applications and services that want to use Amorphie to secure clients and provide a single sign-on solution.




## Model
```json
{
  
}
```

## Passwords
Kullanıcı, birden çok farklı platform için birçok parolaya sahip olabilir. The password of type "*password*" is used by default for the password grant type. Different grant types are given for verification with other passwords.

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