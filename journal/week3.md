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
