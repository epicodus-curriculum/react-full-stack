There are many approaches to styling a React application ranging from standard CSS to CSS objects to the `styled-components` library. We will cover several different approaches throughout this course, starting with CSS objects and inline styles in this lesson.

### Inline Styles with Style Objects

React advocates that an application should be composed of small, self-contained, reusable chunks of code called components.

To make our components fully modular, many React developers believe that a component should contain everything it needs, including CSS. According to proponents of this approach, we should use **inline styles** with **CSS objects**. For demonstration purposes, we'll pretend we have a component called `MyStyledComponent` that looks like this:

<div class="filename">MyStyledComponent.js</div>

```javascript
import React from 'react';

function MyStyledComponent(props) {
  return (
    <div>
      <h1>Hey, I'm a component</h1>
      <h2>But there's something different about me...</h2>
      <h3>Unlike other components you have worked with thus far....</h3>
      <h4>I also include custom CSS styles!</h4>
      <p>Pretty cool, right</p>
    </div>
  );
}

export default MyStyledComponent;
```

This is how `MyStyledComponent` looks in browser:

<img src="https://learnhowtoprogram.s3.us-west-2.amazonaws.com/Intermediate%20JavaScript/React/style-free-component.png" alt="style-free example component." width="100%" />

Let's add styling. We can declare a CSS object right in this component's file:

<div class="filename">MyStyledComponent.js</div>

```js
import React from 'react';

function MyStyledComponent(props) {
  const myStyledComponentStyles = {
    backgroundColor: '#ecf0f1',
    fontFamily: 'sans-serif',
    paddingTop: '50px'
  }
  return (
    <div style={myStyledComponentStyles}>
      <h1>Hey, I'm a component</h1>
      <h2>But there's something different about me...</h2>
      <h3>Unlike other components you have worked with thus far....</h3>
      <h4>I also include custom CSS styles!</h4>
      <p>Pretty cool, right</p>
    </div>
  );
}

export default MyStyledComponent;
```

Here, we define an object called `myStyledComponentStyles`. It contains a series of CSS rules in an object literal. This is a **CSS object**.

In order to actually use this CSS object in our JSX, we add a `style` tag that evaluates to a JSX expression with the `myStyledComponentStyles` variable.

Whenever we use the `style` attribute directly on an HTML or JSX element, it's considered an **inline style** because the style information is attached directly to the code. 

Our component now looks like this in the browser:

<img src="https://learnhowtoprogram.s3.us-west-2.amazonaws.com/Intermediate%20JavaScript/React/inline-styles-in-action.png" alt="example component with inline styles" width="100%" />

The syntax in these CSS objects differs from the CSS syntax we've used in the past. Here are the differences;

* CSS properties that use multiple words are written in camelCase instead of using hyphens. For example, we say `backgroundColor` instead `background-color`.

* The values corresponding to each property are strings. For instance, we say `'sans-serif'` instead of `sans-serif`.

* Each CSS rule in the object is separated by a comma since this is an object literal.

* Because the term `class` is reserved in JSX, we have to use the `className` property when we want to add a specific CSS class to an element in a component. For example, we'd do the following: `<div className='example-class'>` instead of this: `<div class='example class'>`.

*  We can also omit the `px` suffix from any pixel-related values.

We will learn other approaches in future lessons. For now, we will continue to keep our components modular and use CSS objects with inline styles to style our applications.
