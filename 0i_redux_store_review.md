In the last lesson, we built a small application and learned about the Redux library. Redux on its own is relatively simple. However, Redux will become more complicated when we combine it with our React applications using the React Redux library, and start to build more complex reducers. Before we move on, let's review the basics. 

### Redux Summary

* Redux provides a `createStore()` function which we can use to create a Redux store. An application will only ever have a single Redux store.

* `createStore()` takes a reducer as an argument. Reducers take the current state along with an action and determine how the store's state tree should be redrawn. Without reducers, the store wouldn't know how to handle actions.

* The Redux store in our application has an immutable state tree. That means state will be redrawn when it's updated. Remember that immutability is a central tenet of functional programming!

* We use Redux's `getState()` method to get the current state of the store.

* We use Redux's `dispatch()` method to send an action to the store. We pass along the action type — along with any other data needed to complete the action — as an argument to `dispatch()`.

* We can use the `subscribe()` method to listen for any changes to the store. A change in the state tree could trigger any number of things such as an update to the page or a change to the UI.

* Generally, we will use the `getState()` method in combination with the `subscribe()` method. This is because we will be most interested in making changes to our UI when the store is updated.

* Redux uses the **pubsub** pattern. Objects can publish updates to the store (with the `dispatch()` method) while dependents can subscribe to changes (with the `subscribe()` method). Publishers and subscribers don't know about each other.

* When we use React Redux, we won't use Redux's `subscribe()` method directly. However, the React Redux code we write will still be using `subscribe()` under the hood.

* Redux also provides an `unsubscribe()` method. This allows a dependent to stop listening for changes. It's kind of like when we decide to cancel a magazine subscription.
