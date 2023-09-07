We're ready to refactor our Help Queue application to incorporate Redux. We'll start by creating the Redux store.

### 1. Create the Redux Store

First, we need to import `createStore` and our `reducer` into our `index.js` entry point file. This is exactly what we did in our small Redux learning project. The difference is that our new import statements will go in the `index.js` file. Back in Intermediate JavaScript, we learned that webpack needs to have access to an entry point file. The entry point file is where webpack accesses the top level component in our application. Since we want the store to be available everywhere we need it, it needs to be in `index.js`.

Add these import statements to `index.js`:

<div class="filename">index.js</div>

```javascript
...
import { createStore } from 'redux';
import reducer from './reducers/ticket-list-reducer';
...
```

Next, we need to instantiate the store. Add this line directly after the import statements:

<div class="filename">index.js</div>

```javascript
const store = createStore(reducer);
```

As we can see here, we are passing our reducer into the `createStore` function. That means the `store` constant is a Redux store that knows how to handle the actions we've defined in our reducer.

### 2. Configure the React-Redux Provider

Next, we need to wrap our `<App />` component with the `<Provider>` component. As previously discussed, the `<Provider>` component will give all of its child components access to the `connect()` function, which is needed to connect to the Redux store.

Let's start by adding another import statement:

<div class="filename">index.js</div>

```javascript
...
import { Provider } from 'react-redux';
...
```

Now we need to wrap the `<Provider>` component around `<App />` and pass the store into `<Provider>` as a prop. We will update our `root.render()` line of code to do this:

<div class="filename">index.js</div>

```js
...

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  <React.StrictMode>
    <Provider store={store}>
      <App />
    </Provider>
  </React.StrictMode>
);
```

Our `<App />` component is now a child of the `<Provider>` component. We've also passed the Redux store in as a prop to `<Provider>` as well. 

We won't need to explicitly pass `store` as a prop through the other components in our tree — it's already being inherited by `<App />` and all of its children by way of the `<Provider>` component.

### 3. Connect Components to Store

It's time to determine which components in our application should have access to the store. These components will need the `connect()` function.

This can be a complex decision in larger applications — and there's no single correct way to incorporate Redux in an application. Our goal is to keep things simple so we will add the `connect()` function to the one component in our application that already has state: `TicketControl.js`. That way, we will (mostly) only need to update one component to integrate Redux in our application.

We'll import the `connect` function from React Redux at the top of `TicketControl.js`. Then, right before our export statement at the end of the file, we'll wrap our component with the `connect()` function:

<div class="filename">src/components/TicketControl.js</div>

```js
import { connect } from 'react-redux';

...

TicketControl = connect()(TicketControl);

export default TicketControl;
```

The `connect()` function redefines our entire `TicketControl` component as a _new_ `TicketControl` component with additional functionality included. The return value of the `connect()` function is the `TicketControl` component itself, but this time we will have powerful new tools at our disposal: the `dispatch()` and `mapStateToProps()` functions.

Note that it's important that `connect()` is called right before we export `TicketControl`. That ensures that the component that's exported has all necessary React Redux functionality.

