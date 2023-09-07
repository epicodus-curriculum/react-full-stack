We've successfully added two additional reducers to our application — one for handling form visibility and another to combine our reducers into a single root reducer. Now we need to refactor our help application to utilize our new root reducer.

We'll need to make the following changes:

1. Pass the root reducer into our store in `src/index.js` (our application entry point — not the file where our root reducer is stored)!
2. Update our `mapStateToProps` function to handle our new slice of state.
3. Remove `formVisibleOnPage` from `TicketControl`'s state — our Redux store will handle it now.
4. Refactor our application to correctly dispatch the `'TOGGLE_FORM'` action where needed.
5. Ensure that our `TicketControl` component receives information about `formVisibleOnPage` from `this.props` (because our props have been mapped from the store) instead of `this.state` (the component's state).

Because we are refactoring the simplest piece of shared state in our application and moving it into Redux, these changes will be fairly minimal.

### 1. Update the Root Reducer

First, we need to import our new root reducer into our application's entry point file — and then pass the root reducer into our store:

<div class="filename">src/index.js</div>

```js
// import reducer from './reducers/ticket-list-reducer';
import rootReducer from './reducers/index';

const store = createStore(rootReducer);
```

We import `rootReducer` and pass it into the store. Note that our previous reducer is now commented-out. We only need to import the root reducer and any other reducer imports can now be removed from this file.

### 2. Update `mapStateToProps`

Next, we need to update the state we are mapping to props in our `TicketControl` component. Currently, the function returns the following object:

```js
{
  mainTicketList: state
}
```

However, we now want it to return slices of state to be mapped to props. The updated `mapStateToProps` function should look like this:

<div class="filename">components/TicketControl.js</div>

```js
...
const mapStateToProps = state => {
  return {
    mainTicketList: state.mainTicketList,
    formVisibleOnPage: state.formVisibleOnPage
  }
}
...
```

Now each prop corresponds to a property of `state`. We can map as many state slices to props as we need. In this case, we are only using two — but complex applications could easily have more.

We also need to add prop types as well:

<div class="filename">components/TicketControl.js</div>

```js
TicketControl.propTypes = {
  mainTicketList: PropTypes.object,
  formVisibleOnPage: PropTypes.bool
};
```

### 3. Remove `formVisibleOnPage` State From `TicketControl.js`

Next, we will remove the `formVisibleOnPage` property from our `TicketControl` component's state. While we are at it, we will remove any lines where we use React's `setState()` method to update `formVisibleOnPage`. Because Redux will be handling this slice of state, we won't be using React or `setState()` to take care of form visibility any longer.

Several lines of code need to be removed from `TicketControl.js`. They are commented out in the code below. You may comment out these lines in your code or remove them completely.

<div class="filename">components/TicketControl.js</div>

```js
...
constructor(props) {
  super(props);
  console.log(props);
  this.state = {
    // formVisibleOnPage: false,
    selectedTicket: null,
    editing: false
  };
}

handleClick = () => {
    if (this.state.selectedTicket != null) {
      this.setState({
        // formVisibleOnPage: false,
        selectedTicket: null,
        editing: false
      });
    } else {
      // this.setState(prevState => ({
      //   formVisibleOnPage: !prevState.formVisibleOnPage,
      // }));
    }
  }

...

handleAddingNewTicketToList = (newTicket) => {
  const { dispatch } = this.props;
  const { id, names, location, issue } = newTicket;
  const action = {
    type: 'ADD_TICKET',
    id: id,
    names: names,
    location: location,
    issue: issue,
  }
  dispatch(action);
  // this.setState({formVisibleOnPage: false});
}

...
```

It should be clear where we will need to dispatch Redux actions — the exact same place where we previously used `setState()` to change our form's visibility. When refactoring an application to use Redux instead of React for state, this can be a very helpful way to see where the refactor needs to happen. We don't necessarily need to create new methods in our components. We just need to rewire the relevant methods to use Redux instead of React for state.

### 4. Handle Form Visibility State with Redux

We're ready to update `TicketControl.js` to use Redux instead of React for form visibility state. As shown in the previous section, we'll need to dispatch actions to our store in two different methods. You may want to try wiring this up yourself before continuing with this lesson — it's good practice and you learned how to dispatch actions over the weekend homework.

Here are the revised methods. (Note that the commented-out code from the previous section has been removed now.)

<div class="filename">components/TicketControl.js</div>

```js
...

handleClick = () => {
  if (this.state.selectedTicket != null) {
    this.setState({
      selectedTicket: null,
      editing: false
    });
  } else {
    const { dispatch } = this.props;
    const action = {
      type: 'TOGGLE_FORM'
    }
    dispatch(action);
  }
}

...

handleAddingNewTicketToList = (newTicket) => {
  const { dispatch } = this.props;
  const { id, names, location, issue } = newTicket;
  const action = {
    type: 'ADD_TICKET',
    id: id,
    names: names,
    location: location,
    issue: issue,
  }
  dispatch(action);
  const action2 = {
    type: 'TOGGLE_FORM'
  }
  dispatch(action2);
}
```

We deconstruct the dispatch function from `this.props` if needed, define the action in a constant, and then dispatch it.

Our `handleClick()` method just got a bit more clunky — but our goal here is to demonstrate combining reducers. If we were to refactor this now, it would probably be best to break it down into two methods.

Note that the second method above now dispatches two actions. You may be wondering if the code above is smelly or not. It certainly has the potential to be buggy. This is because the `dispatch()` function is asynchronous. If one of our actions depended on the other, we could run into race conditions where the first action isn't complete before the second one starts. In the example above, the two actions aren't dependent on each other. In future lessons, we will learn how to use middleware to deal with this issue.

### 5. Change `this.state` to `this.props` for Form Visibility Property

This is a small change but a necessary one for our application to work correctly. Currently, our conditional rendering is still using `this.state.formVisibleOnPage`. We need to update that line of code to `this.props.formVisibleOnPage` because information about the property is now coming from `mapStateToProps`. There is only one place in our code that uses this property:

<div class="filename">components/TicketControl.js</div>

```js
...
//else if (this.state.formVisibleOnPage) {
else if (this.props.formVisibleOnPage) {
...
```

Change the code from the commented out line (which you can delete) to the active line of code that uses `this.props` instead of `this.state`.

### Conclusion

At this point, we've successfully refactored our application to use a root reducer. We now know how to use our root reducer and `mapStateToProps` to manage multiple reducers and multiple slices of state. We refactored our React Help Queue to use Redux for `formVisibleOnPage` information instead of our component's local state. In the upcoming classwork, you will have the opportunity to refactor the Help Queue to use Redux instead of local state.
