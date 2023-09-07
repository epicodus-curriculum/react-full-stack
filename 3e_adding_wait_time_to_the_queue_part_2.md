In the last lesson, we covered the "business" logic for updating a ticket's elapsed time in the Redux store. In the process, we had to write and test a reducer action, update and test another reducer action, and write action creators.

Now that we're ready to implement this functionality in the UI, we should think carefully about where it's needed.

* We know that a new ticket should have a `timeOpen` and `formattedWaitTime` property, which means that we'll need to add these in `NewTicketForm.js`.
* Next, we'll need to implement a timer in our `TicketControl` component. Fortunately, we added most of the code for that a few lessons ago. We'll go over it again in this lesson.
* When we demonstrated how our timer would work, we created an `updateTicketElapsedWaitTime()` method that logged `"tick"` to the queue. In this lesson, we'll update that method to actually update the elapsed wait time for a ticket.
* Once that's complete, we'll need to pass the `timeOpen` and `formattedWaitTime` properties of a ticket to the `Ticket` component via `TicketList`. (And we can't forget to add PropTypes, either.)
* We'll also need to make a tweak to our `EditTicketForm` so that `timeOpen` and `formattedWaitTime` don't become undefined when we update tickets. 

To recap, we'll need to make changes to the following five components:

* `NewTicketForm.js`;
* `TicketControl.js`;
* `TicketList.js`;
* `Ticket.js`;
* `EditTicketForm.js`.

We'll make the changes in that order.

### Changes to `NewTicketForm.js`

We just need to make a few small changes to `NewTicketForm.js`:

<div class="filename">NewTicketForm.js</div>

```js
...
import { formatDistanceToNow } from 'date-fns';
...

function NewTicketForm(props){
  function handleNewTicketFormSubmission(event) {
    event.preventDefault();
    props.onNewTicketCreation({
      names: event.target.names.value,
      location: event.target.location.value, 
      issue: event.target.issue.value,
      id: v4(),
      timeOpen: new Date(),
      formattedWaitTime: formatDistanceToNow(new Date(), {
        addSuffix: true
      })
    });
  }
...
```

First we need to import date-fns. Then we need to add `timeOpen` and `formattedWaitTime` properties to the ticket object. `timeOpen` will be set to the value of a `new Date()`. That way, when a user adds a ticket via the form and this function is triggered, the current date and time will be created for a ticket's `timeOpen` property. Meanwhile, `formattedWaitTime` will be set to the formatted elapsed time since the ticket was opened. Later, we'll use the date set for `timeOpen` to update the `formattedWaitTime`.

If we wanted to, we could define `timeOpen` and `formattedWaitTime` in `TicketControl.js` instead. After all, these aren't values that are being pulled from the form. `TicketControl.js` actually processes the new tickets and passes them to the Redux store, so why not add these values to `TicketControl`'s `handleAddingNewTicketToList()` method instead?

An argument could be made for that — but there are advantages to creating all the properties a ticket needs in the same place. This keeps our code modular and makes it easier for other developers to read. Since `NewTicketForm` already creates the other properties, it makes sense to create the `timeOpen` and `formattedWaitTime` properties here as well.

### Changes to `TicketControl.js`

Most of our updates will be made in this component. However, we already implemented almost all of the changes we need when we learned about lifecycle methods and implemented a timer. Here's what we have so far from that lesson:

<div class="filename">TicketControl.js</div>

```js
...

componentDidMount() {
    this.waitTimeUpdateTimer = setInterval(() =>
      this.updateTicketElapsedWaitTime(),
    60000
    );
  }

  componentWillUnmount(){
    clearInterval(this.waitTimeUpdateTimer);
  }

  updateTicketElapsedWaitTime = () => {
    // New code will go here.
  }
```

A few things to note:

* We've changed the interval of our `setInterval()` function to `60000` because the Help Queue at Epicodus updates every minute.

* There's no need to use the `componentDidUpdate()` lifecycle method here so it's been removed.

* The only other new code we will need to add to `TicketControl` will go in `updatedTicketElapsedWaitTime()`. Every 60 seconds, this function will be called. As you may have already guessed, we'll be dispatching our new action here.

Let's add the code to implement date-fns now:

<div class="filename">src/components/TicketControl.js</div>

```js
...
import { formatDistanceToNow } from 'date-fns';
...

...
updateTicketElapsedWaitTime = () => {
  const { dispatch } = this.props;
  Object.values(this.props.mainTicketList).forEach(ticket => {
      const newFormattedWaitTime = formatDistanceToNow(ticket.timeOpen, {
        addSuffix: true
      });
    const action = a.updateTime(ticket.id, newFormattedWaitTime);
    dispatch(action);
  });
}
...
```

Note that we've added a new import statement to the top of `TicketControl.js` to import the help function `formatDistanceToNow()`.

Next, let's cover exactly what the `updateTicketElapsedWaitTime` function is doing. This will all be review of concepts we've already covered.

* We start by deconstructing the `dispatch` function from `this.props`.

* We iterate over the values in the `mainTicketList`. For each ticket, we determine the `formattedWaitTime` using the `formatDistanceToNow()` method from date-fns. This time we pass in `ticket.timeOpen` as the first argument. That's because we want to calculate the distance between the time the ticket was first opened that's stored in the `timeOpen` property, and now.

* Note that we use `forEach()`. Technically, we could use `map()` and it would work. However, this function only has side effects (updating the store) and `map()` is supposed to return something without side effects. For that reason, `forEach()` communicates our intentions here.

There's another important thing to note about this code. It's not going to scale up very well. Every minute, this function will dispatch an action for _every single ticket_ in the queue. That wouldn't be very efficient if our queue was meant to handle thousands of tickets. Since this is just a simple implementation — and our queue doesn't need to handle a lot of tickets anyway — we won't worry about it. However, you may want to see if you can refactor it to be more efficient on your own.

### Changes to `TicketList.js`

The `Ticket.js` component needs to have access to the `formattedWaitTime` property. In order to pass these props to `Ticket.js`, though, we first need to pass them to `TicketList.js`, which is the direct parent of `Ticket.js` and the child of `TicketControl.js`:

<div class="filename">src/components/TicketList.js</div>

```js
...
function TicketList(props){
  return (
    <React.Fragment>
      <hr/>
      {Object.values(props.ticketList).map((ticket) => {
        return <Ticket
          whenTicketClicked = { props.onTicketSelection }
          names={ticket.names}
          location={ticket.location}
          issue={ticket.issue}
          formattedWaitTime={ticket.formattedWaitTime}
          id={ticket.id}
          key={ticket.id}/>
  })}
    </React.Fragment>
  );
}
...
```

We just add `formattedWaitTime` as a prop to pass down to `Ticket.js`. We don't need to pass `timeOpen` as a prop because we won't be displaying it in `Ticket.js`. We'll only be showing the `formattedWaitTime`.

### Changes to `Ticket.js`

Now we just need to add `formattedWaitTime` as a prop type to `Ticket.js` and then actually use the prop in our React fragment:

<div class="filename">src/components/Ticket.js</div>

```js
...

function Ticket(props){
  return (
    <React.Fragment>
      <div onClick = {()=> props.whenTicketClicked(props.id)}>
        <h3>{props.location} - {props.names}</h3>
        <p><em>{props.issue}</em></p>
        { /* new code below. */}
        <p><em>{props.formattedWaitTime}</em></p>
        <hr/>
      </div>
    </React.Fragment>
  );
}

Ticket.propTypes = {
  // New prop type added.
  ...
  formattedWaitTime: PropTypes.string
};

...
```

Only two lines of code have been added here. Comments have been included to highlight the new lines of code.

### Changes to `EditTicketForm.js`

If we run our application, everything will work correctly when we add a new ticket. The time for a new ticket will display as `'less than a minute ago'`. If we wait a minute, it will update to `'One minute ago'`.

However, if we edit a ticket, the elapsed time will no longer display. See if you can figure out the issue on your own first — it's a quick fix.

The issue is in the `handleEditTicketFormSubmission()` function. When we call `props.onEditTicket()`, we don't specify that the `timeOpen` and `formattedWaitTime` properties should be included in the ticket object. Remember that when we update a ticket, it's almost exactly the same as adding a new ticket — we just happen to be overwriting the pre-existing ticket with the same `id`. To fix this issue, we just need to pass the `timeOpen` and `formattedWaitTime` properties into the method.

<div class="filename">src/components/EditTicketForm.js</div>

```js
...
function handleEditTicketFormSubmission(event) {
  event.preventDefault();
  props.onEditTicket({
    names: event.target.names.value, 
    location: event.target.location.value, 
    issue: event.target.issue.value, 
    id: ticket.id, 
    timeOpen: ticket.timeOpen, 
    formattedWaitTime: ticket.formattedWaitTime 
  });
}
...
```

At this point, our Help Queue should now show the elapsed time since the ticket was opened regardless of whether the ticket was updated.

### Conclusion

As we've already stated, you won't be expected to add this kind of functionality to your independent projects. The most important takeaway from these lessons isn't using a timer or incorporating date-fns — although it's fun to do both — or getting the Help Queue fully functional. Instead, it's essential to know about how the component lifecycle works — and to have at least a basic understanding of the following methods and what they do:

* `constructor()`: Called when a state-based component is instantiated;
* `render()`: Called both when a component is being loaded and also when it's updated;
* `componentDidMount()`: Called when a component has finished updating the DOM;
* `componentDidUpdate()`: Called immediately after a component updates;
* `componentWillUnmount()`; Called right before a component is unmounted and destroyed.
