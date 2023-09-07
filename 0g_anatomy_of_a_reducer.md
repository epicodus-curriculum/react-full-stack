Over the last several lessons, we've built and tested a reducer that will allow us to take CRUD actions in our Help Queue application. Reducers can get considerably more complex — and we'll eventually learn more advanced techniques such as splitting and combining reducers — but for now, we'll focus on reviewing what we've learned so far. Having a clear understanding of how reducers work will make learning Redux much easier.

* The parameters of a reducer generally look something like this:

```js
(state = {}, action)
```

The first parameter accepts the current state as an argument. It's typical for that state to have a default value. (In the example above, it is `{}`, but it could be another type of object as well.)

The second parameter accepts an object as an argument. This object contains a `type` property that determines the action that should be taken. This object may contain other properties that are needed to update the current state.

* Reducers are always pure functions. This makes them much easier to test. For this reason, they will never directly alter state in our application. All a reducer cares about is taking a thing, applying an action to a copy of that thing, and then returning the altered copy. It doesn't know anything else about our application such as how state will be stored or applied in the UI. **Never use a reducer for side effects!**

* Reducers use a switch case to determine which action should be taken. The format of a switch case in a reducer looks like this:

```js
switch (action.type) {
  case 'ACTION_ONE':
    // ...
  case 'ACTION_TWO':
    // ...
  case 'ACTION_THREE':
    // ...
  default:
    // This will generally just return the unchanged state.
  }
```

A switch case is just vanilla JavaScript. It is similar to having a series of `if` statements chained together. Note that it is **not** like an `if...else` statement — all conditions will be run unless we use `break` or `return` statements — so we should always include those. Otherwise, our reducers will be prone to unintended behavior.

* Action `type`s are strings. The name of the action should be capitalized with all words being separated by underscores. For example, this is correct syntax for an action: `ACTION_ONE`. Note that even though action `type`s are strings, they are often saved as constants. We will go over this in a future lesson.

That is the basics of reducers in a nutshell. If you ever have any confusion about how they work, review this lesson.