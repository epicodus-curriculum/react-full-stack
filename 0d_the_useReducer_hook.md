The first thing to know about the `useReducer()` hook is that it is an alternative to the `useState()` hook. That means `useReducer()` is a way to initialize and manage state in a function component. This also means that anything we can do with the `useReducer()` hook, we can also do with the `useState()` hook. So, when would we use `useReducer()`? Before getting into the use cases and benefits of the `useReducer()` hook, let's first get to know how to use it.

## The `useReducer()` Hook
---

As its name implies, the `useReducer()` hook makes use of a "reducer" function that handles evaluating and transforming state. A **reducer function** is just a plain JavaScript function that follows a specific convention in how it is set up:

* A reducer takes in two arguments: the current state and an **action** that describes how the state should change. An action contains a **type** property that contains the name of the action and it can optionally contain data to add to state. 
* A reducer uses [a switch statement](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/switch) to handle different action types. Each action type will lead to a different way of updating state. 
* A reducer then returns the new state. 

Also note that reducer functions are pure functions. A pure function is a function that meets the following criteria:

* Always returns an output
* Has no side effects
* Does not rely on external variables or state
* Always returns the same answer for a given input

Here's a reducer that we created for the Help Queue project during the React with Redux course section:

```js
const reducer = (state = false, action) => {
  switch (action.type) {
    case 'TOGGLE_FORM':
      return !state;
    default:
      return state;
  }
};
```

We could use this same reducer with the `useReducer()` instead!

Also, if we want to update our state with the `useReducer()` hook we would need to dispatch an action to our reducer, like so:

```js
dispatch({type: 'TOGGLE_FORM'})
```

As we can see, this process using reducers and actions is almost exactly the same as the process we follow when we use Redux. **However, the `useReducer()` hook is not from Redux and it does not create or access a global store like Redux does! It simply shares some of its conventions and the names for its tools.** 

With that brief introduction in mind, let's implement a `useReducer()` hook. For this next practice, we'll revisit the `intro-to-hooks` application that we built [when we first learned how to use the `useState()` and `useEffect()` hooks](/react/react-with-nosql-part-2/introduction-to-hooks-with-the-usestate-hook), and we'll refactor the `Counter` component we created to use a `useReducer()` hook.

### Setting Up our Practice Project

If you have an `intro-to-hooks` project saved to a remote GitHub repo, go ahead and clone down that project now. 

If you don't have an `intro-to-hooks` project, you can bootstrap a new practice project with create-react-app:

```
$ npx create-react-app intro-to-usereducer 
```

Then within the newly created `intro-to-usereducer` project folder, follow these steps:

* Create a new file called `Counter.js` in the `src` folder.
* Create and export an empty `Counter` function component inside of `Counter.js`.
* Import `Counter` into `App.js` and add it to the `return` statement. 

The `App` component should look like this:

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

And here's the logic for the `Counter` component:

```js
import React, { useState, useEffect } from 'react';

function Counter() {
  const [counter, setCounter] = useState(0);
  const [hidden, setHidden] = useState(false);

  useEffect(() => {
    document.title = counter;
  }, [counter]);

  return (
    <React.Fragment>
      {hidden ? <h1>Count Hidden</h1> : <h1>{counter}</h1>}
      <button onClick={() => setCounter(counter + 1)}>Count!</button>
      <button onClick={() => setHidden(!hidden)}>Hide/Show</button>
    </React.Fragment>
  );
}

export default Counter;
```

In the `Counter` component we have a button to show and hide the counter, as well as a button to increment the count by 1 on each click. We also have a `useEffect()` hook that updates the document's `title` attribute with the value of the counter, every time the counter changes in value. 

### Creating Initial State and a Reducer Function

In this refactor, we're going to turn the `counter` state variable into state managed by a `useReducer()` hook. We'll start this refactor by importing `useReducer` from React at the top of our file:

<div class="filename">src/Counter.js</div>

```js
import React, { useState, useEffect, useReducer } from 'react';
```

**The `useReducer()` hook takes two arguments**: 

* A reducer function.
* An object to define initial state. 

Let's create both of those next. We'll create these outside of the `Counter` function component:

<div class="filename">src/Counter.js</div>

