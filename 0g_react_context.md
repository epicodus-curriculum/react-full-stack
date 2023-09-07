Now let's turn our attention to learning a new React tool for sharing data called **context**. In this lesson we'll get a conceptual introduction to context: what it is, its common use cases, and its alternatives. 

Then, in the next lesson, we'll revisit our Help Queue application and update it to use context. We'll put all of the concepts in this lesson into practice by adding a light theme and dark theme to the Help Queue, and a button to toggle between each.

## Context Shares Data
---

Context is a mechanism that allows developers to **share data** in a React application. Context is an object, and we can set up as many context objects as we need. In short, context is an alternative to props, both of which allow developers to pass data down, from one component to the next. The big difference between the two is how the data is passed down: 

* With props, data is explicitly passed down between each component to whatever component that needs it.
* With context, we don't have to explicitly pass data between components. Instead, we create a new "context" and give it data to hold. Then, any component that needs the data stored in the context accesses it directly. 

Check out the following visualization that demonstrates the difference.

![Data flow between React context and props.](https://learnhowtoprogram.s3.us-west-2.amazonaws.com/React/Week-5-React-2020/context-vs-props-data-flow.png)

In both cases, data flows down, but the power of context comes in its ability to share data between two components without having to explicitly pass data between any intermediary components. Because of this, context helps developers avoid prop drilling when data is shared across many components. 

## Context Updates the Components that Consumes its Data
---

The other notable difference between context and props is in its relation to re-rendering. You may remember these key details about props:

* A change in the value of a component's props does **not** trigger a re-render of that component. 
* Only a change in the parent component's state will trigger a re-render of the parent component. This will in turn re-render all of its child components and they will all receive new props. 

With context, the opposite is true:

* A change in the value of the context causes all of the components that consume (meaning, "use") its data will automatically be re-rendered. When we implement context in our Help Queue app in the next lesson, we'll learn exactly how this works. 

## Context Is Not a State Management Tool
--- 

It may be tempting to jump to the conclusion that context is a tool for "state management", however it's not true. Context is a tool for transmitting data, just like props. Neither tools change the value of their data, they just store and transmit data.

If we want the data that context holds to change over time, then we need to use context in conjunction with `useState()`, `useReducer()`, or a class component's `state` object. We'll learn how to do this in the next lesson.

## When to Use Context
---

Context should only be used in two cases:

1. To share data that's considered global.
2. To share data that is used in multiple remote branches of a component tree that you would otherwise need to use heavy prop drilling to reach. 

[The React docs for context](https://reactjs.org/docs/context.html) provides a few examples of what data could be needed on a "global" scale:

* The user's locale, which includes the user's language, time zone, region, and any special preferences.
* Color themes, like light and dark modes. 
* The authenticated user.

In theory, in each of the above cases many components will need the same information in order to function. To help us understand the two use cases for context, let's visualize them. Let's pretend we have an application with a component tree that looks like this:

![An example component tree with `App.js` as the root component and many nested components.](https://learnhowtoprogram.s3.us-west-2.amazonaws.com/React/Week-5-React-2020/context-application-state-1.png)

The root component of our tree is `App.js` and each component in the tree is represented by a square. Let's first visualize what an application that uses globally shared data might look like. We'll update our app's component tree to fill in components that use the global data with the color yellow:

![An example component tree that uses shared data that's considered global.](https://learnhowtoprogram.s3.us-west-2.amazonaws.com/React/Week-5-React-2020/context-application-state-2-global.png)

As we can see, "global" data in a React app doesn't mean that every component uses that data, but that _most_ or _many_ components do. This is a great use case for React context.

The other use case for context is when multiple components that are very far apart in the component tree need access to the same data. Let's visualize what that looks like: 

![An example component tree that uses shared data that is used by multiple components that are very far a part in the component tree.](https://learnhowtoprogram.s3.us-west-2.amazonaws.com/React/Week-5-React-2020/context-application-state-3-multiple-remote.png)

In the above example, the components that use the shared data are in a light purple color, and they are pretty far apart in the component tree. Using context in the above example will help us avoid lifting data (as in ["lifting state up"](https://reactjs.org/docs/lifting-state-up.html)) to the nearest ancestor component (`App.js`) and passing props down through many components (also called "prop drilling") to get the shared data where it needs to go.

## Use Context as a Last Resort
---

With the use cases for context in mind, there's one final important thing to note when using context: the React docs heavily encourage developers to use context to share data only as a last resort. Why? Because it makes components harder to reuse: when a component uses context, it becomes dependent on the context and it can't be used outside of the context. 

But what would we do instead of using context? These are the two alternatives:

* Lift shared data up to a common ancestor and pass data down via props.
* Consider how you can compose your components so that passing props is less cumbersome. 

The React docs explain [the alternatives to using context on the docs](https://reactjs.org/docs/context.html#before-you-use-context), with examples. We'll also revisit this topic when we implement context in the Help Queue application. 

## Summary
---

In this lesson, we've covered the conceptual basics of context:

* Similar to props, context is a mechanism to transfer data between components.
* Context is an object, and we can create as many of them as we need in our app.
* Context updates the components that consume its data when its own value changes.
* If we want to change the value of a context we create, we'll need to use a state management tool like `useState`, `useReducer`, or a class component's `this.state` object. 
* Context should be used as a last resort to share data that's considered "global" in a React app.

In the next lesson, we'll add a button to the Help Queue to toggle between light and dark mode, and we'll use context to do it! In the process, we'll review all of the concepts we learned in this lesson. 

