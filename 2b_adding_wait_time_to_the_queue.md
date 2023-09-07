In this lesson we're going to work with the Firestore server timestamp to add a wait time to our Help Queue project. For this refactor, we'll need to complete a few steps:

1. We'll use a new function called `serverTimestamp()` to generate a timestamp when a ticket is initially created. This will be the exact time when the ticket is added to our database.
2. Then, we'll use this timestamp to generate a formatted wait time using the `date-fns` library. 
3. Finally, we'll set up a `useEffect()` hook with a `setInterval()` function that updates the formatted wait time for all tickets every minute.

We'll also learn how to use a Firestore timestamp to order our tickets from oldest to newest. 
Why bother? Well, we're making use of timestamps for another scenario: preserving the creation order for every document in a collection.

Previously we learned that the auto-generated IDs from Firestore are always random and they do not include any reference as to the order in which each document was created. However, Firestore still orders the documents in a collection alphabetically by its identifier, with numbers taking precedence over letters. This means that the order in which documents appear in the database and our website is subject to change anytime a new document is added. That's no good. So, we'll solve that issue in this lesson, and we'll do so with the help of server timestamps. 

Let's get into this refactor!

## Adding a Server Timestamp at Ticket Creation
---

The first thing we'll do in this refactor is create a server timestamp when a ticket is first created. Later on, we'll use this value to calculate a formatted time that shows how long a ticket has been open. 

To add a Firestore server timestamp to new tickets, we'll need to update `NewTicketForm.js`. Here's the new code:  

<div class="filename">src/components/NewTicketForm.js</div>

```js
...
// new import!
import { serverTimestamp } from "firebase/firestore";

function NewTicketForm(props){

  function handleNewTicketFormSubmission(event) {
    event.preventDefault();
    props.onNewTicketCreation({
      names: event.target.names.value, 
      location: event.target.location.value, 
      issue: event.target.issue.value,
      // new property!
      timeOpen: serverTimestamp()
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

We've added a new import to get access to the `serverTimestamp()` function from `firebase/firestore`. Then, we've added a new `timeOpen` property that's set to the `serverTimestamp()`:

```js
timeOpen: serverTimestamp()
```

As you may guess, the `serverTimestamp()` function returns a new timestamp corresponding to when the ticket gets added as a document in the database.

## Adding a Formatted Wait Time
---

The next step is to add a formatted wait time that displays how long a ticket has been open. Our next refactor will start in `TicketControl.js`, and then move to `TicketList.js` and `Ticket.js`. 

We'll start by installing the `date-fns` library, a popular JavaScript library for working with dates and time. We can use date-fns to manipulate and parse time, which is exactly what we'll use it for. 

In the root directory of your Help Queue project, run the following command:

```
$ npm install date-fns@2
```

We'll be using the same `formatDistanceToNow()` helper function as we did in the last course section "React with Redux". The [documentation for date-fns](https://date-fns.org/docs/Getting-Started) is extensive, and there are many other helper functions available. We recommend checking it out when you have the time — there are many use cases where it can add valuable functionality to an application.

To start, we'll import `formatDistanceToNow` at the top of `TicketControl.js`:

<div class="filename">src/components/TicketControl.js</div>

```js
import { formatDistanceToNow } from 'date-fns';
```

This is how we'll use [the `formatDistanceToNow` helper function](https://date-fns.org/v2.29.1/docs/formatDistanceToNow):

```js
formatDistanceToNow(new Date());
```

This time, we're not going to include the options object that will add "ago" to the end of the formatted time, like "7 minutes ago". You can add it if you like. This is what the syntax looks like:

```js
formatDistanceToNow(new Date(), {
  addSuffix: true
});
```

Since `formatDistanceToNow()` takes a JavaScript date object as its first argument, we'll need to translate the Firestore server timestamp into a JavaScript date. Let's do that next:

<div class="filename">src/components/TicketControl.js</div>

```js
...
import { formatDistanceToNow } from 'date-fns';

function TicketControl() {
  ...

  useEffect(() => { 
    const unSubscribe = onSnapshot(
      collection(db, "tickets"), 
      (querySnapshot) => {
        const tickets = [];
        querySnapshot.forEach((doc) => {
          // new code below!
          const timeOpen = doc.get('timeOpen', {serverTimestamps: "estimate"}).toDate();
          const jsDate = new Date(timeOpen);
          tickets.push({
            names: doc.data().names, 
            location: doc.data().location, 
            issue: doc.data().issue, 
            // new code below!
            timeOpen: jsDate,
            formattedWaitTime: formatDistanceToNow(jsDate),
            id: doc.id
          });
        });
        setMainTicketList(tickets);
      },
      (error) => {
        setError(error.message);
      }
    );

    return () => unSubscribe();
  }, []);

  ...
  
  ...
}

