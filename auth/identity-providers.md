# Identity Providers
## Facebook
JWT access token can be obtained by sending a `POST` payload to `/auth/login`. Authenticate user on Facebook and get an auth token. Use the token as a `token` payload.

```json
{
  token: "FACEBOOK_AUTH_TOKEN",
  provider: "FACEBOOK"
}
```

## Google

 Authenticate user on Google and get an auth token. Use the token as a `token` payload.

```json
{
  token: "GOOGLE_AUTH_TOKEN",
  provider: "GOOGLE"
}
```
## Twitter

 Authenticate user on Twitter and get an auth token and a secret token. Use the token as a `token` payload and secret token as `tokenSecret`

```json
{
  token: "TWITTER_TOKEN",
  tokenSecret: "TWITTER_SECRET_TOKEN",
  provider: "TWITTER"
}
```

## NHS Login

 Redirect user to `/auth/nhs-login`
