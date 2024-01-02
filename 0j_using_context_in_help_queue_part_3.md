In this lesson, we'll complete the functionality of a light and dark theme in the Help Queue. In the process, we'll learn how to use context consumers in class and function components. 

We'll also discuss the optional default value that we can add to context objects, and how we can leverage it to create error handling for our context consumers.

## Adding Context Consumers
---

We need to add context consumers to the following components so we can set the background and text color of the buttons and inputs to match the current theme:

* `ToggleTheme`: one `<button>` element.
* `TicketControl`: one `<button>` element.
* `TicketDetail.js`: two `<button>` elements.
* `ReusableForm.js`: one `<button>` element, two `<input>` elements, and one `<textarea>` input.

In the process of adding context consumers, we'll learn three different ways to consumer context data:

1. Class components: setting the class's `contextType` property to `ThemeContext`.
2. Function components: using the `<ThemeContext.Consumer>` component.
3. Function components: using the `useContext()` hook, as in `const theme = useContext(ThemeContext)`.

The trend in React development is to favor function components over class components, so the `useContext()` hook will be the most popular way to consume context data. However, it's important to know how to consume context data without hooks, because most applications still use class components, and many applications use versions of React earlier than 16.8, the React version when hooks were introduced!  

### Using `<ThemeContext.Consumer>` in `ToggleTheme`

We'll start by learning how to consume provider data in a function component by implementing a consumer component in the `ToggleTheme` component. 

Consumer components render a function that returns the component or element that will consume context data. The context data is passed into the component/element via the function. This is how these components are structured:

```js
<MyContext.Consumer>
  {value => /* render a component or element with the context value */}
</MyContext.Consumer>
```

Note that this is a function: `{value => /* render a component or element with the context value */}`. The `value` parameter represents the context value from the provider component. 

Let's use a consumer component in `App.js`: we'll add a `<ThemeContext.Consumer>` component in place of the `<ToggleTheme>` component, and set the `ToggleTheme` component as the return value of the the `<ThemeContext.Consumer>` component.

Here's the new code:

```js
<ThemeContext.Consumer>
  {contextTheme => <ToggleTheme theme={contextTheme} toggleTheme={toggleTheme}/>}
</ThemeContext.Consumer>
```

And here is how the new code fits into `App.js`:

<div class="filename">src/components/App.js</div>

```js
...

function App(){
  const [theme, setTheme] = useState(themes.light);

  document.body.style.backgroundColor = theme.backgroundColor;
  document.body.style.color = theme.textColor;

  function toggleTheme() {
    ...
  }

  return (
    <ThemeContext.Provider value={theme}>
      <Header />
      {/* new code below */}
      <ThemeContext.Consumer>
        {contextTheme => <ToggleTheme theme={contextTheme} toggleTheme={toggleTheme}/>}
      </ThemeContext.Consumer>
      {/* new code above */}
      <TicketControl />
    </ThemeContext.Provider>
  );
}

export default App;
```

The `contextTheme` parameter represents the data that the provider component transmits via its `value` prop, which is the CSS object for either the light or dark theme. We then use the `contextTheme` parameter as the value of a prop we call `theme`, which we'll use to create styles for our button. We also pass in our `toggleTheme` prop, the same as before.

Next, let's update the `ToggleTheme` component to use the theme data. 

<div class="filename">src/components/ToggleTheme.js</div>

```js
import React from "react";
import PropTypes from "prop-types";

function ToggleTheme(props) {
  const {theme, toggleTheme} = props;

  const styles = { 
    backgroundColor: theme.buttonBackground, 
    color: theme.textColor 
  }

  return (
    <React.Fragment>
      <button style={styles} onClick={toggleTheme}>
        {theme.textColor === "AntiqueWhite" ? "toggle light theme" : "toggle dark theme"}
      </button>
      <hr/>
    </React.Fragment>
  );
}

ToggleTheme.propTypes = {
  toggleTheme: PropTypes.func,
  theme: PropTypes.object
}

export default ToggleTheme;
```

