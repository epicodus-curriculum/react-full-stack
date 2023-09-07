We now have the ability to add tickets to Firestore in our Help Queue application. However, we can't see the tickets in our application yet. There are two ways we can get data from Firestore:

1. We can get all documents in a collection once with the `getDocs()` method. This is very similar to what Rails/.NET coders do with their respective frameworks. When data is needed, we make a request to Firestore.

2. We can set up a listener that actively listens for realtime changes in Firestore. Whenever Firestore is updated, our application will get a snapshot of the data and update our app accordingly.

If we were to go with the first option, we'd have to set up code in our app that calls the `getDocs()` function anytime a ticket gets added, updated, or deleted so that our app is up-to-date with our database. However, that's the exact out-of-the-box functionality that we would get by setting up a listener to listen for realtime updates to our database. So, we'll go with option #2. 

## Reading Firestore Data
---

We'll add our listener to the `TicketControl` component, so that we can update our `mainTicketList` state variable with the data we retrieve from the Firestore database.

To properly set up this listener, we'll need to set up a `useEffect` hooks that does a few things:

* Runs once after our component first renders, 
* Sets up an `onSnapshot` listener that gets all of the ticket data in the `tickets` collection and adds it to an array,
* Calls `setMainTicketList()` passing in the array of tickets in order to update our `mainTicketList` state variable. This in turn will trigger a re-render to our `TicketControl` component, and it will display the updated ticket data.

We'll do this in three phases. In the first phase, we'll set up our `useEffect()` hook and learn the basics of the `onSnapshot()` function. Here's the first round of new code:

<div class="filename">src/components/TicketControl.js</div>

```js
...
// new import!
import { collection, addDoc, onSnapshot } from "firebase/firestore";
import db from './../firebase.js'

function TicketControl() {
    const [formVisibleOnPage, setFormVisibleOnPage] = useState(false);
    const [mainTicketList, setMainTicketList] = useState([]);
    const [selectedTicket, setSelectedTicket] = useState(null);
    const [editing, setEditing] = useState(false);

  useEffect(() => { 
    const unSubscribe = onSnapshot(
      collection(db, "tickets"), 
      (collectionSnapshot) => {
        // do something with ticket data
      }, 
      (error) => {
        // do something with error
      }
    );

    return () => unSubscribe();
  }, []);

  ...

} 

export default TicketControl;
```

First, make sure to import the `onSnapshot` function from `'firebase/firestore'`.

Let's notice a few things about the `useEffect()` hook:

* We've passed in an empty array as the second argument, which means our effect will run once after our component's first render. Just like with event listeners, we only want to create our Firestore database listener once. 
* We return a cleanup function for the `useEffect()` hook to run. `useEffect()` will call this function when the `TicketControl` component unmounts, and it will unsubscribe our database listener; by "unsubscribe", we mean to stop the listener.
* The side effect that we run is creating the `onSnapshot()` listener that listens to changes in our database.

Now let's examine the `onSnapshot()` function:

* First note that we can set up a database listener to listen for changes on a document, a set of documents, or an entire collection. In our case, we're listening for changes to the `tickets` collection. 
* The `onSnapshot()` function takes three arguments:
  * A document or collection reference that we want our listener to listen to.
  * A callback function to handle a successful request. This function will be called the first time that we set up our listener, and anytime there's a change to the `tickets` collection. 
  * A callback function to handle errors that happen when making a database request.
* The `onSnapshot()` function returns a function that we can call at any point to stop the listener. We save this returned function in a variable called `unSubscribe`. We could call this variable anything, like `stop` or `clearListener`.

### Handling a Successful Response

Now that we have a sense of the basics of our new `useEffect()` hook and the `onSnapshot()` function, let's add code to handle a successful response. 

Here's the new code:

<div class="filename">src/components/TicketControl.js</div>

```js
...

function TicketControl() {
  const [formVisibleOnPage, setFormVisibleOnPage] = useState(false);
  const [mainTicketList, setMainTicketList] = useState([]);
  const [selectedTicket, setSelectedTicket] = useState(null);
  const [editing, setEditing] = useState(false);

  useEffect(() => { 
    const unSubscribe = onSnapshot(
      collection(db, "tickets"), 
      (collectionSnapshot) => {
        const tickets = [];
        collectionSnapshot.forEach((doc) => {
            tickets.push({
              names: doc.data().names, 
              location: doc.data().location, 
              issue: doc.data().issue, 
              id: doc.id
            });
        });
        setMainTicketList(tickets);
      }, 
      (error) => {
        // do something with error
      }
    );

    return () => unSubscribe();
  }, []);

  ...

} 

export default TicketControl;
```

