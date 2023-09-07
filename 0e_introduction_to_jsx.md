In this lesson, we'll learn the basics of JSX, a preprocessor that adds special syntax capabilities to JavaScript. Specifically, JSX combines elements of both HTML and JavaScript.

We'll start by deleting the content inside the `App.js` file (but not the file itself). We can also delete the `App.css` and `App.test.js` files that `create-react-app` automatically built for our `help-queue` project. The provided `App.js` is a state component, but we don't need one of those yet. Instead, we'll build a simple function component that uses JSX.

JSX is a declarative language that combines JavaScript with HTML. It makes React code much easier to read, write, and understand. For this reason, almost all React development teams use JSX. While we could technically write React applications with vanilla JavaScript, it would be very cumbersome.

Browsers don't understand JSX so we need to use Babel to compile our JSX code. Fortunately, `create-react-app` will take care of this for us and we don't have to worry about it.

At this point, the `App.js` file should be blank. Add the following code to this file:

<div class="filename">src/App.js</div>

```js
import React from "react";

function App(){
  return (
    <React.Fragment>
      <h1>Help Queue</h1>
      <h3>3a</h3>
      <h3>Thato and Haley</h3>
      <p><em>Firebase entries not saving!</em></p>
      <hr/>
    </React.Fragment>
  );
}

export default App;
```

As we can see, our return statement is mostly standard HTML. In this context, this is actually JSX syntax, which recognizes standard HTML. Under the hood, React is actually using a method called `React.createElement()` to create these elements. While it looks like we are writing HTML, this is actually **syntactic sugar**. Syntactic sugar is when a language or library provides an easier way to write and read code. This way we can write HTML without worrying about calling `React.createElement()` every time we want to create a new element.

We also use a new element called a `<React.Fragment>`. In order to return multiple elements, all the code in a function component's return statement must be wrapped in a single JSX element. Typically, that will be a `<div>` or a `<React.Fragment>`.

If our component returns multiple elements and we don't wrap it in one of these two things, we'll get the following parsing error:

```
Adjacent JSX elements must be wrapped in an enclosing tag.
```

Traditionally, elements were wrapped in a `<div>`. While this still works, it's no longer considered a best practice because it needlessly clutters the DOM with unnecessary divs. The `<React.Fragment>` was created to solve this issue. All components returning more than one element must be wrapped in a `<React.Fragment>`.

While JSX may look like HTML, there are ways in which JSX is more like JavaScript. For instance, if we wanted to add a class to a div, we'd use lower camel case like this:

```jsx
<div className="class-name"></div>
```

We can also evaluate expressions inside curly braces. Update `App.js` to look like the following:

<div class="filename">src/App.js</div>

```js
import React from "react";

function App(){
  const name = "Thato";
  const name2 = "Haley";
  return (
    <React.Fragment>
      <h1>Help Queue</h1>
      <h3>3a</h3>
      <h3>{name} and {name2}</h3>
      <p><em>Firebase entries not saving!</em></p>
      {/* This is a JSX comment. */}
      <hr/>
    </React.Fragment>
  );
}

export default App;
```

In the above example, we are storing the names on the help ticket inside variables. We can then express the values of these variables using curly braces `{}`.

Note also the odd syntax for the comment above. JSX doesn't recognize either JS or HTML comments. To actually add a comment to JSX (which we'll generally avoid), we have to store a JS comment _inside_ curly braces so it will properly be evaluated as a comment. One nice thing about VS Code is that it will automatically use the correct syntax when we use the keyboard shortcut  for comments:

* `Command` + `/` for Mac
* `Ctrl` + `/` for Windows

We will always use curly braces for any JavaScript expression in JSX, whether it's a function call, rendering a variable, or string interpolation. Think about curly braces as a way to escape JSX back and return to vanilla JS. 

Finally, we need to know the syntax for rendering a child component within a parent component. We covered this briefly when we discussed our `index.js` file, which renders the `App` component using the following syntax: `<App />`. React lets us call a component by name using JSX element syntax, in essence creating a custom tag. 

For example, if we were working in `ParentComponent.js` and we wanted to render its child component, syntax for the render method might look something like this: 

```jsx
return (
<React.Fragment>
  <ChildComponent />
</React.Fragment>
);
```

For now, that's really all we need to know about JSX. We will cover using looping and conditionals as well as making forms and other JSX styling syntax in future lessons.

### Separation of Concerns

Before we move on to building out our Help Queue further, there's one other key point to address. We have spent the past few months focusing on keeping our concerns separate. Specifically, we've focused on keeping our UI logic separate from our business logic.

For that reason, it may seem strange to mix HTML and JavaScript syntax as we do with JSX.

However, remember that React is only the view layer. As a view library (not a framework), React only cares about presentation. Its job is to render the virtual DOM — which it will reconcile with the real DOM — leading to a more seamless user experience. Since developers have traditionally used HTML and JS to render the DOM, it makes sense to combine the advantages of both.