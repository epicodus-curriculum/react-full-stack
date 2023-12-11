We're ready to get our edit and delete functionality working again. We will continue to refactor code in `TicketControl.js` to update the edit and delete functionality to alter tickets directly in Firestore. We'll also make use of three Firestore functions:

* `updateDoc()` will allow us to update a document in Firestore.
* `deleteDoc()` will allow us to delete documents in Firestore. 
* `doc()` will allow us to reference a document in the firestore database. With `doc()`, we can specify the location of a new document or the location of an existing document.

## Updating Tickets
---

To update tickets in Firestore, we'll refactor the `handleEditingTicketInList()` function in the `TicketControl` component. 

First, we need to import `updateDoc` and `doc` from `firebase/firestore`:

<div class="filename">src/components/TicketControl.js</div>

```js
import { collection, addDoc, onSnapshot, doc, updateDoc } from "firebase/firestore";
```

Next, we need to refactor the `handleEditingTicketInList()` function. Here's the new code:

<div class="filename">src/components/TicketControl.js</div>

```js
...
import { collection, addDoc, onSnapshot, doc, updateDoc } from "firebase/firestore";
import db from './../firebase.js'

function TicketControl() {
  ...

  const handleEditingTicketInList = async (ticketToEdit) => {
    const ticketRef = doc(db, "tickets", ticketToEdit.id);
    await updateDoc(ticketRef, ticketToEdit);
    setEditing(false);
    setSelectedTicket(null);
  }

  ...

  return (
    ...
  );
}

export default TicketControl;
```

While this code is new, it's rather similar to the process we followed when adding a ticket to Firestore:
 
* First, we create a document reference with the `doc()` function for the ticket that we want to update:
  * The `doc()` function takes 3 arguments: the database instance, the collection name, and the unique document identifier.
  * The `doc()` function returns a `DocumentReference` object, which as its name suggests, is an object that acts as a reference to a document within our Firestore database. 
* Next, we call the `updateDoc()` function. The first argument we pass into this function is the document reference for the ticket we want to update, and the second argument is the new data that the ticket should be updated with.
* Finally, take note that the `updateDoc()` function is asynchronous, so we need to make our `handleEditingTicketInList()` function `async` and apply the `await` keyword before the `updateDoc()` function call.

Note that we can optionally rewrite the above function like so:

```js
const handleEditingTicketInList = async (ticketToEdit) => {
  await updateDoc(doc(db, "tickets", ticketToEdit.id), ticketToEdit);
  setEditing(false);
  setSelectedTicket(null);
}
```

Since we've set up the listener in the last lesson, this means that anytime we update a ticket with the `updateDoc()` function, our listener will be triggered and the `mainTicketList` state variable in the `TicketControl` component will be updated.

## Deleting Tickets
---

To delete documents in Firestore, we'll update the `handleDeletingTicket()` function in the `TicketControl` component. We'll also need the Firestore function `deleteDoc()`, so let's start by updating our import statement from `firebase/firestore`:

<div class="filename">src/components/TicketControl.js</div>

```js
import { collection, addDoc, onSnapshot, doc, updateDoc, deleteDoc } from "firebase/firestore";
```

Next, let's update `handleDeletingTicket()`. Here's the new code:

<div class="filename">src/components/TicketControl.js</div>

```js
...
import { collection, addDoc, onSnapshot, doc, updateDoc, deleteDoc } from "firebase/firestore";
import db from './../firebase.js'

function TicketControl() {
  ...

  const handleDeletingTicket = async (id) => {
    await deleteDoc(doc(db, "tickets", id));
    setSelectedTicket(null);
  } 

  ...

  return (
    ...
  );
}

export default TicketControl;
```

The `deleteDoc()` function is nearly identical to the `updateDoc()` function:

* It's asynchronous and uses `async` and `await` to manage the asynchrony.
* It takes a document reference as an argument that specifies which document in the Firestore database should be deleted. 

The only difference from the `updateDoc()` function is that `deleteDoc()` does not take a second argument for data.

And with that, we've completed CRUD functionality for our Help Queue application! 

## What's Next
---

In the next lesson, we'll wrap up our introduction to Firestore by reviewing how to structure data in Firestore.

In upcoming coursework, we'll expand the functionality of our Help Queue: 

* We'll add authentication and basic authorization.
* We'll use the react-router library to create routes.
* We'll host our project with Firebase.
* We'll learn about Firestore Queries, styled components, and other further exploration opportunities.
* Finally, we'll incorporate a wait time into our Help Queue. 

