Let's update our Help Queue to add new tickets directly to our Firestore database. Since Firestore data is saved in documents, which are grouped into collections, we'll need to create a `tickets` collection to hold individual ticket documents. To do this, we'll update the `handleAddingNewTicketToList` function in `TicketControl.js` and make use two Firestore functions: 

* `collection()` allows us to specify a collection within our firestore database.
* `addDoc()` allows us to add a new document to a Firestore collection.

We'll also refactor our Help Queue to let Firestore set each unique ticket id instead of using the `uuid` library.

## Adding Tickets to Firestore
---

The first thing we need in order to do anything with our database is access to our database instance. This means we need to import the `db` variable from `firbase.js` into `TicketControl`:

<div class="filename">src/components/TicketControl.js</div>

```js
import React, { useEffect, useState } from 'react';
import NewTicketForm from './NewTicketForm';
import TicketList from './TicketList';
import EditTicketForm from './EditTicketForm';
import TicketDetail from './TicketDetail';
// new import!
import db from './../firebase.js';
```

Now that we have access to our `db` variable, we can use it in any request we make, including adding, updating, and deleting Firestore data.

So how do we format a POST request to Firestore? We'll follow the instructions in the Firestore docs on how to [Add Data](https://firebase.google.com/docs/firestore/manage-data/add-data#add_a_document) to a Firestore database. 

Here's our updated code, including a new import statement from the `firebase/firestore` library:

<div class="filename">src/components/TicketControl.js</div>

```js
...
import { collection, addDoc } from "firebase/firestore";

function TicketControl() {
  ...

  const handleAddingNewTicketToList = async (newTicketData) => {
    await addDoc(collection(db, "tickets"), newTicketData);
    setFormVisibleOnPage(false);
  }

  ...
}

export default TicketControl;
```

Let's break down this new code:

* The `firebase/firestore` library will have all of the helper functions that we need to implement CRUD functionality in our app. Right now we just import `collection` and `addDoc`, but later we'll update this statement to import more Firestore helper functions.
* The process of accessing our firestore database is asynchronous, so we use the `async` and `await` keywords:
  * We make our `handleAddingNewTicketToList` function expression `async`, and 
  * We `await` the `addDoc()` function call.
* The `collection()` function allows us to specify a collection within our firestore database. This function takes two arguments: the Firestore database instance, and the name of our collection. This function returns a `CollectionReference` object, which as its name suggests, is an object that acts as a reference to a collection within our Firestore database. 
* The `addDoc()` function allows us to add a new document to a specified collection. This function takes two arguments: a collection reference and the data to be added to the new document. 
  * Take note: the data that we add as the second argument must always be a JavaScript object! Each object key and value will become the Firestore document's field and value. The image below is a representation of this transformation.

