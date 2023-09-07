In the last lesson, we updated the CRUD methods in our Help Queue application. They are now connected to a Redux store. In this lesson, we will use the `mapStateToProps()` function in order to display the state from our Redux store in our application. We will need to make minor changes in two components to do this.

### Mapping State to Props

We're now ready to use an extremely powerful part of React Redux: the `mapStateToProps` function. This function takes a state slice from the store and then maps it to a prop in the component.

Add the following code just above the export statement at the end of `TicketControl`. Note that we've already written the line with the `connect()` function at the bottom of the code snippet below — however, a key addition has been made to that line of code. 

<div class="filename">src/components/TicketControl.js</div>

```js
...

const mapStateToProps = state => {
  return {
    mainTicketList: state
  }
}

// Note: we are now passing mapStateToProps into the connect() function.

TicketControl = connect(mapStateToProps)(TicketControl);
```

In the code snippet above, we define how the `mapStateToProps` function should look. It will always have the following format:

```js
const mapStateToProps = state => {
  return {
    // Key-value pairs of state to be mapped from Redux to React component go here.
  }
}
```

The key-value pairs determine the state slices that should be mapped to the component's props. In our case, we want `mainTicketList` from the store to be mapped to `TicketControl`'s props.

Then we need to pass our newly-defined `mapStateToProps` function into the `connect()` function:

```js
TicketControl = connect(mapStateToProps)(TicketControl);
```

This ensures the `TicketControl` component has the `mapStateToProps` functionality when `connect()` redefines the component.

### Adding PropTypes

We are mapping state from the Redux store to our component's props. That means we need to add prop types to `TicketControl`. Let's do so now:

<div class="filename">src/components/TicketControl.js</div>

```js
...
import PropTypes from "prop-types";

...

TicketControl.propTypes = {
  mainTicketList: PropTypes.object
};

// mapStateToProps function is here.
...
```

The `mainTicketList` in our Redux store is an object so we define it as that prop type.

### Updating Our Code To Use Our New Prop

Next, we need to pass this prop to where it's needed — our `TicketList` component! We could also use the `connect()` function directly in that component. However, we are already using `connect()` in our `TicketControl` component — and it will take a minimal refactor to get the `TicketList` component working correctly.

We will need to change two things in our `TicketControl` component. First, we need to update our `handleChangingSelectedTicket` method. Currently, the method looks like this:

<div class="filename">src/components/TicketControl.js</div>

```js
...
handleChangingSelectedTicket = (id) => {
  const selectedTicket = this.state.mainTicketList.filter(ticket => ticket.id === id)[0];
  this.setState({selectedTicket: selectedTicket});
}
```

`mainTicketList` is no longer part of `this.state` — it's part of the Redux store now and we need to pass it into the component via `this.props`. Also, `mainTicketList` is an object now, not an array. No need to use filter anymore — we can just use bracket notation instead. Here's the updated method:

<div class="filename">src/components/TicketControl.js</div>

```js
...
handleChangingSelectedTicket = (id) => {
  const selectedTicket = this.props.mainTicketList[id];
  this.setState({selectedTicket: selectedTicket});
}
...
```

We only need to change part of one line of code: we will now get the value of `selectedTicket` from `this.props.mainTicketList[id]`.

This is the power of `mapStateToProps`: we don't have to do any fancy additional code to get a specific state slice from the store. We can just use `this.props` — as long as we've defined the state slice we want to map in our `mapStateToProps` function literal.

There is one more line of code we need to change in `TicketControl.js`. We need to change the `ticketList` prop that is currently being passed down to the `TicketList` component. Currently, it looks like this:

<div class="filename">src/components/TicketControl.js</div>

```js
currentlyVisibleState = <TicketList ticketList={this.state.mainTicketList} onTicketSelection={this.handleChangingSelectedTicket} />;
```

We'll extract the key piece of code that needs to be changed here for clarity's sake:

<div class="filename">src/components/TicketControl.js</div>

```js
<TicketList ticketList={this.state.mainTicketList}
```

Currently, our application is trying to pass down a prop called `ticketList` that is set to `this.state.mainTicketList`. Now we have to make a change that's similar to the one we just made — change the prop from `this.state.mainTicketList` to `this.props.mainTicketList`. Here's how the updated code should look:

<div class="filename">src/components/TicketControl.js</div>

```js
currentlyVisibleState = <TicketList ticketList={this.props.mainTicketList} onTicketSelection={this.handleChangingSelectedTicket} />;
```

As we see here, we just needed to change a single word. `this.state` is changed to `this.props`.

Even though it's a small change in the code, there's a huge difference in what it means. At this point, you should have a clear understanding of the difference between `this.state` (which refers to a class component's state) and `this.props` (which refers to the props being passed into a component from a parent component or the Redux store).

We are almost done — everything would work correctly if we hadn't made another change to our application. Previously, all of our tickets were stored in an array. Now they are stored in an object. We will need to make a few small changes to the `TicketList` component to get everything working as it should. There are three comments in the code below to point out the changes. (You should remove the comments from your own code.)

<div class="filename">src/components/TicketList.js</div>

```js
...

function TicketList(props){
  return (
    <React.Fragment>
      <hr />
      {/* We now need to map over the values of an object, not an array. */}
      {Object.values(props.ticketList).map((ticket) =>
        <Ticket
          whenTicketClicked = { props.onTicketSelection }
          names={ticket.names}
          location={ticket.location}
          issue={ticket.issue}
          id={ticket.id}
          key={ticket.id}/>
      )}
      {/* Don't forget to add the curly brace above — otherwise there will be a syntax error. */}
    </React.Fragment>
  );
}


TicketList.propTypes = {
  // The PropType below has been updated — it's now an object, not an array.
  ticketList: PropTypes.object,
  onTicketSelection: PropTypes.func
};

...
```

First, we iterate over the values of the `ticketList`. We do this with the `Object.values()` method, which grabs all the values from the object. Once we have the values, we can map over them.

At this point, everything should work correctly. We have successfully moved all of our shared state from our React application to a Redux store.

Note that our application still has plenty of local state. As we've already mentioned, we're letting React handle that side of things. Once we add a Redux store to an application, it should always handle the application's shared state. However, local state can still be handled by React components. That being said, it's also perfectly okay to add local state to a Redux store as well. During class, you'll have the opportunity to refactor the Help Queue so that the Redux store handles all state — local and shared.
