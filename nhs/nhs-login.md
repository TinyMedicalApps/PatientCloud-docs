# NHS Login

## NHS OpenID Connect scope

There is no email scope available on NHS OpenID Connect by default. You have to ask support to enable it.

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
