Components are the building blocks of React. Everything in a React application is a component.

We will be working with two kinds of components in our applications: **function components** and **class components**. All of the code we write will be added to these components.

All React applications have a single root component called `App`. This `App` is a parent component for all the other components in an application. `create-react-app` automatically builds an `App` component for us, though we will always delete it and start from scratch when we build new applications.

## Function Components
---

Most of our components will be function components. Here's the basic structure of a function component:

```jsx
import React from "react";

function ThisIsAFunctionalComponent(){
  return (
    <div>
      // jsx code goes here
    </div>
  );
}

export default ThisIsAFunctionalComponent;
```

As we can see in the example above, a function component (also known as a functional component) is just a function that returns code stored inside a div. The code inside the div will be JSX code, which we will cover in the next lesson.

We use an `import` statement to add the functionality we need. In this case, we need the `React` library. We also `export` the component so that it will be available to the rest of the application. We must always export components — otherwise, we won't be able to use them elsewhere!

Function components **cannot change state in any way**. They are completely static. For instance, we cannot add code to a function component that conditionally renders a piece of text. That would involve changing something on the page, which means we'd need state.

We should always use function components where possible. State can get very complex in a React application, which is exactly why we want to minimize the number of components that use it. That way, we only add state when it's absolutely needed.

In fact, it's common practice to build out entirely static sites with placeholders for data and then refactor components to class components as needed. We will follow this practice when building out our Help Queue.

## Class Components
---

Class components, on the other hand, are used whenever we need to add state to a component. We won't add class components to our applications until later in this course section, but we'll provide a brief overview of how they look now.

```jsx
import React, { Component } from 'react';

class ThisIsAClassComponent extends React.Component {

  constructor(props) {
    super(props);
    this.state = {};
  }

  render() {
    return (
    );
  }
}

export default ThisIsAClassComponent;
```

As we can see, this component isn't a function. Instead, it's a custom class that extends the base functionality of a `Component` class that React provides.

### Class Components Have a Constructor

Notice that class components contain a constructor which takes `props` as an argument. We will cover `props` further in a future lesson, but for now just know they are properties we can pass between different components.

The constructor in a class component also uses the `super` keyword. `super` is called to access a parent class' constructor. Without using the `super` keyword, our component won't be able to inherit the functionality of the parent `React.Component` class. `super` is plain old JavaScript and isn't specific to React. If you'd like to learn more about the `super` keyword, check out Dan Abramov's great [blog article](https://overreacted.io/why-do-we-write-super-props/) on the topic. 

`this.state = {};` is standard convention for declaring state in ES6 React classes. It's a JavaScript object defined in literal notation. It will include state values in the form of key-value pairs.

### Class Components Always Have a Render Method

Class components must always include a `render()` method. This method will return any JSX content that React should add to its virtual DOM.

Just as with a function component, we use an `import` statement to add the functionality we need. In this case, we need the `React` library and the `Component` class.

And just as with a function component, we export the component so that it will be available to the rest of the application.

Don't worry if the specifics aren't clear yet. We'll get plenty of practice with class components in future lessons. You can always refer back to this lesson for information on the basics of a class component.

## Nesting Components
---

As discussed at the beginning of this lesson, React applications have a root component called `App`. `App` is a **parent** to all the other components in an application. We could also say that all the other components in an application are the **children** of `App`.

* If `Component2` is nested inside `Component1`, we'd say that `Component2` is the child while `Component1` is the parent.

```
Component1 (Parent)
    |
Component2 (Child)
```

Components can also be siblings:

```
      Component1
      |        |
Component2  Component3
```

In the diagram above, `Component2` and `Component3` are siblings.

We will cover nesting in greater detail in future lessons, but for now, this covers the basics!

## React Applications Should Be Modular
---

Before we move on to the next lesson, there is one other important point to note. As we now know, a React application is made up of components. 

While it's possible to create an application that has just a few components, each with a lot of code, this is a bad practice.

Instead, our goal should be to create many small and modular components. This best practice allows us to fully separate our concerns. We will demonstrate this in greater detail in future lessons.

## Conclusion
---

In this lesson, we covered several key points:

* **Function components** are literally functions that returns a React element. They can't store or alter state. We will mostly write function components.

* **Class components** are classes that extend React's `Component` class. They must always include a `render()` method that will return a React element. They are used when we need state.

* **Nesting components** is a big part of developing with React. Components can be parents, siblings, children or any combination thereof.

* **Small, modular components** are the way to go. This makes our code easier to understand and allows us to separate presentational concerns.

If it's still not quite clear what a component is, that's to be expected — we haven't built one yet! However, building and using components will soon become second nature with React. Now that we understand the basic concepts behind components, we're ready to learn some JSX and actually build our first component.
