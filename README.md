## What is Patient Cloud

Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.

Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.

## Getting started on local machine

### Running using Docker

Docker is the fastest way to run Patient Cloud API. The image located on Docker Hub [https://hub.docker.com/r/tinymedicalapps/patientcloud](https://hub.docker.com/r/tinymedicalapps/patientcloud). 

With [Docker Compose](https://docs.docker.com/compose/install/) you can easily configure, install, and upgrade your Docker-based Patient Cloud installation:

Example `docker-compose.yml` with MariaDB:

```Dockerfile
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

```markdown
docker-compose up -d
```

### Running using Node.js

1. Clone Patient Cloud repository:
```markdown
git clone https://github.com/TinyMedicalApps/PatientCloud.git .
```
2. Change directory:
```markdown
cd PatientCloud
```

3. Install dependencies:
```markdown
npm install
```

4. Configure Patient Cloud environment variables and database. Check [Patient Cloud Configuration](#configuration) chapter.

5. Run Patient Cloud
```markdown
npm start
```

## Configuration

Patient Cloud uses environment variables for configuration. Here is a list of all variables:

| Variable      | Description |
| :---  | :---  |
| NODE_ENV      | Set to `development` or `production`       |
| DB_URL   | Database connection URL. Example: `mysql://login:password@host:3306/dbname` |
| EMAIL_TRANSPORT_URL   | SMTP connection URL for sending mails. Example: `smtps://login:password@email-smtp.eu-central-1.amazonaws.com` |
| EMAIL_FROM   | From email. Example: `noreply@patientcloud.ai` |
| NODE_PORT   | Running port: `80` |
| JWT_SECRET   | JSON Web Token secret key string. |
| FRONTEND_URL   | Front end URL. Used for user registration activation, password recovery and oAuth redirect.  |
| NHS_OPEN_ID_ENDPOINT   | NHS OpenID endpoint. `https://auth.sandpit.signin.nhs.uk` for sandbox. |
| NHS_OPEN_ID_CLIENT_ID   | NHS OpenID clientId `digital-health-passport` |
| NHS_OPEN_ID_CLIENT_PASSWORD   | NHS OpenID password `dummy` |
| NHS_OPEN_ID_CLIENT_SCOPE   | NHS OpenID scope `openid profile` |
| NHS_OPEN_ID_CLIENT_KEY   | NHS OpenID private key `private_key.pem` The key located at `/common/keys` directory |
| NHS_OPEN_ID_CLIENT_REDIRECT_URI   | NHS OpenID redirect URL `http://localhost/auth/nhs-callback` Backend NHS callback process endpoint. |


## NHS Login button on a front end client and showing the logged in user

Basically it takes two steps to authenticate a user from at your frontend:
1. Redirect user to authentication endpoint `http://localhost/auth/nhs-login`
2. Receive a JWT token on frontend's URL `/users/login-success/JWT_TOKEN` or in case of error `/users/login-failure` will be called.

Login button example:

```html
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