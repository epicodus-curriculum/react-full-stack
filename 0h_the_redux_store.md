Now we have a reducer that can handle some actions. Everything we've done so far is vanilla JavaScript. We're ready to start working with Redux itself.

In this lesson, we will cover the Redux **store**. A Redux store does the following things:

* Holds our application state;
* Allows us to access state in our application with the function `getState()`;
* Allows us to actually update state (with help from our reducers) using the `dispatch()` function, which takes an action as an argument.

That's all it does. It is a **single source of truth** that holds our application state. Because it is a single source of truth, there can only ever be one store in an application. 

In this lesson, we will use the reducer we built in previous lessons to create a small JavaScript project. The goal of this project is to demonstrate how Redux works. We are doing this independently of React because we need to use special bindings (provided by the `react-redux` package) to use Redux with React. It isn't terribly difficult to set up a project with this package. However, it can be challenging for beginners to separate Redux code from the code needed to connect Redux to React. Our goal here is to keep Redux as simple as possible for now and focus on exploring how the Redux store works.

To explore these concepts in isolation, we're going to create a new project (but don't worry, we'll come back to the other ticket-queue app we've been building soon!). Create a directory called `learning-about-redux` and add the following file:

<div class="filename">index.html</div>

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/redux/4.0.5/redux.min.js"></script>
    <script type="text/javascript" src="learning-about-redux.js"></script>
    <title>Learning About Redux</title>
  </head>
  <body>
  </body>
</html>
```

This file has a link to a CDN for Redux as well as a link to the `learning-about-redux.js` script we are about to create. 

As a quick refresher, **CDN** stands for **Content Delivery Network**, which is a way to tell the browser that is loading our project to fetch resources from a specific library we want to use in our project. We've used CDNs in the past with Bootstrap. Two alternatives to using a CDN are to simply download the files of a library and put them in your project folder, or to set up a package manager like `npm` and load libraries to your project via a `package.json`.  

Next, add a file called `learning-about-redux.js` to the project directory. We aren't going to worry about import statements, webpack, or anything else. Our file will just contain the reducer we built along with a few basic Redux methods and `console.log()` statements to log what is happening.

Here's the code:

<div class="filename">learning-about-redux.js</div>

```js
// REDUCER — this is the same code we wrote in our previous app.

const ticketListReducer = (state = {}, action) => {
  const { names, location, issue, id } = action;
  switch (action.type) {
  case 'ADD_TICKET':
    return Object.assign({}, state, {
      [id]: {
        names: names,
        location: location,
        issue: issue,
        id: id
      }
    });
  case 'DELETE_TICKET':
    const newState = { ...state };
    delete newState[id];
    return newState;
  default:
    return state;
  }
};

// REDUX STORE — Everything below this line is new code!

const { createStore } = Redux;

const store = createStore(ticketListReducer);

console.log(store.getState());

const unsubscribe = store.subscribe(() => console.log(store.getState()));

store.dispatch({
  type: 'ADD_TICKET',
  names: 'Jasmine and Justine',
  location: '2a',
  issue: 'Reducer has side effects.',
  id: 1
});

store.dispatch({
  type: 'ADD_TICKET',
  names: 'Brann and Rose',
  location: '3b',
  issue: 'Problems understanding Redux.',
  id: 2
});

store.dispatch({
  type: 'DELETE_TICKET',
  id: 1
});

