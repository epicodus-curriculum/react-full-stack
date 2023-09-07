We've built a basic plan for our Help Queue and we're ready to start coding our application. Because our site is static for now, we will only use function components. Before we begin, there's an important point to reiterate — `App.js` is the parent component for **all** other components in our application. For that reason, as we add each component to our application, we will also need to add it either to `App.js` or to its parent component. This will become clear soon.

Before we do anything else, let's add a new directory called `components` to the `src` directory of our `help-queue` project. All of our components will be added to this directory including `App.js`. Storing all components in a `components` directory is considered a best practice. However, note that `index.js` should *not* be added to our new `components` directory.

Next, we need to make a change to our `index.js` file so it knows where to find the `App` component. Currently, `index.js` thinks that `App` is in the same directory:

<div class="filename">src/index.js</div>

```js
import App from './App';
```

However, since we've moved our `App` component into a directory called `components`, we need to update that import statement:

<div class="filename">src/index.js</div>

```js
import App from './components/App';
```

We will always make this update when making a new application with `create-react-app`.

Now we're ready to create our first function component. We'll start with our header. Create a new file called `Header.js` and add it to the `components` directory. Note that `Header.js` is capitalized. It is standard naming convention to capitalize component names.

Here's our new component:

<div class="filename">src/components/Header.js</div>

```jsx
import React from "react";

function Header(){
  return (
    <h1>Help Queue</h1>
  );
}

export default Header;
```

As we can see, we've barely added anything — our header just returns a single `<h1>` tag. As always, we need to import `React` and export the component so it's available to the rest of our application. We imported and exported files in the same way when we worked with webpack in Intermediate JavaScript.

Note also that we didn't need to wrap our JSX code in a `<React.Fragment>`. This is because our component is only returning one element. If we were returning multiple elements, we'd need to use a fragment.

This component may seem too small but it really isn't. A more complex header might have more code in it, and yet even if it didn't, it makes sense to separate the header into its own component. After all, it doesn't have anything to do with tickets or a button for a form. Also, we may well need to add more to our header later and it's already nicely separated.

Next, we need to add our component to `App.js`:

<div class="filename">src/components/App.js</div>

```jsx
import React from "react";
import Header from "./Header";

function App(){
  const name = "Thato";
  const name2 = "Haley";
  return (
    <React.Fragment>
      <Header />
      <h3>3a</h3>
      <h3>{name} and {name2}</h3>
      <p><em>Firebase entries not saving!</em></p>
      <hr/>
    </React.Fragment>
  );
}

export default App;
```

We've made a few changes here:

* First, we need to import our new `Header` component.

* Next, we need to add `<Header />` as a child element of `<React.Fragment>`. We've also removed the code for the header from `App` because that code is now in the `Header` component.

Next, we're ready to create our `TicketList` component. Let's add a `TicketList.js` file to `src/components`.

Here's our new component. We'll be moving all of the ticket-related code out of `App.js` and into this component:

<div class="filename">src/components/TicketList.js</div>

```jsx
import React from "react";
import Ticket from "./Ticket";

function TicketList(){
  return (
    <Ticket />
  );
}

export default TicketList;
```

Our ticket list just needs to have a list of tickets. However, we will have a separate `Ticket` component, so for now, our `TicketList` will just hold the `Ticket` component. We will be building this out further in the next few lessons. Note that we are importing the `Ticket` component even though we haven't built it yet. The application will not compile properly if you try to run it now.

Finally, we'll add our `Ticket` component:

<div class="filename">src/components/Ticket.js</div>

```jsx
import React from "react";

function Ticket(){
  const name = "Thato";
  const name2 = "Haley";
  return (
    <React.Fragment>
      <h3>3a</h3>
      <h3>{name} and {name2}</h3>
      <p><em>Firebase entries not saving!</em></p>
      <hr/>
    </React.Fragment>
  );
}

export default Ticket;
```

We've moved most of the code that was originally in our `App` component into our `Ticket` component.

This is what our `App` component should look like after moving the ticket information into our `Ticket` component:

<div class="filename">src/components/App.js</div>

```jsx
import React from "react";
import Header from "./Header";
import TicketList from "./TicketList";

function App(){
  return ( 
    <React.Fragment>
      <Header />
      <TicketList />
    </React.Fragment>
  );
}

export default App;
```

As we can see, our `App` component is really just a container for our other components now.

It may not seem like we've done much yet, but we've successfully separated our Help Queue into multiple components. We'll add a component with a button for showing a form when we are ready to work with state.

The importance of making small, modular components may not be obvious when our application is so simple. However, it's extremely important to practice separating components like this. It's not just a best practice — it's what makes React so modular and DRY. It will also make your life much easier when you work with larger React applications.

The article [Thinking in React](https://facebook.github.io/react/docs/thinking-in-react.html), which is featured in the React documentation, outlines how professional React developers approach creating projects.

Before continuing, read the first two steps in the article above (_Break the UI Into a Component Hierarchy_ and _Build a Static Version in React_). These steps discuss how to mock-up a React application, break its UI into a component hierarchy, and build a static version. This is an essential skill to learn.