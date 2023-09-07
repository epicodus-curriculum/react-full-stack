Because our Help Queue is a simple application, we can handle all of our actions with a single reducer. However, what should we do once our reducers start handling multiple slices of state?

In this lesson, we'll create an additional reducer to handle the `formVisibleOnPage` boolean in our React application. Then we will learn how to use Redux's `combineReducers()` function to combine all of our individual reducers into a single root reducer.

Then, in the next lesson, we will integrate our combined reducers into our React application.

If the Help Queue were a production application, we'd probably leave `formVisibleOnPage` as local state instead of adding it to our Redux store. However, making this change provides the simplest use case in our application for making an additional reducer, combining it, and then doing minimal refactoring in our application to get it working. On the other hand, handling `selectedTicket` with the Redux store would require quite a bit more refactoring in our `TicketControl` component because it is changed in so many places. You will have the chance to move all local state into Redux during class time, which is excellent practice for improving your Redux skills. As stated in prior lessons, it's fine if Redux handles local state but there's no one size fits all approach.

## Creating a New Reducer
---

First, let's create our new reducer. We need to test it and get it working independently before we combine it with our existing reducer.

As always, we'll start with a test. Our first test will just check to make sure that the reducer can accept a boolean value (`false`) and return the default state if no action type is provided.

We'll create a `form-visible-reducer.test.js` file in `__tests__` and add the following test:

<div class="filename">__tests__/reducers/form-visible-reducer.test.js</div>

```javascript
import formVisibleReducer from '../../reducers/form-visible-reducer';

describe("formVisibleReducer", () => {

  test('Should return default state if no action type is recognized', () => {
    expect(formVisibleReducer(false, { type: null })).toEqual(false);
  });
});
```

We can run `$ npm run test` and verify that our tests fails.

Now let's create our new reducer and add just enough code to make our test pass:

<div class="filename">src/reducers/form-visible-reducer.js</div>

```javascript
const reducer = (state = false, action) => {
  return state;
};

export default reducer;
```

This reducer will always return a boolean with a default state of `false`. Our test should now pass!

It's time to test for the next simplest behavior. Can our reducer successfully toggle between true and false?

<div class="filename">__tests__/reducers/form-visible-reducer.test.js</div>

```javascript
import formVisibleReducer from '../../reducers/form-visible-reducer';

describe("formVisibleReducer", () => {

  // First test omitted for brevity.

  test('Should toggle form visibility state to true', () => {
    expect(formVisibleReducer(false, { type: 'TOGGLE_FORM' })).toEqual(true);
  });

});
```

Given a value of `false` and an action type called `'TOGGLE_FORM'`, our new reducer should return `true`.

Our test will fail as expected.

Now let's add the logic to make our second test pass:

<div class="filename">src/reducers/form-visible-reducer.js</div>

```javascript
const reducer = (state = false, action) => {
  switch (action.type) {
  case 'TOGGLE_FORM':
    return !state;
  default:
    return state;
  }
};

export default reducer;
```

As we can see, this is a very simple reducer. If the `'TOGGLE_FORM'` action is passed into the reducer, the boolean state will be toggled. That's all there is to it! Our test will now pass.

## Root Reducer
---

Our application now has two reducers. However, our `store` can only take a single reducer as an argument.

We'll need to create an additional reducer called a **root reducer** that handles the work of combining our application's reducers. It may seem like extraneous code when our application is so small, but root reducers can help us keep our code modular and allow us to create many different reducers that can handle different slices of state.

We've tested both of the reducers we've created so far and we'll have to test this one, too. After all, we have to make sure that our root reducer correctly combines state slices.

Let's create a test file at `__tests__/reducers/index-reducer.test.js`. It's common to combine reducers in a file called `reducers/index.js` — that's why we name our test file `index-reducer.test.js`.

Before we move on, we will address the naming convention. Why put the root reducer in a file called `index.js`?

When a file is responsible for retrieving logic from other files in its directory and importing it all into one big module, it's known as a **module index**. Our `index.js` file will hold a root reducer that combines logic from all of our other reducer files — hence the naming convention.

Now we're ready to move on to tests. Here's our first one:

<div class="filename">__tests__/reducers/index-reducer.test.js</div>

```javascript
import rootReducer from '../../reducers/index';

describe("rootReducer", () => {

  test('Should return default state if no action type is recognized', () => {
    expect(rootReducer({}, { type: null })).toEqual({
      mainTicketList: {},
      formVisibleOnPage: false
    });
  });

});
```

As with our other reducer tests, the first — and simplest — behavior we can test is that the reducer returns the default state.

The default state for our root reducer is:

```js
{
  mainTicketList: {},
  formVisibleOnPage: false
}
```

