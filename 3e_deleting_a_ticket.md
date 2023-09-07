In the last lesson, we added the ability for a user to see an individual ticket's detail page. We had to alter or create four different components in order to add this relatively small piece of functionality.

However, some good news. Adding delete functionality to our application is going to be much easier. This is because we planned out our application carefully. We will only need to alter two components — `TicketControl` (which holds our shared state) and `TicketDetail` (where the delete button will live).

If we'd chosen to place `TicketDetail` below `Ticket` in the component tree, we'd have a prop-drilling nightmare. Our method for deleting tickets would affect four different components because we'd need to pass it as a prop through `TicketList`, `Ticket` and `TicketDetail`.

Here are the steps we will need to take to add delete functionality to our Help Queue:

* Write a `handleDeletingTicket` method in `TicketControl`. This method will mutate the state of the `mainTicketList`.
* Pass this method down as a prop to `TicketDetail`. We will call this prop `onClickingDelete`.
* Add a button to `TicketDetail` with an `onClick` event handler that will trigger `onClickingDelete`.
* As always, we will need to add a PropType for `onClickingDelete`.

### `handleDeletingTicket` Method

We will start by adding a method to our `TicketControl` component that will delete a ticket from the `mainTicketList` based on its ID. Once again, we can write this method in isolation without worry about how it will interact with other components yet.

<div class="filename">TicketControl.js</div>

```js
handleDeletingTicket = (id) => {
  const newMainTicketList = this.state.mainTicketList.filter(ticket => ticket.id !== id);
  this.setState({
    mainTicketList: newMainTicketList,
    selectedTicket: null
  });
}
```

`handleDeletingTicket` will take an `id` as a parameter.

We will use the `filter()` method again. This time, though, we want the `newMainTicketList` to filter everything that _doesn't_ have the ticket ID that will be passed into the method. In other words, we are _filtering out_ the ticket that has the specified ID because we want it to be deleted from the list.

Next, we need to set the state of the `mainTicketList` to be equal to our filtered `newMainTicketList`.

Finally, we will also need to set `selectedTicket` back to null. That way, once a ticket is closed, `TicketControl` will ensure that the `TicketList` component is showing.

### Passing `handleDeletingTicket` as Prop to `TicketDetail`

Next, we need to pass our `handleDeletingTicket` method as a prop to `TicketControl`. This just involves a small change in our `render()` method:

<div class="filename">TicketControl.js</div>

```js
render(){
   ...
  if (this.state.selectedTicket != null) {
    currentlyVisibleState = <TicketDetail ticket = {this.state.selectedTicket} onClickingDelete = {this.handleDeletingTicket} />
    buttonText = "Return to Ticket List";
  }
...
}
```

### Adding a Delete Button and PropType to `TicketDetail`

We just need to make a few small changes to our `TicketDetail` component:

<div class="filename">TicketDetail.js</div>

```jsx
import React from "react";
import PropTypes from "prop-types";

function TicketDetail(props){
  const { ticket, onClickingDelete } = props; //new code
  
  return (
    <React.Fragment>
      <h1>Ticket Detail</h1>
      <h3>{ticket.location} - {ticket.names}</h3>
      <p><em>{ticket.issue}</em></p>
      <button onClick={()=> onClickingDelete(ticket.id) }>Close Ticket</button> { /* new code */ }
      <hr/>
    </React.Fragment>
  );
}

TicketDetail.propTypes = {
  ticket: PropTypes.object,
  onClickingDelete: PropTypes.func // new code
};

export default TicketDetail;
```

First, we add a button with an `onClick` handler. When the button is clicked, `onClickingDelete(ticket.id)` will be executed. Once again, we need to use `() =>` in our JSX curly braces because our function has parens with an argument. Also note that we're destructuring props to create an onClickingDelete method, just like we did for ticket. 

Finally, we need to add a `PropType` for `onClickingDelete`.

And that's it! We didn't have to add too much code, in large part because we didn't do any prop-drilling.