![A representation of how JS objects get turned into Firestore documents.](https://learnhowtoprogram.s3.us-west-2.amazonaws.com/React/Week-4-React-2020/firestore-JS-obj-to-doc.png)

Also, if it's easier to read and reason about, we can re-write the new code in `handleAddingNewTicketToList` to separate the `collection()` and `addDoc()` function calls onto multiple lines:

<div class="filename">src/components/TicketControl.js</div>

```js
...
import { collection, addDoc } from "firebase/firestore";

function TicketControl() {
  ...

  const handleAddingNewTicketToList = async (newTicketData) => {
    const collectionRef = collection(db, "tickets");
    await addDoc(collectionRef, newTicketData);
    setFormVisibleOnPage(false);
  }

  ...
}

export default TicketControl;
```

So with that, are we done? Not quite! Let's review a few important notes about adding and removing Firestore collections, and then we'll re-configure our code to let Firestore apply the unique IDs for each ticket.

### How it Works: Creating New Collections with `collection()`

It's important to note that the process of creating new collections is highly automated through the `collection()` helper function. This is how it works: when we create a new document and specify the name of a collection that we want to add the document to, Firestore will look in the database to see if a collection with the specified name exists, and if not, Firestore will simply create a new collection.

### How it Works: Deleting Collections

The process of deleting a collection is also automated. Whenever we remove the last remaining document in a collection, Firestore will automatically delete that collection from the database. 

If you want to remove a collection with many documents still inside of it from your code, you'll need to delete each document (and any subcollections) individually. Again, when all documents are deleted from a collection, Firestore will automatically delete that collection. So all of this is to say, there's no Firestore helper function that deletes an entire collection.

However, you can manually delete an entire collection from the Firebase console. If you want to do this, follow these steps:

* Open your Firestore database, 
* Within the _Data_ tab, click the three vertical dots next to the name of the collection. In the image below this is circled in red 
* From the menu that pops up, click _Delete collection_.

![How to delete a collection via the online Firestore database UI.](https://learnhowtoprogram.s3.us-west-2.amazonaws.com/React/Week-4-React-2020/firestore-manual-delete.png)

### Using Firestore Auto-Generated IDs

Instead of using the `uuid` library, we'll use IDs that are auto-generated by Firestore. Also, instead of saving the unique ticket ID as a property of each ticket object, we'll set it as the name of our Firestore document. Let's get into it!

We've actually already done most of the leg work to make this happen: the `addDoc` function doesn't have a location to specify the document's ID, so when we use it to add a new document, Firestore knows that it needs to auto-generate one for us. 

We do, however, need to update `NewTicketForm.js` to not generate an ID or an `id` property for our ticket. Remove the following two lines of code from `NewTicketForm.js`:

* `import { v4 } from 'uuid';`
* `id: v4()`

```js
import React from "react";
// import { v4 } from 'uuid';  <-- Remove this line!
import PropTypes from "prop-types"; 
import ReusableForm from "./ReusableForm";

function NewTicketForm(props){

  function handleNewTicketFormSubmission(event) {
    event.preventDefault();
    props.onNewTicketCreation({
      names: event.target.names.value, 
      location: event.target.location.value, 
      issue: event.target.issue.value, 
      // id: v4()  // <--- Remove this line!
    });
  }

  return (
    ...
  );
}

NewTicketForm.propTypes = {
  onNewTicketCreation: PropTypes.func
};

export default NewTicketForm;
```

So where does this auto-generated ID appear? It gets added as the the document's identifier. To understand this, let's add a new ticket to Firestore via our Help Queue app, and then inspect the newly created ticket in the online Firestore UI.

Go ahead and serve your Help Queue app and add a new ticket now. The ticket data that we'll use for this example has the following data:

* names: "Fatima and Quincy"
* location: "3B"
* issue: "state variable not updating as expected"

Now, navigate to the Firebase console, then open your Help Queue project, then select _Firestore Database_ from the left-hand vertical menu. This will open the Firestore database UI from which we can inspect our database data. You should see something very similar to what's in the image below:

* The leftmost column lists our collections. Here we have our `tickets` collection listed.
* The middle column lists the documents in the selected collection. As we can see, documents are listed by their ID. An ID can be any string, and we've used Firestore's auto-generated ID. 
* The rightmost column list the data from the selected document.

![Data in the Firestore database: a `tickets` collection with one ticket in it. The ticket has an auto-generated ID from Firestore.](https://learnhowtoprogram.s3.us-west-2.amazonaws.com/React/Week-4-React-2020/firestore-ticket-data-and-id.png)

### A Consideration When Using Random IDs

If you are using a random ID generator, whether from the UUID library or Firestore, these IDs are typically not created to mark the order in which each document was created. What's more, Firestore lists the documents in a collection alphabetically by their ID (with numbers taking precedence over letters). That means that the order in which our list of tickets appear in our app can (and oftentimes do) change every time we add a new ticket to the list.

If the creation order of each document in a collection is important, there are two solutions to try out:

1. Use a Firestore [Server Timestamp](https://firebase.google.com/docs/firestore/manage-data/add-data#server_timestamp) to mark when a document was initially created, or updated. You can then use this timestamp to sort your documents by their timestamp at creation when you query the database. We'll learn how to do this when we add [a wait time to the Help Queue](https://new.learnhowtoprogram.com/react/react-with-nosql/adding-wait-time-to-the-queue) 
2. Create a custom method or use an external library that gives unique IDs that also are prefixed with a number that marks the order in which it was created.  

### Adding a New Document Without a Firestore Auto-Generated ID

You may be wondering what our code would look like if we did not want Firestore to auto-generate an ID for us. Well, we'd need to use new methods: `setDoc()` and `doc()`. This is what our code would look like:

```js
import { v4 } from 'uuid';
import { setDoc, doc } from 'firebase/firestore;

function TicketControl() {
  ...

  const handleAddingNewTicketToList = async (newTicketData) => {
    await setDoc(doc(db, "tickets", v4()), newTicketData);
    setFormVisibleOnPage(false);
  }

  ...
}

export default TicketControl;
```

Let's break this down!

* The `doc()` function allows us to reference a document in the firestore database. With `doc()`, we can specify the location of a new document or the location of an existing document. A few notes:
  * The `doc()` function takes 3 arguments: the database instance, the collection name, and the unique document identifier. In the above example, we've used the `uuid` library's `v4()` function to generate a unique ID. 
  * The `doc()` function returns a `DocumentReference` object, which as its name suggests, is an object that acts as a reference to a document within our Firestore database. 
* The `setDoc()` function is similar to the `addDoc()` function: we can use it to add or update a specified document with the data that's passed in as its second argument. 

In our Help Queue app, we won't use the above code, and instead let Firestore auto-generated unique IDs for us.

Up next, we'll learn how to read data from Firestore. In the process, we'll learn how to take Firestore's auto-generated ID and add it as a property to each ticket in our local `mainTicketList` so that we can continue to loop through the ticket list and display each ticket.
