Over the next two lessons, we will learn some simple techniques that will allow us to clean up our code. These techniques are considered Redux best practices.

In this lesson, we'll learn how to use **action creators** to simplify what an action looks like in our React applications.

Then, in the next lesson, we'll learn how to use **action constants** to replace the strings we've been using to represent action types.

### Action Creators

Action creators are little helper functions that make it easier to use actions in our React applications.

Let's look at an example. In our Help Queue, whenever we want to delete a ticket, we need to define an action like this:

```js
const action = {
  type: 'DELETE_TICKET',
  id: id
}
```

It's okay if we have to do this in our code every once in a while, but imagine if we had many places in our code where we have to delete tickets. It would be nice to DRY this code up a bit.

We can do this by writing a JavaScript function to handle the delete action:

```js
const deleteTicket = id => ({
  type: 'DELETE_TICKET',
  id
})
```

Note that we're defining a deleteTicket method, identifying id as a parameter, and implicitly returning an object literal (we wrap it in parentheses, otherwise it will return undefined). Note also that we're using a shorthand syntax to set `id: id`. We can simply declare `id` and javascript will handle the rest. 

Now, whenever we need to define a delete action in a component, we can do the following:

```js
const action = deleteTicket(id);
```

This code is cleaner and less prone to bugs.

We currently have three action types in our Help Queue application. Let's add action creators for all of them — and make sure to test our new action creators along the way.

#### Files for Action Creators

We'll create a new subdirectory in `__tests__` called `actions`. Inside the `actions` directory, create a file called `index.test.js`. All of our tests for action creators will go here.

Next, we'll create a directory in `src` called `actions`. This `actions` directory should have a file called `index.js`. The actual action creators will go in this file.

#### Testing and Writing Action Creators

We've already used the `'DELETE_TICKET'` action as an example so let's start by writing a test for this action:

<div class="filename">__tests__/actions/index.test.js</div>

```js
import * as actions from './../../actions';

describe('Help Queue actions', () => {
  it('deleteTicket should create DELETE_TICKET action', () => {
    expect(actions.deleteTicket(1)).toEqual({
      type: 'DELETE_TICKET',
      id: 1
    });
  });
});
```

Note that we will be importing all of our action creators as `actions`. As expected, this test should fail because we haven't written any actions yet.

You may be wondering why we aren't creating a ticket to be deleted. Well, we've already tested that the `'DELETE_TICKET'` action properly deletes tickets. In this test, we just need to make sure that our action creator creates the right action — the action itself doesn't need to be executed.

Now let's add our first action creator:

<div class="filename">src/actions/index.js</div>

```js
export const deleteTicket = id => ({
  type: 'DELETE_TICKET',
  id
});
```

That's all we need. Our test will now pass.

Next, we'll do a test for our `'TOGGLE_FORM'` action creator:

```js
...
  it('toggleForm should create TOGGLE_FORM action', () => {
    expect(actions.toggleForm()).toEqual({
      type: 'TOGGLE_FORM'
    });
  });
...
```

And here's the action:

<div class="filename">src/actions/index.js</div>

```js
export const toggleForm = () => ({
  type: 'TOGGLE_FORM'
});
```

It may not seem like much reduction in code but it will be easier to call `toggleForm()` in our application instead of `{ type: 'TOGGLE_FORM' }`.

Finally, let's add the test for our `'ADD_TICKET'` action:

```js
...
  it('addTicket should create ADD_TICKET action', () => {
    expect(actions.addTicket({
      names: 'Jo and Jasmine', 
      location: '3E', 
      issue: 'Redux not working!', 
      id: 1
    })).toEqual({
      type: 'ADD_TICKET',
      names: 'Jo and Jasmine',
      location: '3E',
      issue: 'Redux not working!',
      id: 1
    });
  });
...
```

Note that `addTicket()` will take the entire object and then deconstruct it in the action creator. That will save us the trouble of deconstructing the ticket every time we call `addTicket()` in our React application.

Verify this fails. Here's the code to make our test pass:

<div class="filename">src/actions/index.js</div>

```js
export const addTicket = (ticket) => {
  const { names, location, issue, id } = ticket;
  return {
    type: 'ADD_TICKET',
    names: names,
    location: location,
    issue: issue,
    id: id
  }
}
```

#### Adding Action Creators to Help Queue

Now that we've tested and written action creators for all of our action types, we are ready to add them to our Help Queue application. Since we only have one component handling state (`TicketControl.js`), we'll only need to update that file.

We'll import all of our action creators. We'll only need to update four methods total, which are shown below:

<div class="filename">src/components/TicketControl.js</div>

```js
...
import * as a from './../actions';

...

  handleClick = () => {
    if (this.state.selectedTicket != null) {
      this.setState({
        selectedTicket: null,
        editing: false
      });
    } else {
      const { dispatch } = this.props;
      const action = a.toggleForm();
      dispatch(action);
    }
  }

...

  handleAddingNewTicketToList = (newTicket) => {
    const { dispatch } = this.props;
    const action = a.addTicket(newTicket);
    dispatch(action);
    const action2 = a.toggleForm();
    dispatch(action2);
  }

  handleEditingTicketInList = (ticketToEdit) => {
    const { dispatch } = this.props;
    const action = a.addTicket(ticketToEdit);
    dispatch(action);
    this.setState({
      editing: false,
      selectedTicket: null
    });
  }

...

  handleDeletingTicket = (id) => {
    const { dispatch } = this.props;
    const action = a.deleteTicket(id);
    dispatch(action);
    this.setState({selectedTicket: null});
  }

```

We import our actions as `a` for brevity, but it's also fine to use `actions` as well. It's a common practice in the React community to use a single letter in this case, so you'll likely see this in other React code.

Our refactor cuts out a few lines of code and removes the need to spell out every action. In addition to making our code a little cleaner, it will be less prone to bugs as well.

Going forward, make sure to test and write action creators for all of your actions.