Let's summarize what we're doing with this code: we're looping through the collection of returned ticket documents to construct an array of JavaScript ticket objects. When we've finished constructing the array, we call `setMainTicketList()` to update the `mainTicketList` state variable with the array of tickets.

There's a few things to note in this process. 

First, it's important to note that how the Firestore database stores our data is not the same as how we structure that same data in JavaScript. That's why we need to manually create a JavaScript array, loop through the returned collection (represented by the `collectionSnapshot` parameter), create a JavaScript object for each ticket, and push it to our array. 

Second, it's during this process that we create our ticket object's `id` property and set it to the auto-generated id from Firestore. We can access the document identifier by accessing the `id` property of each document in the returned collection: 

```js
collectionSnapshot.forEach((doc) => {
    tickets.push({
      ...
      id: doc.id // this code
    });
});
```

Third, we need to take a closer look at the Firestore object types that we're accessing here. As previously noted, the `collectionSnapshot` parameter represents the response from our database. We can name this parameter whatever we want, but since we're accessing a collection, we descriptively call our parameter `collectionSnapshot`. In terms of Firestore object types, this parameter is a [`QuerySnapshot`](https://firebase.google.com/docs/reference/js/firestore_.querysnapshot) object that's made up of one or more [`DocumentSnapshot`](https://firebase.google.com/docs/reference/js/firestore_.documentsnapshot) objects. Each of these object types have their own properties and methods. This is important to note, because when we call `collectionSnapshot.forEach(...)`, we're actually calling a [`QuerySnapshot`](https://firebase.google.com/docs/reference/js/firestore_.querysnapshot) method, and not JavaScript's `Array.prototype.forEach()` method. 

However, [`QuerySnapshot`](https://firebase.google.com/docs/reference/js/firestore_.querysnapshot) has a handy `docs` property that returns an array of the collection's data. That means we can call any JavaScript array method on `collectionSnapshot.docs`. Here's an example of using `Array.prototype.map()` instead of `Array.prototype.forEach()`:

```js
const tickets = collectionSnapshot.docs.map((doc) => {
  return {
    names: doc.data().names, 
    location: doc.data().location, 
    issue: doc.data().issue, 
    id: doc.id
  };
});
```

The lesson here is that you should always check the [API reference](https://firebase.google.com/docs/reference/js/firestore_) of the tools you are working with when you run into issues doing something you expect you might be able to do. Why the API reference? It lists object types (also called "classes") in detail, including any properties and methods of those objects, as well as the parameter and return types for any functions. 

Since each document in the `CollectionSnapshot` that we're looping through is a `DocumentSnapshot` object, we need to use the methods available for that object type to access the document's data. In our code, we're using the `DocumentSnapshot.data()` method, but we could use [the `DocumentSnapshot.get()` method](https://firebase.google.com/docs/reference/js/firestore_.documentsnapshot.md#documentsnapshotget) instead, but we'll leave that for further exploration.

The `DocumentSnapshot.data()` method returns all of a documents data in the form of a JavaScript object, mapping over the Firestore document fields and values to JS object keys and values. So for example, in `doc.data().names`:

* `doc` accesses the Firestore document, a `DocumentSnapshot` object.
* `.data()` returns the Firestore document's data into a JavaScript object.
* `.names` accesses the `names` key to get its value.

Because `.data()` transforms all of a document's data into a JS object, we could shorten our code with the [spread operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax):

```js
const unSubscribe = onSnapshot(
  collection(db, "tickets"), 
  (collectionSnapshot) => {
    const tickets = [];
    collectionSnapshot.forEach((doc) => {
        tickets.push({
          ... doc.data(), // Spread operator in use!
          id: doc.id
        });
    });
    setMainTicketList(tickets);
  }, 
  (error) => {
    // do something with error
  }
);
```

Updating your code to use the spread operator is entirely optional, and you should only do it if you understand how it works. 

As always, there's many ways to structure our code. To learn about other handy methods and properties for `DocumentSnapshot` and `QuerySnapshot`, take a look at the Firestore API reference when you have the time:

* [`DocumentSnapshot`](https://firebase.google.com/docs/reference/js/firestore_.documentsnapshot)
* [`QuerySnapshot`](https://firebase.google.com/docs/reference/js/firestore_.querysnapshot)

The last thing to note with the addition of this new code is actually a reminder: all of the code in this first callback function will run every time there's an update in our Firestore database. This is all thanks to the built-in functionality of the `onSnapshot()` function!

Next, let's handle errors.

### Handling Errors

As described on the docs for [handling listener errors](https://firebase.google.com/docs/firestore/query-data/listen), these can be caused by issues with security permissions or invalid queries (like listening to a collection or document that doesn't exist). Also, if an error does occur with our listener, it will automatically stop listening. Given these constraints, these issues usually will almost always be sorted out in development before any code gets shipped. 

However, we can still set up general error handling to ensure that if errors do come up with our listener, they at least get printed to the DOM. To do this, we'll set up a new state variable called `error` to track any errors that occur.

Here's what our updated code looks like (pay attention to the comments as you review the code): 

<div class="filename">src/components/TicketControl.js</div>

```js
import React, { useEffect, useState } from 'react';
import NewTicketForm from './NewTicketForm';
import TicketList from './TicketList';
import EditTicketForm from './EditTicketForm';
import TicketDetail from './TicketDetail';
import { collection, addDoc, doc, updateDoc, onSnapshot, deleteDoc } from "firebase/firestore";
import db from './../firebase.js'

function TicketControl() {
  ...
  // new code!
  const [error, setError] = useState(null);
  
  useEffect(() => { 
    const unSubscribe = onSnapshot(
      collection(db, "tickets"), 
      (collectionSnapshot) => {
        ...
      }, 
      (error) => {
        // new code!
        setError(error.message);
      }
    );

    return () => unSubscribe();
  }, []);

  ...

  // new code!
  if (error) {
    currentlyVisibleState = <p>There was an error: {error}</p>
  } else if (editing) {      
    ...
  } else if (selectedTicket != null) {
    ...
  } else if (formVisibleOnPage) {
    ...
  } else {
    ...
  }

  return (
    <React.Fragment>
      {currentlyVisibleState}
      {/* New code below! */}
      {error ? null : <button onClick={handleClick}>{buttonText}</button>}
    </React.Fragment>
  );
}

export default TicketControl;
```

A Firestore error is returned as a [`FirestoreError`](https://firebase.google.com/docs/reference/js/firestore_.firestoreerror) object and it has a `message` property with a description of the error that occurred. So, if an error does occur with our listener, we call `setError(error.message)`.

Later in our conditional that determines the UI, we first check to see if there's an error, and if so, to display it. Finally, in our return statement, we make sure to only display the button element if there is not an error.

Optionally, if you want to check that this code works, we cause a security permissions issue by updating the Firestore database rules to only allow reading and writing data if a user is authenticated. To do this, navigate to your Firestore database, and then select the _Rules_ tab. Within the input box, comment out the existing `allow` statement. It should look something like this:

```js
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /{document=**} {
      // allow read, write: if
      //   request.time < timestamp.date(2021, 12, 12);
    }
  }
}
```

Then, add this new `allow` statement below the commented out rules: `allow read, write: if request.auth != null;`. Your rules should now look similar to this code snippet:

```js
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /{document=**} {
      // allow read, write: if
      //   request.time < timestamp.date(2021, 12, 12);
      allow read, write: if request.auth != null;
    }
  }
}
```

Then, publish your changes. The new rules could take a few moments to take effect. 

Finally, test out your app! You should see the following message on screen:

```
There was an error: Missing or insufficient permissions.
```

When you've tested your app to your heart's content, make sure to revert your database rules to what they were previously! We still need to add update and delete functionality.

## Summary
---

Now that we have the listener set up in `TicketControl.js`, anytime we change the database from our app or from the Firestore Database console (via the online Firebase console for the Help Queue project), the listener will automatically call the first callback function (so long as there is not an error) that we set up in the `onSnapshot()` function:

```js
// the first callback function within `onSnapshot()`
(collectionSnapshot) => {
  const tickets = [];
  collectionSnapshot.forEach((doc) => {
      tickets.push({
        names: doc.data().names, 
        location: doc.data().location, 
        issue: doc.data().issue, 
        id: doc.id
      });
  });
  setMainTicketList(tickets);
}
```

As we know, this callback function handles creating an array of tickets. 

Since we call `setMainTicketList(tickets)` to update the `mainTicketList` state variable from within the listener, this will trigger a re-render of our `TicketControl` component so that our application is always showing the most up-to-date data from our database.