`connect()` is what is known as a **higher-order component**. This is a common term in React. A higher-order component is a function that takes an existing component, wraps it with additional functionality, and then returns it so it can be used elsewhere in an application. To learn more about HOCs, check out the [React documentation](https://facebook.github.io/react/docs/higher-order-components.html).

Even if you find the concept of higher-order components confusing, you can still implement the necessary boilerplate to get Redux working. React Redux takes care of this for you so you don't have to think about it.

### 4. Dispatch Actions to Mutate Store State

We are now ready to use `dispatch()` in our `TicketControl` component. We currently have three methods in our component that we will need to modify to use `dispatch()`

* `handleAddingNewTicketToList` adds tickets to our list.

* `handleEditingTicketInList` updates tickets in our list.

* `handleDeletingTicket` deletes tickets from our ticket list.

We will also remove `mainTicketList` from the component's state object because React will no longer handle the ticket list — we'll let Redux do that instead.

Let's start by removing `mainTicketList` and then work our way through updates to our three methods. Don't worry — the updates to our methods will be relatively similar to each other.

First, let's update the state object in `TicketControl`'s constructor:

<div class="filename">src/components/TicketControl.js</div>

```js
...
constructor(props) {
    super(props);
    console.log(props);
    this.state = {
      formVisibleOnPage: false,
      selectedTicket: null,
      editing: false
    };
  }
...
```

Note that `this.state` no longer has a `mainTicketList` property. That's the only change we've made here. We'll still let the `TicketControl` component handle all local state.

Next, let's update our `handleAddingNewTicketToList` method. We will need to make several changes.

<div class="filename">src/components/TicketControl.js</div>

```js
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
    this.setState({formVisibleOnPage: false});
  }
...
```

We could call `this.props.dispatch` in our method but it's common to deconstruct `dispatch` from `this.props`. This makes our code a little cleaner.

Also, we need to deconstruct the values from `newTicket` so we can actually pass them into our action, which requires four different properties.

Next, we store our action in a constant. This action should look familiar — it's the `'ADD_TICKET'` action we created and tested.

Next comes the actual Redux magic: we call `dispatch(action);`. This automatically dispatches our action and updates the store — much as we did with our little Redux learning application in the browser. The difference is that React Redux provides a seamless connection between React and Redux, making our coding lives much easier.

Note that the method we just rewrote still handles local state. We could move the code that updates the `formVisibleOnPage` property to our store but we won't. Ultimately, it's up to us to decide whether or not local state belongs in the Redux store or if React should handle it. Neither approach is considered a bad practice — though in a larger application, too much local state cluttering up the Redux store could become a code smell.

Our application isn't set up to show data from the store yet but we can still confirm that our updated method is functioning correctly by adding the following code in our entry point `index.js` file just after the line where we instantiate our store:

<div class="filename">src/index.js</div>

```js
...
const store = createStore(reducer);

store.subscribe(() =>
  console.log(store.getState())
);
...
```

Remember the `subscribe()` method that Redux provides? Generally, we won't use Redux's `subscribe()` or `getState()` in our "production" code, but it's excellent for testing. This is a great way to keep an eye on the current state of the store.

If we run our application and open the console, we'll see a message each time a new ticket is added to the store. However, we still have more work to do before we can display the contents of the store in our application.

Next, let's update our remaining methods. See if you can update them on your own before taking a look at the code — the process is pretty much exactly the same. We just need to define our action and dispatch it:

<div class="filename">src/components/TicketControl.js</div>

```js
...
handleEditingTicketInList = (ticketToEdit) => {
    const { dispatch } = this.props;
    const { id, names, location, issue } = ticketToEdit;
    const action = {
      type: 'ADD_TICKET',
      id: id,
      names: names,
      location: location,
      issue: issue,
    }
    dispatch(action);
    this.setState({
      editing: false,
      selectedTicket: null
    });
  }

...

  handleDeletingTicket = (id) => {
    const { dispatch } = this.props;
    const action = {
      type: 'DELETE_TICKET',
      id: id
    }
    dispatch(action);
    this.setState({selectedTicket: null});
  }
...
```
* Note that we use the `ADD_TICKET` action to edit our ticket as well. How can our `ADD_TICKET` action do this? Well, the only difference between when we are adding and editing a ticket is the `id` property. If it's a new `id`, a new ticket will be added to the store. If it's an `id` that already exists, the existing ticket will be replaced. At this point, it might be more accurate to name the action `ADD_OR_UPDATE_TICKET`, but we will keep the action name the same for consistency.

* In both cases, we deconstruct `this.props` to get the `dispatch` function.

* If necessary, we deconstruct any objects that need to be used in the action. (We do this for editing a ticket but not for deleting a ticket since deleting a ticket only needs an `id` in addition to the action's `type`.)

* Next, we define the action itself. Defining all these actions may not seem very DRY, but we'll learn a nifty technique to clean our actions up in a future lesson.

* Once the action is defined, we can `dispatch()` it.

Note that you won't be able to check out the update and delete functionality in the browser just yet — that's because the ticket list from our store won't display and we need to click on a ticket detail to either update or delete it.

We're ready to update our component so it can actually display the ticket list! We'll do that in the next lesson.
