State in React applications can get complicated very quickly. Fortunately, React community members have developed open-source tools to make managing state easier. In this course section, we'll focus on learning **Redux**, which is one of the most popular tools for managing state in React.

## Introduction to Redux
---

[Dan Abramov](https://github.com/gaearon) and [Andrew Clark](https://github.com/acdlite) developed the open-source library **Redux** in 2015. As explained in the [Redux documentation](http://redux.js.org/docs/introduction/Motivation.html), their goal was to simplify managing complex state:

> As the requirements for JavaScript single-page applications have become increasingly complicated, our code must manage more state than ever before...

> **Managing this ever-changing state is hard.** If a model can update another model, then a view can update a model, which updates another model, and this, in turn, might cause another view to update. At some point, **you no longer understand what happens in your app as you have lost control over the _when, why,_ and _how_ of its state**...

> As if this wasn't bad enough, consider the new requirements becoming common in front-end product development. As developers, we are expected to handle optimistic updates, server-side rendering, fetching data before performing route transitions, and so on. **We find ourselves trying to manage a complexity that we have never had to deal with before**...

The documentation continues on to describe how Redux aims to simplify all this:

> Redux attempts to make state mutations predictable by imposing certain **restrictions on how and when updates can happen**.

Essentially, Redux enforces a specific structure as well as conventions for state.

## Three Principles of Redux
---

As outlined in the [Three Principles](http://redux.js.org/docs/introduction/ThreePrinciples.html) section of its documentation, Redux enforces state management with three main rules:

### 1. Single Source of Truth

In a Redux application, state must be stored as an object tree in a **store**. An **object tree** (also commonly referred to as a **state tree**) is a big object that can contain multiple **slices** of state (like the state slices we used in the React Fundamentals course section). And a store is exactly what it sounds like: a Redux-managed location to _store_ application state. The store acts as the "single source of truth" for the whole app.

### 2. State is Read-Only

Redux state is strictly read-only. **We cannot modify state directly**. The only way to change state in Redux is to dispatch an **action**. The action communicates our intention to change state to the **store**. We won't use `setState()` to update state with Redux.

### 3. Changes are Made with Pure Functions

Redux requires that we make changes to state using pure functions. This makes our code easier to test and we can ensure no unintended side effects will occur.

## Anatomy of Redux
---

So what does this look like in action? Let's discuss each Redux item that needs to be added and configured in a React application to get it up and running.

### Stores

The Redux **store** is our "single source of truth" where all application state resides. All application state lives at the "top" of our component tree and trickles down into necessary components.

### Reducers

Eventually we'll want to update (or _mutate_) state in our store. In Redux, we do this with **reducers**. They communicate our intended **actions** to the store.

Reducers are just functions that take the current state and an action as arguments. We can use plain old JavaScript to write them. They create a _new_ version of state using this information, similar to the manner we constructed new versions of our state in the last course section to provide as arguments to `setState()`.

Generally, each slice of state in the Redux store has its own dedicated reducer. Because an application can have many slices of state, as we saw in the last course section, Redux applications will often have many different reducers.

Reducers must be **pure functions** to avoid unintended side effects.

### Actions

Actions are objects that describe something that happened. They're **dispatched** to the Redux store and handled by **reducers**. The reducer receives the action and executes logic based on the action's **type** that alters state. Data included with the action is called a **payload**. **Actions are the _only_ way to invoke state updates in Redux.**