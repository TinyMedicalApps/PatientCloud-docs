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
