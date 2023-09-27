In this lesson, we will add the final piece of CRUD functionality to our Help Queue application: the ability to update a ticket. The actual Help Queue doesn't have this functionality — however, it's helpful for us to learn and apply CRUD functionality in our own applications so we are including it here. Also, update functionality is required on this upcoming independent project.

Before we add an `EditTicketForm` component, we will need to do a bit more planning. In the interest of keeping things simple, our new component will be a direct child of `TicketControl`. Here's an updated diagram of our components:

![Our component tree will have our new `EditTicketForm` as a direct child of `TicketControl`.](https://learnhowtoprogram.s3.us-west-2.amazonaws.com/React/Week-1-React-2019/adding-editticketform-component-updated.jpg)

There are a few benefits to this approach:

* Our `TicketControl` component already handles state so we will only need to lift state from the `EditTicketForm` to `TicketControl`.
* There won't be any prop-drilling because props will only be passed down one level to `EditTicketForm`.
* `TicketControl` is already handling which component is showing so we can use its local state to determine whether `EditTicketForm` is showing.

However, this is just one approach to building out the CRUD in our application. It is not necessarily the best way or the most scalable. The key takeaway here is applying problem-solving skills and React knowledge to add CRUD functionality. At the same time, we need to remind ourselves that there is no canonical, opinionated way to do things in React — unlike with Rails or .NET.

We will start by covering the steps we need to take to add edit functionality to our applications. Before reading through these steps, we recommend trying to outline these steps on your own.

One thing that can be helpful to consider is what the user will need to do in order to update a ticket. We can apply what we've learned about Behavior Driven Development here.

**Behavior #1** 

When a user navigates to the ticket detail page, they should be able to click an edit button that takes them to an edit form.

#### **Implementation**:

* We will need to add a slice of state to `TicketControl` to determine whether the `EditTicketForm` is showing or not. The default state will be `editing: false`.
* We will need to add a method to `TicketControl` that will set `editing` to `true`. We will call this method `handleEditClick`.
* Next, we will need to pass down `handleEditClick` to the `TicketDetail` component (and add a PropType).
* Then we will add a button to `TicketDetail` that triggers the `handleEditClick` method. When the button is clicked, `editing` will be set to `true`.
* We also have to add a conditional to render the `EditTicketForm`. We will need to pass the ticket to be edited as a prop down to `EditTicketForm`. Since we will already have a `selectedTicket` set to an actual ticket, we can just pass the `selectedTicket` as a prop.
* Finally, we need to create our `EditTicketForm`. It will use the `ReusableForm` component we made in the last lesson.

**Behavior #2** 

When a user fills out the edit form and clicks the edit button, the specified ticket should be edited in the queue.

#### **Implementation**:

* We will need to add a method to `TicketControl` that will update a ticket. We will do so in `TicketControl` because that is where our state currently resides. We will call this method `handleEditingTicketInList`.
* `handleEditingTicketInList` will update the state in the `mainTicketList` to reflect the edited ticket.
* `handleEditingTicketInList` will also need to update `selectedTicket` to `false` because the ticket will be deleted — and we don't want the `TicketDetail` component showing.
* `handleEditingTicketInList` also has to update `editing` to `false` so the `EditTicketComponent` doesn't show.
* We will need to pass `handleEditingTicketInList` to our new `EditTicketForm` as a prop.
* Next, we will need to add a button to `EditTicketForm` that will trigger a `handleEditTicketFormSubmission` function when clicked.
* Finally, we will add a function called `handleEditTicketFormSubmission` which will capture values from the edit form and then trigger the `handleEditingTicketInList` method in the `TicketControl` component.

That's a lot of stuff to handle! It's mostly little pieces of code here and there but it can feel overwhelming, especially when you're learning React. This will become second nature with practice. Careful planning (drawings help) and writing out the steps needed to add functionality can make this process easier.

## Behavior #1: Toggle Edit Form
---

We will start by writing the code to toggle between a ticket's detail and an edit form. We won't worry about the code for actually editing a ticket yet.

#### Add New State Slice

First we will add a new state slice:

<div class="filename">TicketControl.js</div>

```js
this.state = {
      formVisibleOnPage: false,
      mainTicketList: [],
      selectedTicket: null,
      editing: false // new code
    };
```

The default state of our new state slice is `editing: false`.

#### Add Method for Showing the Edit Form

Next, we will add a method for showing the edit form. This method needs to go in `TicketControl`, which handles the local state that determines which component should show.

<div class="filename">TicketControl.js</div>

```js
handleEditClick = () => {
  console.log("handleEditClick reached!");
  this.setState({editing: true});
}
```

While the `console.log()` isn't technically necessary, it's a good way to make sure our method is being reached.

#### Pass `handleEditClick` As Prop To `TicketDetail` Component

Now we need to update the props passed into our `TicketDetail` component:

<div class="filename">TicketControl.js</div>

```js
...
currentlyVisibleState = 
<TicketDetail 
  ticket = {this.state.selectedTicket} 
  onClickingDelete = {this.handleDeletingTicket} 
  onClickingEdit = {this.handleEditClick} />
...
```

We pass down our method as a prop with the following code: `onClickingEdit = {this.handleEditClick}`.

#### Add "Update" Button with `onClick` Handler to `TicketDetail`

We will need to add an "Update" button to our `TicketDetail` component. When a user clicks this button, the edit form will show. This button will go right above the code for our delete button:

<div class="filename">TicketDetail.js</div>

```jsx

...
<button onClick={ props.onClickingEdit }>Update Ticket</button> { /* new code */ }
<button onClick={()=> props.onClickingDelete(ticket.id) }>Close Ticket</button>
....
```

Also, we can't forget to add a prop type for our new prop:

<div class="filename">TicketDetail.js</div>

```js
TicketDetail.propTypes = {
  ticket: PropTypes.object,
  onClickingDelete: PropTypes.func,
  onClickingEdit: PropTypes.func // new code
};
```

At this point, when we click on the button, we should get the following message in the console: `handleEditClick reached!`.

If we don't get this message in the console, it's time to debug. Check and make sure that props have correctly been passed down, that methods and functions are correctly named, and that a prop type has been added.

### Add A Conditional That Triggers When `editing` Is `true`

Next, we will add a new conditional to the `render()` method. This conditional will trigger if `editing` is set to true:

<div class="filename">TicketControl.js</div>

```js
...
import EditTicketForm from './EditTicketForm';
...
render()
...
    if (this.state.editing ) {      
      currentlyVisibleState = <EditTicketForm ticket = {this.state.selectedTicket} />
      buttonText = "Return to Ticket List";
    } else if (this.state.selectedTicket != null) {
      ...
    }
```

First, we need to import our `EditTicketForm`, which hasn't been created yet. (Our application will fail to compile until we add that component.)

We will pass the current `selectedTicket` to the `EditTicketForm`. Since a user will have to click on a ticket to access the "Update" button, `selectedTicket` will already be set to the ticket we want.

Note that we also need to change the next conditional to an `else if`. If it remained an `if` statement, that conditional would also be met since there is a `selectedTicket` — which means that the `TicketDetail` component would show even if `editing` is set to true.

#### Add `EditTicketForm` Component

This is the final step for our first behavior. Once we have an `EditTicketForm` in place, a user will be able to navigate to the edit page. We will use the `ReusableForm` component we created in the last lesson for our form.

<div class="filename">EditTicketForm.js</div>

```js
import React from "react";
import ReusableForm from "./ReusableForm";

function EditTicketForm (props) {
  return (
    <React.Fragment>
      <ReusableForm 
        buttonText="Update Ticket" />
    </React.Fragment>
  );
}

export default EditTicketForm;
```

The "Update Ticket" button won't do anything yet — this is part of the second behavior that we have yet to implement.

At this point, a user should be able to access the edit form and return to the main page. If the application doesn't work correctly, it's time to debug. While debugging can be frustrating, it's a great opportunity to get a better understanding of how React works.

## Behavior #2: Editing a Ticket
---

Fortunately, this behavior will be much easier to implement now that we have our form in place. We just need to create a new method for updating a ticket and then pass it down to the `EditTicketForm` so it can be attached to a click handler.

#### Write Method for Updating Ticket

As stated in our implementation for this behavior, the method needs to do the following things:

* Update the state of `mainTicketList` to show the edited ticket;
* Set `selectedTicket` to `false` because the previously `selectedTicket` will no longer exist (since it's been deleted).
* Set `editing` to `false` so the `TicketList` component shows instead of the `EditTicketForm` component.

Here's our new method:

<div class="filename">TicketControl.js</div>

```js
handleEditingTicketInList = (ticketToEdit) => {
  const editedMainTicketList = this.state.mainTicketList
    .filter(ticket => ticket.id !== this.state.selectedTicket.id)
    .concat(ticketToEdit);
  this.setState({
      mainTicketList: editedMainTicketList,
      editing: false,
      selectedTicket: null
    });
}
```

Let's start by taking a look at the first part of our new method:

```js
const editedMainTicketList = this.state.mainTicketList
      .filter(ticket => ticket.id !== this.state.selectedTicket.id)
      .concat(ticketToEdit);
```

Note that we've broken this up into multiple lines to make it more readable. This is a common technique when we chain multiple functions together. It is not required (and it does not change the functionality of the code), but it can make chained functions easier on the eyes.

We filter the previous version of the ticket out of the list with `filter()` and then add the edited version of the ticket to the list with `concat()`. While we could've edited the ticket directly, this is easier and doesn't involve mutating the ticket — just replace it with the new version.

Next, we set the `mainTicketList` to be equal to the list with the updated ticket.

Finally, we update `editing` to `false` and `selectedTicket` to `null`.

#### Pass New Method Down to `EditTicketForm` as a Prop

Next, we need to pass our new method down as a prop to our `EditTicketForm` component:

<div class="filename">TicketControl.js</div>

```js
currentlyVisibleState = <EditTicketForm ticket = {this.state.selectedTicket} onEditTicket = {this.handleEditingTicketInList} />
```

We've added this code: `onEditTicket = {this.handleEditingTicketInList}`.

#### Add PropType to `EditTicketForm` Component

Next, let's add a prop type to `EditTicketForm` for the function we are passing down as a prop. We also need to import `PropTypes` as well.

<div class="filename">EditTicketForm.js</div>

```js
import PropTypes from "prop-types";
...
EditTicketForm.propTypes = {
  ticket: PropTypes.object,
  onEditTicket: PropTypes.func
};
```

#### Add Event Handler in `EditTicketForm` Along with Function to Capture Form Values

Lastly, we'll add a function called `handleEditTicketFormSubmission` that captures form values and triggers the `handleEditingTicketInList` method in `TicketControl`. We also need to update our JSX so the event handler in our form refers to our new `handleEditTicketFormSubmission` function.

<div class="filename">EditTicketForm.js</div>

```js
...
function EditTicketForm(props){
  const { ticket } = props;
  
  function handleEditTicketFormSubmission(event) {
    event.preventDefault();
    props.onEditTicket({names: event.target.names.value, location: event.target.location.value, issue: event.target.issue.value, id: ticket.id});
  }

  return (
    <React.Fragment>
      <ReusableForm 
        formSubmissionHandler={handleEditTicketFormSubmission} /* new code */ 
        buttonText="Update Ticket" />
    </React.Fragment>
  );
}
```

Our function grabs values from the form and then triggers the `handleEditingTicketInList` via the `onEditTicket` prop.

We also update the event handler on our `ReusableForm` component to be `handleEditTicketFormSubmission`.

At this point, our Update functionality should be working and we'll have full CRUD for our Help Queue.

There is still a small bug, however. If you want to practice debugging, see if you can find it and fix it on your own.

If we get to the editing a ticket form and click "Return to Ticket List," it won't actually return us to the ticket list. And if we click that button and _then_ try to edit a ticket, we'll get an error.

The issue, once again, comes from the `handleClick()` method in `TicketControl.js`. We need to make sure that `editing: false` when the method is triggered. Here's the updated method:

<div class="filename">TicketControl.js</div>

```js
handleClick = () => {
  if (this.state.selectedTicket != null) {
    this.setState({
      formVisibleOnPage: false,
      selectedTicket: null,
      editing: false // new code
    });
  } else {
    this.setState(prevState => ({
      formVisibleOnPage: !prevState.formVisibleOnPage,
    }));
  }
}
```

The cause of the bug should now make sense. When a user navigated to the edit form and clicked the "Return to Ticket List" button, `selectedTicket` is not `null` — however, our conditional set `formVisibleOnPage` to `false` and `selectedTicket` to `null` _without_ setting `editing` to `false`. That way, if a user then tried to fill out the edit form, they'd get an error because the edit form would no longer be associated with a specific ticket. It's an error that seems a bit weird at first — but which becomes clear once we take a closer look at the code.

At this point, a senior dev on the team would say it's time to refactor. This method is really doing a lot — it's handling four different button clicks seamlessly. On the one hand, it's great to get a method to do so much, but on the other hand, it's going to get increasingly buggy, especially in a larger code base. As you create your React applications this week, it's important to think about the tradeoffs in your design decisions, and to discuss them with your peers.

## Summary
---

In this lesson, we added several behaviors to our Help Queue application. First, we planned out the new behaviors our application needs and listed all the steps we need to take to implement these behaviors. While it's not necessary to write down all of these steps, it can be helpful for newcomers to React.

Next, we added functionality to show an edit form (local state) and then update a ticket in our `mainTicketList` (shared state). Once again, we had to deal with a lot of little pieces — and it's considerably more involved than adding a few routes in Rails or .NET. It may even seem like we needed to add a huge and overly complicated amount of code when we could do a fairly simple implementation with vanilla JS. 

However, we've written dynamic, modular and scalable code that lends itself well to further expansion. If all the steps are still overwhelming, trust the process — learning a new library or framework is always challenging and React is no different. In a few weeks, working with these concepts will become second nature.
