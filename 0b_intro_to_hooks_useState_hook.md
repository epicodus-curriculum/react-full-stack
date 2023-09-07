In this course section, we'll be leaving behind class components to focus on a purely functional approach to developing React components. To do this, we'll need to use special functions that are called **hooks**. [As the React docs](https://reactjs.org/docs/hooks-overview.html) explain,

> Hooks are functions that let you “hook into” React state and lifecycle features from function components. Hooks don’t work inside classes — they let you use React without classes.

Hooks were released in version 16.8 of React as a solution to many pain points for React developers. We can boil down these pain points into two main issues: 

1. How can we use React state and lifecycle features in a function component without having to refactor it into a class component? 
2. Is there an easier way to *re*use stateful logic in multiple places? 

The advent of hooks solved both of these issues. 

But before we get too much into the weeds of React's motivation to create hooks, let's introduce ourselves to the basics of hooks by learning how to use React's `useState` hook. 

In this course section, we'll also learn about the `useEffect` hook. Then, in the next course section, we'll learn how to use the `useReducer` and `useContext` hooks. 

## The `useState` Hook
---

We'll learn to use React's built-in `useState` hook by looking at an example of a simple counter app. This example includes a button that increases the value of a counter, a button to show and hide, and a display of the counter's value. You do not need to code along with this lesson, though you are welcome to do so.

We'll start by creating a new app so we can implement `useState`. Navigate to your desktop in your terminal, and input this command:

```js
$ npx create-react-app intro-to-hooks
```

Next, replace the code in `src/App.js` with the following code:

<div class="filename">src/App.js</div>

```js
import './App.css';
import Counter from './Counter';

function App() {
  return (
    <div className="App">
      <Counter />
    </div>
  );
}

export default App;
```

We'll keep the styling with the class `App` that centers the content on the page. 

Next, create a file called `Counter.js` in the `src` folder with the following code:

<div class="filename">src/Counter.js</div>

```javascript
import React, { useState } from 'react';

function Counter() {
  return(
    <React.Fragment>
    </React.Fragment>
  )
}

export default Counter;
```

Here we've set up a basic function component and we've imported the `{ useState }` hook from react. Now we're ready to implement the `useState` function.

<div class="filename">src/Counter.js</div>

```javascript
...

function Counter() {
  const [counter, setCounter] = useState(0);

  return(
    <React.Fragment>
    </React.Fragment>
  )
}

...
```

The `useState` hook returns an array that we destructure into two variables. The first variable contains the state value, and the second variable is a function that we can use to set the state value. We could also rewrite `const [counter, setCounter] = useState(0);` like so:

```js
  const counterState = useState(0);
  const counter = counterState[0];
  const setCounter = counterState[1];
```

However, that's not common practice.

As far as naming conventions, the first variable should be named after the state the variable represents. Since we're setting up a counter, we name our state `counter`. The second variable should always start with `set` followed by the first variable, like we have with `setCounter`.

The `useState` hook also takes an argument, which will set the state property's initial value. We can initialize this with a number, a boolean, a string, an object, or even `null`. For our counter, we initialize `useState` with the number 0.

Now we're ready to actually utilize this new functionality. We'll create a button to update the value of the counter — and we'll also display the value of the counter as well. 

<div class="filename">src/Counter.js</div>

```javascript
...

function Counter() {
  const [counter, setCounter] = useState(0);

  return (
    <React.Fragment>
      <h1>{counter}</h1>
      <button onClick={() => setCounter(counter + 1)}>Count!</button>
    </React.Fragment>
  );
}

...
```

We can simply call `counter` using JSX, which will display that property's current value. We also create an `onClick` listener so that a user can click a button to trigger the `setCounter` method. We need this to be a callback function so we can pass in an argument, otherwise it'll run on page load. This will replace the current value of `counter`, overwriting its previous value.

With just a couple of lines, we have local state in a function component! Very cool!

### `useState` As Compared To `this.state`

Now, let's compare what our functional `Counter` component would look like as a class component. We won't add this to our `intro-to-hooks` application:

```js
class Counter extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      counter: 0
    };
  }

  render() {
    return (
      <React.Fragment>
        <h1>{this.state.counter}</h1>
        <button onClick={() => this.setState({counter: this.state.counter + 1})}>Count!</button>
      </React.Fragment>
    );
  }
}

export default Counter;
```

As we can see, instead of storing `counter` as a slice of state in our state object, like this:

```js
  constructor(props) {
    super(props);
    this.state = {
      count: 0
    };
  }
```

We instead store state in a variable, as returned by our `useState` function: 

```js
const [counter, setCounter] = useState(0);
```

And instead of updating state with the `this.setState` method, like this:

```js
<button onClick={() => this.setState({counter: this.state.counter + 1})}>Count!</button>
```

We instead use `setCounter`, like this:

```js
<button onClick={() => setCounter(counter + 1)}>Count!</button>
```

### Multiple State Variables

Let's say we want to add another property to our `Counter` component's local state. We could approach that by doing the following:

<div class="filename">src/Counter.js</div>

```javascript

...
function Counter() {

  const [bundle, setBundle] = useState({"hidden": false, "counter": 0});

  return (
    <React.Fragment>
      {bundle.hidden ? <h1>Count Hidden</h1> : <h1>{bundle.counter}</h1>} 

      <button onClick={() => setBundle({...bundle, "counter": bundle.counter +1})}>Count!</button>
      <button onClick={() => setBundle({...bundle, "hidden": !bundle.hidden})}>Hide/Show</button>
    </React.Fragment>
  );
}

...
```

React's `useState` hook accepts any data type as an argument, including objects. So just like in the above example, we could create as many properties as we like and call on them using dot notation. 

But while the above approach works, it isn't recommended. The React documentation instead recommends creating multiple instances of `useState` and calling on them as separate variables. Take a look at this approach instead:  

<div class="filename">src/Counter.js</div>

```javascript

...
function Counter() {

  const [counter, setCounter] = useState(0);
  const [hidden, setHidden] = useState(false);

  return (
    <React.Fragment>
      {hidden ? <h1>Count Hidden</h1> : <h1>{counter}</h1>}
      <button onClick={() => setCounter(counter + 1)}>Count!</button>
      <button onClick={() => setHidden(!hidden)}>Hide/Show</button>
    </React.Fragment>
  );
}

...
```

It's not only easier to read the state variable declarations, it's easier to use the state in our JSX, because each state variable has a separate name and updater function. This structure puts into practice the design principle called separation of concerns. 

That said, it may just make more sense in your application to combine two state slices into one object that is created and managed via `useState`, just like we saw in this example:

```js
const [bundle, setBundle] = useState({"hidden": false, "counter": 0});
```

If you opt for this, just know that the _setState_ function (i.e.: `setBundle`) replaces the previous state variable, instead of merging the new state with the old state as with the `this.setState` method. That's why when we want to update the count within our `bundle` state, we need to include all values in the `setBundle` function:

```js
<button onClick={() => setBundle({...bundle, "counter": bundle.counter +1})}>Count!</button>
```

As we can see, we use [spread syntax](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax#description) to copy over the object saved in `bundle`, and then specify that we want to update the `"counter"` key.

In contrast, if the _Count!_ button only ran this code:

```js
<button onClick={() => setBundle({"counter": bundle.counter +1})}>Count!</button>
```

Then we'd have no more `"hidden"` key and our application would break.

So, with this lesson we've learned how to use the very useful `useState` hook, which means we can now use state in our function components. But what about our component lifecycle methods? What if I want to run a side effect when my component mounts or updates? In the next lesson, we'll learn about the `useEffect` hook which gives us the same ability to run side effects in function components as lifecycle methods let us do in class components. 

After that, we'll review best practices for using hooks. Then, we'll move onto updating our Help Queue application to use hooks.

If you'd like to learn more about the `useState` hook, check the official [React Docs on the `useState` hook](https://reactjs.org/docs/hooks-state.html).
