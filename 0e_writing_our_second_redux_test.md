We're ready to move on to our second test. Our goal is to give our Help Queue full CRUD functionality with Redux. In order to do that, we will need to be able to add a ticket.

Before we start writing the test, let's think about how this will work in terms of our reducer.

As we briefly discussed in the last lesson, a reducer takes two arguments. One is the current state and one is an action to alter the current state.

That means that our updated reducer will need to accept a new action in order to actually add a ticket to our list of tickets. We will call this action `ADD_TICKET`. It is Redux convention to capitalize actions and separate words with an underscore.

### Writing a Test for Adding Tickets

In order to write a test for adding tickets, we'll need to provide some ticket data at the top of the describe block:

<div class="filename">ticket-list-reducer.test.js</div>

```javascript
import ticketListReducer from '../../reducers/ticket-list-reducer';

describe('ticketListReducer', () => {

  let action;
  const ticketData = {
    names: 'Ryan & Aimen',
    location: '4b',
    issue: 'Redux action is not working correctly.',
    id: 1
  };
  //Don't remove the test we've already written!
  ...
});
```

We add two things to the code above:

1. We declare an `action` but don't define it yet. Each of our new tests will define what the action should be (whether that is adding, updating, or deleting a ticket).

2. We create a `ticketData` constant that provides ticket information for testing purposes.

Next, let's write a test for adding tickets:

<div class="filename">ticket-list-reducer.test.js</div>

```javascript
import ticketListReducer from '../../reducers/ticket-list-reducer';

describe('ticketListReducer', () => {

  let action;
  const ticketData = {
    names: 'Ryan & Aimen',
    location: '4b',
    issue: 'Redux action is not working correctly.',
    id: 1
  };

  ...

  test('Should successfully add new ticket data to mainTicketList', () => {
    const { names, location, issue, id } = ticketData;
    action = {
      type: 'ADD_TICKET',
      names: names,
      location: location,
      issue: issue,
      id: id
    };

    expect(ticketListReducer({}, action)).toEqual({
      [id] : {
        names: names,
        location: location,
        issue: issue,
        id: id
      }
    });
  });

});
```

In our new test, we use ES6 destructuring syntax to provide keys from our `ticketData` to the scope of our test.

In the last lesson, we briefly touched on the fact that our `action` can contain more than just the action's `type`. In the test above, our `action` has a `type` of `ADD_TICKET`. However, our reducer won't be able to do anything useful unless it also has information about the ticket it is supposed to add. That's why our reducer takes an object as an argument instead of just a string for the action type itself. Because it takes an object, it can take multiple key-value pairs that include additional information about the action the reducer will need to take.

This is also helpful because it's a code smell to pass too many arguments into a function. Functions with lots of arguments result in buggy code that's difficult to read. We can keep our reducers smelling spiffy by just passing in two arguments — the initial state and information about the action that should change the initial state.

Our new test should successfully fail:

```javascript
 FAIL  src/__tests__/reducers/ticket-list-reducer.test.js
  ticketListReducer
    ✓ Should return default state if no action type is recognized (6ms)
    ✕ Should successfully add new ticket data to mainTicketList (6ms)

  ● ticketListReducer › Should successfully add new ticket data to mainTicketList

    expect(received).toEqual(expected) // deep equality

    - Expected
    + Received

    - Object {
    -   "1": Object {
    -     "id": 1,
    -     "issue": "Redux action is not working correctly.",
    -     "location": "4b",
    -     "names": "Ryan & Aimen",
    -   },
    - }
    + Object {}

      26 |       id: id
      27 |     };
    > 28 |     expect(ticketListReducer({}, action)).toEqual({
         |                                           ^
      29 |       [id] : {
      30 |         names: names,
      31 |         location: location,

      at Object.test (src/__tests__/reducers/ticket-list-reducer.test.js:28:43)

Test Suites: 1 failed, 1 total
Tests:       1 failed, 1 passed, 2 total
Snapshots:   0 total
Time:        2.801s
Ran all test suites related to changed files.
```