```js
import React, { useState, useEffect, useReducer } from 'react';

const initialState = {
  counter: 0
}

function reducer(state, action) {
  switch (action.type) {
    case 'increment':
      return {
        counter: state.counter + 1
      };
    default:
      throw new Error(`There is no action matching ${action.type}.`);
  }
}

function Counter() {
  ...
}

export default Counter;
```

First notice that we've created our initial state and reducer outside of the `Counter` function component, but still within `Counter.js`. This organization is common practice, however, we can also initialize these variables within the `Counter` function component, or in an entirely separate file. There's no right answer as to what's the best organization practice, and it usually depends on what's best for testing and minimizing the complexity of components.

In the `initialState` variable, we've created an object with one key, `counter`, which starts with a value of `0`. This is the state that we'll use when we initialize our `useReducer()` hook.

In the `reducer()` function declaration, we've followed the convention of reducer functions by doing the following:

* Taking in an argument for state and an argument for an action.
* Setting up a switch statement based on the `action` object's `type` property. Our switch statement has:
  * An `'increment'` case that increments the `counter` state variable by `1`
  * A `default` case that throws an error if the `action.type` property does not match any of the available reducer action types.

Previously we would return the state in the `default` case, and this is acceptable. However, it's much better to use the default switch case for error handling. Why? When we throw errors, we fail loudly, and this ultimately makes it easier to debug the issue.

There's one difference to note between Redux reducer functions and those used by the `useReducer()` hook: **initial state is not initialized by [a default parameter](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Default_parameters)** in the reducer function. Instead, initial state is passed into the `useReducer()` hook as an argument. We'll see what this looks like in just a moment.

A default parameter value would look like `state = {counter: 0}` in the following code:

```js
function reducer(state = {counter: 0}, action) {
  ...
}
```

### Invoking the `useReducer()` Hook

Now we're ready to use the `useReducer()` hook. 

Here's how we'll update the `Counter` component:

<div class="filename">src/Counter.js</div>

```js
import React, { useState, useEffect, useReducer } from 'react';

const initialState = {
  counter: 0
}

function reducer(state, action) {
  switch (action.type) {
    case 'increment':
      return {
        counter: state.counter + 1
      };
    default:
      throw new Error(`There is no action matching ${action.type}.`);
  }
}

function Counter() {
  // Here we've replaced the useState hook originally used for counter state.
  const [state, dispatch] = useReducer(reducer, initialState);
  const [hidden, setHidden] = useState(false);

  useEffect(() => {
    // Now we need to access state.counter to get the counter value.
    document.title = state.counter;
  }, [state.counter]);

  return (
    <React.Fragment>
      {/* Same here: we need to access state.counter to get the counter value. */}
      {hidden ? <h1>Count Hidden</h1> : <h1>{state.counter}</h1>}
      {/* Now we use dispatch() to send an action to our reducer to update state. */}
      <button onClick={() => dispatch({type: 'increment'})}>Count!</button>
      <button onClick={() => setHidden(!hidden)}>Hide/Show</button>
    </React.Fragment>
  );
}

export default Counter;
```

There are a lot of updates here, so let's look at them one by one. First, let's look at the `useReducer()` hook itself:

```js
const [state, dispatch] = useReducer(reducer, initialState);
```

The `useReducer()` hook takes two arguments:

* A reducer function
* Initial state

Just like with the `useState()` hook, he `useReducer()` hook returns two variables that we destructure from an array:

* The state. We've called this variable `state`, though we could have called this `counterState` instead.
* A function to update state. We've called this variable `dispatch`, though we could have called this `dispatchCounter` instead.

The remaining updates that we make to the `Counter` component has to do with using the `state` and `dispatch` tools that are returned from the `useReducer()` hook. 

First, if we want to access the `counter` state, we now need to do so through by accessing the `state` object first:

```js
state.counter
```

Second, if we want to update the counter state, we need to create an action object with a `type` property that matches the name of a case in our reducer:

```js
dispatch({type: 'increment'})
```

And with that we've covered the basics of using the `useReducer()` hook! However, there's still plenty to cover as to best practices and use cases.

### Best Practices

Other than universal best practices like using descriptive variable names, the `useState()` and `useReducer()` hooks share a core best practice:

**1. Practice separation of concerns.** 

You should always use multiple `useReducer()` hooks to manage multiple and different state values. For example, you could make the argument that the `hidden` state variable should be added to our new `useReducer()` hook so that all of the counter related actions are in one place. But is that practicing good separation of concerns? By "good separation of concerns" we are asking the following: does hiding and showing a part of the UI have to do with managing the counter's value? I would say that it does not. 

