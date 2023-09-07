In the last lesson, we learned how to use action creators to clean up our code. In this lesson, we'll make another small refactor to use action constants.

So far, all of our action types have been strings. For instance, we have an action type of `'TOGGLE_FORM'` that determines whether a form should be toggled in our application.

However, in larger applications (or even smaller ones), using strings for action types can make it harder to track bugs.

Try copying the following code in the browser or running it in VS Code:

```js
const actionConditional = (condition) => {
  if (condition === 'TOGGLE_FORM') {
    return "Form toggled!"
  } else {
    return "Nothing happened :("
  }
}

actionConditional('TOGLE_FORM')
```

Our function will work properly but it won't return the result we expect. That's because there's an error in our code. We've passed the string `'TOGLE_FORM'` into our function instead of `'TOGGLE_FORM'`. Because of this misspelling, our function will run correctly but the correct conditional will not be triggered.

We've all introduced typos in our code — and sometimes they can be difficult to spot, especially when they don't throw an error.

Wouldn't it be nice if the typo above did throw an error?

Well, we can avoid these kinds of typos (or at least use JavaScript's error handling to avoid them) by saving our strings in constants.

Now try the following code:

```js
const TOGGLE_FORM = 'TOGGLE_FORM';

const actionConditional = (condition) => {
  if (condition === TOGGLE_FORM) {
    return "Form toggled!"
  } else {
    return "Nothing happened :("
  }
}

actionConditional(TOGLE_FORM);
```

In the example above, we have the exact same typo. The difference, however, is that we've saved the `'TOGGLE_FORM` string in a constant. Now, if we misspell the constant, we'll get the following error along with the line number where the error occurred:

```javascript
Uncaught ReferenceError: TOGLE_FORM is not defined
```

This could save us a lot of trouble going forward. It's better for our code to fail and throw us a specific error than to have a silent bug that breaks everything for a seemingly inexplicable reason — at least until we hunt the error down.

### Organizing Constants in a React Application

It's typical to save constants in a separate file, especially in a larger application that has many constants. We'll do the same in our application.

React and Redux aren't opinionated about where we store these constants. Since we'll only be using them in our action creators — and because these specific constants will define action types — we will store them in our `actions` directory in a file called `ActionTypes.js`. If we were to create other constants in our application that aren't related to action types, they wouldn't belong here.

<div class="filename">src/actions/ActionTypes.js</div>

```js
export const ADD_TICKET = 'ADD_TICKET';
export const DELETE_TICKET = 'DELETE_TICKET';
export const TOGGLE_FORM = 'TOGGLE_FORM';
```

We will export these all individually. Going forward, any time you add an action type to your applications, make sure to use constants.

### Importing Constant Files

Finally, we need to update our tests, reducers and action creators to use our constants instead of strings for action types.

We'll demonstrate how this looks in one file — our `form-visible-reducer.js` — because this file will have the fewest changes. The process will be exactly the same for all other files — and you can do a find and replace to ensure all action types have been updated correctly.

<div class="filename">reducers/form-visible-reducer.js</div>

```js
import * as c from './../actions/ActionTypes';

const reducer = (state = false, action) => {
  switch (action.type) {
  case c.TOGGLE_FORM:
    return !state;
  default:
    return state;
  }
};

export default reducer;
```

* First, we store our constants in the `c` variable. We could call this anything but `c` or `constants` is standard convention. We are using `c` for brevity.

* Next, we need to update the action type itself. Our action type will now be written as `c.TOGGLE_FORM` instead of `'TOGGLE_FORM'`.

As expected, this will cause all tests related to toggling a form to fail.

Go ahead and follow the steps above for all files that include the following:

* Reducers;
* Tests for reducers;
* Action creators;
* Tests for action creators.

Note that the relative path to the `constants` directory will vary depending on the file where you are adding an import statement. At this point, you should have a clear sense of how relative paths work and should be able to do this on your own.

Finally, run your tests and make sure they work, check that your project still compiles, and run the application and verify all functionality is working correctly.

This is the kind of refactor that you might be asked to do as a junior developer. Often, new developers take on "code janitor" roles as they learn a codebase. This means cleaning up code that more experienced developers might not have the time to deal with. This kind of code cleanup is absolutely essential and can be a great way to learn a codebase and start making contributions to a team immediately.

In this lesson, we learned how to use constants for our action types. There are multiple benefits of this — the biggest benefit is making our application more robust and easier to debug. Using constants can also help us make our intentions clearer to other developers — and make our code easier to read.