In the last lesson, we covered the tricky concept of unidirectional data flow. In this lesson, we'll apply what we've learned. To recap, we'll need to do the following:

1. Move `mainTicketList` into state.
2. Create a method in our parent `TicketControl` component. This method will take any form data and turn it into an actual ticket.
3. Pass this method down to the child `NewTicketForm` component as a prop — and also add PropTypes.
4. Add our new method to the existing function in our child component so that it's triggered when a user submits the form.

In order to do this, we only need to add a few lines of code — and make small modifications to a few other lines. Despite the relatively small amount of code being added, we are still working with challenging new concepts. Be patient with yourself and follow along slowly. If it doesn't all click immediately (and it probably won't), trust the process and keep practicing these concepts in class and on your own.

### Step 1: Move mainTicketList into State

Let's start by adding a mainTicketList property to state and passing it down as a prop to `TicketList` :

<div class="filename">src/components/TicketControl.js</div>

```js
class TicketControl extends React.Component {

  constructor(props) {
    super(props);
    this.state = {
      formVisibleOnPage: false,
      mainTicketList: [] // new code
    };
  }

...

    if (this.state.formVisibleOnPage) {
      currentlyVisibleState = <NewTicketForm />;
      buttonText = "Return to Ticket List";
    } else {
      currentlyVisibleState = <TicketList ticketList={this.state.mainTicketList} />; // new code
      buttonText = "Add Ticket"; 
    }
...

```

Notice we're initializing mainTicketList as an empty array. We're doing this because we don't want this application to start with fake tickets. The queue should be empty until we start adding tickets via our form. (We'll be removing our array of dummy tickets from `TicketList` in just a moment.) Also, notice how we're passing mainTicketList down to `TicketList`. We include it as a prop and target its place in state with `this.state.mainTicketList`. Here we're calling it `ticketList`, so that's the name we'll use to access it as a prop in `TicketList`.

### Step #2: Pass New Props to TicketList Component and Add propTypes

In the first step, we passed `mainTicketList` state from `TicketControl.js` down to our `TicketList` component. Now we need to actually add props and prop types to `TicketList.js`. We'll also remove the `mainTicketList` constant that holds our dummy tickets as well.

<div class="filename">src/components/TicketList.js</div>

```js
import React from "react";
import Ticket from "./Ticket";
import PropTypes from "prop-types";

// remove const mainTicketList = [ ... ]. We no longer want these.

function TicketList(props) { // Add props as parameter.
  return (
    <React.Fragment>
      <hr />
      {props.ticketList.map((ticket, index) => // Loop through the list passed down from TicketControl.
        <Ticket names={ticket.names}
          location={ticket.location}
          issue={ticket.issue}
          key={index} />
      )}
    </React.Fragment>
  );
}

// Add propTypes for ticketList.
TicketList.propTypes = {
  ticketList: PropTypes.array
};

export default TicketList;
```

We've made several changes here. Now that we are passing `ticketList` down through `props`, we need to `import prop-types` and add a prop type of array for our `ticketList`. We also remove our `mainTicketList` constant which stored three fake tickets — we won't need these anymore!

Now we'll be able to make changes to our ticket list and, ultimately, display tickets as they're added.

### Step 3: Create a Method to Handle Our Form Submission

We'll start with step #1: creating a method in our `TicketControl` component.

<div class="filename">TicketControl.js</div>

```js
...

class TicketControl extends React.Component {

  constructor(props) {
    ...
  }

...

  handleAddingNewTicketToList = (newTicket) => {
    const newMainTicketList = this.state.mainTicketList.concat(newTicket);
    this.setState({mainTicketList: newMainTicketList,
                  formVisibleOnPage: false });
  }
...
```

Our new method is called `handleAddingNewTicketToList` because it does just that — handle the process of adding a new ticket in our `mainTicketList`. It takes a `newTicket` as a parameter. 

It's common practice to prefix the name of an event handler function with `handle`. Any props containing that function will be prefixed with `on`. (We will add our prop in the next step.) This is because the prop will be used _when_ the event occurs, but the function itself is what _actually handles_ the necessary actions. It also ensures the names are similar enough to easily determine which props and functions correspond, yet different enough to determine when we're referencing a function and when we're referencing a prop containing a function.

Next, we create a constant called `newMainTicketList`. Remember that we should **never** alter state directly. Instead, we will let React do that with the `setState()` method.