export default TicketControl;
```

Let's break down this new code. First we go through the process of turning the Firestore server timestamp into a JS Date object:

```js
const timeOpen = doc.get('timeOpen', {serverTimestamps: "estimate"}).toDate();
const jsDate = new Date(timeOpen);
```

The code `doc.get('timeOpen', {serverTimestamps: "estimate"})` gets the value of the `timeOpen` field for the current document; this value is a [Firestore `Timestamp` object](https://firebase.google.com/docs/reference/js/firestore_.timestamp.md). Then we call the `Timestamp.toDate()` method on the server timestamp to turn it into data that's formatted for JavaScript. 

Then we pass in the `timeOpen` variable — a Firestore timestamp that's been transformed into data that's formatted for JavaScript — and pass it into the JavaScript Date constructor: `new Date(timeOpen)`. 

We then add two new properties to our ticket object:

```js
tickets.push({
  ...
  timeOpen: jsDate,
  formattedWaitTime: formatDistanceToNow(jsDate),
  ...
});
```

The `timeOpen` property is set to the `jsDate` variable, and the `formattedWaitTime` property is set to a time that's formatted by `date-fns`. 

Why not leave `timeOpen` as a server Timestamp? We'll use the `timeOpen` property later to update the `formattedWaitTime` every minute. Let's do that part next!

### Adding a Side Effect: Updating `formattedWaitTime` Every Minute

To update the `formattedWaitTime` property every minute, we'll need to make use of the  JavaScript `setInterval()` function and React's `useEffect()` hook. 

Here's the new code:

<div class="filename">src/components/TicketControl.js</div>

```js
...
import { formatDistanceToNow } from 'date-fns';

function TicketControl() {
  const [formVisibleOnPage, setFormVisibleOnPage] = useState(false);
  const [mainTicketList, setMainTicketList] = useState([]);
  ...

  useEffect(() => {
    function updateTicketElapsedWaitTime() {
      const newMainTicketList = mainTicketList.map(ticket => {
        const newFormattedWaitTime = formatDistanceToNow(ticket.timeOpen);
        return {...ticket, formattedWaitTime: newFormattedWaitTime};
      });
      setMainTicketList(newMainTicketList);
    }

    const waitTimeUpdateTimer = setInterval(() =>
      updateTicketElapsedWaitTime(), 
      60000
    );

    return function cleanup() {
      clearInterval(waitTimeUpdateTimer);
    }
  }, [mainTicketList])

  ...
}

export default TicketControl;
```

Let's first note that our `useEffect()` hook depends on the `mainTicketList` state variable, because we look through it within the `useEffect()` hook; that's why we've added `mainTicketList` to the dependencies array:

```js
  useEffect(() => {
    // code for side effect
  }, [mainTicketList])
```

This also means that our effect will get called anytime the `mainTicketList` state variable updates. 

Next, let's take a look at the effect itself, all of the code inside of the callback function that we pass as the first argument to the `useEffect()` hook:

```js
() => {
  function updateTicketWaitTime() {
    const newMainTicketList = mainTicketList.map(ticket => {
      const newFormattedWaitTime = formatDistanceToNow(ticket.timeOpen);
      return {...ticket, formattedWaitTime: newFormattedWaitTime};
    });
    setMainTicketList(newMainTicketList);
  }

  const waitTimeInterval = setInterval(() =>
    updateTicketWaitTime(), 
    60000
  );

  return function cleanup() {
    clearInterval(waitTimeInterval);
  }
}
```

First we set up a helper function called `updateTicketWaitTime()`. This function handles two main tasks:

* Mapping through the `mainTicketList` state variable to update the `formattedWaitTime` for each ticket, and return a new array of updated tickets.
* Passing the updated version of the ticket list to `setMainTicketList()` so that our state variable is updated and our `TicketControl` component re-renders with the updated list of tickets.

We then call the `updateTicketWaitTime()` when we set up the interval. As a reminder, JavaScript's `setInterval()` function takes two arguments: a function that should be run on every interval, and the length of the interval in milliseconds. We've set up our interval to run `updateTicketWaitTime()` every minute, which corresponds to `60000` milliseconds.

The `setInterval()` function itself returns a reference to itself, which we store in the variable `waitTimeInterval`. We can then use this reference to stop/clear the interval. That's exactly what we do with the function that we return from the effect:

```js
() => {
  ...

  return function cleanup() {
    clearInterval(waitTimeInterval);
  }
}
```

The function that we return from the effect is the clean up function that will run when the component unmounts, and before every rerun of the effect. What we do is call `clearInterval(waitTimeInterval)`, stopping the interval from running. This is an important step, because it prevents the creation of multiple intervals!

### Displaying `formattedWaitTime` in the `Ticket` Component

Next, let's update our `Ticket` component to display the `formattedWaitTime`. In this case, we'll need to update `TicketList.js` to pass in the `formattedWaitTime` as a prop to each `<Ticket />` component. Let's start there. 

Here's the updated code:

<div class="filename">src/components/TicketList.js</div>

```js
import React from "react";
import Ticket from "./Ticket";
import PropTypes from "prop-types";

