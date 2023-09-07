In the lesson called _Planning Our Application: Part 3_, we updated the plan for our Help Queue application to include a `TicketDetail` component. A user should be able to click on a ticket and then go to that ticket's detail page.

Before starting, this might be a good time to take a deep breath. We aren't going to be adding too many new lines of code to our application in this lesson. However, we have to do a lot of little things in many different places. This will be a long lesson, so pay close attention.

Here are the things we need to do to add ticket detail functionality to our site:

* Create a placeholder `TicketDetail` component (it will be updated to take props later in this lesson).
* Update `TicketControl` to include a `selectedTicket` state slice.
* Create a method in `TicketControl` that will handle when a ticket is clicked.
* Create a new conditional in `TicketControl` to handle the `TicketDetail` component.
* Use props to pass down our method for handling a ticket click first to our `TicketList` component and then to the `Ticket` component, where the method will be attached to a ticket.
* Once the `selectedTicket` state is properly being updated, update the `TicketDetail` component to accept `TicketDetail` props.
* Add PropTypes for props as needed.

This is a lot of stuff — but we are being exhaustive in outlining each step we need to take to update our code. Take your time with this lesson — and also when you are adding functionality across multiple components in your own React applications.

### Create a Placeholder `TicketDetail` Component

Let's start with a placeholder `TicketDetail` component:

<div class="filename">TicketDetail.js</div>

```js
import React from "react";

function TicketDetail(props){
  return (
    <React.Fragment>
      <h1>Ticket Detail</h1>
      <hr/>
    </React.Fragment>
  );
}

export default TicketDetail;
```

There's nothing new or unusual here — just a component rendering a single hard-coded element. We will update this component to use props after we successfully get the `TicketControl` component to update a selected ticket.

### Update the `TicketControl` Component

Now let's make some changes to our `TicketControl` component. We will do the following:

* Add a state slice for `selectedTicket`. It will have a default state of `null`.
* Add a method called `handleChangingSelectedTicket` for handling a click event on a ticket. When a user clicks on the ticket, it will activate this method, which will change the value of `selectedTicket` to an actual ticket object.
* Add a conditional for rendering the `TicketDetail` component when `selectedTicket` is not `null`. This will ensure the user actually sees the `TicketDetail` component.

#### Update `TicketControl` State

We will start by updating our state:

<div class="filename">TicketControl.js</div>

```js
...
    this.state = {
      formVisibleOnPage: false,
      mainTicketList: [],
      selectedTicket: null // new code
    };
...
```

Our `selectedTicket` starts as `null` because no ticket has been selected yet.

#### Write Method to Handle Click Event on Ticket

Next, let's write a method that will take the `id` property of a ticket, find the correct ticket, and then set `selectedTicket` equal to that ticket. Since we know what this method should do, we can write it in isolation without worrying how it will interact with other components. We'll add this method to `TicketControl` after the constructor and before the `render()` method (which is where all our custom methods should go in a class component).

<div class="filename">TicketControl.js</div>

```js
...
  handleChangingSelectedTicket = (id) => {
    const selectedTicket = this.state.mainTicketList.filter(ticket => ticket.id === id)[0];
    this.setState({selectedTicket: selectedTicket});
  }
...
```

We will use `filter()` (which is perfect for functional programming) to filter our mainTicketList down to tickets where `ticket.id` equals the `id` passed into our method. Because we are using UUIDs now, we know that only one ticket will ever have a matching `id`.

Because `filter()` returns an array, we need to specify that we want the first (and only element) in that array. We use bracket notation to do that.

Finally, we use the `setState` method to mutate the state of the `selectedTicket` state slice.

Our method isn't connected to our user interface yet — but its functionality will be in place by the end of the lesson.

### Update `TicketControl`'s `render()` Method

Now we need to make some updates to our `render()` method. We've included comments detailing the three changes we are making. The first two changes are related to adding a conditional to our conditional rendering. The third involves passing our new method as a prop.

<div class="filename">TicketControl.js</div>

```js
...
import TicketDetail from './TicketDetail';

...

  render(){
    let currentlyVisibleState = null;
    let buttonText = null; 
    
    if (this.state.selectedTicket != null) {
      currentlyVisibleState = <TicketDetail ticket = {this.state.selectedTicket} />
      buttonText = "Return to Ticket List";
      // While our TicketDetail component only takes placeholder data, we will eventually be passing the value of selectedTicket as a prop.
    }
    else if (this.state.formVisibleOnPage) {
      // This conditional needs to be updated to "else if."
      currentlyVisibleState = <NewTicketForm onNewTicketCreation={this.handleAddingNewTicketToList}  />;
      buttonText = "Return to Ticket List";
    } else {
      currentlyVisibleState = <TicketList ticketList={this.state.mainTicketList} onTicketSelection={this.handleChangingSelectedTicket} />;
      // Because a user will actually be clicking on the ticket in the Ticket component, we will need to pass our new handleChangingSelectedTicket method as a prop.
      buttonText = "Add Ticket";
    }

    ...
  }
```