We take `this.state.mainTicketList` and call `concat()` on it. Unlike `push()`, which directly alters the array its called on, `concat()` makes a _copy_ of that array. Anything passed into `concat()` (in this case, our `newTicket`) will be concatenated to the end of the new array.

Next, we set the value of `mainTicketList` to the `newMainTicketList` variable we just created. As noted, we are using `setState()` to make our direct change to state.

Finally, once the ticket has successfully been submitted, we want to set `formVisibleOnPage` to false again so that the user will see the queue, not the form.

### Step 4: Pass Method Down to Child Component as a Prop

Now we need to pass our new `handleAddingNewTicketToList` method down to our `NewTicketForm` component as a prop. We'll start by making a small update to our `TicketControl`'s `render()` method:

<div class="filename">TicketControl.js</div>

```js
...
render(){
    ...
    if (this.state.formVisibleOnPage) {
      currentlyVisibleState = <NewTicketForm onNewTicketCreation={this.handleAddingNewTicketToList} /> { // new code in this line }
      buttonText = "Return to Ticket List";
    } else {
      ....

```

We will pass `this.handleAddingNewTicketToList` as a prop to the `NewTicketForm`. It will be saved in the prop `onNewTicketCreation`. As noted in Step 1, we prefix the prop with `on`. This differentiates the method in our parent component (which will actually handle the event) from the function in our child component (which is triggered when the event happens).

Next, we need to make a few changes to our child `NewTicketForm` so that it will accept props.

<div class="filename">NewTicketForm.js</div>

```js
import React from "react";
import PropTypes from "prop-types"; //import PropTypes

function NewTicketForm(props){ // Make sure to add props as a parameter.
  ...
}

// We also need to add PropTypes for our new prop.

NewTicketForm.propTypes = {
  onNewTicketCreation: PropTypes.func
};

export default NewTicketForm;
```

We do two things here:
1. Ensure we are passing `props` into our function component. Otherwise, our `NewTicketForm` won't have access to props from `TicketControl`.
2. We add a `PropTypes` for `onNewTicketCreation`. Remember that `this.handleAddingNewTicketToList` is passed down to the child component as `onNewTicketCreation`.

### Step 5: Add a Unique ID and Utilize the Parent Component's Method in the Child Component

We're almost done! We need to two things:

* Import the UUID library to assign unique IDs to new tickets.
* Update the `handleNewTicketFormSubmission` function so that it creates a new ticket object and uses our `onNewTicketCreation` prop to send the ticket object to the parent component, `TicketControl`.

Here's the updated code:

<div class="filename">NewTicketForm.js</div>

```js
...
import { v4 } from 'uuid'; // new code

function NewTicketForm(props){
  ...

  function handleNewTicketFormSubmission(event) {
    event.preventDefault();
    props.onNewTicketCreation({
      names: event.target.names.value, 
      location: event.target.location.value, 
      issue: event.target.issue.value, 
      id: v4()
    });
  }

...
```

Because a function component doesn't have `this` as a reference like a class component, we need to directly refer to the `props` passed into the function component. That's why we do `props.onNewTicketCreation()` instead of `this.onNewTicketCreation()` (as we'd do if this were a class component).

We create an object with all of the ticket properties and pass it as the argument to `props.onNewTicketCreation()`. We also create a unique ID with the UUID library. 

Take note that if we need to get a number from our form, we'd want to parse any form values at this point. For example, say we wanted to track the number of students who need help; in this case, we might have a property called `numberOfStudents` and we'd want to make sure we parse the corresponding form value:

```js
props.onNewTicketCreation({
  ...
  numberOfStudents: parseInt(event.target.numberOfStudents.value)
});
```

Remember that `onNewTicketCreation()` is the callback from the parent component even though it has a different name now. The following method is invoked in `TicketControl`:

<div class="filename">TicketControl.js</div>

```js
handleAddingNewTicketToList = (newTicket) => {
    const newMainTicketList = this.state.mainTicketList.concat(newTicket);
    this.setState({
      mainTicketList: newMainTicketList,
      formVisibleOnPage: false
    });
  }
```

When we call `props.onNewTicketCreation({names: names.value, location: location.value, issue: issue.value});` in the `NewTicketForm` component, this object is passed in as an argument to the `newTicket` parameter, updating the `mainTicketList`.

Try it out in the browser. Now when we add a ticket via the form, it will be added to the queue!