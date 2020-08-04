# README

## What is Patient Cloud

Patient Cloud is being designed by Tiny Medical Apps to improve the developer experience when connecting to NHS APIs such as NHS Login and FHIR Services. It is a thin microservice nodejs architecture \(using NestJS\). Initially connecting to NHS Login Sandpit and FHIR enabled synthetic servers. On our roadmap is:

* [x] NHS Login
* [ ] FHIR Server Proxy
* [ ] GraphQL warapper for FHIR APIs
* [ ] National Event Managemnt Service \(NEMS\)
* [ ] National Record Locator \(NRL\)
* [ ] NHS Directory of Services
* [ ] NHS Content

## Getting started on local machine

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

4. Configure Patient Cloud environment variables and database. Check [Patient Cloud Configuration](./#configuration) chapter.
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

## NHS Login button on a front end client and showing the logged in user

Basically it takes two steps to authenticate a user from at your frontend: 1. Redirect user to authentication endpoint `http://localhost/auth/nhs-login` 2. Receive a JWT token on frontend's URL `/users/login-success/JWT_TOKEN` or in case of error `/users/login-failure` will be called.

Login button example:

```markup
<a href="http://localhost/auth/nhs-login">NHS Login</a>
```

Successful login endpoint. Next.js React example:

```jsx
import { useRouter } from 'next/router';
import jwt from 'jsonwebtoken';

const WithStaticProps = () => {
  const router = useRouter();
  const { token } = router.query;
  const decoded = jwt.decode(token);

  return (
    <div>
      <h1>Login successful</h1>
      <p>Received a JWT</p>
      {decoded && (
        <>
          <p>id: {decoded.id}</p>
          <p>name: {decoded.name}</p>
          <p>email: {decoded.email}</p>
          <p>role: {decoded.role}</p>
        </>
      )}
      <p>
        <Link href="/">
          <a>Go home</a>
        </Link>
      </p>
    </div>
  );
};

export default WithStaticProps;
```

