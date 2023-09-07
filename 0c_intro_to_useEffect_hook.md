As we learned in the last lesson, hooks allow us to use React state and lifecycle features in function components. Historically, these features were only available in class components. Now that we know how to use state in our function components with the `useState` hook, let's learn how to perform side effects with the `useEffect` hook. 

Keep in mind that a **side effect** is not a React-specific term; instead, it's a way of describing functions in general. A function has side effects when it changes something outside of its own scope. Often this looks like making a network request to an API, but this also includes changing the value of a variable that exists outside of the scope of the function. Another good example of a side effect is updating a value in the DOM. 

As we know, when a function does not have side effects, we call it a **pure function**, where for any given input, you can always expect the same output. These functions are predictable, easy to test, simple to reason about, and easy to maintain and refactor.

With that review, let's dive into what the `useEffect` hook can do for us! For the examples in this lesson, we'll add to our `intro-to-hooks` application, by expanding the functionality of our `Counter` component and creating a brand new `Timer` component. Later on when we connect our Help Queue application to a NoSQL database via Google's Firebase, we'll use the `useEffect` hook to fetch data for us.

## `useEffect`
---

We should use the `useEffect` hook when we want to run code in any of the following cases:

* After our component is first rendered. This corresponds to the `componentDidMount` lifecycle method.
* When a state variable in our component changes. This corresponds to looking at the `prevState` variable available in the `componentDidUpdate` lifecycle method to determine if state has changed, and to only make an update if it has.
* After every re-render of our component. This corresponds to the `componentDidUpdate` lifecycle method.

The last case is the default behavior for the `useEffect` hook. Let's take a look. 

Open the `Counter` component, and add this code:

<div class="filename">src/Counter.js</div>

```js
import React, { useState, useEffect } from 'react';

function Counter() {
  const [counter, setCounter] = useState(0);
  const [hidden, setHidden] = useState(false);

  useEffect(() => {
    console.log("effect!");
  });

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

First, we import the `useEffect` hook at the top of the file, and then we call it within our `Counter` component. There's a couple things to note about the `useEffect` hook:

* `useEffect` is first run after the first render of the component.
* Without further configuration, `useEffect` runs after every re-render of the component.
* `useEffect` takes a callback function as an argument, which acts as our "effect". This function is called every time the `useEffect` hook runs.
* `useEffect` doesn't return anything.

If we run our app and click on our _Count!_ and _Hide/Show_ buttons, we'll see our `"effect!"` message logged each time. That's because every time we use `setCounter()` or `setHidden()` to update a state variable, our component re-renders to display the new state value, and every time our component re-renders, the `useEffect` hook calls the function we pass into it.

That's the basics of the `useEffect` hook. As noted earlier, the default configuration of the `useEffect` hook matches the functionality of two class component lifecycle methods: `componentDidMount`, since `useEffect` runs after the first render, and `componentDidUpdate`, since `useEffect` runs after every re-render of our component. 

Let's try adding something other than a `console.log()`. Update your `useEffect` hook to perform the following side effect:

```js
  useEffect(() => {
    console.log("effect!");
    document.title = counter;
  });
```

With `document.title = counter`, we're updating the value of the `<title>` tags of our HTML to the current value of our counter. Now if we click on our _Count!_ and _Hide/Show_ buttons, we'll see our `"effect!"` message logged each time and the title will match the current value of our `counter`. 

It's important to note that the function we pass as an argument into `useEffect` (a callback function) is newly created each time `useEffect` is run. This is the argument we are passing into `useEffect`:

```js
() => {
  console.log("effect!");
  document.title = counter;
}
```

This allows the `useEffect` hook to access the most up-to-date value of our `counter` state variable.

### Skipping Effects

As is, we can optimize our code in the `useEffect` hook. How? Well, we really only need to update the title of our HTML document to the new counter value when the value of our counter changes. Right now, it will get updated every re-render, which is caused by any change in state, including the showing and hiding of our counter. 

React developers have a solution for this, and this is what it looks like:

```js
  useEffect(() => {
    console.log("effect!");
    document.title = counter;
  }, [counter]);
