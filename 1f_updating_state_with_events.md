In this lesson, we'll handle our first event in a React application. We've handled many events before — every time we use functions to respond to a click or a submit button, we are handling an event. There are some fundamental differences to handling events in React, but for the most part, the process is very similar:

* First, we add a click handler to an element (such as a button).
* Next, that click handler will trigger some code, often a function. We need to write that function as well.

## Adding a Click Handler to JSX
---

Here's how our click handler will look:

```jsx
<button onClick={this.handleClick}>Add ticket</button>
```

Here, we take a `button` element and add an `onClick` handler to it. We need to specify the function `onClick` will trigger. As always, we need to use curly braces to make sure that JSX properly evaluates any JS code to the right of our `onClick` handler.

In the example above, our `onClick` handler will trigger `this.handleClick`. As you can probably guess `handleClick()` is the function that will be called when the handler is triggered. But what is `this`? In this case, we are going to be rendering an object that's an instance of the `TicketControl` component. `this` refers to the specific instance that is being rendered.

Note that we don't use `this` with function components — just class components. But we won't worry about that right now. We will get a chance to add functions to function components soon enough.

Note that there are a few syntactical differences between how we do this in React as opposed to how we'd accomplish the same thing with JavaScript. Instead of `onclick`, we use `onClick` (as always, case is important). In plain old JavaScript, we'd wrap the function being called in a string. For instance, we might do this: `<button onclick="doSomething()">`. In JSX, we use curly braces. Other than these syntactical differences, attaching click handlers in React is very similar to how we might attach a click handler in a vanilla JavaScript application.

Now let's actually add our event handler to our component. Our event handler will go in the `render()` method of `TicketControl.js`:

<div class="filename">src/components/TicketControl.js</div>

```js
...

  render(){
    let currentlyVisibleState = null;
    let addTicketButton = null; // new code
    if (this.state.formVisibleOnPage) {
      currentlyVisibleState = <NewTicketForm />
    } else {
      currentlyVisibleState = <TicketList />
      addTicketButton = <button onClick={this.handleClick}>Add ticket</button> // new code
    }
    return (
      <React.Fragment>
        {currentlyVisibleState}
        {addTicketButton} { /* new code */}
      </ React.Fragment>
    );
  }

...
```

Before we continue, note that there are two different kinds of comments above — this is expected. Comments in JSX syntax need to be wrapped in curly braces, unlike the other comments, which are standard JS comments.

We've added three lines of code:

* First, we create a new variable called `addTicketButton` and set its value to `null`.
* Next, if `this.state.formVisibleOnPage` is set to `false`, we will set the value of our `addTicketButton` variable to our button with its click handler.
* Finally, we make sure that `{addTicketButton}` will be returned from our function. If its value is still `null`, there's nothing to add to the DOM. However, if it has a value, the button will be added to the DOM.

You may wonder why we have this button here instead of in the `TicketList` component. Well, this button has nothing to do with our `TicketList` component so it shouldn't be there. However, it _does_ have a lot to do with `TicketControl` — it's a large part of determining which component should be showing!

There's one other reason the button isn't in another component. If it were in a child component, we'd have to pass our `this.handleClick` function to the child component using something called **unidirectional data flow**. That makes things a bit more complicated and we aren't quite there yet.

## Calling a Function in a State Component
---

Next, we need to write the function that we've associated with our event handler. That way, when an event actually triggers the handler, the function is called. Any functions we write will go after the constructor and before the `render()` method. Let's write a `handleClick()` method now. Note that we are using the term "method" here. Remember that a method is a function that's called on an object. `handleClick()` is a method that has to be called on an instance of the `TicketControl` class — it can't be called elsewhere.

<div class="filename">src/components/TicketControl.js</div>

```js
...

class TicketControl extends React.Component {

  constructor(props) {
    super(props);
    this.state = {
      formVisibleOnPage: false
    };
  }

  handleClick = () => {
    this.setState({formVisibleOnPage: true});
  }

...
```

