# NestJS Auth Service

NestJS backend only keeps users' Google Identity ID in database. Email, phone number and address saved in Google Identity database.

Authentication response object:
```json
{
  id: "string",
  role: "UserRole",
  token: "string",
  type: "string",
  email: "string",
  emailVerified: "boolean",
  displayName: "string",
  photoURL: "string",
}
```