```

Notice that we've added a second argument to our `useEffect` hook: `[counter]`. This second argument is called **a dependency array**, and it can contain one or more state variables or props within it. When we add a dependency array to our `useEffect` hook, we're saying that whether our effect should run depends on whether the value of the state variables in our dependency array have changed. 

When we add `counter` as our dependency, we're specifically directing `useEffect` to run  the effect only if the value of `counter` changes. If `counter` does not change, the `useEffect` hook will skip the effect.

We can test this out. Now if we run our `intro-to-hooks` application and click on our _Count!_ and _Hide/Show_ buttons, we'll only see our `"effect!"` message logged when we click on the _Count!_ button. 

As noted above, adding a dependency array to the `useEffect` hook performs the same functionality as comparing `prevState` with current state in a `componentDidUpdate` lifecycle method:

```js
componentDidUpdate(prevProps, prevState) {
  if (prevState.counter !== this.state.counter) {
    document.title = counter;
  }
}
```

### Only Running the Effect Once

You can tell the `useEffect` hook to run its effect once by passing in an empty dependency array:

```js
  useEffect(() => {
    console.log("effect!");
    document.title = counter;
  }, []);
```

In this case, we're saying that our effect does not depend on the change of any state variables or props in our component, and it should only run once, after the first render.

We won't use this in our `intro-to-hooks` application now, but you can try it out if you like. Later on we'll use an empty dependency array to set up a subscription to our NoSQL database (provided by Firebase) once, after the first render of our component.

### Performing Clean-Up Tasks

Let's look at one last example with the `useEffect` hook to understand how we can perform clean up tasks. In this example, we'll create a timer that counts up from 0 every second. We'll also be able to pause the timer as well!

First, let's update our `App` component in the `intro-to-hooks` application to import and render a new `Timer` component. Here's the code:

<div class="filename">src/App.js</div>

```js
import './App.css';
import Counter from './Counter';
import Timer from './Timer';

function App() {
  return (
    <div className="App">
      <Counter />
      <Timer />
    </div>
  );
}

export default App;
```

Next, let's create a new file called `Timer.js`, also in the `src` folder. Here's the code that we'll add inside of `Timer.js` to create the new `Timer` component:

<div class="filename">src/Timer.js</div>

```js
import React, { useState, useEffect } from 'react';

function Timer() {
  const [isActive, setIsActive] = useState(false);
  const [timer, setTimer] = useState(0);

  useEffect(() => {
    let interval;

    if (isActive) {
      interval = setInterval(() => {
        setTimer(timerState => timerState + 1)
      }, 1000
    )}

    return () => clearInterval(interval);
  }, [isActive]);

  return (
    <React.Fragment>
      {isActive ? <h1>{timer}</h1> : <h1>Timer Stopped</h1>}
      <button onClick={() => setIsActive(!isActive)}>Start/Stop</button>
    </React.Fragment>
  );
}

export default Timer;
```

We're doing quite a few things in our new `Timer` component, some of which should look familiar: 

* We're using two state variables, `timer` and `isActive`, to track the value of our timer and whether it is active or not. 
* We include a button to start and stop the timer.
* We use a `useEffect` hook to set up an interval when our timer is active, and to remove it when our timer is stopped.

Let's take a closer look at our `useEffect` hook.

```js
  useEffect(() => {
    let interval;

    if (isActive) {
      interval = setInterval(() => {
        setTimer(timerState => timerState + 1)
      }, 1000
    )}

    return () => clearInterval(interval);
  }, [isActive]);
```

Notice that we have one dependency that we've passed into the `useEffect` dependency array: `[isActive]`. That's because we want our effect to run only when the value of `isActive` changes. 

When we first set up the interval with JavaScript's `setInterval` function, we specify that we only want to create a new interval if the timer is started, or in code, if `isActive` is `true`. The interval itself specifies that we should call the `setTimer` function to update the `timer` state every second. 

But how do we stop the timer? That's where the optional `useEffect` clean up mechanism comes in handy! To use this mechanism, we simply need to return a function from the callback function we pass into the `useEffect` hook:

```js
  useEffect(() => {
    ...

    return () => clearInterval(interval);
  }, [isActive]);
