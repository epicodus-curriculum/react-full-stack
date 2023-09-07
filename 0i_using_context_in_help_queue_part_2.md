With our Help Queue project set up and our updates planned, we're ready to implement context. In this lesson, we'll do three things:

* Create a context for our light and dark theme. 
* Learn about context providers and consumers.
* Create a context provider.
* Implement a state management tool to change the value of the context.

## Implementing Context
---

Let's create a context for our theme. Start by creating a new folder called `context` within the `src` folder, and within that a file called `theme-context.js`. 

Within `theme-context.js`, we'll add our CSS style object that we created earlier as well as some new code: 

<div class="filename">src/context/theme-context.js</div>

```js
import React from 'react';

export const themes = {
  light: {
    backgroundColor: "AntiqueWhite",
    textColor: "DarkSlateGrey",
    buttonBackground: "Lavender", 
    inputBackground: "Gainsboro"
  },
  dark: {
    backgroundColor: "DarkSlateGrey",
    textColor: "AntiqueWhite",
    buttonBackground: "#232b3c",
    inputBackground: "#45516d"
  }
};

export const ThemeContext = React.createContext();
```

First we've imported React from `'react'`, and then we've declared our CSS style object and saved it to the `themes` variable. Notice that we're exporting `themes` so that we can use it where we need to in our app. 

Then, we create a new context with React's `creatContext()` method:

```js
export const ThemeContext = React.createContext();
```

The convention is to name context objects in Upper Camel Case, calling it whatever is representative of the data that the context will hold. We've called our context `ThemeContext`, because it holds theme data.

It's also common convention to include the data relevant to the context within the same file, which is why we're including the CSS object within `theme-context.js`. However, it would be fine to save this information in the component that manages the theme state.

And with that we've created our context! But it's not that useful yet: we haven't associated a value with our `ThemeContext` and we haven't put it to use in our app. Next, let's learn about the tools that `ThemeContext` exposes: provider and consumer components.

### Context Providers and Consumers

Context uses provider and consumer components to share data between components. In fact, the `ThemeContext` we created earlier is an object with two properties: a provider and a consumer:

```js
export const ThemeContext = React.createContext();
console.log(ThemeContext.Provider);
console.log(ThemeContext.Consumer);
```

Since React components are just functions, we can `console.log` these values, like in the above code snippet. However, we won't get helpful information when look inside of the `ThemeContext.Provider` and `ThemeContext.Consumer` variables. The takeaway here is that these two components are generated from the `createContext()` method, and they are the mechanism by which context works:

* Provider components provide data to a section of the (or entire) component tree
* Consumer components enable individual components to use the data that the provider component exposes. 

Provider and consumer components wrap around the components that they are modifying to give and gain access to the context. Using the Help Queue as an example, if we wanted to provide `ThemeContext` data to our component tree, we might wrap `App.js` in `<ThemeContext.Provider>`, and any component that consumes that data would be wrapped with `<ThemeContext.Consumer>`:

![The Help Queue component tree with ThemeContext provider and consumer components.](https://learnhowtoprogram.s3.us-west-2.amazonaws.com/React/Week-5-React-2020/context-help-queue-component-tree-context-provider-consumer.png)

As demonstrated in the component tree above, we can use as many consumer components as we need in our component tree. However, we'll use just one provider component that is lifted to the lowest common ancestor to all of the consumer components that consume the provider's data. 

Remember that data flows down in React, and this is true for context provider and consumer components as well. This means that if we can only use a consumer component lower in the component tree from a provider component. Using the example above, we purposefully place our provider component around `App.js` so that the four child and grandchild components of `App.js` can access the context data. 

Take note that the above component tree is for demonstration purposes only. It's very close to how we'll implement a provider and consumers, but we'll use slightly different tools and locate the provider component within `App.js`, but not wrapped around `<App>`. This will become clear soon.

Ultimately, it's important to understand that provider and consumer components enact a subscription-based relationship: consumer components are subscribed to the corresponding provider component, and any time the value of the provider component changes, all of the components that consume the providers data will be re-rendered. Using the example component diagram above, this would mean anytime the value of the provider component changes in `App.js`, the following four components will re-render: `TicketControl.js`, `TicketDetail.js`, `ReusableForm.js`, and `ToggleTheme.js`. Pretty neat! 

Next, let's implement a provider. 

### Adding a Provider Component

Since we've determined that `App.js` is our lowest common ancestor to all of the components that will need access to the `ThemeContext` data, let's add a provider there. First, we'll import `ThemeContext` and our CSS `themes` object from `theme-context.js`:

<div class="filename">src/components/App.js</div>

```js
import { ThemeContext, themes } from "../context/theme-context";
```

Next, let's wrap our component tree with the `ThemeContext.Provider` component. In the following code notice that we're replacing the `<React.Fragment>` components with the `<ThemeContext.Provider>` components:

```js
import React from "react";
import Header from "./Header";
import TicketControl from "./TicketControl";
import ToggleTheme from "./ToggleTheme";
import { ThemeContext, themes } from "../context/theme-context";

function App(){
  return (
    <ThemeContext.Provider value={themes.light}>
      <Header />
      <ToggleTheme/>
      <TicketControl />
    </ThemeContext.Provider>
  );
}

export default App;
```

Notice that we've added a prop called `value` to our `<ThemeContext.Provider>` component. This is how we designate a value for our context provider. The prop must always be called `value`. 

We've set the value of the `value` prop to `themes.light`. That will be our starting theme. However, this theme is static! If we want the value of our theme to change, we'll need to implement a state management tool. Let's do that next. 

### Adding State

We'll use the `useState()` hook to manage our state. Let's start with an import and declare a new state variable:

<div class="filename">src/components/App.js</div>

```js
import React, { useState } from "react";
import Header from "./Header";
import TicketControl from "./TicketControl";
import ToggleTheme from "./ToggleTheme";
import { ThemeContext, themes } from "../context/theme-context";

function App(){
  const [theme, setTheme] = useState(themes.light);

  return (
    <ThemeContext.Provider value={themes.light}>
      <Header />
      <ToggleTheme/>
      <TicketControl />
    </ThemeContext.Provider>
  );
}

export default App;
```

Our state variable is called `theme` and it is set to the light theme in our CSS `themes` object. 

The next update we need to make is to set the `value` prop of our `<ThemeContext.Provider>` to the value of our state variable `theme`:

<div class="filename">src/components/App.js</div>

```js
...

function App(){
  const [theme, setTheme] = useState(themes.light);

  return (
    <ThemeContext.Provider value={theme}> {/* new code! */}
      ...
    </ThemeContext.Provider>
  );
}

export default App;
```

Now the value of our provider component is directly tied to our state variable. That means we can call `setTheme()` to change the current theme for our provider and its consumers. Next, let's wire up our button in the `ToggleTheme` component to do just that!

### Wiring Up the `ToggleTheme` Button

The first thing we'll need to do is pass a callback function down to the `ToggleTheme` component so that it can invoke a change in state in `App.js`. Remember that callback functions enable us to pass data up from a child component to a parent component, while maintaining React's unidirectional data flow.

We'll create a `toggleTheme()` function in `App.js` that calls the `setTheme()` state updater function:

<div class="filename">src/components/App.js</div>

```js
...

function App(){
  const [theme, setTheme] = useState(themes.light);

  function toggleTheme() {
    setTheme(theme => 
      theme.textColor === "AntiqueWhite" ? themes.light : themes.dark
    );
  }

  return (
    ...
  );
}

export default App;
```

Notice that we pass a function to the `setTheme()` function call:

```js
theme => theme.textColor === "AntiqueWhite" ? themes.light : themes.dark
```

This is an arrow function that makes use of all of the available shortcuts: omitting parens with one parameter and an implicit return. We could otherwise write this arrow function like so:

```js
(theme) => { 
  return theme.textColor === "AntiqueWhite" ? themes.light : themes.dark 
}
```

We're passing in a function to the `setTheme()` function in order to access the current value of the `theme` state variable.

The ternary operator checks to see if we're on the light theme by checking the value of the `textColor` property; if we're on the light theme, then it updates state to the dark theme, and vice versa. 

Note that the ternary operator could be re-written like so:

```js
(theme) => { 
  if (theme.textColor === "AntiqueWhite") {
    return themes.light
  } else {
    return themes.dark
  }
}
```

Next, we'll need to pass the `toggleTheme()` function to the `ToggleTheme` component:

<div class="filename">src/components/App.js</div>

```js
...

function App(){
  const [theme, setTheme] = useState(themes.light);

  function toggleTheme() {
    setTheme(theme => 
      theme.textColor === "AntiqueWhite" ? themes.light : themes.dark
    );
  }

  return (
    <ThemeContext.Provider value={theme}>
      <Header />
      <ToggleTheme toggleTheme={toggleTheme}/> {/* new code! */}
      <TicketControl />
    </ThemeContext.Provider>
  );
}

export default App;
```

Next let's update the `ToggleTheme` component to accept props, list prop types, and use the `toggleTheme()` function. 

<div class="filename">src/components/ToggleTheme.js</div>

```js
import React from "react";
import PropTypes from "prop-types";

function ToggleTheme(props) {
  const { toggleTheme } = props;

  return (
    <React.Fragment>
      <button onClick={toggleTheme}>
        Toggle Theme
      </button>
      <hr/>
    </React.Fragment>
  );
}

ToggleTheme.propTypes = {
  toggleTheme: PropTypes.func
}

export default ToggleTheme;
```

At this point if we run our project, we'll be able to click our toggle theme button and change the value of the `theme` state variable. But we won't be able to tell that anything is working. That's because we're not actually using the `theme` state anywhere! Let's fix this issue by adding some styles so that we can see the new toggle theme functionality in action.

### Setting the Background and Text Color

The first styling we'll add is updating the `<body>` tag's background color and text color to match the current theme. We'll first show you the two new lines of code, and then explain it. Here's what we'll add to `App.js`:

<div class="filename">src/components/App.js</div>

```js
...

function App(){
  const [theme, setTheme] = useState(themes.light);

  document.body.style.backgroundColor = theme.backgroundColor; // new code!
  document.body.style.color = theme.textColor; // new code!

  function toggleTheme() {
    setTheme(theme => 
      theme.textColor === "AntiqueWhite" ? themes.light : themes.dark
    );
  }

  return (
    <ThemeContext.Provider value={theme}>
      <Header />
      <ToggleTheme toggleTheme={toggleTheme}/>
      <TicketControl />
    </ThemeContext.Provider>
  );
}

export default App;
```

Let's break down the new code we've added. We've used [dot notation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Property_accessors) to access the nested properties of the `document` object:

* The `document` object represents the Document Object Model (DOM) for our Help Queue app. The DOM is [a Web API](https://developer.mozilla.org/en-US/docs/Web/API).
* The `document.body` property represents the `<body>` tags in the DOM.
* The  `document.body.style` property represents the `<body>` tags' `style` attribute, which sets inline HTML styles.
* The `style.backgroundColor` and `style.color` represent the CSS properties `background-color` and `color` respectively. We can set the value of these properties to change the background and text colors for the body tags.

So this JavaScript:

```js
document.body.style.backgroundColor = theme.backgroundColor; 
document.body.style.color = theme.textColor;
```

Is the same as this CSS:

```css
body {
  background-color: "blue";
  color: "white";
}
```

However, the JavaScript is dynamic in nature. Instead of hardcoding values like in the example CSS above (`"blue"` and `"white"`), we're able to set the values of the background color and text color to the current theme. Very cool!

Go ahead and test this out now: run your project and press the "toggle theme" button; you'll see the background and text color change from a light theme to a dark theme. At this point we still need to update the background colors of our button and input elements to complete the functionality. We'll do that in the next lesson when we learn how to create context consumers for function and class components.