Our new `handleClick()` method uses arrow notation. This is very important. We will go over why in the next lesson when we discuss binding functions.

Let's take a look at the code in our new method. There's only a single line of code:

```jsx
this.setState({formVisibleOnPage: true});
```

As we discussed in [Introduction to State](/react/react-fundamentals/introduction-to-state), we should only ever modify state in a pure React application with the `setState()` method. In its simplest form, `setState()` takes an object as an argument. The object contains any key-value pairs that our application should update.

Now if we run our application, we can successfully click on "Add Ticket" and our form will show.

## Toggling a Boolean When Updating State
---

When we add a working form to our application, our submit button will return users to the list of tickets. However, what if a user changes their mind and wants to return to the queue, anyway? Let's add a button that users can click to return to the queue from the form page without submitting a ticket. That way, we can learn about efficiently toggling a boolean in React state.

First, we'll update our `handleClick()` method:

<div class="filename">src/components/TicketControl.js</div>

```js
...
  handleClick = () => {
    this.setState(prevState => ({
      formVisibleOnPage: !prevState.formVisibleOnPage
    }));
  }
...
```

As we can see, `setState()` just got more complex. This method can just take an object — and more often than not, just passing in an object will suit our needs.

However, if we want to refer to the previous state (such as with toggling a boolean or incrementing a counter), we need to know a little more about `setState()`.

`setState()` can optionally take two arguments (we will only discuss the first here). This is the actual first argument that `setState()` can take:

```js
(state, props) => stateChange
```

We can choose to just pass in an object (the `stateChange`), but we can also pass in an arrow function that takes the current `state` and `props` as arguments. As we just mentioned, there are plenty of use cases where we need to know about the current state. Here are some examples:

* **We want to toggle a boolean.** That means we need to know the current state of the boolean so we can toggle it to its opposite state.
* **We want to increment or decrement a value.** A prime example is a counter that we need to increment by one or some other value each time a button is clicked.
* **We want to update the state of a game.** Let's say we are making a game where the location of pieces is constantly changing. We need to know the previous state to determine where pieces can move next.

Now let's return to the `setState()` method in `handleClick()`:

```js
this.setState(prevState => ({
  formVisibleOnPage: !prevState.formVisibleOnPage
}));
```

We pass in the current state of the `formVisibleOnPage` boolean to `prevState`. Now that we know this value, we can say the new state should be `!prevState.formVisibleOnPage` (the opposite of the old state).

We recommend experimenting with adding counters, booleans, and other states that need updating to your applications to get more practice with this slightly more complex implementation of `setState()`.

### Refactoring Our Button to Toggle Between Components

Now that we've updated our `handleClick()` method, we just need to update our `render()` method so that we have a button to toggle back to the Help Queue with our form. Let's do this with the minimum amount of code possible. We'll need to refactor just a little.

<div class="filename">src/components/TicketControl.js</div>

```jsx
  render(){
    let currentlyVisibleState = null;
    let buttonText = null; // new code
    if (this.state.formVisibleOnPage) {
      currentlyVisibleState = <NewTicketForm />;
      buttonText = "Return to Ticket List"; // new code
    } else {
      currentlyVisibleState = <TicketList />;
      buttonText = "Add Ticket"; // new code
    }
    return (
      <React.Fragment>
        {currentlyVisibleState}
        <button onClick={this.handleClick}>{buttonText}</button> { /* new code */ }
      </React.Fragment>
    );
  }
```

Now that we know we need our button in both components, we can move it out of the conditional and into our `return()` statement. We only need to change the text on the button so we create a new variable called `buttonText`. It's the same button regardless of which component the user is looking at — but the text will make it seem like it's a different button.

At this point, we've successfully added local state and we can use a button to toggle back and forth between two components. In the process, we've also learned more about how `setState()` works. Make sure to take the time to practice working with local state and getting to learn the ins and outs of `setState()`.
