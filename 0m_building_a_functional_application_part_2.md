In the last lesson, we built a function factory for incrementing a plant's attributes. However, we still have no place to save that information. If we wanted this application to be truly functional, we could retrieve the state from the DOM when we need to update it. Feel free to try this approach in class — but don't forget that it's not very efficient to query the DOM!

We are going to store our state inside a function. This approach will give us more practice with closures, one of the most important concepts we can understand in terms of taking our skills to the next level. Note that this lesson contains more challenging content. We recommend reading it several times, carefully recreating the code in class, and being patient with yourself.

Here's our function for storing state. Note that all the function names are abstracted. We could potentially reuse this function elsewhere as needed, too.

```js
const storeState = () => {
  let currentState = {};
  return (stateChangeFunction) => {
    const newState = stateChangeFunction(currentState);
    currentState = {...newState};
    return newState;
  }
}
```

1. First, our outer function is stored in the constant `storeState`. It does not take an argument. The only job of the outer function is to store the `currentState` of an object.
2. The `currentState` of an object will be initialized as a `{}`. Note that we use `let` because the `currentState` will be mutated each time the inner function is called.
3. Our outer function returns an anonymous inner function that takes one parameter called `stateChangeFunction`. This inner function will take a function as an argument. We can do this because functions are first-class citizens in JavaScript. The function that we pass in will specify the exact change that should be made to `currentState`. Note that we've already written the function that will be passed in as an argument — we will demonstrate how it works soon.
4. The line `const newState = stateChangeFunction(currentState);` will take the function we pass in as an argument and then call it on `currentState`. Instead of mutating `currentState`, we will save the new state in a constant called `newState`.
5. Now it's time to break the rules. We are going to need to update the `currentState`. We will make a copy of `newState` and assign it to `currentState`. (This is similar to what React does with its `setState()` method. We'll learn about `setState()` in the next course section.)
6. Finally, our inner function will return the `newState`. Why are we returning `newState` instead of `currentState`? Well, in this particular use case, it doesn't matter which we do because both `newState` and `currentState` are equal. In a basic React application, we'd update the state and then return that state. In that case, it makes more sense to return `currentState`. But here's another interesting use case which we'll learn about in a few weeks when we use Firestore, a cloud database solution. With Firestore, we might think of `currentState` as being the state of our database. However, because it takes time to update and return `currentState` (an async operation), we can provide a quick snapshot of state to users by just returning the equivalent of `newState`.

Next, we will need to store our function in another constant like this:

```js
const stateControl = storeState();
```

Here, we are actually invoking the `storeState()` function and creating a closure over the `currentState` variable in the outer function.

Why are we calling it `stateControl` instead of something like `stateChanger`? Well, we might also just want to _look_ at the current state — not change it — so `stateChanger` wouldn't be the best name in that situation.

Let's take a look at the value of `stateControl`:

```js
(stateChangeFunction) => {
  const newState = stateChangeFunction(currentState);
  currentState = {...newState};
  return newState;
}
```

As we can see, `stateControl` holds the inner function. It also retains the `currentState` variable from the outer function. When `storeState()` is called and stored in the `stateControl` variable, `currentState` is set to `{}`.

Now let's try passing one of our feeding functions into `stateControl`. Specifically, we'll pass in the feeding function `blueFood()` which we created in the last lesson. This function increments the food level of a plant by 5.

```js
const fedPlant = stateControl(blueFood);
> { soil: 5 }
```

Here's what just happened:

1. We passed in the variable `blueFood` into `stateControl`. This invokes the inner function inside `storeState()`. (Be careful here — we don't want to pass in `blueFood()` because we don't want the function to be invoked yet!)
2. `blueFood` is passed in as an argument for the `stateChangeFunction` parameter. Now `const newState = blueFood(currentState);`.
3. When `blueFood(currentState)` is called, it invokes the following function:

```javascript
(state) => ({
  ...state,
  ["soil"] : (state["soil"] || 0) + 5
})
```

Remember that `5` replaces the `value` variable and `"soil"` replaces the `prop` variable because `blueFood` increments `soil` by 5. If this isn't clear, you may want to review how we used a curried function in the last lesson to create `blueFood` in the first place.

4. `currentState` is passed into the `state` parameter. Because `currentState` doesn't have a `soil` property yet, it defaults to `0` before `5` is added. This is because we are using the `||` operator to ensure the default value of the `soil` property is 0 if it hasn't been defined.

Now, if we pass in `greenFood`, we'll get the following:

```js
const plantFedAgain = stateControl(greenFood);
> { soil: 15 }
```

Our function is successfully storing the plant's state!

As you can see in the example above, `plantFedAgain` only has a `soil` property. That's because our `currentState` variable begins as an empty object. We could do the following to initialize the plant with all three properties:

```js
const storeState = () => {
  let currentState = { soil: 0, light: 0, water: 0 }; //Small change made to function here.
  return (stateChangeFunction) => {
    const newState = stateChangeFunction(currentState);
    currentState = {...newState};
    return newState;
  }
}
```

While the version above will work for our small application, it's not very reusable. What if we eventually want to add other attributes or use it for other kinds of objects entirely?