```

Here we return an anonymous arrow function that makes use of an implicit return. Note that we can rewrite this return statement as follows:

```js
  useEffect(() => {
    ...

    return function() {
      clearInterval(interval);
    }
  }, [isActive]);
```

And what does this function do? It calls JavaScript's built-in `clearInterval` function to clear the interval we created. 

When we return a function from a `useEffect` hook, it will run this function when our component unmounts to clean up our effects. However, since React runs effects every re-render (unless we specify otherwise), React also performs this clean up before re-running the effect on a subsequent render.  

In the case of our `Timer` component, we've specified that our effect should only run when the `isActive` state variable changes. So, anytime `isActive` changes our interval will be cleared and then re-created only if `isActive` is set to `true`.

### When a Dependency Changes too Often

You may have noticed something else that's new in the `useEffect` hook that we've created for our `Timer` component. Notice that when we create the interval, we're passing in a function when we call `setTimer` to update the value of the `timer` state: 

```js
  useEffect(() => {
    let interval;

    if (isActive) {
      interval = setInterval(() => {
        // Notice the argument we pass into setTimer
        setTimer(timerState => timerState + 1)
      }, 1000
    )}

    return () => clearInterval(interval);
  }, [isActive]);
```

This is in contrast to what we've done up until now, which is to directly pass in a new value for the state variable. For our `timer` state variable, this would look like `setTimer(timer + 1)`. 

Note that the arrow function makes use of an implicit return, which can be hard to read and reason about. If you prefer, the same arrow function above can be re-written as follows:

```js
setTimer(timerState => {
  return timerState + 1
})
```

So why pass in a callback function? It allows us to use the `timer` state, without having to pass in `timer` as a dependency to our `useEffect` hook. To understand this, let's look at the alternative. 

If we don't use a callback function, we would have to pass in the `timer` state variable as a dependency like so:

```js
  useEffect(() => {
    let interval;

    if (isActive) {
      interval = setInterval(() => {
        // Notice the updated code below.
        setTimer(timer + 1)
      }, 1000
    )}

    return () => clearInterval(interval);
  }, [isActive, timer]); // Notice the new dependency.
```

While our timer will work as expected, the issue with this code is that it's less efficient because our effect will be called every time the value of the `timer` state variable changes! That's every second. The solution here is to use the option to pass in a function to `setTimer` instead of a new state value:

```js
setTimer(timerState => timerState + 1)
```

With this callback function, our `useState` hook will handle passing in the `timer` state as the value for the `timerState` parameter. In turn, this code will increment `timer` state by 1. What's more, we don't have to pass in `timer` to the `useEffect` dependency array. This means that our effect will only be called when `isActive` changes, which is exactly what we want. 

This last topic we just covered is a slightly more of an advanced topic with using the `useEffect` hook, but a pertinent one none-the-less. If you want to read more about it, visit this entry called [What can I do if my effect dependencies change too often?](https://reactjs.org/docs/hooks-faq.html#what-can-i-do-if-my-effect-dependencies-change-too-often).

## Resources and Next Steps
---

Whew! We've covered a lot in this lesson. If anything about the `useState` or `useEffect` hook isn't feeling entirely clear, know that we'll be using both of these hooks again when we implement them in our Help Queue application. Before we start in on the Help Queue, we're going to wrap up our introduction to hooks by reviewing the rules of hooks, their benefits, and how you should use them in React applications.

Note that it's normal for unexpected things to happen when we're first learning about hooks and how to implement them. If you run into issues, you should always start by referencing [the React docs on Hooks](https://reactjs.org/docs/hooks-intro.html).

For docs specifically on the `useEffect` hook, visit these links:

* [Using the Effect Hook](https://reactjs.org/docs/hooks-effect.html)
* [Hook API Reference: useEffect](https://reactjs.org/docs/hooks-reference.html#useeffect)
* [Hooks FAQ](https://reactjs.org/docs/hooks-faq.html)