### Adding Logic to Make Our New Test Pass

Now we're ready to add logic to the reducer to pass our new test. Ultimately, a reducer is just a pure function that contains a conditional. What the reducer does is dependent on the action passed in as an argument.

We'll use a switch case to do this. A switch case is really just simplified syntax for writing a conditional statement. If you need a refresher on switch cases, [revisit this lesson](https://www.learnhowtoprogram.com/intermediate-javascript/object-oriented-javascript/switch-cases).

<div class="filename">ticket-list-reducer.js</div>

```javascript
const reducer = (state = {}, action) => {
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
  default:
    return state;
  }
};

export default reducer;
```

* We use ES6 object destructuring to destructure the other properties from the `action` object into the variables `names`, `location`, `issue`, and `id`.

* Next, we state that our `switch` will be based on the `action.type`. Because the `action` parameter takes an object, the reducer needs to look at the `action`'s `type` property to determine the action it should take.

* As always, we don't want to mutate objects in pure functions. Instead, we use `Object.assign()` to clone the `state` object. We discussed `Object.assign()` when we learned about functional programming, but it's a complex enough function that it's worth reviewing. In order for this to work correctly, `Object.assign()` must take three arguments:
  
  * The first argument **must** be an empty object `{}`. Otherwise, `Object.assign()` will directly mutate the state we pass in instead of making a clone of it first. We don't want to do that!

  * The second argument is the object that will be cloned. In the reducer action above, it's the ticket list state we pass into our function.

  * The third argument is the change that should be made to our new copy. This will always be the new ticket that should be added to our ticket list state.

* `Object.assign()` creates a new key-value pair where the key is the ticket's `id` and the value is an object with all of the ticket's properties.

* We return the value from `Object.assign()`. Our reducer hasn't altered anything. Instead, it made a copy of the state that was passed in as argument, altered the copy, and then returned the altered copy so it can be used elsewhere in our code.

This is really, really important to understand — and cause a lot of problems otherwise. According to the Redux documentation, "Redux assumes that you never mutate the objects it gives to you in the reducer. **Every single time, you must return the new state object.**" (The emphasis is theirs.) If we don't do this, we can get bad bugs that are very difficult to fix. The Redux store (which stores our data and which we'll discuss soon) may still update. However, because the original state has been mutated, Redux won't work properly and the React application won't get re-rendered correctly. In other words, if we mutate state in a reducer, we can end up with a situation where the data gets updated but React doesn't properly re-render components to reflect that. That is bad — and often quite challenging to fix.

If we run our tests, they will both pass:

```javascript
 PASS  src/__tests__/reducers/ticket-list-reducer.test.js
  ticketListReducer
    ✓ Should return default state if no action type is recognized (2ms)
    ✓ Should successfully add new ticket data to mainTicketList

Test Suites: 1 passed, 1 total
Tests:       2 passed, 2 total
Snapshots:   0 total
Time:        0.488s, estimated 1s
Ran all test suites related to changed files.
```

At this point, our reducer can now successfully handle the action for adding a ticket. Note, however, that we still haven't used Redux yet. Everything we've done so far is vanilla JavaScript. We've created a pure function that will take the current state, make a copy of that state, add a ticket to that copy, and then return the copy itself.

As we can see here, the reducer is only responsible for handling specific actions. It will never actually store any state. Even though we've written an action, our new ticket isn't stored anywhere. The Redux store will handle that. However, we aren't going to worry about Redux just yet. Instead, let's add full CRUD functionality to our reducer first.

Before we continue, note that our `ADD_TICKET` action actually already provides update functionality. This is a basic fact of key-value pairs in an object. If we add a ticket with a key that already exists in our ticket list, the old values associated with the key will be replaced with the new values. If you'd like more practice with testing, try writing a test that confirms this.