As we can see, it will store multiple slices of state. We could combine many reducers if we wished — and our root reducer can have many state slices.

Now it's time to actually create our root reducer. Create a new file called `index.js` in the `reducers` directory and add the following code:

<div class="filename">reducers/index.js</div>

```js
import formVisibleReducer from './form-visible-reducer';
import ticketListReducer from './ticket-list-reducer';
import { combineReducers } from 'redux';

const rootReducer = combineReducers({
  formVisibleOnPage: formVisibleReducer,
  mainTicketList: ticketListReducer
});

export default rootReducer;
```

Our root reducer has three import statements. The first two are the reducers it needs to access. Any reducers being combined in the root reducer must be imported.

The final import statement is the `combineReducers()` function from Redux. This is _not_ part of React Redux — this is core Redux functionality. Whenever we create a reducer that combines other reducers, we need to import this function.

`combineReducers()` takes an object as an argument. That object contains key-value pairs. The key represents the state slice while the value represents the reducer that handles actions related to that state slice. Our `formVisibleReducer` handles the `formVisibleOnPage` state slice while our `ticketListReducer` handles the `mainTicketList` state slice.

And that's a root reducer. It just combines other reducers into a single file. The `combineReducers()` function makes this process very easy. We can create as many reducers as we need, keeping our code modular and our individual files small (both coding best practices) and then use our root reducer to combine all these smaller reducers. The root reducer will then be passed into our application's store.

We _could_ stop our testing here but something doesn't feel quite right. We haven't actually tested that our root reducer is connected to our other reducers yet.

In this case, we don't want to retest each individual reducer's functionality again — we've already done that. Instead, we'll have some basic **smoke tests**. A smoke test is just a simple test to ensure the basic functionality works. It isn't comprehensive testing, but it will get the job done.

In this case, we just want to test that our root reducers will receive values from any reducers that it combines. In order to do that, we'll actually need to create a store.

<div class="filename">__tests__/index-reducer.test.js</div>

```js
...
import { createStore } from 'redux';

let store = createStore(rootReducer);

...
```

In effect, we are creating a little Redux application in our tests that is separate from our React application. We are creating a store so we can dispatch a few actions and check that our reducers are working together.

First, we'll add a few tests to ensure that our root reducer is returning the default state of each individual reducer:

<div class="filename">__tests__/index-reducer.test.js</div>

```js
import rootReducer from '../../reducers/index';
import { createStore } from 'redux';
import formVisibleReducer from '../../reducers/form-visible-reducer';
import ticketListReducer from '../../reducers/ticket-list-reducer';

...

test('Check that initial state of ticketListReducer matches root reducer', () => {
  expect(store.getState().mainTicketList).toEqual(ticketListReducer(undefined, { type: null }));
});

test('Check that initial state of formVisibleReducer matches root reducer', () => {
  expect(store.getState().formVisibleOnPage).toEqual(formVisibleReducer(undefined, { type: null }));
});
...
```

These tests are checking the same thing, first with our reducer for the ticket list and then with our reducer for form visibility: does the default state of each individual reducer (`mainTicketList` and `formVisibleOnPage`) match the default state for each state slice in the root reducer (`rootReducer.mainTicketList` and `rootReducer.formVisibleOnPage`)? This is part of the reason we need to instantiate a store — so we can use Redux's `getState()` method.

These tests will pass because we've already correctly created our root reducer. These are essentially "sanity checks" to make sure everything works together.

We'll do one more pair of tests. These tests will ensure that when we pass actions into our combined reducers, the root reducer reflects those changes.

<div class="filename">__tests__/index-reducer.test.js</div>

```javascript
...
test('Check that ADD_TICKET action works for ticketListReducer and root reducer', () => {
  const action = {
    type: 'ADD_TICKET',
    names: 'Ryan & Aimen',
    location: '4b',
    issue: 'Redux action is not working correctly.',
    id: 1
  }
  store.dispatch(action);
  expect(store.getState().mainTicketList).toEqual(ticketListReducer(undefined, action));
});

test('Check that TOGGLE_FORM action works for formVisibleReducer and root reducer', () => {
  const action = {
    type: 'TOGGLE_FORM'
  }
  store.dispatch(action);
  expect(store.getState().formVisibleOnPage).toEqual(formVisibleReducer(undefined, action));
});
...
```

In both of these tests, we dispatch an action. We then expect our root reducer to properly handle those actions by passing them into our individual reducers. The store's state slice should be updated accordingly — and should be equal to the return result of the individual reducer that handled the action.

Now that we've set up and tested a root reducer, we are ready to update our React application to use it! We'll do that in the next lesson.
