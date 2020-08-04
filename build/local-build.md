# Local Build

### Running using Docker

Docker is the fastest way to run Patient Cloud API. The image located on Docker Hub [https://hub.docker.com/r/tinymedicalapps/patientcloud](https://hub.docker.com/r/tinymedicalapps/patientcloud).

With [Docker Compose](https://docs.docker.com/compose/install/) you can easily configure, install, and upgrade your Docker-based Patient Cloud installation:

Example `docker-compose.yml` with MariaDB:

```text
version: '3'
services:
  patientcloud:
    image: tinymedicalapps/patientcloud
    environment:
      - DB_URL=mysql://patientcloud:patientcloud@db:3306/patientcloud
      - NODE_PORT=80
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

4. Configure Patient Cloud environment variables and database. Check [Patient Cloud Configuration](https://github.com/TinyMedicalApps/PatientCloud-docs/tree/7fdfbb3860a45eaf8fddff03c7c3d3308b1c1cdd/containers/README.md#configuration) chapter.
5. Run Patient Cloud

   ```text
   npm start
   ```

## Configuration

Patient Cloud uses environment variables for configuration. Here is a list of all variables:

| Variable | Description |
| :--- | :--- |
| NODE\_ENV | Set to `development` or `production` |
| DB\_URL | Database connection URL. Example: `mysql://login:password@host:3306/dbname` |
| EMAIL\_TRANSPORT\_URL | SMTP connection URL for sending mails. Example: `smtps://login:password@email-smtp.eu-central-1.amazonaws.com` |
| EMAIL\_FROM | From email. Example: `noreply@patientcloud.ai` |
| NODE\_PORT | Running port: `80` |
| JWT\_SECRET | JSON Web Token secret key string. |
| FRONTEND\_URL | Front end URL. Used for user registration activation, password recovery and oAuth redirect. |
| NHS\_OPEN\_ID\_ENDPOINT | NHS OpenID endpoint. `https://auth.sandpit.signin.nhs.uk` for sandbox. |
| NHS\_OPEN\_ID\_CLIENT\_ID | NHS OpenID clientId `digital-health-passport` |
| NHS\_OPEN\_ID\_CLIENT\_PASSWORD | NHS OpenID password `dummy` |
| NHS\_OPEN\_ID\_CLIENT\_SCOPE | NHS OpenID scope `openid profile` |
| NHS\_OPEN\_ID\_CLIENT\_KEY | NHS OpenID private key `private_key.pem` The key located at `/common/keys` directory |
| NHS\_OPEN\_ID\_CLIENT\_REDIRECT\_URI | NHS OpenID redirect URL `http://localhost/auth/nhs-callback` Backend NHS callback process endpoint. |