We've made a few additions:

* We destructure `theme` from `props`.
* We create a new CSS object based on the styles that are relevant to the button. We save this object in the variable `styles`.
* We add a new `style={styles}` attribute to the `<button>` element to apply the theme styles.
* We update the "Toggle Theme" button text to conditionally display "toggle light theme" or "toggle dark theme", depending on whether the theme is dark or light. 
* We list the prop type of the `theme` prop.

Now if you run the project, you'll see that the toggle button changes along with the select theme. 

### Using `contextType` in the Class Component `TicketControl`

Next, we'll learn how to consume provider data in a class component. We'll do a few things:

* Create a `contextType` property for the `TicketControl` component that's set to the `ThemeContext` provider value. 
* Call `this.context` anywhere we need to use the context value.
* Create button styles based on the context theme value and apply it to the button in `TicketControl`.

We'll add the new code and then explain it down below. Most of our updates will be concentrated at the bottom of the `TicketControl` component.

Here's the new code:

<div class="filename">src/components/TicketControl.js</div>

```js
...
// We import ThemeContext to create a new consumer.
import { ThemeContext } from "../context/theme-context";

class TicketControl extends React.Component {

  ...

  ...

  render(){
    // We access the context value.
    let theme = this.context;
  
    // We create our button styles.
    const buttonStyles = { 
      backgroundColor: theme.buttonBackground, 
      color: theme.textColor, 
    }

    let currentlyVisibleState = null;
    let buttonText = null; 

    if (this.state.editing ) {      
      ...
    } else if (this.state.selectedTicket != null) {
      ...
    } else if (this.state.formVisibleOnPage) {
      ...
    } else {
      ...
    }

    return (
      <React.Fragment>
        {currentlyVisibleState}
        {/* We've added a new style attribute to the button below. */}
        <button style={buttonStyles} onClick={this.handleClick}>{buttonText}</button> 
      </React.Fragment>
    );
  }

}

// We've created a contextType property and set it to ThemeContext.
TicketControl.contextType = ThemeContext;

export default TicketControl;
```

First we start by importing `ThemeContext` from `theme-context.js`. Anytime we want to create a provider or consumer, we'll need to use `ThemeContext`. 

Then next thing we do is add a new property to the `TicketControl` class called `contextType` and set its value to the `ThemeContext`. This creates a context consumer for our `TicketControl` class that is connected to the provider component in `App.js`. Notice that we do this below the class declaration:

<div class="filename">src/components/TicketControl.js</div>

```js
...
import { ThemeContext } from "../context/theme-context";

class TicketControl extends React.Component {
  ...
  ...
}

// We create the new contextType property below the class declaration.
TicketControl.contextType = ThemeContext; 

export default TicketControl;
```

Once we have the consumer set up via the `TicketControl.contextType` property, we can then access the context value from anywhere within the class by invoking `this.context`.

In the `TicketControl` component, we only need to use the theme context's value to create our button styles, so we can apply them as an inline style to our `<button>` element. All of this code is in the `render()` method of the component:

<div class="filename">src/components/TicketControl.js</div>

```js
...
import { ThemeContext } from "../context/theme-context";

class TicketControl extends React.Component {
  ...
  ...

  render(){
    let theme = this.context;
  
    const buttonStyles = { 
      backgroundColor: theme.buttonBackground, 
      color: theme.textColor, 
    }
    
    ...

    return (
      <React.Fragment>
        {currentlyVisibleState}
        <button style={buttonStyles} onClick={this.handleClick}>{buttonText}</button> 
      </React.Fragment>
    );
  }

}

TicketControl.contextType = ThemeContext;

export default TicketControl;
```

### Using the `useContext()` hook in `ReusableForm` and `TicketDetail`

Next, we'll learn how to use the `useContext()` hook to create a context consumer. Take note that consumer components and the `useContext()` hook should only be used with function components. 

As hooks tend to do, the `useContext()` hook greatly simplifies how to set up a consumer in a function component. We'll update the `TicketDetail` component first. Let's take a look at the new code, and then discuss the implementation details.