If you are ever on the fence about separation of concerns, consider real refactors that you may want to make to your app and its state. For example, what if you no longer want the show/hide feature to be in the `Counter`, but instead  use it in `App.js` to handle showing and hiding both the `Timer` and `Counter` components? While making this change is trivial in a small application, it stands to reason that managing the `hidden` state separately from the `counter` state would make this refactor an easier process to complete.

However, let's say we wanted to refactor our app to include the functionality to decrement the `counter` and to reset the `counter`. In this case, we would expand our existing `useReducer()` to manage this new functionality as well, since it all directly relates to the `counter` state.

### When to Use `useReducer()`

[The React docs](https://reactjs.org/docs/hooks-reference.html#usereducer) state that you should generally use `useReducer()` in two cases:

1. When you have complex state that has multiple sub-values.
2. When your state update depends on the previous state value.

That said, we don't have to! We can manage complex state and access previous state using the `useState()` hook, as well.

It's recommended to use `useReducer()` to manage complex state because writing a reducer inherently involves organizing state updates into named actions, which makes it easier to read and reason about. Also, we can create complex objects and make updates to deeply nested properties in the reducer switch cases, which is not as easy to create as an argument to an update function from the `useState()` hook.

Similarly, it's recommended to use `useReducer()` to access the previous state value, because that's what the `state` variable represents in a reducer, and it can be easier to work with as a result:

```js
// The state variable is always equal to the previous state.
function reducer(state, action) {
  ...
}
```

Whereas with the `useState()` hook, we'd have to pass in a function to access the previous state, just like in the example below. Again, accessing the previous state isn't particularly harder to do with `useState()`, it's just a bit easier with `useReducer()`.

```js
const [counter, setCounter] = useState(0);
// How to access previous state in a state update:
setCounter(prevState => preState + 1);
```

There's other reasons you may end up using the `useReducer()` hook instead of `useState()`. For one, you might choose `useReducer()` because you feel more comfortable using it. That is completely acceptable. Similarly, your development team or company may prefer to use `useReducer()` and the conventions it dictates for code structure. As a baseline, you should be familiar with the `useReducer()` hook and be able to implement it in your code, whether or not you use it regularly.

### Benefits and Features of `useReducer()`

There are other benefits and features that can also influence your decision on whether to use the `useReducer()` hook: 

* Reducers are easier to test.
* You can incorporate programing patterns like action creators and action constants with `useReducer()` that make our code less buggy.
* You can better connect error handling to your state by setting up the `default` case in a reducer to throw or return an error.
* If you declare the reducer for the `useReducer()` hook within the component that uses it (not just in the same file, but within the component), the reducer function can read the component's props. Also, every time the component is re-rendered, the reducer function will be newly created and access the props again; this means that the reducer will always have access to updated props. The React docs doesn't go into this possibility at all, so if you are interested in learning more, you should do some research.
* Instead of passing down callback functions to child components so that they can trigger state updates, you can instead pass down the `dispatch()` function. This can be easier to manage, since you are only passing in one `dispatch()` function, instead of many different callback functions. This can also optimize performance by removing extra callback functions. Why? `dispatch()` is created once, while these callback functions are newly created every time the component re-renders. Less functions equals less memory usage which equals improved performance.

### Next Steps

To learn more about the `useReducer()` hook, visit [the React docs](https://reactjs.org/docs/hooks-reference.html#usereducer). In the docs, you'll can learn more about:

* [How to specify the initial state.](https://reactjs.org/docs/hooks-reference.html#specifying-the-initial-state) This reiterates what we learned in this lesson.
* [React bails out of a dispatch and does not re-render components when state does not change.](https://reactjs.org/docs/hooks-reference.html#bailing-out-of-a-dispatch) We haven't covered this topic explicitly, and this is true of the `useState()` hook as well.
* [How to create initial state lazily.](https://reactjs.org/docs/hooks-reference.html#lazy-initialization) This is a brand new topic.

Up next, we're going to refactor our New York Times API call app to use the `useReducer()` hook. If you want to practice more with the `useReducer()` hook before moving on, try adding the following functionality to the `Counter` component:

* A button that decrements the count by 1.
* A button that resets the count to 0.