We could also give the outer function a parameter like `initialState` and do the following:

```js
const storeState = (initialState) => {
  let currentState = initialState; // We could pass in an initial state to the object instead of starting with an empty object as well.
  return (stateChangeFunction) => {
    const newState = stateChangeFunction(currentState);
    currentState = {...newState};
    return newState;
  }
}
```

This would work correctly. However, for our use case, it won't be necessary for our plant to start with any properties. You may find that you need to set an initial value when you practice building out applications using closures in class. For instance, let's say your application has multiple different plants, each with different starting attributes. You'd need to pass in the `initialState` of the object to set its properties as we do in the example above.

A note worth mentioning again: it is very important that we pass in a variable holding a function into `stateControl` and **not** the invoked function. This would not work:

```js
const blueFood = changeState("soil")(5);

const fedPlant = stateControl(blueFood());

```

Specifically, passing in `blueFood()` as an argument to `stateControl()` won't work: `stateControl(blueFood());`. 

Why? `blueFood` needs to take the `currentState` as an argument, and can only do so inside the body of the `storeState` function itself. If we invoke the `blueFood` function too early without an argument like we did above, we'll get an error:

```javascript
Uncaught TypeError: Cannot read property 'soil' of undefined
```

This is because the `blueFood` function expects to be given an object as an agrument. This would be a good reason to add some error handling to the function to deal with this use case. Try doing so in your own applications!

Before we move on, there is one other important issue we might want to cover — it's not necessarily as relevant in this little plant application but it will likely be something you'll want to access in projects during this section.

We've covered how to change state — but what if we just want to access it but not change it?

Well, let's look at our `storeState` function literal again:

```js
const storeState = () => {
  let currentState = {};
  return (stateChangeFunction) => {
    const newState = stateChangeFunction(currentState);
    currentState = {...newState};
    return newState;
  }
}
```

We know that we need to do something with this particular function in order to see the state because this is where our `currentState` is enclosed — there's no way to access it other than this function!

Well, in this case, we just need the `stateChangeFunction` be a function that takes the original state and then returns it. In other words, the `stateChangeFunction` needs to be the following:

```js
state => state
```

Well, it would be a bit annoying to have to pass that in wherever we want to see the current state. However, we can do something else — since functions are first class citizens, we can pass this in as a default parameter like this:

```js
const storeState = () => {
  let currentState = {};
  return (stateChangeFunction = state => state) => {
    const newState = stateChangeFunction(currentState);
    currentState = {...newState};
    return newState;
  }
}
```

Here's the change:

```js
return (stateChangeFunction = state => state)
```

It may look strange, but what we are saying here is that if `stateChangeFunction` is `undefined` (no argument is passed in), the `stateChangeFunction` should be `state => state`.

That means all we need to do is call `stateControl()` (without arguments) in order to just return the current state!

## Bringing It All Together
---

Finally, here's how we could implement this in the browser. This example has been kept very simple and can only increment soil. This example does not include webpack, testing, or separation of logic. Try adding this functionality on your own. Note also that manipulating the DOM will always lead to functions that produce side effects. There's no way around it!

<div class="filename">plant.js</div>

```js
// This function stores our state.
const storeState = () => {
    let currentState = {};
    return (stateChangeFunction = state => state) => {
      const newState = stateChangeFunction(currentState);
      currentState = {...newState};
      return newState;
    }
  }
  
const stateControl = storeState();

// This is a function factory. 
// We can easily create more specific functions that 
// alter a plant's soil, water, and light to varying degrees.
const changeState = (prop) => {
  return (value) => {
    return (state) => ({
      ...state,
      [prop] : (state[prop] || 0) + value
    })
  }
}

// We create four functions using our function factory. 
// We could easily create many more.
const feed = changeState("soil")(1);
const blueFood = changeState("soil")(5);

const hydrate = changeState("water")(1);
const superWater = changeState("water")(5);

window.onload = function() {

  // This function has side effects because we are manipulating the DOM.
  // Manipulating the DOM will always be a side effect. 
  // Note that we only use one of our functions to alter soil. 
  // You can easily add more.
  document.getElementById('feed').onclick = function() {
    const newState = stateControl(blueFood);
    document.getElementById('soil-value').innerText = `Soil: ${newState.soil}`;
  };

  // This function doesn't actually do anything useful in this application 
  // — it just demonstrates how we can "look" at the current state 
  // (which the DOM is holding anyway). 
  // However, students often do need the ability to see the current state 
  // without changing it so it's included here for reference.
  document.getElementById('show-state').onclick = function() {
    // We just need to call stateControl() without arguments 
    // to see our current state.
    const currentState = stateControl();
    document.getElementById('soil-value').innerText = `Soil: ${currentState.soil}`;
  };
};
```

<div class="filename">index.html</div>

```html
<html>
  <head>
    <script type="text/javascript" src="plant.js"></script>
    <title>Grow Your Plant</title>
  </head>
  <body>
    <button id="feed">Add soil</button>
    <button id="show-state">Current Stats</button>
    <h1>Your Plant's Values</h1>
    <h3><div id="soil-value">0</div></h3>
  </body>
</html>
```