Here's the new code:

<div class="filename">src/components/TicketDetail.js</div>

```js
// We import the useContext hook.
import React, { useContext } from "react";
import PropTypes from "prop-types";
// We import ThemeContext.
import { ThemeContext } from "../context/theme-context";

function TicketDetail(props){
  const { ticket, onClickingDelete, onClickingEdit } = props; 
  
  // We create our consumer.
  const theme = useContext(ThemeContext);

  // We create our styles.
  const styles = { 
    backgroundColor: theme.buttonBackground, 
    color: theme.textColor 
  }
  
  return (
    <React.Fragment>
      <h2>Ticket Detail</h2>
      <h3>{ticket.location} - {ticket.names}</h3>
      <p><em>{ticket.issue}</em></p>
      {/* We apply our styles to each button. */}
      <button style={styles} onClick={onClickingEdit}>Update Ticket</button>
      <button style={styles} onClick={()=> onClickingDelete(ticket.id)}>Close Ticket</button>
      <hr/>
    </React.Fragment>
  );
}

TicketDetail.propTypes = {
  ...
};

export default TicketDetail;
```

First we import `useContext` from `'react'` and the `ThemeContext` from `theme-context.js`.

Then, we call the `useContext` hook:

```js
const theme = useContext(ThemeContext);
```

To get the current provider value returned, all we need to do is provide the context we want to use to the `useContext()` hook as an argument. It's as simple as that! Invoking `useContext()` also creates a consumer that is subscribed to any change in the context provider's value. 

The only remaining update we make is creating button styles from the `theme` value and applying them as inline styles to each button. This process is the same as with the `ToggleTheme` and `TicketControl` components.

Let's update the `ReusableForm` component next. We'll do all of the above steps, along with one additional step: creating form input styles and applying them as inline styles to each form input. We won't step through all of the new code, but there are comments in the code snippet that describe each update, so pay attention to them as you read through the code: 

Here's the new code:

<div class="filename">src/components/ReusableForm.js</div>

```js
// We import useContext and ThemeContext
import React, { useContext } from "react";
import { ThemeContext } from "../context/theme-context";
import PropTypes from "prop-types";

function ReusableForm(props) {
  // We create a ThemeContext consumer and 
  // get access to the provider value.
  const theme = useContext(ThemeContext);

  // We create button styles.
  const buttonStyles = { 
    backgroundColor: theme.buttonBackground, 
    color: theme.textColor, 
  }

  // We create input styles.
  const inputStyles = { 
    backgroundColor: theme.inputBackground,
    color: theme.textColor, 
  }
  
  return (
    <React.Fragment>
      <form onSubmit={props.formSubmissionHandler}>
      <label>Pair Names:
          <br/>
          {/* We add input styles. */}
          <input
            style={inputStyles}
            type='text'
            name='names' />
        </label>
        <br/>
        <label>Location:
          <br/>
          {/* We add input styles. */}
          <input
            style={inputStyles}
            type='text'
            name='location' />
        </label>
        <br/>
        <label>Describe your issue:
          <br/>
          {/* We add input styles. */}
          <textarea
            style={inputStyles}
            name='issue' />
        </label>
        <br/>
        {/* We add button styles. */}
        <button style={buttonStyles}  type='submit'>{props.buttonText}</button>
      </form>
      <hr/>
    </React.Fragment>
  );
}

ReusableForm.propTypes = {
  ...
};

export default ReusableForm;
```

At this point the functionality for toggling between a light and a dark theme is complete! Go ahead and run your application and test it out. 

### Leveraging the Default Context Value for Consumer Error Handling

We need to remember that we cannot consume context data outside of a provider component. That means if we add a context consumer to a part of our component tree that does not have a provider component upstream from the consumer component, our code will break. We can try this out by moving `<TicketControl>` outside of the `<ThemeContext.Provider>` components in `App.js`:

<div class="filename">src/components/App.js</div>

