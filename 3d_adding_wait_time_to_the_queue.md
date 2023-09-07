**Note:** The next two lessons provide a walk through of implementing wait times in the queue. Most of the material reviews concepts we've learned already. You will not be expected to implement lifecycle methods outside of `constructor()` and `render()` in your independent project.

---

We are now ready to implement time elapsed functionality to Help Queue tickets. This is a sizable refactor and we'll need to complete the following steps:

* Add a constant for the `UPDATE_TIME` action type.
* Test and write a reducer method for updating elapsed time in the UI.
* Update our `ADD_TICKET` action to handle ticket properties for `timeOpen` and `formattedWaitTime`. `timeOpen` will store when a ticket was opened while `formattedWaitTime` will store a string with user-friendly elapsed wait time (such as "2 minutes ago").
* Test and write an action creator to handle the `UPDATE_TIME` action type.
* Add date-fns where needed.
* Add properties called `timeOpen` and `formattedWaitTime` to tickets.
* These properties mean that we will need to refactor other reducer actions like `ADD_TICKET`.

All of these steps may seem overwhelming at first. However, we can always break the problem-solving process down into smaller steps. In this case, it helps to have a bit of tunnel vision. In this lesson, we'll take care of testing and writing reducer actions and action creators. In the next lesson, we'll move on to updating our components to handle this new functionality.

### Adding a Constant for `UPDATE_TIME` Action Type

Because we'll want our action type to be a constant called `UPDATE_TIME`, not a string, let's start by adding a new constant to our `ActionTypes`:

<div class="filename">src/actions/ActionTypes.js</div>

```js
...
export const UPDATE_TIME = 'UPDATE_TIME';
```

### Writing a Test for `UPDATE_TIME` Reducer Action

Next, we need to decide where our  `UPDATE_TIME` reducer action will go. We could create a new reducer for it — or we could put it in our ticket list reducer, which already handles updates to tickets. Our `UPDATE_TIME` action will update the `formattedWaitTime` property on all of our tickets — so it makes sense to put it in the ticket list reducer. It would be an equally valid choice to create a new reducer, though.

As always, we'll start with a test. Because we are adding the action to our ticket list reducer, the tests for this action should go in the corresponding test file: `ticket-list-reducer.test.js`.

Here is our first test for the `UPDATE_TIME` action:

<div class="filename">__tests__/reducers/ticket-list-reducer.test.js</div>

```javascript
...
import { formatDistanceToNow } from 'date-fns';
...

describe('ticketListReducer', () => {

  ...
  
  let action;

  const ticketData = {
    names: 'Ryan & Aimen',
    location: '4b',
    issue: 'Redux action is not working correctly.',
    timeOpen : new Date(),
    formattedWaitTime: formatDistanceToNow(new Date(), {
      addSuffix: true
    }),
    id: 1
  };

  ...

  test('Should add a formatted wait time to ticket entry', () => {
    const { names, location, issue, timeOpen, id } = ticketData;
    action = {
      type: c.UPDATE_TIME,
      formattedWaitTime: '4 minutes ago',
      id: id
    };
    expect(ticketListReducer({ [id] : ticketData }, action)).toEqual({
      [id] : {
        names: names,
        location: location,
        issue: issue,
        timeOpen: timeOpen,
        id: id,
        formattedWaitTime: '4 minutes ago'
      }
    });
  });
...
```

We start by importing date-fns and its helper function `formatDistanceToNow()` because we'll want to ensure our sample ticket data for the `formattedWaitTime` property uses a date-fns-formatted time.

Then, we update the `ticketData` variable with our sample ticket data to include a `timeOpen` property which we set to `new Date()`, and a `formattedWaitTime` property that we set to a new formatted date from date-fns.

Next, our test simply verifies that the `UPDATE_TIME` action correctly adds a `formattedWaitTime` property. It's not leveraging date-fns because that isn't the simplest possible behavior we can test. For now, we just want to see that `UPDATE_TIME` can update that property.