First, we add a conditional that will render the `TicketDetail` component if `selectedTicket != null`. If `this.state.selectedTicket` isn't null, it will be set to the value of the `selectedTicket`.

Right now, the `TicketDetail` is just a hard-coded placeholder. However, we will eventually be passing `selectedTicket` into the `TicketDetail` component. For that reason, we will pass in a prop called `ticket` that we will set to the value of `selectedTicket`. We will attach this prop to `TicketDetail` soon.

We also update our previous `if` statement to an `else if` statement. We won't use two `if` statements — if the `TicketDetail` component should be rendered, there's no reason to check if the `NewTicketForm` should be rendered.

Last, we need to pass our new `handleChangingSelectedTicket` method down to the `TicketList` component as a prop so we add the following code to `<TicketList />`:

```jsx
onTicketSelection={this.handleChangingSelectedTicket}
```

We are saving the value of `this.handleChangingSelectedTicket` in the `onTicketSelection` prop.

### Pass Props Through `TicketList` Component

We need to make several small changes to our `TicketList` component:

* We will no longer use the `index` of our iterator as a `key`. We will use a ticket's actual `id` property instead.
* We will add a PropType for our new `onTicketSelection` prop.
* We will also pass down several props to the `Ticket` component.

Here are the updates:

<div class="filename">TicketList.js</div>

```js
...

function TicketList(props){

  return (
    <React.Fragment>
      <hr/>
      {props.ticketList.map((ticket) =>
        <Ticket
          whenTicketClicked = { props.onTicketSelection }
          names={ticket.names}
          location={ticket.location}
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

...
```

First, we no longer need to pass `index` into our `map()` function. We've updated the `key` to be equal to `ticket.id`.

Note that we also have to pass in an `id` prop. This is because we can't pass a `key` to a child component as a prop. However, our `Ticket` component will still need access to its own `id`, hence a separate `id` prop which is also set to `ticket.id`.

We will also pass `props.onTicketSelection` down to the `Ticket` component as a prop. The `Ticket` component will handle determining whether it has been selected — not the `TicketList` component. For the sake of clarity, we are naming the prop being passed down to the `Ticket` component `whenTicketClicked`. Because `onTicketSelection` is itself a prop from the `TicketControl` component, it is one of `TicketList`'s props, which is why we need to prefix `onTicketSelection` with `props`.

Finally, we also need to add a PropType: `onTicketSelection: PropTypes.func`.

### Add Method To `Ticket` Component

Now it's time to pass the `handleChangingSelectedTicket` method down to our `Ticket` component. It has been passed down from `TicketControl` to `TicketList` as a prop called `onTicketSelection`. Then `TicketList` passed it down to `Ticket` as a prop called `whenTicketClicked`.

It may already seem a bit convoluted — and we are only passing this method down through a few components. It should be clear why prop drilling through many components should generally be avoided. Fortunately, we are only passing this prop down from a parent to a child to that child's child. That's not a big deal — but our code would get messy if we drilled much further.

Let's take a look at our updated `Ticket` component. All we are adding is a div with an `onClick` handler and a few `PropTypes`. Once again, we've also added a few comments — you should remove these from your own code if you are following along.

<div class="filename">Ticket.js</div>

```js
...

function Ticket(props){
  return (
    <React.Fragment>
      <div onClick = {() => props.whenTicketClicked(props.id)}>
        { /* We add a div with an onClick function. Don't forget to close out the div below! */}
        <h3>{props.location} - {props.names}</h3>
        <p><em>{props.issue}</em></p>
        <hr/>
      </div>
    </React.Fragment>
  );
}

Ticket.propTypes = {
  names: PropTypes.string,
  location: PropTypes.string,
  issue: PropTypes.string,
  id: PropTypes.string, // new PropType
  whenTicketClicked: PropTypes.func // new PropType
};

...
```

We wrap an individual ticket in a div that looks like this:

```js
<div onClick = {() => props.whenTicketClicked(props.id)}>
```

Because `TicketList` is iterating through each individual ticket, each ticket will have its own div with an `onClick` handler attached.

