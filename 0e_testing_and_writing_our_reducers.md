We're now ready to start refactoring our New York Times (NYT) API application to use the `useReducer()` hook to handle state. However, we're going to go a few steps further than we did in the last lesson: we're going to write action creators and action constants for our actions, and we're going to fully test our reducer and action creators. 

While we don't have to use action creators or action constants with `useReducer()`, it's good to take the time to practice testing. Remember that reducers, action constants, and action creators are just pure JavaScript functions.

## Project Planning and Setup
---

Open up your NYT API app, and all the following directories to `src`:

* `__tests__`
* `reducers`
* `actions`

Next, add the following directories to `src/__tests__`:

* `reducers`
* `actions`

Now we're ready to start planning our application state — and how our reducers will update it.

### Planning Our Initial State

When we use the `useState()` hook to manage the state related to our API call, we have three variables:

* `isLoaded`, initialized to `false`
* `topStories`, initialize to an empty array
* `error`, initialized to `null`

The question we need to answer is whether we should create one reducer to manage all of this state, or separate this state into multiple reducers, or even leave some of the state to be managed by a `useState()` hook. What do you think we should do?

Well, we know that the values of `isLoaded`, `topStories`, and `error` are set based on the success or failure of the API call. This is a good indication that these state variables are all related and it is best that we manage them within the same `useReducer()` hook. So, we'll do just that.

Here's what our initial state will look like:

```js
{
  isLoaded: false,
  topStories: [],
  error: null
}
```

### Planning our Actions

We'll need to have two actions, one for the success of the API call and another for a failure:

* `'GET_TOP_STORIES_SUCCESS'`: This action will be dispatched when we receive a response for a successful API call. It will set `isLoaded` to `true` and will include a `topStories` property with the API response's payload.
* `'GET_TOP_STORIES_FAILURE'`: This action will be dispatched when we receive a response from a failed API call. It will set `isLoaded` to `true` and will include an `error` property the API response's error message.

### Add Constants for Reducer Actions

Before we go any further, let's create constants for our actions, just as we did in the React with Redux course section:

<div class="filename">src/actions/ActionTypes.js</div>

```js
export const GET_TOP_STORIES_FAILURE='GET_TOP_STORIES_FAILURE'
export const GET_TOP_STORIES_SUCCESS='GET_TOP_STORIES_SUCCESS'
```

## Testing 
---

Now that we have everything set up, we can start testing.

### Testing and Writing Our Reducer's Initial State

For our first test, our reducer should just return the unchanged state if no action is specified.

Here's our test:

<div class="filename">src/__tests__/reducers/top-stories-reducer.test.js</div>

```js
import topStoriesReducer from '../../reducers/top-stories-reducer';

describe('topStoriesReducer', () => {

  const initialState = {
    isLoaded: false,
    topStories: [],
    error: null
  };

  test('should successfully throw a new error if a non-matching action type is passed into it', () => {
    expect(
      () => {
        topStoriesReducer(initialState, {type: null })
      }
    ).toThrowError("There is no action matching null.");
  });
});
```

We start by importing our reducer (which we haven't created yet — we'll do that in a moment). Then we store the `initialState` in a constant in our `describe` block. Finally, our test verifies that if no action type is specified, a new error is thrown with the message `"There is no action matching null."`. 

Next, we need to create our reducer with a switch and a default case:

<div class="filename">src/reducers/top-stories-reducer.js</div>

```js
const topStoriesReducer = (state, action) => {
  switch (action.type) {
    default:
      throw new Error(`There is no action matching ${action.type}.`);
  }
};

export default topStoriesReducer;
```

For now, our reducer throws an error for the default case, just like we tested for. If we run our tests, they will pass.

### Testing and Writing `GET_TOP_STORIES_SUCCESS`

Now we're ready to write a test for our `GET_TOP_STORIES_SUCCESS` action. This action will be triggered if our API call is successful. 

Here's the test:

<div class="filename">src/__tests__/reducers/top-stories-reducer.test.js</div>

```js
import * as c from './../../actions/ActionTypes';

describe('topStoriesReducer', () => {

  let action; // Don't forget to declare action as a variable.

  ... // previous initialState variable.

  test('successfully getting top stories should change isLoaded to true and update topStories', () => {
    const topStories = "An article";
    action = {
      type: c.GET_TOP_STORIES_SUCCESS,
      topStories
    };

    expect(topStoriesReducer(initialState, action)).toEqual({
        isLoaded: true,
        topStories: "An article",
        error: null
    });
  });
});
```

First, we need to make sure we import our constants from `ActionTypes.js` and create an `action` variable that we can reuse throughout the tests.

