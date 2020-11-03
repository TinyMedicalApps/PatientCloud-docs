# Social Login

## Identity Providers

1. Email/Password
2. Facebook
3. Google
4. NHS

## Obtaining a JWT token on Identity Platform

Install The Firebase JavaScript SDK and use your `apiKey` and `authDomain` to initialize Firebase App.

1. Firebase JavaScript SDK reference (https://firebase.google.com/docs/reference/js)
2. Firebase JavaScript SDK npm package (https://www.npmjs.com/package/firebase)
3. Firebase JavaScript SDK auth reference (https://firebase.google.com/docs/reference/js/firebase.auth)

```html
<script src="https://www.gstatic.com/firebasejs/7.19.0/firebase.js"></script>
<script>
  var config = {
    apiKey: 'YOUR_API_KEY_SECRET',
    authDomain: 'YOUR_AUTH_DOMAIN',
  };
  firebase.initializeApp(config);

  var fbProvider = new firebase.auth.FacebookAuthProvider();
  var googleProvider = new firebase.auth.GoogleAuthProvider();
  var nhsProvider = new firebase.auth.OAuthProvider('oidc.nhs');

  firebase.auth().onAuthStateChanged(function (user) {
    if (user) {
      document.getElementById('message').innerHTML = 'Welcome, ' + user.email;
    } else {
      document.getElementById('message').innerHTML = 'No user signed in.';
    }
  });

  function auth(provider) {
    firebase
      .auth()
      .signInWithPopup(provider)
      .then(function (result) {
        firebase
          .auth()
          .currentUser.getIdToken(/* forceRefresh */ true)
          .then(function (idToken) {
            document.getElementById('token').innerHTML = idToken;
          })
          .catch(function (error) {
            console.log('error', error);
          });
        console.log('User information: ', result);
      });
  }
</script>
<div>Identity Platform Quickstart</div>
<div id="message">Loading...</div>
<div id="token"></div>
<button onclick="auth(fbProvider);">Facebook SignIn</button>
<button onclick="auth(googleProvider);">Google SignIn</button>
<button onclick="auth(nhsProvider);">NHS SignIn</button>
```

## Accessing backend

We have different backend for each environment

| Name       | API endpoint                      | Swagger URL                           |
| :--------- | :-------------------------------- | ------------------------------------- |
| Stage      | `https://sandpit.patientcloud.ai` | `https://sandpit.patientcloud.ai/api` |
| Production | `https://api.patientcloud.ai`     | `https://api.patientcloud.ai/api`     |

### Swagger

Add Identity Platform JWT token to bearer input field. Press 'Authorize' button to access bearer input field.

### Fetch API

```js
const jwtToken = 'eyJhbGciOi...'; // the token we received from Identity Platform
const data = fetch('https://sandpit.patientcloud.ai/auth/test', {
      method: 'GET',
      headers: {
        'Content-Type': 'application/json',
        Authorization: `bearer ${jwtToken}`
      }
    })
      .then(response => response.json())
      .catch(error => console.error(error));
  };
```

## Getting Google Identity authorization through NestJS's social login

Redirect user to NestJS social endpoints to get Google Identity custom token.

| Social Network | API endpoint      |
| :------------- | :---------------- |
| Google         | `/auth/google`    |
| LinkedIn       | `/auth/linkedin`  |
| Facebook       | `/auth/facebook`  |
| NHS            | `/auth/nhs-login` |

After successful registration FRONTEND_URL will be called with GI custom token in URL. You need to use the token to get GI authorization on frontend app.

### Google Identity custom token authorization example

Let's set our FRONTEND_URL to https://example.com. After successful authentication https://example.com/users/login-success/CUSTOM_TOKEN will be called. In case of failure https://example.com/users/login-failure will be called.

```html
<script src="https://www.gstatic.com/firebasejs/7.19.0/firebase.js"></script>
<script>
  var config = {
    apiKey: 'YOUR_API_KEY_SECRET',
    authDomain: 'YOUR_AUTH_DOMAIN',
  };
  firebase.initializeApp(config);

  firebase.auth().onAuthStateChanged(function (user) {
    if (user) {
      document.getElementById('message').innerHTML = 'Welcome, ' + user.email;
    } else {
      document.getElementById('message').innerHTML = 'No user signed in.';
    }
  });

  //token - this is the token we get from NestJS. The CUSTOM_TOKEN from URL
  firebase.auth().signInWithCustomToken(token).catch(function(error) {
  // Handle Errors here.
  var errorCode = error.code;
  var errorMessage = error.message;
  // ...
});
```

### Google Identity custom token workflow

1. NestJS obtains user information from oAuth token and looks for email in backend database, if no email found the new user will be added to database and to Google Identity.
2. NestJS sends back to frontend Google Identity token
3. Frontend uses the token to authenticate itself.