As we discussed in the last lesson, we need to use an arrow function so the expression isn't evaluated immediately. We pass in the ticket's id via `props.id`.

At this point, we can click on a ticket and the `selectedTicket` state slice in `TicketControl` will update, showing our placeholder `TicketDetail` component. Once again, it may seem like information is being passed up from `Ticket` all the way to `TicketControl`, but the opposite is happening. Instead, think of `TicketControl` expanding its scope all the way down to `Ticket` with the help of props and callbacks.

### Update `TicketDetail` Component to Render `SelectedTicket`

Earlier in this lesson, we added the following line of code to `TicketControl`:

<div class="filename">TicketControl.js</div>

```jsx
currentlyVisibleState = <TicketDetail ticket = {this.state.selectedTicket} />
```

We specified that the value of `this.state.selectedTicket` should be passed as a prop called `ticket` down to our `TicketDetail` component. 

Now we just need to use those props in `TicketDetail`. Here's our updated `TicketDetail` component:

<div class="filename">TicketDetail.js</div>

```js
import React from "react";
import PropTypes from "prop-types";

function TicketDetail(props){
  const { ticket } = props;
  
  return (
    <React.Fragment>
      <h1>Ticket Detail</h1>
      <h3>{ticket.location} - {ticket.names}</h3>
      <p><em>{ticket.issue}</em></p>
      <hr/>
    </React.Fragment>
  );
}

TicketDetail.propTypes = {
  ticket: PropTypes.object
};

export default TicketDetail;
```

Note that we use object destructuring (`const { ticket } = props;`) to derive the `ticket` object from our props. Otherwise, for a ticket attribute like `location`, we'd need to say `props.ticket.location` instead of just `ticket.location`. It is common — but not necessary — to use object destructuring with props in React.

We also specify that `ticket` will have a PropType of `object`.

#### Finishing Up

We are almost done. However, there's still a problem with our application. When we navigate to ticket detail and click on the _Return to Ticket List_ button, nothing will happen.

Let's take a look at that button in the `TicketControl` component:

<div class="filename">TicketControl.js</div>

```js
...
<button onClick={this.handleClick}>{buttonText}</button>
...
```

The issue must be in the `handleClick` method, so let's take a look at that next:

<div class="filename">TicketControl.js</div>

```js
...
  handleClick = () => {
    this.setState(prevState => ({
      formVisibleOnPage: !prevState.formVisibleOnPage
    }));
  }
...
```

Currently, the `handleClick` method only toggles the visibility of our form. We also need it to set `selectedTicket` to `null` so the queue can show. But it's not quite that simple. When we click on a ticket's detail, `formVisibleOnPage` is set to `false`. We don't want to toggle it back to `true`! That's why we see the form for adding a ticket when we click on the button.

Let's update the `handleClick` method so it can also handle returning to the queue from the ticket detail page. The solution isn't going to be ideal, but it will get the job done:

<div class="filename">TicketControl.js</div>

```js
...
  handleClick = () => {
    if (this.state.selectedTicket != null) {
      this.setState({
        formVisibleOnPage: false,
        selectedTicket: null
      });
    } else {
      this.setState(prevState => ({
        formVisibleOnPage: !prevState.formVisibleOnPage,
      }));
    }
  }
...
```

We know that if `this.state.selectedTicket` isn't `null` then we must be on the ticket detail page. In that case, we know that `formVisibleOnPage` should be set to false and `selectedTicket` should be set to `null`. Otherwise, we know that we must be on the add ticket page or the ticket list page — so our `else` statement can just continue to toggle the `formVisibleOnPage` state.

Now our button toggles between all of our components.

However, we are at the point when this function is probably trying to do too much. It felt like nice, clean code and a reusable function before — and while we've adapted it for yet another use, it's not quite as clean anymore. We definitely wouldn't want this function to have a ton of conditionals, each for handling a different kind of click! So this is the point where we might want to consider refactoring this function as our application gets larger. Because everything is working, we won't do a refactor here — but it's always good to pay close attention and consider when we might want to refactor code.

If you've been holding your breath (hopefully not), you can breathe out now. All of these steps may seem overly complicated at first. There are a lot of moving parts in a React application, especially once we start passing around a lot of props. Good planning is very important. Ultimately, the content in this lesson will be more likely to click if you code along with it.

Practice is also important, and at least in the short term, try to look at bugs as a potential teacher as opposed to a source of frustration. Bugs _will_ happen, especially at first. It can be challenging to keep track of all the props that need to be passed around — along with all the other little details that come with adding a core piece of functionality. However, React error messages tend to be very informative, so follow your errors up and down the component tree until you see where everything connects.