Note that we've created a constant called `topStories` which is storing a string. Our reducer doesn't care what the payload will look like — for the purposes of our test, we just want to make sure our new action will update the `topStories` property correctly.

Our test will verify that when the `GET_TOP_STORIES_SUCCESS` action is triggered, `isLoaded` will be set to `true` and the `topStories` property will be updated to the payload (in this case, a string).

Once we make sure the test fails, we can update our reducer to make it pass:

<div class="filename">src/reducers/top-stories-reducer</div>

```js
import * as c from '../actions/ActionTypes';

const topStoriesReducer = (state, action) => {
  switch (action.type) {
    case c.GET_TOP_STORIES_SUCCESS:
      return {
        ...state, 
        isLoaded: true,
        topStories: action.topStories
      };
    default:
      throw new Error(`There is no action matching ${action.type}.`);
  }
};

export default topStoriesReducer;
```

Our new action returns a new state object: we use JavaScript's spread syntax to make a copy of the `state` object, and we specify that `isLoaded` is set to `true` and the `topStories` property is set to `action.topStories` — the payload we've passed into our action.

If we run our tests, our latest test will pass.

### Testing and Writing `GET_TOP_STORIES_FAILURE`

Next we'll test and write the second action — `GET_TOP_STORIES_FAILURE`. Both the test and the reducer action will look very similar to `GET_TOP_STORIES_SUCCESS`. Here's the test:

<div class="filename">src/__tests__/reducers/top-stories-reducer.test.js</div>

```js
...
  test('failing to get topStories should change isLoaded to true and add an error message', () => {
    const error = "An error";
    action = {
      type: c.GET_TOP_STORIES_FAILURE,
      error
    };

    expect(topStoriesReducer(initialState, action)).toEqual({
        isLoaded: true,
        topStories: [],
        error: "An error"
    });
  });
...
```

We create an `error` constant that holds a string. The action itself looks very similar to `GET_TOP_STORIES_SUCCESS` — the only difference is the payload. We'll expect the new state to have `isLoaded` set to `true` and `error` set to `"An error"`. Meanwhile, `topStories` will remain an empty array since it won't change if we don't get a successful payload.

Verify that the test fails. Then, we can update our reducer:

<div class="filename">src/reducers/top-stories-reducer</div>

```js
import * as c from '../actions/ActionTypes';

const topStoriesReducer = (state, action) => {
  switch (action.type) {
    case c.GET_TOP_STORIES_SUCCESS:
      return {
        ...state, 
        isLoaded: true,
        topStories: action.topStories
      };
    case c.GET_TOP_STORIES_FAILURE:
      return {
        ...state,
        isLoaded: true,
        error: action.error
      };
    default:
      throw new Error(`There is no action matching ${action.type}.`);
  }
};

export default topStoriesReducer;
```

As we can see, the actions for success and failure are very similar — they just have different payloads.

At this point, our reducer is complete.

### Testing and Writing Action Creators

Next, we'll write action creators for our reducer actions. We'll also test these action creators. Since this is a review of something we've learned how to do previously, we will run through this quickly. 

Here are the tests:

<div class="filename">src/__tests__/actions/index.test.js</div>

```js
import * as actions from './../../actions';
import * as c from './../../actions/ActionTypes';

describe('top stories reducer actions', () => {
  it('getTopStoriesSuccess should create GET_TOP_STORIES_SUCCESS action', () => {
    const topStories = "An article";
    expect(actions.getTopStoriesSuccess(topStories)).toEqual({
      type: c.GET_TOP_STORIES_SUCCESS,
      topStories
    });
  });

  it('getTopStoriesFailure should create GET_TOP_STORIES_FAILURE action', () => {
    const error = "An error";
    expect(actions.getTopStoriesFailure(error)).toEqual({
      type: c.GET_TOP_STORIES_FAILURE,
      error
    });
  });
});
```

These tests just verify that the JavaScript functions we'll create to generate our reducer actions actually do so successfully.

Here are the functions to make our new tests pass:

<div class="filename">src/actions/index.js</div>

```js
import * as c from './ActionTypes';

export const getTopStoriesSuccess = (topStories) => ({
  type: c.GET_TOP_STORIES_SUCCESS,
  topStories
});

export const getTopStoriesFailure = (error) => ({
  type: c.GET_TOP_STORIES_FAILURE,
  error
});
```

Note that we export each action creator separately.

## Summary
---

At this point, we've planned out the initial state of our reducer and how our reducer will change it. We created constants for each of our reducer actions and then used test-driven development to  create a reducer that will update state when we make an API call. Finally, we tested and wrote action creators that will make it easier to dispatch our actions in our application.

However, we still haven't refactored our application to use the `useReducer()` hook! Let's do that next and wrap up this practice project.
