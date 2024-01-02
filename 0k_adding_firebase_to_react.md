We're ready to connect our Help Queue application to Firebase! To do this, we'll need to complete three steps:

* Install `firebase`.
* Hide sensitive Firebase configuration data in a `.env` file.
* Create a Firebase configuration file.

And that's it! 

At the end of this lesson, we'll briefly talk about the possibility of using a **binding library** that offers tooling specifically for React applications that implement Firebase.

Go ahead and get started by opening up your Help Queue repo. Continue with the same repo that we previously refactored to use the `useState` hook. 

## Step 1: Install Firebase
---

First, we'll need to install Firebase in our project:

```javascript
npm install firebase@9
```

Note that it's important to use the version pinned in this lesson. Because Firebase changes frequently, using a different version may mean different steps to setting up your application's configuration.

### Step 2: Add `.env` File

First, head on over to your Help Queue project settings in Firebase to get the Firebase configuration settings. From the project homepage, select the cog icon at the top-left of the screen, select _Project settings_, and scroll down until you find the help-queue-web app that we created in the last lesson. In this section, you'll find a code snippet that has a `firebaseConfig` variable that looks something like this:

```js
const firebaseConfig = {
  apiKey: "YOUR-UNIQUE-CREDENTIALS",
  authDomain: "YOUR-PROJECT-NAME.firebaseapp.com",
  projectId: "YOUR-UNIQUE-PROJECT-NAME",
  storageBucket: "YOUR-UNIQUE-URL",
  messagingSenderId: "YOUR-UNIQUE-CREDENTIALS",
  appId: "YOUR-UNIQUE-APPID"
};
```

Note that we have replaced all the values of the key-value pairs with generic placeholders. That's because the values for each Firebase application will be different; more importantly, it's sensitive data that we don't want to expose to the internet for anyone to access and use, including potentially malicious users. To conceal this information, we need to set up a `.env` file and an environment variable for each unique key in the `firebaseConfig` object.

Fortunately, `create-react-app` automatically comes with `dotenv`, the npm package we used in Intermediate JavaScript to store sensitive API keys in an `.env` file.

First, we need to add `.env` to our `.gitignore` file and make a commit. This step needs to come before we make the `.env` file itself. As you may recall from Intermediate JavaScript, if we push an updated `.gitignore` file at the same time as we push the file that should be ignored, GitHub won't know it's supposed to ignore it — meaning it will be added to the repo.  

Note that `create-react-app` automatically adds a number of these kinds of files to our `.gitignore` including `.env.local`, `.env.development.local`, and so on. `create-react-app` does this because in larger projects, it can be helpful to have multiple files for environment variables. They can be split up for testing, production, and development. For more information on different environment variable file types in `create-react-app`, see [Adding Custom Environment Variables](https://create-react-app.dev/docs/adding-custom-environment-variables/). Since our application is small, we will stick to the basic `.env` file. 

Next, create a `.env` file in the root directory of the project. Environment variables can only be set up for strings, not objects. For that reason, each key-value pair in the `firebaseConfig` object needs to be broken down into its own constant like this:

<div class="filename">.env</div>

```js
REACT_APP_FIREBASE_API_KEY = "YOUR-UNIQUE-CREDENTIALS"
REACT_APP_FIREBASE_AUTH_DOMAIN = "YOUR-PROJECT-NAME.firebaseapp.com"
REACT_APP_FIREBASE_PROJECT_ID = "YOUR-PROJECT-FIREBASE-PROJECT-ID"
REACT_APP_FIREBASE_STORAGE_BUCKET = "YOUR-PROJECT-NAME.appspot.com"
REACT_APP_FIREBASE_MESSAGING_SENDER_ID = "YOUR-PROJECT-SENDER-ID"
REACT_APP_FIREBASE_APP_ID = "YOUR-PROJECT-APP-ID"
```

Replace the placeholder string values above with the value of each key from your own Firebase application.

**Note:** It is very important that every environment variable in your application is prefaced by `REACT_APP`. Otherwise, the environment variable **won't work**. This is a safeguard put in place by `create-react-app` to ensure that sensitive environment variables aren't accidentally exposed in our applications.

### Step 3: Create Configuration File and Initialize Firebase

Next, we'll create a file in our `src` directory called `firebase.js`. This is where we'll initialize Firebase in our application and create a database reference. Note that Firebase is not prescriptive about what we name our configuration file or where we located, so it's up to us to pick a name and location that is sensible and intuitive.

Add the following code to the file:

<div class="filename">src/firebase.js</div>

```js
import { initializeApp } from "firebase/app";
import { getFirestore } from 'firebase/firestore';

const firebaseConfig = {
  apiKey: process.env.REACT_APP_FIREBASE_API_KEY,
  authDomain: process.env.REACT_APP_FIREBASE_AUTH_DOMAIN,
  projectId: process.env.REACT_APP_FIREBASE_PROJECT_ID,
  storageBucket: process.env.REACT_APP_FIREBASE_STORAGE_BUCKET,
  messagingSenderId: process.env.REACT_APP_FIREBASE_SENDER_ID,
  appId: process.env.REACT_APP_FIREBASE_APP_ID 
}

const app = initializeApp(firebaseConfig);
const db = getFirestore(app);

export default db;
```

The scripts in this file are the same configuration scripts that we copied from the Help Queue project settings in the Firebase console, except for a two differences: 

* First, all the values in our `firebaseConfig` are environment variables. We aren't exposing our sensitive data.
* Second, we import the `getFirestore` function from `firebase/firestore` to use at the bottom of the file to get access to our Firestore database. 

Now, let's work through `firebase.js` from top to bottom:

1. We start by importing `firebase/app` and `firebase/firestore`.
2. Then we define our firebase config in `firebaseConfig`. This information points to the exact web application that we created within the Firebase Help Queue project.
3. Next, we call the `initializeApp` function, passing in our `firebaseConfig` as the argument. The `initializeApp` function creates and initializes an instance of our Firebase web app, which we save in the variable `app`. We can then use `app` to access a variety of services that are connected to our web app, like our Firestore database.
4. Next, we call the `getFirestore` function, passing in `app`. This function returns the Firestore database instance that's associated with our `app`. We store our firestore database instance in the variable `db`. 
5. Finally, we export the `db` variable, as our only and default export. We'll use this variable when we make requests to our database to read and update data.

At this point, we've successfully added Firebase and Firestore to our application. Woo-hoo! 

## Binding Libraries
---

Our next step is to actually start communicating with our database. For this, we actually have two options:

* Use Firebase's built-in functions and tools to communicate with our database.
* Use [an external library](https://firebaseopensource.com/platform/web/) that provides tools for using Firebase that are meant to work specifically with React. These libraries are often called **binding libraries**, as they help two separate libraries work together.

Binding libraries can provide some abstractions that can be confusing to folks who are new to React. Some of these abstractions are based on using React context, which we won't work with until the next course section. What's more, binding libraries can cause confusion about what tooling or helper function comes from where.

For all of these reasons, we're going to stick to learning how to use Firebase's built-in functions and tools. We'll also learn how to reference the firebase docs and we'll get continued practice using React's built-in tools, like the `useEffect` hook.

However, if you plan on using React and Firebase in your capstone project, we recommend exploring the available binding libraries for React and Firebase to see if they can improve your development experience, or move you past any issues that you run into when implementing a feature from Firebase.