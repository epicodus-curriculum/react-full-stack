In the last lesson, we added a reducer action called `ADD_TICKET`. Our reducer can now take the current state of our ticket list and return the ticket list's new state with a new ticket added to the list. It does this without making alterations to the current state or storing that data anywhere.

In this lesson, we will add an action called `DELETE_TICKET`. We will start by writing a test, making sure it fails, and then getting it to pass by adding new code to our reducer.

We will not need to reinvent the wheel to add delete functionality to our reducer. We'll still be using vanilla JavaScript — and we won't be applying any fancy new concepts. The goal of this lesson is both to reinforce and demystify what reducers are and how they work. It's also helpful to continue getting back into the swing of testing with Jest.

### Writing a Test for Deleting a Ticket

Our new test will need some additional sample data. We'll start by creating a sample `currentState` that already includes two tickets. Our test will determine whether or not the `DELETE_TICKET` action in our reducer successfully returns a new state with the correct ticket removed.

Here's our new test. Note that we've omitted the code that's not directly relevant to our new test — but don't actually remove that code from your own tests!

<div class="filename">__tests__/reducers/ticket-list-reducer.test.js</div>

```js

import ticketListReducer from '../../reducers/ticket-list-reducer';

describe('ticketListReducer', () => {

  const currentState = {
    1: {
      names: 'Ryan & Aimen',
      location: '4b',
      issue: 'Redux action is not working correctly.',
      id: 1 
    }, 2: {
      names: 'Jasmine and Justine',
      location: '2a',
      issue: 'Reducer has side effects.',
      id: 2 
    }
  }

  // Don't remove other test ticket data — it's been removed for clarity here.

  // Previously written tests go here

  test('Should successfully delete a ticket', () => {
    action = {
      type: 'DELETE_TICKET',
      id: 1
    };
    expect(ticketListReducer(currentState, action)).toEqual({
      2: {
        names: 'Jasmine and Justine',
        location: '2a',
        issue: 'Reducer has side effects.',
        id: 2 
      }
    });
  });

});
```

We've added a new constant that represents a `currentState` that already holds two tickets.

Our test has an `action` with a `type` of `DELETE_TICKET` and an `id` of `1`. That's all the information we need for deleting a ticket.

Once our reducer successfully completes the `DELETE_TICKET` action, we'd expect our reducer to take the `currentState` (which holds two tickets) and return an object that only has one ticket with an `id` of `2`.

If we run the test, it will fail as expected. Now let's implement the code. We can't use `filter()` as we did in the last course section. That's because in the React Fundamentals section we stored all our ticket data in an array (and we can `filter()` arrays) while now all of our ticket data is being stored in an object `{}` — and JavaScript doesn't provide native support for filtering this kind of object.

You may wonder why we are storing objects within objects in this way now — instead of continuing to use an array. Either will work fine for our use case. However, an object full of key-value pairs is much more similar to a database than an array. That's because both a database and an object have unique keys (as opposed to an array's index, which can change as values are added and removed).

Here's the code:

<div class="filename">src/reducers/ticket-list-reducer.js</div>

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
  case 'DELETE_TICKET':
    let newState = { ...state };
    delete newState[id];
    return newState;
  default:
    return state;
  }
};

export default reducer;
```

Here's the new code: 

```js
case 'DELETE_TICKET':
    let newState = { ...state };
    delete newState[id];
    return newState;
```

We start by making a copy of the state — then we use the `delete` function to remove the key-value pair that corresponds to the action. We aren't being fully "pure" here — `delete` directly alters the object it's called on. However, at the very least, we are making a copy of the original object and altering that one instead of altering the original itself.

Why not do this in a purer way? Well, JavaScript doesn't offer an approach that's both native and functional. There are functional JavaScript libraries that will handle this — for example, [the Underscore's `.omit()` function does the trick](https://underscorejs.org/#omit) — but we will keep things simple here.

Finally, we return our `newState`.

If we run our tests, they should all pass:

```javascript
 PASS  src/__tests__/reducers/ticket-list-reducer.test.js
  ticketListReducer
    ✓ Should return default state if no action type is recognized (6ms)
    ✓ Should successfully add new ticket data to mainTicketList (7ms)
    ✓ Should successfully delete a ticket (8ms)

Test Suites: 1 passed, 1 total
Tests:       3 passed, 3 total
Snapshots:   0 total
Time:        0.603s
Ran all test suites related to changed files.
```

We've successfully created a reducer that will take actions to add, update, and delete tickets — all the CRUD functionality we will need for our Help Queue's ticket list. Once again, note that all reducers must be pure functions and we aren't actually adding, updating or removing tickets from anywhere yet. The Redux store will take care of that.

In the next lesson, we will review what we've learned about reducers. After that, we'll be ready to move on to exploring Redux further.