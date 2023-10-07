In this lesson, we'll build a small application that will use `fetch()` to make an API call to the New York Times (NYT). This example is very similar to the one that the [React documentation](https://reactjs.org/docs/faq-ajax.html) provides, but with a few modifications to follow best practices. This example will also be reminiscent of the setup in the React with NoSQL course section when we fetched data from a Firestore database.

## Making an API Call with `fetch()`
---

Let's start out by bootstrapping a new React project with create-react-app. In your desktop or folder of choice, run the following command:

```
$ npx create-react-app react-with-api
```

Next, let's get access to a New York Times developer API key. If you already have a New York Times API key from a previous Epicodus project, you are welcome to use that one. If you don't, go to [Get Started](https://developer.nytimes.com/get-started) at the NYT's Developer site and complete the steps to getting an API key. Their documentation is excellent.

Within the `react-with-api` project, let's add `.env` to the `.gitignore` file. Once you've made a commit, go ahead and create a `.env` file in the root directory of the project.

Finally, add the following to the `.env` file. Don't forget that all environmental variables need to have `REACT_APP` as a prefix or they won't work.

<div class="filename">.env</div>

```
REACT_APP_API_KEY=[Your NYT API key goes here]
```

### Creating the Components

Our application will have two components: `App.js` and `TopStories.js`. The `TopStories` component will contain information from the [NYT's Top Stories API](https://developer.nytimes.com/docs/top-stories-product/1/overview). We could only use `App.js` to make our API call, but we're separating it into `TopStories` in case you want to build this project out further on your own by adding on other API calls, functionality like searching for articles or listing what's most popular, books, or movies. 

We'll add both `App.js` and `TopStories.js` to a `components/` folder. Their file paths will look like this:

* `react-with-api/src/components/App.js`
* `react-with-api/src/copmonents/TopStories.js`

That means we'll need to update the import for `App.js` in `index.js` to the following:

<div class="filename">src/index.js</div>

```js
...
import App from './components/App';

...
```

Here's the code that we'll add to `App.js`:

<div class="filename">components/App.js</div>

```js
import React from 'react';
import './App.css';
import TopStories from './TopStories';

function App() {
  return (
    <React.Fragment>
      < TopStories />
    </React.Fragment>
  );
}

export default App;
```

It's just a container for a `TopStories` component that we haven't created yet. The `TopStories` component will have the bulk of our code. 

### Adding State

We'll continue to use hooks in function components to manage state and use component lifecycle features. Let's start by adding some local state:

<div class="filename">components/TopStories.js</div>

```js
import React, { useState } from 'react';

function TopStories() {
  const [error, setError] = useState(null);
  const [isLoaded, setIsLoaded] = useState(false);
  const [topStories, setTopStories] = useState([]);
}

export default TopStories;
```

We've created three state variables: `error`, `isLoaded`, and `topStories`:

* The initial state of the `error` property is `null`. If the API call has an error, this property will be changed.
* The initial state of `isLoaded` is `false`. We'll use the `useEffect()` hook to make the API call once, after our component first renders. Once the call is complete, `isLoaded` will be switched to `true`.
* Finally, `topStories` is an empty array. It will contain the NYT top stories in JSON format. Once the API call is made, we will update `topStories` to hold this information.

### Making the API Call

Next, let's set up a `useEffect()` hook to make the API call and update our state:

<div class="filename">components/TopStories.js</div>

```js
import React, { useState, useEffect } from 'react';

function TopStories () {
  const [error, setError] = useState(null);
  const [isLoaded, setIsLoaded] = useState(false);
  const [topStories, setTopStories] = useState([]);

  useEffect(() => {
    fetch(`https://api.nytimes.com/svc/topstories/v2/home.json?api-key=${process.env.REACT_APP_API_KEY}`)
      .then(response => response.json())
      .then((jsonifiedResponse) => {
          setTopStories(jsonifiedResponse.results)
          setIsLoaded(true)
        })
      .catch((error) => {
        setError(error)
        setIsLoaded(true)
      });
    }, [])
}

export default TopStories;
```

Since we've passed in an empty array dependency, this `useEffect()` hook will run once, after our component is first rendered.

We use the built-in `fetch()` API (a Web API, not React) to make our API call. The URL for the API call is in backticks so that we can use a template string for our API key, which is stored in an environmental variable.

Once the API call is complete, the response will be converted to JSON. Then, once it's been converted, we'll have our results. If all went well, we call `setTopStories()` with the top stories data and set `isLoaded` to `true` by calling `setIsLoaded(true)`. Because the topStories are stored in a property of the response called `results`, we update the `topStories` state to be `jsonifiedResponse.results`.

If an error happens, we set `isLoaded` to true and set the `error` property to the error that the API call returns.

As noted in the [Intermediate JavaScript lesson on `fetch()`](/intermediate-javascript/asynchrony-and-apis-part-2/fetch-api), a catch block with `fetch()` will only catch internal server errors — not issues like a 404 error code. Because of this, we'll need to add in additional error checking. 

### Adding Additional Error Handling

We can check for 400 and 500 level issues by using the `fetch()` API's `ok` property, or by using an API's custom error responses. We could also use a combination of the two! As always, take time to research error handling on an API's documentation, and try breaking your API call to see what error messages are returned. 

We can break an API call by doing any of the following:

* Removing a character from the API key.
* Changing the request URL.
* Putting in phony input, if the API call includes query parameters.

It turns out the NYT's documentation on error messages is non-existent. However, if we break the API call in Postman, we can see that the NYT API does in fact return custom error messages stored in a `fault` key. To keep things simple for this lesson, we'll stick to using the `fetch()` API's `ok` property to provide additional error handling. 

Here's our updated code.

<div class="filename">components/TopStories.js</div>

```js
import React, { useState, useEffect } from 'react';

function TopStories () {
  const [error, setError] = useState(null);
  const [isLoaded, setIsLoaded] = useState(false);
  const [topStories, setTopStories] = useState([]);

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
          setTopStories(jsonifiedResponse.results)
          setIsLoaded(true)
        })
      .catch((error) => {
        setError(error.message)
        setIsLoaded(true)
      });
    }, [])
}

export default TopStories;
```

### Returning UI

Finally, we need to create a `return` statement. In this case we'll use conditionals to determine which of the following our users will see:

* A "...Loading..." message
* The daily top stories
* An error

Here's the conditional with three possible returns:

<div class="filename">components/TopStories.js</div>

```js
import React, { useEffect, useState } from 'react';

function TopStories () {
  ...

  ...

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

If there's an `error`, we'll return an error message. As long as the error's default state of `null` isn't changed, this conditional won't be triggered.

If `isLoaded` is `false`, we'll render a "...Loading..." message.

Otherwise, we'll return the top stories. Note that we are using the `index` as a key for each list item just as we did in the React Fundamentals course section. In a more complex application, there could be valid reasons to give each story object in the `topStories` array their own IDs (to then use as a key), but in this particular use case, we are doing the basics needed for React to complete the reconciliation process.

While each story object has many properties we can display, we're listing only the `title` and `abstract` properties.

We can do a simple implementation of an API call in this manner — and it works great. But what if our state gets more complicated? Let's say we not only get the top stories data from the NYT home page, but from the [arts, science, and technology](https://developer.nytimes.com/docs/top-stories-product/1/overview) sections as well? In this case, it's better to reach for another tool to manage complex state: the `useReducer()` hook. In the next lesson, we'll get to know how this hook works. Then, we'll refactor our NYT API call application to use it!