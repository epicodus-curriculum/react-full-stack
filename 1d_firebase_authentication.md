Let's imagine that we want to expand our Help Queue's functionality for online Epicodus students. We don't want people that aren't students to access the queue — so we'll make the queue accessible only if a user is signed in. Over the next couple of lessons, we'll add this functionality. In this lesson, we'll add Firebase authentication to our `SignIn` component. Then, in the next lesson, we'll add basic authorization so that only signed-in users can add tickets.

There are many, many ways to add Firebase authentication and authorization — ranging from doing everything from scratch (including creating custom hooks for authentication) to using small open-source libraries. In the world of React, the options can be a bit overwhelming.

The solution we'll use combines the following:

* Using the simplest implementation that the Firebase documentation offers;
* Avoiding adding additional libraries if they aren't needed (we won't add any).

## Adding Firebase Authentication
---

Regardless of how we choose to incorporate Firebase authentication, we need to activate it in the Firebase console before we start adding it to our application.

Navigate to your Help Queue project in the Firebase console, expand the _Build_ menu in the left-hand menu, and then click _Authentication_.

![The _Authentication_ menu item from the _Build_ menu.](https://learnhowtoprogram.s3.us-west-2.amazonaws.com/React/Week-4-React-2020/firebase-authentication.png)

By default, authentication isn't enabled. So to get started, we'll click the _Get started_ button. This will open to the _Sign-in method_ tab within the authentication console. 

We'll see quite a few different ways Firebase can authenticate users ranging from an email and password to sign in with Google, Facebook or GitHub. We'll use an email and password to have users sign in.

So, within the _Sign-in providers_ section, select _Email/Password_. In the image below this is circled in red:

![Select _Email/Password_ from the many listed options for _Sign-in providers_.](https://learnhowtoprogram.s3.us-west-2.amazonaws.com/React/Week-4-React-2020/firebase-auth-email-password-option.png)

On the next screen make sure to enable the first option _Email/Password_. We're not going to work with an email link (a passwordless sign in), though you are welcome to explore that on your own time. Finally, click the _Save_ button to save your selection and complete the setup process. 

We'll now see _Email/Password_ listed under a _Sign-in providers_ section.

![_Email/Password_ is not listed as an active authentication provider in the _Authentication_ section of the Firebase console. Also, the _Users_ tab is highlighted in a red circle.](https://learnhowtoprogram.s3.us-west-2.amazonaws.com/React/Week-4-React-2020/firebase-auth-users-tab-option.png)

Note that there is a _Users_ tab at the upper left corner of the screen. This is circled in red in the above image. If we click on this tab, we'll see there are no users yet. This is a great place to add admins manually for a smaller site so other users can't access that functionality via the browser. We won't add users manually right now but keep this in mind if you just want admins to access the UI of a site.

### Accessing Authentication in our Source Code

Now that we've set up Firebase authentication in the Firebase console, we need to update our Firebase configuration to configure authentication locally. We'll do this with [the `getAuth()` function](https://firebase.google.com/docs/reference/js/auth.md#getauth). 

Here's the updated code:

<div class="filename">src/firebase.js</div>

```js
import { initializeApp } from "firebase/app";
import { getFirestore } from 'firebase/firestore';
import { getAuth } from "firebase/auth";

const firebaseConfig = {
  apiKey: process.env.REACT_APP_FIREBASE_API_KEY,
  authDomain: process.env.REACT_APP_FIREBASE_AUTH_DOMAIN,
  projectId: process.env.REACT_APP_FIREBASE_PROJECT_ID,
  storageBucket: process.env.REACT_APP_FIREBASE_STORAGE_BUCKET,
  messagingSenderId: process.env.REACT_APP_FIREBASE_SENDER_ID,
  appId: process.env.REACT_APP_FIREBASE_APP_ID 
};

const app = initializeApp(firebaseConfig);
const auth = getAuth(app);
const db = getFirestore(app);

export { db, auth };
```

First we import our `getAuth()` function from `"firebase/auth"`.

Similar to the `getFirestore()` function, `getAuth()` returns [an `Auth` instance](https://firebase.google.com/docs/reference/js/auth.auth.md#auth_interface) that's associated with our firebase app. In the code above, we save our `Auth` instance in a variable called `auth`. Later when we need to access authentication functions, we'll use this variable to reference the authentication that's associated with our Help Queue web app, and to get valuable information like the currently signed in user.

Since we'll be using this variable in other parts of our code, we'll need to export it, so take note of the updated export statement:

```js
export { db, auth };
```

Now, we're exporting an object with two variables inside of it. With this change, the `db` variable is no longer a **default** export. Instead, both `db` and `auth` are now called **named** exports. This means that when we import these variables in other files, the names of the imports need to exactly match the names of the exported variables. So, our very next step needs to be updating the import statement for `db` in `TicketControl.js` to destructure the `db` variable from the object we exported from `firebase.js`:

<div class="filename">src/components/TicketControl.js</div>

```js
import { db } from './../firebase.js'
```

Now we're ready to create sign up, sign in, and sign out forms. In the interest of keeping things simple, we'll add all of this functionality to the same component, `SignIn`.

### Signing Up

Within the `SignIn` component, we'll start by creating a sign up form. 

<div class="filename">src/components/SignIn.js</div>

```js
import React from "react";

function SignIn(){  

  return (
    <React.Fragment>
      <h1>Sign up</h1>
      <form onSubmit={doSignUp}>
        <input
          type='text'
          name='email'
          placeholder='email' />
        <input
          type='password'
          name='password'
          placeholder='Password' />
        <button type='submit'>Sign up</button>
      </form>
    </React.Fragment>
  );
}

export default SignIn
```

We create a simple form that will trigger a function called `doSignUp()` on submission. This will have two fields: `email` and `password`. (A password confirmation field would be nice but we are keeping this simple.) Note that the `password` field has a `type` attribute set to `password` so the characters won't be visible.

Now let's write the corresponding `doSignUp()` function. Here's the new code:

<div class="filename">src/components/SignIn.js</div>

```js
import React from "react";
import { auth } from "./../firebase.js";
import { createUserWithEmailAndPassword } from "firebase/auth";

function SignIn(){  
  function doSignUp(event) {
    event.preventDefault();
    const email = event.target.email.value;
    const password = event.target.password.value;
    createUserWithEmailAndPassword(auth, email, password)
      .then((userCredential) => {
        // User successfully signed up 
      })
      .catch((error) => {
        // There was an error with sign up
      });
  }

  return (
    ...
  );
}

export default SignIn
```

To sign up a new user, we need to import our `Auth` instance from `firebase.js` and a function called `createUserWithEmailAndPassword` from `firebase/auth`, so we have two new import statements at the top of the file.

Then we have our `doSignUp()` function. In this function, we create an `event` parameter and call `event.preventDefault()` to prevent the default behavior of submitting a form (a page reload). Then, we grab the value of the `email` and the `password` from the form.

Next, Firebase authentication comes into play. Let's take a closer look at that code:

<div class="filename">src/components/SignIn.js</div>

```js
createUserWithEmailAndPassword(auth, email, password)
  .then((userCredential) => {
    // User successfully signed up 
  })
  .catch((error) => {
    // There was an error with sign up
  });
```

The `createUserWithEmailAndPassword()` function takes three arguments: the auth instance, an email, and a password. It returns a promise, which means we can attach `.then()` to it to handle a successful response. We can also attach a `.catch()` to catch any errors. In either case, we'll want to let the user know whether or not their sign up was successful, so let's use our `useState` hook to do just that. 

Here's the new code:

<div class="filename">src/components/SignIn.js</div>

```js
// New import below!
import React, { useState } from "react";

function SignIn(){  
  // New code below!
  const [signUpSuccess, setSignUpSuccess] = useState(null);

  function doSignUp(event) {
    event.preventDefault();
    const email = event.target.email.value;
    const password = event.target.password.value;
    createUserWithEmailAndPassword(auth, email, password)
      .then((userCredential) => {
        // New code below!
        setSignUpSuccess(`You've successfully signed up, ${userCredential.user.email}!`)
      })
      .catch((error) => {
        // New code below!
        setSignUpSuccess(`There was an error signing up: ${error.message}!`)
      });
  }

  return (
    <React.Fragment>
      <h1>Sign up</h1>
      {/* New code below! */}
      {signUpSuccess}
      <form onSubmit={doSignUp}>
        <input
          type='text'
          name='email'
          placeholder='email' />
        <input
          type='password'
          name='password'
          placeholder='Password' />
        <button type='submit'>Sign up</button>
      </form>
    </React.Fragment>
  );
}

export default SignIn
```

With these updates we create a state variable called `signUpSuccess` that will deliver a message to the user when they sign up: a success message, or an error message. Its initial value is set to `null` so that it won't display unless a sign up event happens and there's a response (successful or not).

We've also made use of some helpful information from the `userCredential` parameter that's passed into the `.then()` on a successful sign up:

```js
`You've successfully signed up, ${userCredential.user.email}!`
```

The `userCredential` parameter represents a Firebase [`UserCredential`](https://firebase.google.com/docs/reference/js/auth.usercredential.md#usercredential_interface) object. This object has a property called `user`, which represents the newly created user. In terms of Firebase object types this user is a [`User` object](https://firebase.google.com/docs/reference/js/auth.user.md#user_interface) that also extends functionality from the [`UserInfo` class](https://firebase.google.com/docs/reference/js/auth.userinfo.md#userinfo_interface). That's why we can access information about the new user like their email! 

At this point, we can run our application and sign up a new user. Take note that your password will need to be at least 6 characters long. Try it out — and check out the _Users_ tab in the _Authentication_ area within the Firebase console. You'll see that a new user has been added!

### Signing In

The process looks very similar for signing in. To keep things simple, we'll place this functionality in the same component we've been using for auth. Here's the form:

<div class="filename">src/components/SignIn.js</div>

```js
...

function SignIn(){  
  ...

  return (
    <React.Fragment>
      {/* Signup form lives here */}

      <h1>Sign In</h1>
      <form onSubmit={doSignIn}>
        <input
          type='text'
          name='signinEmail'
          placeholder='email' />
        <input
          type='password'
          name='signinPassword'
          placeholder='Password' />
        <button type='submit'>Sign in</button>
      </form>
    </React.Fragment>
  );
}

export default SignIn
```

It's almost exactly the same as our sign in form, just with different variable names.

Next, let's create the `doSignIn()` function. We'll need to make use of a new function from `firebase/auth`, and we'll also create a new state variable to hold a success or error message for the sign in process. 

<div class="filename">src/components/SignIn.js</div>

```js
...
// new import below!
import { createUserWithEmailAndPassword, signInWithEmailAndPassword } from "firebase/auth";

function SignIn(){  
  const [signUpSuccess, setSignUpSuccess] = useState(null);
  // new state variable
  const [signInSuccess, setSignInSuccess] = useState(null);

  function doSignUp(event) {
    ...
  }

  // new sign in function
  function doSignIn(event) {
    event.preventDefault();
    const email = event.target.signinEmail.value;
    const password = event.target.signinPassword.value;
    signInWithEmailAndPassword(auth, email, password)
      .then((userCredential) => {
        setSignInSuccess(`You've successfully signed in as ${userCredential.user.email}!`)
      })
      .catch((error) => {
        setSignInSuccess(`There was an error signing in: ${error.message}!`)
      });
  }

  return (
    <React.Fragment>
      ...

      <h1>Sign In</h1>
      {/* New sign in success message*/}
      {signInSuccess}
      <form onSubmit={doSignIn}>
        <input
          type='text'
          name='signinEmail'
          placeholder='email' />
        <input
          type='password'
          name='signinPassword'
          placeholder='Password' />
        <button type='submit'>Sign in</button>
      </form>
    </React.Fragment>
  );
}

export default SignIn
```

The new `doSignIn()` function looks very similar to the function we just created for signing up. The main difference here is that we are using the `signInWithEmailAndPassword()` method, and the variable names are for signing _in_. 

### Signing Out

Signing out doesn't even require a form. We just need a sign-out button in our jsx that triggers the `signOut()` function from `firebase/auth`. 

Here's the new code:

<div class="filename">src/components/SignIn.js</div>

```js
import React, { useState } from "react";
import { auth } from "./../firebase.js";
// new import below!
import { createUserWithEmailAndPassword, signInWithEmailAndPassword, signOut } from "firebase/auth";

function SignIn(){  
  const [signUpSuccess, setSignUpSuccess] = useState(null);
  const [signInSuccess, setSignInSuccess] = useState(null);
  // new state variable
  const [signOutSuccess, setSignOutSuccess] = useState(null);

  function doSignUp(event) {
    ...
  }

  function doSignIn(event) {
    ...
  }

  // new function
  function doSignOut() {
    signOut(auth)
      .then(function() {
        setSignOutSuccess("You have successfully signed out!");
      }).catch(function(error) {
        setSignOutSuccess(`There was an error signing out: ${error.message}!`);
      });
  }

  return (
    <React.Fragment>
      ...

      {/* New sign out button*/}
      <h1>Sign Out</h1>
      {signOutSuccess}
      <br />
      <button onClick={doSignOut}>Sign out</button>
    </React.Fragment>
  );
}

export default SignIn
```

In the new code we do a few things:

* We import the `signOut` function from `firebase/auth`.
* We create a state variable called `signOutSuccess` that will hold the sign out success or failure message.
* We create a `doSignOut()` function that calls the Firebase `signOut()` function. `signOut()` takes one argument: our `Auth` instance, `auth`, which contains all of the information of the currently signed in user that Firebase needs to sign that user out. 
* In the `return` statement, we add a new "Sign Out" area that shows a button and the `SignOutSuccess` message.

At this point, we can sign up, sign in, and sign out — but none of this is very helpful without authorization. We'll cover that in the next lesson.