We destructure the `ticketData` values, including `timeOpen`. Our `UPDATE_TIME` action needs to have an `id` and a `formattedWaitTime` in its payload. The `id` is necessary to determine which ticket should be updated. The `formattedWaitTime` will hold a date-fns-formatted time.

To properly test that a ticket is being updated to include the `formattedWaitTime`, we need to start with an initial state that already has a ticket. That's why we `expect(ticketListReducer({ [id] : ticketData }, action))` to be equal to the `ticketData` provided in our expect statement's initial state, but with one key difference — the included ticket should now have the `formattedWaitTime` property with a value of `'4 minutes ago'`.

### Adding Reducer Logic for `UPDATE_TIME`

Next, we'll verify that our new test fails. Then we're ready to add logic to make it pass. We'll add an `UPDATE_TIME` `case` to our existing ticket list reducer:

<div class="filename">src/reducers/ticket-list-reducer.js</div>

```js
import * as c from './../actions/ActionTypes';

const reducer = (state = {}, action) => {
  const { names, location, issue, id, formattedWaitTime, timeOpen } = action;
  switch (action.type) {
  case c.ADD_TICKET:
    ...
  case c.DELETE_TICKET:
    ...
  case c.UPDATE_TIME:
    const newTicket = Object.assign({}, state[id], {formattedWaitTime});
    const updatedState = Object.assign({}, state, {
      [id]: newTicket
    });
    return updatedState;
  default:
    return state;
  }
};

export default reducer;
```

Let's take a look at our new `UPDATE_TIME` action:

* First of all, when we deconstruct `action`, we now need to extract `timeOpen` and `formattedWaitTime`.

* Then, within `UPDATE_TIME`, we use `Object.assign()` to grab the ticket that needs to be updated (we use `state[id]` to do this to get the specific ticket from the list of tickets). `Object.assign()` makes a copy of this ticket and then adds the `formattedWaitTime` to it. (Note that `{formattedWaitTime}` is an object with the `formattedWaitTime` key-value pair in it.)

* Then we use `Object.assign()` again — this time to make a copy of the entire ticket list. The `updatedTicket` will be added to this copy of the ticket list. Since the `updatedTicket`'s id already exists in the copy of the ticket list, the old ticket will be replaced with the updated ticket.

* Finally, we return the updated state.

If we run our tests, they will all pass.

This is really all we need for now — our `UPDATE_TIME` action just needs to change the `formattedWaitTime` of a ticket. We won't use our reducer to handle date-fns. Remember that reducers are pure functions — the same input should always return the same output. However, if our reducer handled determining the time a ticket is put in, the computed `formattedTimeValue` of one inputted ticket could be completely different from that of another inputted ticket — which is not pure at all — and quite difficult to test.

### Updating and Testing the `ADD_TICKET` Action

Astute observers may have noted that our `ADD_TICKET` action doesn't handle `timeOpen` or `formattedWaitTime`. If these properties aren't added to tickets when they are created, our application won't work properly.

Let's update our `ADD_TICKET` test now:

<div class="filename">__tests__/reducers/ticket-list-reducer.test.js</div>

```js
...

test('should successfully add a ticket to the ticket list that includes date-fns-formatted wait times', () => {
    const { names, location, issue, timeOpen, formattedWaitTime, id } = ticketData;
    action = {
      type: c.ADD_TICKET,
      names: names,
      location: location,
      issue: issue,
      timeOpen: timeOpen,
      formattedWaitTime: formattedWaitTime,
      id: id
    };
    expect(ticketListReducer({}, action)).toEqual({
      [id] : {
        names: names,
        location: location,
        issue: issue,
        timeOpen: timeOpen,
        formattedWaitTime: 'less than a minute ago',
        id: id
      }
    });
  });
```

Here we modify our `ADD_TICKET` test to include the `timeOpen` and `formattedWaitTime` properties. We've also altered the description of the test a bit to make it clearer what exactly we are testing.

This test will fail as expected. We'll only need to make a small tweak to our `ADD_TICKET` action to get the test passing again:

<div class="filename">src/reducers/ticket-list-reducer.js</div>

