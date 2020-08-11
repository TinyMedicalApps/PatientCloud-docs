# NestJS Overview

## NestJS libraries used

### TypeORM

We use TypeORM as an ORM. UUID used in every table, it takes more space then integer AUTOINCREMENT field, but it allows us better scalabilty and we can build domain entities with primary key which we can generate ourselves. There is no database preference in the code any SQL database can be used.

**Configuration**

TypeORM configuration initialized in `src/app.module.ts` file:
|Variable|Description|
|:--|:--|
|type|`mysql` or `postgres`|
|url| Connection URL string including name, password and address|
|entities|Directory where ORM should look for entity files |
|synchronize| ORM will keep database tables in sync with entities if set to `true` |
|logging| Log all queries and commands to console |

### Passport.js

Passport.js used for OAuth and OpenID Connect authentications.

**Strategies**
|Name|Description|
|:--|:--|
|`passport-facebook`| Facebook OAuth|
|`passport-google-oauth20`| Google OAuth|
|`passport-linkedin-oauth2`| LinkedIn OAuth|
|`openid-client`| OpenID Connect|

**Configuration**

All configurations are stored in `src/auth/strategies` files and can be overridden by environment variables.

### Swagger

OpenAPI is used for API description and documentation. While the application is running, Swagger is accessible at `/api` url. OpenAPI JSON schema can be downloaded at `/api-json` address.

### NestJSX Crud

NestJSX Crud is used for simple CRUD operations. It is very powerful library with authentication support. Library configured in corresponding module's controller file. Full documentation available here (https://github.com/nestjsx/crud/wiki/Controllers#description).

### Nodemailer

Email notifications are using Nodemailer library through `@nest-modules/mailer` wrapper. Handlebars template engine for email templates.

**Configuration**

Email settings are set in environment variables and initilized in `src/app.module.ts` file.

| Variable     | Description                       |
| :----------- | :-------------------------------- |
| transport    | Connection URL                    |
| template.dir | Full path to Handlebars templates |
