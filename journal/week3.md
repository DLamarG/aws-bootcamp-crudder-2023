# Week 3 — Decentralized Authentication

## Installing Amplify
```
npm i aws-amplify --save
```

# Adding Amplify envars to yaml file
```
Amplify.configure({
  "AWS_PROJECT_REGION": process.env.REACT_APP_AWS_PROJECT_REGION,
  "aws_cognito_identity_pool_id": process.env.REACT_APP_AWS_COGNITO_IDENTITY_POOL_ID,
  "aws_cognito_region": process.env.REACT_APP_AWS_COGNITO_REGION,
  "aws_user_pools_id": process.env.REACT_APP_AWS_USER_POOLS_ID,
  "aws_user_pools_web_client_id": process.env.REACT_APP_CLIENT_ID,
  "oauth": {},
  Auth: {
    //We are not using an Identity Pool
    //indentityPoolId: process.env.REACT_APP_IDENTITY_POOL_ID
    region: process.env.REACT_AWS_PROJECT_REGION,
    userPoolId: process.env.REACT_APP_AWS_USER_POOLS_ID,
    userPoolWebClientId: process.env.REACT_APP_AWS_USER_POOLS_WEB_CLIENT_ID,
  });
```

```
  REACT_APP_AWS_PROJECT_REGION=
  REACT_APP_AWS_COGNITO_IDENTITY_POOL_ID=
  REACT_APP_AWS_COGNITO_REGION=
  REACT_APP_AWS_USER_POOLS_ID=
  REACT_APP_CLIENT_ID=
```

## Sign-out Page
```
import { Auth } from 'aws-amplify';

const [user, setUser] = React.useState(null);


"Update Profile Info"

import { Auth } from 'aws-amplify';

const signOut = async () => {
  try {
    await Auth.signOut({ global: true });
    window.location.href = "/"
  } catch (error) {
    console.log('error signing out: ', error);
  }
}
```

## SignUp Page
```
import { Auth } from 'aws-amplify';

const onsubmit = async (event) => {
    event.preventDefault();
    setErrors('')
    try {
        const { user } = await Auth.signUp({
            username: email,
            password: password,
            attributes: {
                name: name,
                email: email,
                preferred_username: username,
            },
            autoSignIn: {
              enabled: true,
            }
        });
        console.log(user);
        window.location.href = ' /confirm?email=${email}'
    } catch (error) {
        console.log(error);
        setErrors(error.message)
    }
    return false
  }
```
## Confirmation Page
```
import { Auth } from 'aws-amplify';


const resend_code = async (event) => {
    setCognitoErrors('')
    try {
        await Auth.resendSignUp(email);
        console.log('code resent successfully');
        setCodeSent(true)
    } catch (err) {
      // does not return a code
      // does cognito always return english
      // for this to be an okay match?
      console.log(err)
      if (err.message == 'Username cannot be empty'){
        setCognitoErrors("You need to provide an email in order to send Resend Activation Code")
      } else if (err.message == "Username/client id combination not found."){
        setCognitoErors("Email is invalid or cannot be found.")
      }
    }
}

const onsubmit = async (event) => {
    event.preventDefault();
    setCognitoErrors('')
    try {
        await Auth.confirmSignUp(email, code);
        window.location.href = "/"
    } catch (error) {
        setCognitoErrors(error.message)
    }
    return false
}
```

## Recovery Page
```
import { Auth } from 'aws-amplify';

const onsubmit_send_code = async (event) => {
    event.preventDefault();
    setErrors('')
    Auth.forgotPassword(username)
    .then((data) => setErrors(err.message) );
    .catch((err) => setCognitoErrors(err.message) );
    return false
}

const onsubmit_confirm_code =async (event) => {
    event.preventDefault();
    setErrors('')
    if (password == passwordAgain){
        Auth.forgotPasswordSubmit(username, code, password)
        .then((data) => setFormState('success'))
        .catch((err) => setErrors(err.message) );
    } else {
        setErrors('Passwords do not match')
    }
    return false
}
```

## Authenticating Server Side
```
Add in the `HomeFeedPage.js` a header eto pass along the access token

  js
  headers: {
    Authrization: `Bearer ${localStroage.getItem("access_token")}
  }
```


## SignIn Page
````
import { Auth } from 'aws-amplify';

 const onsubmit = async (event) => {
    event.preventDefault();
    try {
      const user = await Auth.signIn(email, password);
      localStorage.setItem("access_token", user.signInUserSession.accessToken.jwtToken);
      window.location.href = "/";
    } catch (error) {
      console.log('Error!', error);
      if (error.code === 'UserNotConfirmedException') {
        window.location.href = "/confirm";
      } else {
        setErrors(error.message);
      }
    }
  };
```