function TicketList(props){

  return (
    <React.Fragment>
      <hr/>
      {props.ticketList.map((ticket) =>
        <Ticket 
          whenTicketClicked={props.onTicketSelection}
          names={ticket.names}
          location={ticket.location}
          // new prop!
          formattedWaitTime={ticket.formattedWaitTime}
          issue={ticket.issue}
          id={ticket.id}
          key={ticket.id}/>
      )}
    </React.Fragment>
  );
}

TicketList.propTypes = {
  ticketList: PropTypes.array,
  onTicketSelection: PropTypes.func
};

export default TicketList;
```

Next, we'll update `Ticket.js` to  display the new `formattedWaitTime` prop, and add it to our prop-types.

Here's the new code:

<div class="filename">src/components/Ticket.js</div>

```js
import React from "react";
import PropTypes from "prop-types";

function Ticket(props){

  return (
    <React.Fragment>
      <div onClick = {() => props.whenTicketClicked(props.id)}>
        <h3>{props.location} - {props.names}</h3>
        <p><em>{props.issue}</em></p>
        {/* new code below! */}
        <p><em>{props.formattedWaitTime}</em></p>
        <hr/>
      </div>
    </React.Fragment>
  );
}

Ticket.propTypes = {
  names: PropTypes.string,
  location: PropTypes.string,
  issue: PropTypes.string,
  // new code below!
  formattedWaitTime: PropTypes.string,
  id: PropTypes.string,
  whenTicketClicked: PropTypes.func
}

export default Ticket;
```

At this point, we've completed our refactor. Serve your Help Queue and test out the changes we made; you should now see a formatted time listed that shows how long a ticket has been open. 

Before proceeding, delete any old tickets created without a formatted wait time. Doing this will help us avoid errors when we sort the Firestore tickets by their creation date. 

## Ordering Tickets by Creation Timestamp
---

Now that we have a timestamp associated with each ticket, let's sort our tickets by their creation date. To do this we need to change our `onSnapshot()` Firestore listener in `TicketControl` to use a `query()`.

Here's the updated code:

<div class="filename">src/components/TicketControl.js</div>

```js
...
// Two new imports: query and orderBy
import { collection, addDoc, doc, updateDoc, onSnapshot, deleteDoc, query, orderBy } from "firebase/firestore";
...

function TicketControl() {
  ...

  useEffect(() => { 
    // new code below!
    const queryByTimestamp = query(
      collection(db, "tickets"), 
      orderBy('timeOpen')
    );
    const unSubscribe = onSnapshot(
      // new code below!
      queryByTimestamp, 
      (querySnapshot) => {
        const tickets = [];
        querySnapshot.forEach((doc) => {
          const timeOpen = doc.get('timeOpen', {serverTimestamps: "estimate"}).toDate();
          const jsDate = new Date(timeOpen);
          tickets.push({
            names: doc.data().names, 
            location: doc.data().location, 
            issue: doc.data().issue, 
            timeOpen: jsDate,
            formattedWaitTime: formatDistanceToNow(jsDate),
            id: doc.id
          });
        });
        setMainTicketList(tickets);
      },
      (error) => {
        setError(error.message);
      }
    );

    return () => unSubscribe();
  }, []);

  ...
  ...
}

export default TicketControl;
```

We start by updating our import statement from `firebase/firestore` to also import `query` and `orderby`. 

Then, in our effect, we start by constructing a query:

```js
const queryByTimestamp = query(
  collection(db, "tickets"), 
  orderBy('timeOpen')
);
```

The `queryByTimestamp` query gets all documents in the `"tickets"` collection and orders them by the value set in each ticket's `timeOpen` field. The order of the tickets will be ascending: older tickets will be at the top and newer tickets will be at the bottom. 

If we wanted descending order instead, we could specify that by adding a second argument to the `orderBy()` function:

```js
const queryByTimestamp = query(
  collection(db, "tickets"), 
  orderBy('timeOpen', 'desc')
);
```

The only other change we make is updating the first argument in the `onSnapshot()` function call to use the `queryByTimeStamp` variable, which represents our Firestore query:

```js
useEffect(() => { 
  const queryByTimestamp = query(
    collection(db, "tickets"), 
    orderBy('timeOpen')
  );
  const unSubscribe = onSnapshot(
    queryByTimestamp, // new code!
    ...,
    ...
  );

  return () => unSubscribe();
}, []);
```

And that's it! Now our tickets are organized by their creation date. 

The best thing about this refactor is that we didn't have to write our own function to sort the Firestore data. That's a big advantage of using Firestore: it's flexible like all NoSQL databases are, but it contains enough structure (in the form of collections and documents) and built-in helper functions to make filtering and sorting data easy and sweat-free!

Next up, let's learn how to host our Help Queue web app with Firebase.