```js
...

const reducer = (state = {}, action) => {
  const { names, location, issue, id, formattedWaitTime, timeOpen } = action;
  switch (action.type) {
  case c.ADD_TICKET:
    return Object.assign({}, state, {
      [id]: {
        names: names,
        location: location,
        issue: issue,
        id: id,
        timeOpen: timeOpen,
        formattedWaitTime: formattedWaitTime
      }
    });
  ...
  }
};

export default reducer;
```

* Once again, when we deconstruct `action`, we now also need to extract `timeOpen` and `formattedWaitTime`.

* A ticket now needs to include `timeOpen` and `formattedWaitTime` properties.

Our test should now pass!

### Adding an Action Creator for `UPDATE_TIME`

We'll also want to add (and test) an action creator for the `UPDATE_TIME` action.

Here's the test:

<div class="filename">__tests__/actions/index.test.js</div>

```js
...
  it('updateTime should create UPDATE_TIME action', () => {
    expect(actions.updateTime(1, 'less than a minute ago')).toEqual({
      type: c.UPDATE_TIME,
      id: 1,
      formattedWaitTime: 'less than a minute ago'
    });
  });
...
```

The `updateTime()` action creator has two parameters. The first is a ticket's id while the second is the formatted wait time that should be passed into that ticket. This should be equivalent to the `UPDATE_TIME` action type, which takes in properties for an `id` and a `formattedWaitTime`.

Now for the code to get our test passing:

<div class="filename">src/actions/index.js</div>

```js
export const updateTime = (id, formattedWaitTime) => ({
  type: c.UPDATE_TIME,
  id: id,
  formattedWaitTime: formattedWaitTime
});
```

At this point, we've tested and written code for our new `UPDATE_TIME` action, updated and tested code for our `ADD_TICKET` action, and tested and written code for an `updateTime()` action creator. We're ready to move on to updating our components, right?

Not quite. Before we move on, there's a gotcha that could come back to bite us if we don't deal with it now. What about the action creator for our `ADD_TICKET` action? It doesn't deal with `timeOpen` or `formattedWaitTime` yet. If we try to add or update a ticket via that action creator, it won't even acknowledge those properties.

So we need to update the test for that action creator first: 

<div class="filename">__tests__/actions/index.test.js</div>

```js
...
it('addTicket should create ADD_TICKET action', () => {
    expect(actions.addTicket({
      names: 'Jo and Jasmine', 
      location: '3E', 
      issue: 'Redux not working!', 
      timeOpen: 0,
      formattedWaitTime: 'less than a minute ago', 
      id: 1
    })).toEqual({
      type: c.ADD_TICKET,
      names: 'Jo and Jasmine',
      location: '3E',
      issue: 'Redux not working!',
      timeOpen: 0,
      formattedWaitTime: 'less than a minute ago',
      id: 1
    });
  });
...
```

The only changes we've made here is to add the `timeOpen` and `formattedWaitTime` properties to both the left and right sides of our `expect` statement. 

Notice that `timeOpen` and `formattedWaitTime` are each set  to fake data. Well, we can do this here, since we already tested that `timeOpen` and `formattedWaitTime` are properly set to a new date and a new date formatted by date-fns when we tested the `ADD_TICKET` action in our ticket-list-reducer. With our `addTicket()` action creator, our only goal is to make sure that our action creator properly includes these two properties as it creates our action. So, fake data works well here.

To make it pass, we just need to make a few tweaks to our `addTicket()` action creator:

<div class="filename">src/actions/index.js</div>

```js
...
export const addTicket = (ticket) => {
  const { names, location, issue, id, formattedWaitTime, timeOpen } = ticket;
  return {
    type: c.ADD_TICKET,
    names: names,
    location: location,
    issue: issue,
    id: id,
    formattedWaitTime,
    timeOpen: timeOpen
  }
}
...
```

We need to deconstruct a few more properties (`formattedWaitTime` and `timeOpen`) and then make sure those two properties are included in the `ADD_TICKET` action.

Now we are finally ready to move on to updating our components and UI. We'll cover that in the next lesson.