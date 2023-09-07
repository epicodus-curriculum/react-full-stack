In this lesson, we'll briefly cover the concept of state. We'll discuss shared state versus local state. We'll also briefly cover how we define and change state in a pure React application. Then, over the next several lessons, we'll add state to our Help Queue application so we can dynamically add new tickets.

We can use two types of data in a React component: props and state. We've already used props in a React application and we also covered state in the last course section in our introduction to functional programming.

As a quick reminder, state is anything in an application that we need to store and change. For instance, in our Help Queue, each time we add a new ticket, we need to update the application's state to hold the new ticket. Likewise, we'd need to update the application's state to edit or delete a ticket.

State is something that can potentially change. In contrast, a component _cannot_ change its props. State is fluid and ever-changing while props are not.

The components we've built so far have all been functional. They cannot handle state. However, as we discussed in our introduction to components, we can use class-based components to handle state. As a rule, we should **only define a component as a class if it _absolutely requires_ state.** If a component does not require state, it should be a stateless function component. Avoiding unnecessary use of state is an important rule in React. (Note: There's actually more to this rule but we won't go into great detail here. If you're feeling particularly brave, check out [this article](https://overreacted.io/how-are-function-components-different-from-classes/) to learn more about the nuances of why React favors function components.)

## Shared State Versus Local State
---

There can be two different types of state in a React application — **shared state** and **local state**. We will be experimenting with both as we add state to our Help Queue application.

### Local State

Local state lives in a single component and is never used in other components. It is much simpler than shared state because we don't have to worry about sharing data in multiple components. An example of local state is hiding and showing information. We will use local state in our Help Queue project to determine whether a user should see a list of tickets or a form for adding a ticket. 

It's obvious where local state should live — in the component that needs it!

### Shared State

Shared state is shared by multiple components and can get complicated very quickly. An example of shared state is the main list of all tickets in the Help Queue. Our ticket list component will need access to all the tickets so it can render them. However, our form will also need to be able to pass information about new tickets to the main list of tickets as well.

Our application will keep shared state fairly simple — only two components will need to access this main ticket list.

There are many challenges to working with shared state. For example, where should it live? In order for components to have access to the same state, that shared state should be lifted to **the lowest common ancestor for all the components that need that state**.

We'll demonstrate what this means with the following diagram.

![The following diagram demonstrates how to lift state between multiple components](https://learnhowtoprogram.s3.us-west-2.amazonaws.com/React/Week-1-React-2019/state-diagram.jpg)

In this diagram, there are six different components. Let's say that component D and component E need access to shared state. The lowest common ancestor for those components is component B. In this case, we'd only need to lift this shared state to component B.

However, let's say that component F also needs access to the same state. That means component B is no longer a common ancestor. At this point, component A is the lowest common ancestor that components D, E, and F share. Now our shared state needs to be lifted all the way to component A.

This will become clearer as we refactor our Help Queue application to use both local and shared state.

You may be wondering what to do if our application has complex state and many different components have many different kinds of shared state. At that point, it's typical to use a library like Redux that is specifically designed to handle complex state. We will cover Redux in the next course section. For the time being, we will focus on learning the ins and outs of adding state to a basic React application without any external libraries.

### Creating and Updating State in a React Application

As we discussed in the [React Components](https://www.learnhowtoprogram.com/react/react-fundamentals/react-components) lesson, class components have a constructor that looks like this:

```js
constructor(props) {
  super(props);
  this.state = {};
}
```

We can define any default state a component should have in the constructor. **It is the only place we should define default state in a pure React application.** (An application with Redux or another state management library will have other ways of defining state.)

We can add any number of key-value pairs to `this.state = {}` to define the default state.

If we want to update state, we will use the following React method:

```js
this.setState({property: update})
```

In the example above, `property` represents the property that needs to be updated while `update` represents the new value a property should have. This is the simplest way to use `setState()`, though we can also do other things beyond just passing in objects.

Whenever we want to change state in React, we need to use the `setState()` method. It is a very bad practice to bypass React and try to change state in other ways. The whole point of `setState()` is to allow React to do its job — which is to be a state management system that efficiently creates a virtual DOM and reconciles it with the actual DOM. So it should go without saying that we should **never** do something like this:

```js
this.state = {property: update}
```

The main issue with manipulating state directly like this is that it will *not* cause the component to re-render as setState() would. If the component doesn't re-render, our changes to state won't create any change in the DOM. **Always use the `setState()` method to update state in a pure React application.**

There's one more very important thing we need to know about the `setState()` method. **`setState()` is an async method.** This makes sense — async methods can be challenging to work with but they allow JavaScript applications to be more efficient. Because `setState()` is async, there are a lot of potential gotchas. For instance, if you try to `console.log` the value of a component's state directly after calling the `setState()` method, you can't expect it to give you reliable results. There is a way to accurately log the current state of a component, but we will cover that in a future lesson.

In this lesson, we've covered the differences between shared and local state. We also briefly covered how to define and update state in a React application. However, these concepts are probably still fuzzy because we haven't actually applied them yet. Don't worry — we are about to apply them. We recommend referring back to this lesson if you still have confusion about these concepts after we add local and shared state to our Help Queue application.