```js
...

function App(){
  ...

  return (
    <React.Fragment>
      <ThemeContext.Provider value={theme}>
        <Header />
        <ThemeContext.Consumer>
          {contextTheme => <ToggleTheme theme={contextTheme} toggleTheme={toggleTheme}/>}
        </ThemeContext.Consumer>
      </ThemeContext.Provider>
      {/* We've moved <TicketControl> outside of <ThemeContext.Provider> */}
      <TicketControl />
    </React.Fragment>
  );
}

export default App;
```

When we do this, we'll get uncaught console errors similar to this one:

```js
Uncaught TypeError: Cannot read properties of undefined (reading 'buttonBackground')
```

There's only one solution to the issue of placing a consumer component outside of a provider component: move either component so that they are connected! In the above example case, the solution is simply to move the `TicketControl` component back within the `ThemeContext.Provider` component. 

However, there's two different changes we can make in our code to help us better identify the issue at hand: a good change and a not so good change. We'll cover the not so good change we can make first, which is to give our theme context a default value. This is different from the value that we provide the provider component, which is required. 

Here's how we set up a default value for our theme context:

<div class="filename">src/context/theme-context.js</div>

```js
import React from 'react';

export const themes = {
  light: {
    ...
  },
  dark: {
    ...
  }
};

// We've passed in themes.light to the React.createContext() method!
export const ThemeContext = React.createContext(themes.light);
```

We just need to pass in an argument to the `React.createContext()` method call. When we do that, context consumers will use that value anytime they are located outside of a context provider. 

However, this change is not so helpful because our code now silently fails: our buttons and inputs that are in `TicketControl`, `TicketDetail`, and `ResuableForm` will always be set to the light theme, and we'll be left to debug what's going on. 

The better change we can make to our code is leaving the default context value as `undefined` and using it as a signal that our consumer component has been placed outside of the range of a provider component. Let's see how this works. 

First, remove the default value from the `React.createContext()` method:

<div class="filename">src/context/theme-context.js</div>

```js
// We won't be using a default context value.
export const ThemeContext = React.createContext();
```

Next, open up the `ToggleTheme` component and add the following if statement:

<div class="filename">src/components/ToggleTheme.js</div>

```js
import React from "react";
import PropTypes from "prop-types";

function ToggleTheme(props) {
  const {theme, toggleTheme} = props;

  // New if statement below!
  if (!theme) {
    throw new Error("ThemeContext must be used within a ThemeContext.Provider!");
  }

  const styles = { 
    ...
  }

  return (
    ...
  );
}

ToggleTheme.propTypes = {
  ...
}

export default ToggleTheme;
```

We've added a conditional that checks if the context value saved in the variable `theme` is falsey, and if so, we throw an new error stating that `"ThemeContext must be used within a ThemeContext.Provider!"`.

Now if we place a theme context consumer outside of the range of the provider component, we'll get loud errors with a clear message:

```js
Uncaught Error: ThemeContext must be used within a ThemeContext.Provider!
```

The big takeaway here is that we're leveraging an `undefined` default context value to our advantage to provide ourselves with better error messages. This may not seem like a big deal in the context of the Help Queue, since we don't have plans to expand the functionality or reuse the components we've created anymore than we already are. However, this sort of error handling can be helpful in a large React application that reuses many of its components and has many contributors to its code base. 

The next step is to add the following if statement where ever you access the context provider value:

```js
if (!theme) {
  throw new Error("ThemeContext must be used within a ThemeContext.Provider!");
}
```

We won't walk through that process in this lesson, so if you are coding along with these lessons, you'll need to make the relevant updates to `TicketControl`, `ReusableForm`, and `TicketDetail` yourself.

At this point there's nothing more to do with this Help Queue refactor. In the next lesson, we'll take time to review context best practices, alternatives, and further exploration.

---
**[<i class="glyphicon glyphicon-folder-open"></i>  Example GitHub Repo for Help Queue with Light/Dark Theme using Context](https://github.com/epicodus-lessons/react-help-queue-with-context)**
