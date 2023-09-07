In this lesson, we'll incorporate the React Router library and create a "separate" sign-in page that uses client-side routing. In the next lesson, we'll actually incorporate Firebase authentication.

[React Router](https://reactrouter.com/) is an external library that makes routing much easier in React applications. First, we'll install the following version of React Router:

```
$ npm install react-router-dom@6
```

React Router provides a number of tools that will make it easy for us to add client-side routing. In this lesson, we'll learn about the following:

* A router component called `<BrowserRouter>`. We'll add this to our root component (`<App>`). It will give us access to other pieces of React Router functionality.
* A component called `<Routes>`. Whenever we want to delineate between multiple routes, we will wrap a `<Routes>` component around them.
* A `<Route>` component that defines the component the application should route to as well as the path that the route corresponds to.
* Finally, a `<Link>` component will provide actual links to client-side routes in our application.

We will just cover this part of React Router's functionality in this lesson, but we recommend checking out the excellent [React Router documentation](https://reactrouter.com/docs/en/v6) as well.

We'll start by adding a `SignIn`  component with placeholder text:

<div class="filename">src/components/SignIn.js</div>

```js
import React from "react";

function SignIn(){  
  return (
    <h1>Sign In</h1>
  );
}

export default SignIn
```

Eventually, this component will use Firebase authentication. For now, though, we are just focused on setting up a router that will route between our application and the `SignIn` component.

## Adding a Router
---

Next, we need to make some changes to our `App` component. Because it's the root level component, it's the best place to put our router so that the rest of our application has access to its functionality. Also, we'll want to route between the `SignIn` component and the `TicketControl` component anyway, so most of our router functionality will be in `App`, which is just above `TicketControl` in our component tree.

Here's our updated `App` component:

<div class="filename">src/components/App.js</div>

```js
...
import SignIn from "./SignIn";
import { BrowserRouter as Router, Routes, Route } from "react-router-dom";

function App(){
  return ( 
    <Router>
      <Header />
      <Routes>
        <Route path="/sign-in" element={<SignIn />} />
        <Route path="/" element={<TicketControl />} />
      </Routes>
    </Router>
  );
}

...
```

In addition to importing our placeholder `SignIn` component, we also add the following imports:

<div class="filename">src/components/App.js</div>

```js
import { BrowserRouter as Router, Routes, Route } from "react-router-dom";
```

Notice the `as` syntax in `BrowserRouter as Router`. This just makes the naming a little easier — we can call the component `<Router>` instead of `<BrowserRouter>` and save a little typing. We don't add the `<Link>` component because we won't actually have links in this component.

Next, we wrap all the content in our return statement inside a `<Router>` component. Now all of `App`'s children will have this functionality as well.

Our `Header` component should show regardless so that comes next — and is outside of the `<Routes>` component where our application's routing will be determined.

Think of the `<Routes>` component as being like a conditional — it will render only one of the routes contained inside the `<Routes>` component. (It's also possible that a route won't be rendered at all if no URL matches it.) If we don't include the `<Routes>` component, multiple routes could be rendered. Sometimes we might actually want that, but in the case of our application, we only want the sign in page _or_ the queue to be rendered.

Next, we need to determine our actual routes using `<Route>` components. Let's look more closely at the code contained in our `<Route>` components:

<div class="filename">src/components/App.js</div>

```js
<Route path="/sign-in" element={<SignIn />} />
<Route path="/" element={<TicketControl />} />
```

We always need to specify the route's `path`; otherwise, how will we ever be able to route to it via a URL? The `path` should _always_ begin with a `/` (just like an actual path in a URL). Otherwise, there will be errors. The name of the path doesn't need to match the name of the component, though for clarity and naming purposes, it often will. 

For each `<Route>` component that we create, we need to pass in an `element` prop that's set to the component that we want rendered for the corresponding path. This completes the functionality of the `<Route>` component: when the `<Route>`'s path matches the URL, its `element` will be rendered.

We can now run our application and navigate to our sign in route manually by typing in `localhost:3000/sign-in`.

However, we don't want our users to have to type in the path manually each time they want to go to the sign in page. The next step is to add links in our header that will allow users to navigate between different routes.

## Adding Links to Routes
---

Many applications have a navbar at the top of the page with links to various parts of the site. This navbar will show regardless of which page we're on. We already have a `Header` component that renders at the top of our site regardless of the page's content. At this point, it only has an `h` tag. Let's update this component to be more useful:

<div class="filename">src/components/Header.js</div>

```js
...
import { Link } from "react-router-dom";

function Header(){
  return (
    <React.Fragment>
      <h1> Help Queue</h1>
      <ul>
        <li>
          <Link to="/">Home</Link>
        </li>
        <li>
          <Link to="/sign-in">Sign In</Link>
        </li>
      </ul>
    </React.Fragment>
  );
}

...
```

First, we need to `import { Link } from "react-router-dom;`. This feature provides a `<Link>` component which we can use to create links to routes in our sites. React Router will automatically render these as `href`s on the page.

The syntax looks like this:

```js
<Link to="/sign-in">Sign In</Link>
```

In the example above, the `to` property **must** match the route we specified in the `<Route>` component for our `SignIn` component:

```js
<Route path="/sign-in" element={<SignIn />} />
```

Fortunately, if there's a typo, it's easy to troubleshoot. If a route isn't rendering as expected in the browser, start by double-checking the browser's URL.

At this point, we can run our application and then click on the links in the header to toggle between the Help Queue and the sign in page.

We're now ready to add authentication to our application!

For more information on using React Router, check out the [React Router documentation](https://reactrouter.com/docs/en/v6).