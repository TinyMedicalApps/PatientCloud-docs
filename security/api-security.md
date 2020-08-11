# API Security

## JWT

Java Web Tokens are used to get access to secured endpoints and extracted from Authorization's Bearer header.

**Configuration**

JWT configuration initialized in `src/auth/strategies/jwt.strategy.ts` file.

**Payload**
|Field|Description|
|:--|:--|
|`id`| User id|
|`name`| First name and last name|
|`email`| Email|
|`role`| `user` or `superuser`|

**Getting a token**
Multiple strategies can be used to obtain a JWT token:
|Endpoint|Provider|
|:--|:--|
|`/auth/facebook`| Facebook OAuth|
|`/auth/google`| Google OAuth|
|`/auth/linkedin`| LinkedIn OAuth|
|`/auth/nhs-login`| NHS OpenID Connect|
|`/auth/login`| Local user and password |

**Configuration**

**Refresh**
Each token has a limited lifetime. You need to obtain a new one or you can refresh it on `/auth/refresh` endpoint.

## Guards

We have different types of guards to secure endpoints.

### Auth guard

NestJS/Passport guard. Uses given strategy to protect route. More on strategies at [Strategies](#strategies)

### Roles guard

Roles guard used to protect an endpoint by user role. We can use it as a authorization. There are two roles: `superuser` and `user`

## Strategies

| File                   | Description    |
| :--------------------- | :------------- |
| `facebook.strategy.ts` | Facebook OAuth |
| `google.strategy.ts`   | Google OAuth   |
| `linkedin.strategy.ts` | LinkedIn OAuth |
| `oidc.strategy.ts`     | OpenID Connect |
| `jwt.strategy.ts`      | Java Web Token |