unsubscribe();
```

We start with the reducer we've already created. We don't need to rehash this here, so let's skip right to the new code.

* We start by using destructuring to get the `createStore` function from `Redux`. (Remember that Redux is coming from the CDN.) We need this function to actually build our store.

* Next, we create our store:

```js
const store = createStore(ticketListReducer);
```

`createStore()` takes a reducer as an argument. The reducer tells the store how it should handle actions. Otherwise, the store wouldn't know how to handle any actions.

We save our created store in a constant called `store`. This `store` holds our application's state tree. The state tree is immutable — which means that the store will replace it with a new immutable structure each time it needs to be updated. Think of it as cleaning the old representation of state off a whiteboard and then drawing up a new representation.

You may wonder why we are saving the store as a `const` if the store itself can have changing representations of state. Well, `const` is still a signal to other developers that they shouldn't modify the store at all. We'll let Redux handle that behind the scenes!

* Next, we can check the state of our store with `getState()`. Note that `getState()` is a method — we need to call it **on** our store.

Our learning application doesn't have a UI — we are just checking changes to state with `console.log()`, so make sure to open the console when you run the project in the browser.

The following line logs our initial state in the console:

```js
console.log(store.getState());
```

If we run our code in the browser, the console will show that the initial state is an empty object as expected. (This will be the first line that shows up in the console.)

However, this isn't very helpful for logging changes to the store's state.

* In order to actually log changes to state, we need to listen for changes with the `subscribe()` method. You may remember that we [briefly discussed the **pubsub** pattern](https://new.learnhowtoprogram.com/react/functional-programming-with-javascript/state) when we learned about functional programming. 

A quick refresher: the pubsub pattern is a variant of the observer pattern. Various dependents (in an application) will listen for changes by **subscribing** to the store. Meanwhile, other objects will be **publishing** changes to the store. The publishers and subscribers don't need to know about each other — instead, the store acts as an intermediary.

We subscribe to our store with the following:

```js
const unsubscribe = store.subscribe(() => console.log(store.getState()));
```

Let's take a closer look at this code. We are subscribing to the store with the `subscribe()` method and storing that in a constant called `unsubscribe`.

Note that we can call the code above **without** saving it in the `unsubscribe` constant. However, then we won't be able to unsubscribe to it later.

In our code, we are passing an anonymous function to `store.subscribe()`. Each time there's a change in the store, our subscription will be triggered, causing this code to run:

```js
console.log(store.getState());
```

When we subscribe, it just means our code is actively listening. For example, when we subscribe to a magazine, it doesn't mean we get an issue immediately. It just means that the next time an issue is published, we will get that issue.

* We've subscribed — now what about getting the new issue of that magazine? Well, we need to **publish** it first. In this metaphor, the Redux store is our magazine. The store needs to be updated (a new issue of the magazine created) in order to trigger our subscription. In our Help Queue project, the store is holding our tickets. How do we update the store? We do this with the `dispatch()` method. We dispatch three actions in the code below:

```js
store.dispatch({
  type: 'ADD_TICKET',
  names: 'Jasmine and Justine',
  location: '2a',
  issue: 'Reducer has side effects.',
  id: 1
});

store.dispatch({
  type: 'ADD_TICKET',
  names: 'Brann and Rose',
  location: '3b',
  issue: 'Problems understanding Redux.',
  id: 2
});

store.dispatch({
  type: 'DELETE_TICKET',
  id: 1
});
```

Each time an action is dispatched, it will trigger our subscription and log the result of `store.getState()` to the console.

If we run this application in the browser, we'll see four messages logged in the console. Our store changes as actions are dispatched to it!

Finally, we can call `unsubscribe()` to end our subscription. In the magazine metaphor, this means we've read enough issues of the magazine and we don't want more issues to come in the mail. For our Help Queue project, this means that we're no longer interested in tracking any store updates. When working with Redux, developers often use a dev tool like `store.subscribe(() => console.log(store.getState()));` for debugging purposes.

When we use bindings to combine Redux and React, we won't use the `subscribe()` method ourselves. Instead, we'll use React Redux to handle subscribing for us. We'll do that because React Redux will optimize the subscription process for us — and make it easier in the process. However, even though we won't be using `subscribe()` any longer, it's still important to know how it works.

In the next lesson, we'll review what we've learned so far about Redux. Even though the basics are relatively straightforward, it's very important to understand just what's happening in a Redux store.
