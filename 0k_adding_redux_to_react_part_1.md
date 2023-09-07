Here at Epicodus, we usually build a project from the ground up when we learn about a new technology. We build new projects every class session or every few class sessions in order to reinforce key concepts.

However, this isn't typical in the workplace. It's very unlikely you'll go into a new job and start a project from scratch. Instead, you'll be working on a larger codebase with other developers.

In this scenario, imagine that some developers are already working on a Help Queue application for a code school. You've been asked to incorporate Redux into the project.

In this lesson, that's exactly what we'll start doing. We'll work on refactoring our Help Queue application to use Redux.

Then, in the next lesson, we'll review and go through the steps we need to take to add Redux to a new project.

Let's return to the Help Queue repo we've been working on. We'll start by adding Redux and React Redux bindings to our project:

```javascript
$ npm install redux@4.2.0 react-redux@8.0.2
```

If you've been working along with the weekend homework up to this point, you should've added the following code to the Help Queue project that we started in the React Fundamentals course section. If for some reason you want to start fresh from here, this is the starter repo that contains all the code from the last course section:

---
**[<i class="glyphicon glyphicon-folder-open"></i>  Starter Project for Help Queue](https://github.com/epicodus-lessons/react-help-queue-starter-project)**

You'll need to add the following code so the project is up to date with the work we've done so far this weekend. If you're already following along and your code is up to date, you can skip to the next lesson now.

* You should have a `src/reducers` directory with the following file:

<div class="filename">src/reducers/ticket-list-reducer.js</div>

```js
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
    const newState = { ...state };
    delete newState[id];
    return newState;
  default:
    return state;
  }
};

export default reducer;
```

This is the reducer that we've already written for the Help Queue — no surprises here.

* Create a `src/__tests__/reducers` directory with the following file:

<div class="filename">src/__tests__/reducers/ticket-list-reducer.test.js</div>

```js
import ticketListReducer from '../../reducers/ticket-list-reducer';

describe('ticketListReducer', () => {

  let action;

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

  const ticketData = {
    names: 'Ryan & Aimen',
    location: '4b',
    issue: 'Redux action is not working correctly.',
    id: 1
  };

  test('Should return default state if no action type is recognized', () => {
    expect(ticketListReducer({}, { type: null })).toEqual({});
  });

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

These are the tests we've already written for our reducer.

At this point, we are ready to actually incorporate Redux into our Help Queue.
