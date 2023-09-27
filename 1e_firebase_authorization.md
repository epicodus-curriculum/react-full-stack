In the last lesson, we added Firebase authentication to our `SignIn` component. However at this point, it really doesn't make a difference because our application doesn't care whether a user is signed in or not. In this lesson, we'll add some basic authorization to lock down the queue if a user isn't signed in.

In order to do this, we'll make some changes to `TicketControl.js`.

## Adding Basic Authorization to Help Queue
---

We'll start by importing our `Auth` instance from `firebase.js` so we can access user data, like the currently signed in user.

<div class="filename">src/components/TicketControl.js</div>

```js
import { db, auth } from './../firebase.js';
```

Next, we'll add a new `if` and `else if` statement, and we'll add all of our ticket UI logic within the new `else if` statement. We'll look at the new code as integrated with the existing ticket UI logic, and then we'll look at the new code on its own.  

<div class="filename">src/components/TicketControl.js</div>

```jsx
...
import { db, auth } from './../firebase.js'

function TicketControl() {
  ...

  ...
  

  if (auth.currentUser == null) {
    return (
      <React.Fragment>
        <h1>You must be signed in to access the queue.</h1>
      </React.Fragment>
    )
  } else if (auth.currentUser != null) {

    let currentlyVisibleState = null;
    let buttonText = null; 

    if (error) {
      currentlyVisibleState = <p>There was an error: {error}</p>
    } else if (editing) {      
      currentlyVisibleState = <EditTicketForm 
      ticket = {selectedTicket} 
      onEditTicket = {handleEditingTicketInList} />
      buttonText = "Return to Ticket List";
    } else if (selectedTicket != null) {
      currentlyVisibleState = <TicketDetail 
      ticket={selectedTicket} 
      onClickingDelete={handleDeletingTicket}
      onClickingEdit = {handleEditClick} />
      buttonText = "Return to Ticket List";
    } else if (formVisibleOnPage) {
      currentlyVisibleState = <NewTicketForm 
      onNewTicketCreation={handleAddingNewTicketToList}/>;
      buttonText = "Return to Ticket List"; 
    } else {
      currentlyVisibleState = <TicketList 
      onTicketSelection={handleChangingSelectedTicket} 
      ticketList={mainTicketList} />;
      buttonText = "Add Ticket"; 
    }
    return (
      <React.Fragment>
        {currentlyVisibleState}
        {error ? null : <button onClick={handleClick}>{buttonText}</button>} 
      </React.Fragment>
    );
  }
}

export default TicketControl;
```

For additional clarity, here's the new code we added, on its own:

<div class="filename">src/components/TicketControl.js</div>

```jsx
...
import { db, auth } from './../firebase.js'

function TicketControl() {
  ...

  if (auth.currentUser == null) {
    return (
      <React.Fragment>
        <h1>You must be signed in to access the queue.</h1>
      </React.Fragment>
    )
  } else if (auth.currentUser != null) {
    ... 
    
    return (
      ...
    );
  }  // Don't forget the closing curly bracket!
}

export default TicketControl;
```

We set up our conditional UI logic by checking the value of `auth.currentUser`. You may readily guess what this represents, but let's get a little bit into the weeds. 

We previously learned that the `auth` variable represents the authentication instance that's connected to our Help Queue web app that we created with Firebase. Well the variable `auth` is a Firebase [`Auth` object type](https://firebase.google.com/docs/reference/js/auth.auth.md#auth_interface). `Auth` has a property called `currentUser`, which logs the currently signed in user. Notably, if there is no user signed in, then `currentUser` is set to `null`. 

So, when `auth.currentUser == null` is true, then we display a message stating that the user needs to sign in to see the list of tickets. When `auth.currentUser != null`, that means a user is signed in, and we show them the list of tickets. 

If you are wondering where the `error` variable is coming from, we originally created a state variable for `error` to hold any errors generated in the process of querying our database. For a review, check out the lesson [Viewing Tickets from Firestore](https://new.learnhowtoprogram.com/lessons/viewing-tickets-from-firestore).

Take note that when `auth.currentUser` is set to a signed in user, that user is a [`User` object](https://firebase.google.com/docs/reference/js/auth.user.md#user_interface) that extends functionality from the [`UserInfo` class](https://firebase.google.com/docs/reference/js/auth.userinfo.md#userinfo_interface). These classes contain properties that store the user's personal information, like their email, display name, phone number, and photo URL. That's the making of a user profile right there! Currently, we only gather an email and password, so if you want to add functionality for a user profile, you'll have to explore that in a project you create for this course section, or on your own. 

At this point, we're ready to try out the authentication we created. Go ahead and do so now!

In this lesson, we've demonstrated how we can use authorization to determine what a component should render. The use case we provided in this lesson is simple — and larger applications will need more robust authentication and authorization. To learn more information on different ways a user can be authenticated, visit the Firebase [documentation for authentication](https://firebase.google.com/docs/auth/web/start).
