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

