Now it's time to refactor the `TopStories` component to use the `useReducer()` hook. Let's first take a look at the updated code, and then review the changes in detail down below.

If you want to take on a challenge, try refactoring the `TopStories` component to use the `useReducer()` hook by yourself before looking at the updated code below.

<div class="filename">src/components/TopStories.js</div>

```js
// We remove the useState hook and add the useReducer hook.
import React, { useEffect, useReducer } from 'react';
import topStoriesReducer from './../reducers/top-stories-reducer';
// We import our action creators.
import { getTopStoriesFailure, getTopStoriesSuccess } from './../actions/index';

// We create initial state for the useReducer hook.
const initialState = {
  isLoaded: false,
  topStories: [],
  error: null
};

function TopStories () {
  // We initialize the useReducer hook.
  const [state, dispatch] = useReducer(topStoriesReducer, initialState);

  useEffect(() => {
    fetch(`https://api.nytimes.com/svc/topstories/v2/home.json?api-key=${process.env.REACT_APP_API_KEY}`)
      .then(response => {
        if (!response.ok) {
          throw new Error(`${response.status}: ${response.statusText}`);
        } else {
          return response.json()
        }
      })
      .then((jsonifiedResponse) => {
        // We create an action and then dispatch it.
        const action = getTopStoriesSuccess(jsonifiedResponse.results)
        dispatch(action);
      })
      .catch((error) => {
        // We create an action and then dispatch it. 
        const action = getTopStoriesFailure(error.message)
        dispatch(action);
      });
    }, [])
  
  // we destructure error, isLoaded, and topStories from the state variable.
  const { error, isLoaded, topStories } = state;

  if (error) {
    return <h1>Error: {error}</h1>;
  } else if (!isLoaded) {
    return <h1>...Loading...</h1>;
  } else {
    return (
      <React.Fragment>
        <h1>Top Stories</h1>
        <ul>
          {topStories.map((article, index) =>
            <li key={index}>
              <h3>{article.title}</h3>
              <p>{article.abstract}</p>
            </li>
          )}
        </ul>
      </React.Fragment>
    );
  }
}

export default TopStories;
```

Let's go over the changes we've made to `TopStories.js`. Most of this should be familiar from what we've learned about the `useReducer()` hook in the `Counter` example. 

* First, we need to import `useReducer` from React. We'll also need to remove the `useState` import since we're no longer using it.
* Next, we need to import the two action creators that we'll use to generate our actions: `getTopStoriesFailure` and `getTopStoriesSuccess`. 
* Next, outside of the `TopStories` component, we create our initial state that will be added as an argument to the `useReducer()` hook. We could instead put the initial state in a separate file, or even in `top-stories-reducer.js`, and import it into `TopStories.js`. Whatever you do, you should be consistent across your entire application.
* We remove the three `useState()` statements and replace them with a `useReducer()` hook, passing in the `initialState` and `topStoriesReducer`. We save the state and dispatch function that's returned from the `useReducer()` hook in the variables `state` and `dispatch`; remember that we can call these variables anything so long as it is descriptive of what it represents.
* Later in the `useEffect()` hook and API call logic, when we receive a successful response or a failure response, we use either of our two action creator functions to generate an action, and then we dispatch that action using the `dispatch()` function.
* Finally, the last step in this refactor is to destructure the `error`, `isLoaded`, and `topStories` variables from the `state` variable. 

Now we can run our application and everything will be working correctly. In other words, our API application is complete! We now have the tools to make API calls with `fetch()` in React applications with both the `useState()` and `useReducer()` hooks. 

## Next Steps
---

As you build React apps, we encourage you to continue to use hooks to manage state and avoid class components. That includes whether you use hooks in conjunction with a state library like Redux, or standalone. If you would like to learn how to make an API call in a class component, [visit the React docs.](https://reactjs.org/docs/faq-ajax.html)

Also, keep in mind that there are many, many custom React hooks out there. [Check out this excellent list of resources on hooks](https://github.com/rehooks/awesome-react-hooks), which includes tutorials and links to various custom hooks. Among other things, these custom hooks offer the following additional functionality to function components:

* Handling form state
* Using the Fetch API
* Drawing SVGs
* Handling media queries

---
**[<i class="glyphicon glyphicon-folder-open"></i>  API Project with React and the `useReducer()` Hook](https://github.com/epicodus-lessons/react-with-api-and-useReducer)**

