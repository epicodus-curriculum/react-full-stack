Our project is set up and ready to go. It's time to write our first test. In the process, we will learn a bit more about reducers. Once we've finished testing and building our first reducer, we will review what reducers are and how they work.

As always, we should start by testing the simplest behavior we can. In this case, the simplest behavior we can test is the initial state of our Help Queue. The initial state will be an empty object `{}`. That is because our list of tickets is an object — each key-value pair inside that object represents a ticket.

Here's our test:

<div class="filename">src/__tests__/reducers/ticket-list-reducer.test.js</div>

```js
import ticketListReducer from '../../reducers/ticket-list-reducer';

describe('ticketListReducer', () => {
  test('Should return default state if there is no action type passed into the reducer', () => {
    expect(ticketListReducer({}, { type: null })).toEqual({});
  });
});
```

We will start by importing our reducer. (Note that we haven't actually created any reducers yet.) We'll store the imported code in a variable called `ticketListReducer`. We will have to jump up two directories in our relative path (`../../`) to reach the `reducers` directory where our ticket list reducer will be stored.

Our test syntax should look somewhat familiar from JavaScript — though it's been just long enough that the work we did with Jest and vanilla JS may seem like a distant memory already. It's a common issue for coders and it's completely normal. We often need to move back and forth between tools, libraries and frameworks, and it's not always easy to remember the little details!

A quick refresher:

* `describe` blocks are for grouping together related tests. All of our `ticketListReducer` tests will be grouped together in one `describe` block.
* `test` refers to the individual test. This test will make sure our reducer returns the correct default value.
* Our `expect` statement lets our test know what the expected value will be.

In the test above, we expect the following:

```js
expect(ticketListReducer({}, { type: null })).toEqual({});
```

As we can see here, our reducer will take two arguments. The first argument is the current state while the second argument is an action that will be applied to the current state. Note that the action's type is stored inside an object. This object can potentially contain other things besides the name of the action itself. We will cover this further in the next lesson.

Remember that reducers are pure functions. For that reason, they will never hold any information about an application's current state. If they knew about application state and could directly alter it, they wouldn't be pure functions! That would be a side effect — and it would make our application more prone to bugs and other issues.

In other words, our reducer needs to know two things:

* What is the thing that needs to be changed (the current state)?
* How should that thing be changed (what action should be applied to that thing)?

As long as the reducer knows these two things, it will be able to take care of the rest — returning the new state of the thing once the change has been applied to it.

Looking at our test above, our expectation should be clear. If our current state is an empty object `{}` and we do nothing to it (the action type is `null`), then the reducer should return the current state `{}`.

### Failing Our First Test

Next, we want to add just enough code to our reducer to have a successful fail. Here's the code:

<div class="filename">src/reducers/ticket-list-reducer.js</div>

```js
const reducer = (state = {}, action) => {

};

export default reducer; 
```

Note that our `export` statement includes `default` because this file will only have one function inside it — the reducer for our ticket list. That way, when we import the reducer into our test or anywhere else, we can store all the code directly inside a single variable such as `ticketListReducer`.

Next, our function has two parameters. The first is the `state` that will need to be changed while the second is the `action` that will be applied to that state.

Our first parameter has a default value. This is an ES6 feature. We can pass default arguments as parameters to functions. That way, if an argument isn't passed into the parameter when the function is called, the function can just use the default.

Let's try running our test with `npm test`:

```js
 FAIL  src/__tests__/reducers/ticket-list-reducer.test.js
  ● ticketListReducer › Should return default state if no action type is recognized

    expect(received).toEqual(expected)
    
    Expected value to equal:
      {}
    Received:
      undefined
    
    Difference:
    
      Comparing two different types of values. Expected object but received undefined.
      
      at Object.test (src/__tests__/reducers/ticket-list-reducer.test.js:6:146)
          at <anonymous>

 PASS  src/App.test.js

Test Suites: 1 failed, 1 passed, 2 total
Tests:       1 failed, 1 passed, 2 total
Snapshots:   0 total
Time:        1.433s
Ran all test suites.
```

Our test fails as expected. We want an empty object `{}` but instead our reducer returns `undefined`. It should be clear why this is happening — our reducer isn't returning anything yet!

However, it's important to have a meaningful fail. That way, we can make sure that our test is correctly wired up to the reducer — and also that we don't get a false positive.

## Passing Our First Test
---

We only need to make a small change to our reducer to get our first test passing:

<div class="filename">src/reducers/ticket-list-reducer.js</div>

```js
const reducer = (state = {}, action) => {
  return state;
};

export default reducer;
```

If no action has been specified, what goes in is exactly what should come out. Our test passes and returns `{}`.

At this point, we aren't even using Redux yet — we're just using vanilla JavaScript to write and test a pure function.

In the next lesson, we will work on our second test — and continue practicing with reducers!