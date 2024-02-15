# Week 3 — Decentralized Authentication

## Installing Amplify
npm i aws-amplify --save

# Adding Amplify envars to yaml file
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


  REACT_APP_AWS_PROJECT_REGION=
  REACT_APP_AWS_COGNITO_IDENTITY_POOL_ID=
  REACT_APP_AWS_COGNITO_REGION=
  REACT_APP_AWS_USER_POOLS_ID=
  REACT_APP_CLIENT_ID=



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


## SignIn Page

import { Auth } from 'aws-amplify';

const onsubmit = async (event) => {
    setErrors('')
    event.preventDefault();
    try {
      Auth.signIn(username, password)
        .then(user => {
          localStorage.setItem("access_token", user.signInUserSession.accessToken.jwtToken)
          window.location.href = "/"
        })
        .catch(err => { console.log('Error!', err) });
    } catch (error) {
      if (error.code == 'UserNotConfirmedException') {
        window.location.href = "/confirm"
      }
      setErrors(error.message)
    }
    return false
  }
