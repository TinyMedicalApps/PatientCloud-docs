# Local Build

### Running using Docker

Docker is the fastest way to run Patient Cloud API. The image located on Docker Hub [https://hub.docker.com/r/tinymedicalapps/patientcloud](https://hub.docker.com/r/tinymedicalapps/patientcloud).

With [Docker Compose](https://docs.docker.com/compose/install/) you can easily configure, install, and upgrade your Docker-based Patient Cloud installation:

Example `docker-compose.yml` with MariaDB (check [Patient Cloud Configuration](#configuration) chapter for configuration options):

```text
version: '3'
services:
  patientcloud:
    image: tinymedicalapps/patientcloud
    volumes:
      - google.credentials.json:/config/google.credentials.json
    environment:
      - DB_URL=mysql://patientcloud:patientcloud@db:3306/patientcloud
      - NODE_PORT=80
      - NODE_ENV=development
      - JWT_SECRET=your_secret_key_add_your_own
      - FRONTEND_URL=http://localhost:3000
      - NHS_OPEN_ID_ENDPOINT=https://auth.sandpit.signin.nhs.uk
      - NHS_OPEN_ID_CLIENT_ID=digital-health-passport
      - NHS_OPEN_ID_CLIENT_PASSWORD=dummy
      - NHS_OPEN_ID_CLIENT_SCOPE=openid profile
      - NHS_OPEN_ID_CLIENT_KEY=private_key.pem
      - NHS_OPEN_ID_CLIENT_REDIRECT_URI=http://localhost/auth/nhs-callback
      - SESSION_SECRET=your_session_secret_key_add_your_own
      - GOOGLE_APPLICATION_CREDENTIALS=/config/google.credentials.json
  db:
    image: mariadb:10.5
    volumes:
      - mysqldata:/var/lib/mysql
    environment:
      - MYSQL_ROOT_PASSWORD=root
      - MYSQL_DATABASE=patientcloud
      - MYSQL_USER=patientcloud
      - MYSQL_PASSWORD=patientcloud

volumes:
  mysqldata:
    driver: 'local'
```

Make sure you are in the same directory as `docker-compose.yml` and start Patient Cloud:

```text
docker-compose up -d
```

### Running using Node.js

1. Clone Patient Cloud repository:

   ```text
   git clone https://github.com/TinyMedicalApps/PatientCloud.git .
   ```

2. Change directory:

   ```text
   cd PatientCloud
   ```

3. Install dependencies:

   ```text
   npm install
   ```

4. Configure Patient Cloud environment variables and database. Check [Patient Cloud Configuration](#configuration) chapter.
5. Run Patient Cloud

   ```text
   npm start
   ```

## Configuration

Patient Cloud uses environment variables for configuration. Here is a list of all variables:

| Variable                        | Description                                                                                                    |
| :------------------------------ | :------------------------------------------------------------------------------------------------------------- |
| NODE_ENV                        | Set to `development` or `production`                                                                           |
| DB_URL                          | Database connection URL. Example: `mysql://login:password@host:3306/dbname`                                    |
| EMAIL_TRANSPORT_URL             | SMTP connection URL for sending mails. Example: `smtps://login:password@email-smtp.eu-central-1.amazonaws.com` |
| EMAIL_FROM                      | From email. Example: `noreply@patientcloud.ai`                                                                 |
| NODE_PORT                       | Running port: `80`                                                                                             |
| JWT_SECRET                      | JSON Web Token secret key string.                                                                              |
| FRONTEND_URL                    | Front end URL. Used for user registration activation, password recovery and oAuth redirect.                    |
| SENTRY_DSN                      | https://sentry.io connection URL                                                                               |
| NHS_OPEN_ID_ENDPOINT            | NHS OpenID endpoint. `https://auth.sandpit.signin.nhs.uk` for sandbox.                                         |
| NHS_OPEN_ID_CLIENT_ID           | NHS OpenID clientId `digital-health-passport`                                                                  |
| NHS_OPEN_ID_CLIENT_PASSWORD     | NHS OpenID password `dummy`                                                                                    |
| NHS_OPEN_ID_CLIENT_SCOPE        | NHS OpenID scope `openid profile`                                                                              |
| NHS_OPEN_ID_CLIENT_KEY          | NHS OpenID private key `private_key.pem` The key located at `/common/keys` directory                           |
| NHS_OPEN_ID_CLIENT_REDIRECT_URI | NHS OpenID redirect URL `http://localhost/auth/nhs-callback` Backend NHS callback process endpoint.            |
| GOOGLE_APPLICATION_CREDENTIALS  | Full path on local file system to Google application credentials, e.g.: /config/google.credentials.json        |
| SENTRY_DSN                      | Sentry.io connection URL e.g. https://xxxxxxxxxxxxx@o123123.ingest.sentry.io/1231233                           |
| TRAILS_URL                      | Trails audit connection URL e.g.: https://1.1.1.1:8080                                                         |
