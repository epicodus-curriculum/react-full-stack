Let's apply what we've learned about hooks to our Help Queue application. To do this, we'll build off of the version of our Help Queue that we completed by the end of the React Fundamentals course section. That means we won't be using Redux in this course section. 

After we complete our refactor to use hooks, we'll move on to incorporating a NoSQL database from Google's Firebase. 

If you have a Help Queue repo from the React Fundamentals course section that you created, you are welcome to build off of that repo. Alternatively, you can use the following repo, which contains all the code from the React Fundamentals course section: 

---
**[<i class="glyphicon glyphicon-folder-open"></i>  Starter Project for Help Queue](https://github.com/epicodus-lessons/react-help-queue-starter-project)**

As you read through these lessons as part of your weekend homework, know that you are not required to code along. The first classwork of the course section will prompt you to work through these lessons to refactor the Help Queue and connect it to a NoSQL database.

Our Help Queue refactor will involve completing these two items:

* Turn the class component `TicketControl` into a function component.
* Turn all state in `TicketControl` into state variables created and managed by the `useState` hook.

We'll run into a variety of compilation errors throughout this refactor since we'll be updating many variable names. So, hold off on building and running your project until we're done. 

Also take note that in this refactor we're not reorganizing where our state lives by moving state into other components or abstracting any stateful logic into a custom hook. 

Since we have multiple child components that need access to the same shared state, the `TicketControl` component remains the best location for our Help Queue's shared state. Also, since our Help Queue application is so small, there just isn't a good use case for abstracting any shared state into a custom hook. 

Remember that hooks don't change React fundamentals: 

* How we begin a React project by designing the UI so we can focus on component composition and reusability.
* Later, how we determine where shared state should live by "lifting state up".
* Working with the unidirectional data flow by using props and callback functions. 
* And more! Like considering whether to use external libraries like Redux to manage shared state in a central store. 

It's worth a reminder that not all applications need Redux. To get an in-depth look into this, read more [in this article](https://medium.com/@dan_abramov/you-might-not-need-redux-be46360cf367) and on [the Redux docs](https://redux.js.org/faq/general#when-should-i-use-redux).

## Refactoring Help Queue to Use Hooks
---

The first refactor we'll make is turning `TicketControl` into a function component. To do this, we'll need to do a few things:

* Update the class component declaration to a function component
* Comment out the constructor.  We'll refactor this more later, and then we'll remove it completely.
* Turn the class methods into functions by adding `const` in front of each declaration
* Remove the `render()` method. 

Here's what our updated code should look like:

<div class="filename">src/components/TicketControl.js</div>

```js
import React from 'react';
import NewTicketForm from './NewTicketForm';
import TicketList from './TicketList';
import EditTicketForm from './EditTicketForm';
import TicketDetail from './TicketDetail';

function TicketControl() {

  // constructor(props) {
  //   super(props);
  //   this.state = {
  //     formVisibleOnPage: false,
  //     mainTicketList: [],
  //     selectedTicket: null,
  //     editing: false
  //   };
  // }

  const handleClick = () => {
    ...
  }

  const handleDeletingTicket = (id) => {
    ...
  }

  const handleEditClick = () => {
    ...
  }

  const handleEditingTicketInList = (ticketToEdit) => {
    ...
  }

  const handleAddingNewTicketToList = (newTicket) => {
    ...
  }

  const handleChangingSelectedTicket = (id) => {
    ...
  }

  let currentlyVisibleState = null;
  let buttonText = null; 

  if (this.state.editing ) {      
    ...
  } else if (this.state.selectedTicket != null) {
    ...
  } else if (this.state.formVisibleOnPage) {
    ...
  } else {
    ...
  }

  return (
    <React.Fragment>
      {currentlyVisibleState}
      <button onClick={this.handleClick}>{buttonText}</button> 
    </React.Fragment>
  );
}

export default TicketControl;
```

### Updating the `formVisibleOnPage` State

Next, we'll update the `formVisibleOnPage` state to be created and managed by a `useState` hook. To do this, we'll need to do a few things:

* First, we'll import the `useState` hook from React.
* Then, we'll declare a `formVisibleOnPage` state variable, with a `setFormVisibleOnPage` updater function by calling the `useState` hook and passing in an initial value of `false`. 
* Finally, we'll update all locations where we call `this.state.formVisibleOnPage` and `this.setState(...)` to reference the new `formVisibleOnPage` state variable and call the `setFormVisibleOnPage` updater function.

Here's the updated code:

<div class="filename">src/components/TicketControl.js</div>

```js
// new import!
import React, { useState } from 'react';
...

function TicketControl() {

  const [formVisibleOnPage, setFormVisibleOnPage] = useState(false);

  const handleClick = () => {
    if (this.state.selectedTicket != null) {
      // new code!
      setFormVisibleOnPage(false);  
      this.setState({
        formVisibleOnPage: false,
        selectedTicket: null,
      });
    } else {
      // new code!
      setFormVisibleOnPage(!formVisibleOnPage);
    }
  }

  const handleDeletingTicket = (id) => {
    ...
  }

  const handleEditClick = () => {
    ...
  }

  const handleEditingTicketInList = (ticketToEdit) => {
    ...
  }

  const handleAddingNewTicketToList = (newTicket) => {
    const newMainTicketList = this.state.mainTicketList.concat(newTicket);
    this.setState({mainTicketList: newMainTicketList});
    // new code
    setFormVisibleOnPage(false)
  }

  const handleChangingSelectedTicket = (id) => {
    ...
  }

  let currentlyVisibleState = null;
  let buttonText = null; 

  if (this.state.editing ) {      
    ...
  } else if (this.state.selectedTicket != null) {
    ...
    // removed this.state
  } else if (formVisibleOnPage) {
    ...
  } else {
    ...
  }
  
  return (
    ...
  );
}

export default TicketControl;
```

### Updating the `mainTicketList` State

Next, we'll update the `mainTicketList` state to be created and managed by a `useState` hook. To do this, we repeat the same process we did with the `formVisibleOnPage` state:

* First, we'll declare a `mainTicketList` state variable, with a `setMainTicketList` updater function by calling the `useState` hook and passing in an initial value of an empty array `[]`. 
* Then, we'll update all locations where we call `this.state.mainTicketList` and `this.setState(...)` to reference the new `mainTicketList` state variable and call the `setMainTicketList` updater function.

We've also made an update to the formatting and indentation when calling components and listing props so that it is easier to read. Here's a snippet from the larger code block below that highlights the updated formatting:

```js
else {
  currentlyVisibleState = 
    <TicketList 
      onTicketSelection={this.handleChangingSelectedTicket} 
      ticketList={mainTicketList} />;
  buttonText = "Add Ticket"; 
}
```

Here's the updated code:

```js
...

function TicketControl() {
  
  const [formVisibleOnPage, setFormVisibleOnPage] = useState(false);
  const [mainTicketList, setMainTicketList] = useState([]);

  const handleClick = () => {
    ...
  }

  const handleDeletingTicket = (id) => {
    // new code!
    const newMainTicketList = mainTicketList.filter(ticket => ticket.id !== id);
    setMainTicketList(newMainTicketList);
    // this.setState({
    //   mainTicketList: newMainTicketList,
    //   selectedTicket: null
    // });
  }

  const handleEditClick = () => {
    this.setState({editing: true});
  }

  const handleEditingTicketInList = (ticketToEdit) => {
    // new code!
    const editedMainTicketList = mainTicketList
      .filter(ticket => ticket.id !== this.state.selectedTicket.id)
      .concat(ticketToEdit);
    // new code!
    setMainTicketList(editedMainTicketList);
    // this.setState({
    //   mainTicketList: editedMainTicketList,
    //   editing: false,
    //   selectedTicket: null
    // });
  }

  const handleAddingNewTicketToList = (newTicket) => {
    // new code!
    const newMainTicketList = mainTicketList.concat(newTicket);
    // new code!
    setMainTicketList(newMainTicketList);
    setFormVisibleOnPage(false)
  }

  const handleChangingSelectedTicket = (id) => {
    ...
  }

  let currentlyVisibleState = null;
  let buttonText = null; 

  if (this.state.editing ) {      
    ...
  } else if (this.state.selectedTicket != null) {
    ...
  } else if (formVisibleOnPage) {
    ...
  } else {
    currentlyVisibleState = 
      <TicketList 
        onTicketSelection={this.handleChangingSelectedTicket} 
        // new code!
        ticketList={mainTicketList} />;
    buttonText = "Add Ticket"; 
  }

  return (
    ...
  );
}

export default TicketControl;
```

### Updating the `formVisibleOnPage` State

We'll finish our refactor to using the `useState` hook by updating the remaining `selectedTicket` and `editing` state slices. The process is exactly the same as with `mainTicketList` and `formVisibleOnPage`. In one case, we'll need to update a variable called `selectedTicket` to `selection` in order to not clash with our state variables. As you read through the updated code below, pay attention to the comments.

Here's the updated code: 

<div class="filename">src/components/TicketControl.js</div>

```js
...

function TicketControl() {

  const [formVisibleOnPage, setFormVisibleOnPage] = useState(false);
  const [mainTicketList, setMainTicketList] = useState([]);
  const [selectedTicket, setSelectedTicket] = useState(null);
  const [editing, setEditing] = useState(false);

  const handleClick = () => {
    if (selectedTicket != null) {
      setFormVisibleOnPage(false);
      // new code!
      setSelectedTicket(null);
      setEditing(false);
    } else {
      setFormVisibleOnPage(!formVisibleOnPage);
    }
  }
  
  const handleDeletingTicket = (id) => {
    const newMainTicketList = mainTicketList.filter(ticket => ticket.id !== id);
    setMainTicketList(newMainTicketList);
    // new code!
    setSelectedTicket(null);
  }

  const handleEditClick = () => {
    // new code!
    setEditing(true);
  }
  
  const handleEditingTicketInList = (ticketToEdit) => {
    const editedMainTicketList = mainTicketList
    // new code: selectedTicket.id
    .filter(ticket => ticket.id !== selectedTicket.id)
    .concat(ticketToEdit);
    setMainTicketList(editedMainTicketList);
    // new code!
    setEditing(false);
    setSelectedTicket(null);
  }

  const handleAddingNewTicketToList = (newTicket) => {
    ...
  }

  const handleChangingSelectedTicket = (id) => {
    // new code: updated variable name to 'selection'
    // so there's no clash with the state variable 'selectedTicket'
    const selection = mainTicketList.filter(ticket => ticket.id === id)[0];
    // new code!
    setSelectedTicket(selection);
  }

  let currentlyVisibleState = null;
  let buttonText = null; 

  // new code: editing
  if (editing) {      
    currentlyVisibleState = 
      <EditTicketForm 
        // new code: selectedTicket
        ticket = {selectedTicket} 
        onEditTicket = {this.handleEditingTicketInList} />
    buttonText = "Return to Ticket List";
  // new code: selectedTicket
  } else if (selectedTicket != null) {
    currentlyVisibleState = 
      <TicketDetail 
        // new code: selectedTicket
        ticket={selectedTicket} 
        onClickingDelete={this.handleDeletingTicket}
        onClickingEdit = {this.handleEditClick} />
    buttonText = "Return to Ticket List";
  } else if (formVisibleOnPage) {
    ...
  } else {
    ...
  }

  return (
    ...
  );
}

export default TicketControl;
```

### Removing Remaining References to `this`

We've refactored all of our state to use the `useState` hook, so what's left? There's still quite a few references to `this` remaining when we pass down our helper functions as props to the components that `TicketControl` renders. These helper functions all used to be class methods that we'd need to reference with `this`, like `this.handleEditingTicketInList`. So let's review the big if/else statement at the end of `TicketControl` and remove all references to `this`. 

Here's the updated code with the new formatting we're using to list components and props:

<div class="filename">src/components/TicketControl.js</div>

```js
...

function TicketControl() {
  ...

  let currentlyVisibleState = null;
  let buttonText = null; 

  if (editing) {      
    currentlyVisibleState = 
      <EditTicketForm 
        ticket = {selectedTicket} 
        onEditTicket = {handleEditingTicketInList} />;
    buttonText = "Return to Ticket List";
  } else if (selectedTicket != null) {
    currentlyVisibleState = 
      <TicketDetail 
        ticket={selectedTicket} 
        onClickingDelete={handleDeletingTicket}
        onClickingEdit = {handleEditClick} />;
    buttonText = "Return to Ticket List";
  } else if (formVisibleOnPage) {
    currentlyVisibleState = 
      <NewTicketForm 
        onNewTicketCreation={handleAddingNewTicketToList}/>;
    buttonText = "Return to Ticket List"; 
  } else {
    currentlyVisibleState = 
      <TicketList 
        onTicketSelection={handleChangingSelectedTicket} 
        ticketList={mainTicketList} />;
    buttonText = "Add Ticket"; 
  }

  return (
    <React.Fragment>
      {currentlyVisibleState}
      <button onClick={handleClick}>{buttonText}</button> 
    </React.Fragment>
  );
}

export default TicketControl;
```

### Confirming a Successful Refactor

Now it's time to verify that our project still works as expected after this refactor. 

Optionally you can do a sweep of your code with `ctrl` + `f` to look for and remove any remaining `this` or `this.state` references before building your project. Or, you can directly build your project, and work through any error messages that pop up that point to missed steps in the refactor process.

Also, I invite you to pause and review the code in `TicketControl` — what do you think of it? Is it easier to read and reason about? What do you like most or least about this refactor?