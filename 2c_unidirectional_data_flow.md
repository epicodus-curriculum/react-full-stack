So far we have only been working with local state. However, when a user inputs data in our form, we somehow need to get that data from our `NewTicketForm` component to its parent `TicketControl` component.

Before we do that, we need to learn more about **unidirectional data flow**. Unidirectional data flow is a language-agnostic term for applications that have data flowing in only one direction.

In the case of React applications, data can only flow from a parent component _down_ to a child component. That's why shared state should always be lifted to a common ancestor. Only child components will ever be able to access that state. Components that are higher up the component tree (above a component with state) have no way to know about that state because of unidirectional data flow. In fact, components in React are so modular that they don't even know their parents exist. It's the job of parent components to keep track of their children, not the other way around. 

The same is true with props. We can only pass props _down_ from a parent component to a child component. That's the whole point of unidirectional data flow. It may seem like a limiting concept, but it makes planning, building, and debugging an application much easier. If state and props could flow in every direction, our applications would quickly become a mess.

So if data can only be passed _down_, then how can we get information from a child component up to a parent component?

The answer: we need to use callbacks. Here's how it works.

1. We define a method in a parent component that has state.
2. The parent component passes this method into the child component as a prop. Functions can be props just like any other data type.
3. We will add this method to a function in our child component in the form of a callback.
4. When the child executes the function that contains the parent component's callback, the method in the parent component is invoked. Because the callback resides in the parent component, the parent component can access any data that's passed into it. This works similarly to a closure.

This may feel like we're breaking the rules of unidirectional data flow because the parent component can access information from the callback executed in the child component.

However, that's not the case. The parent component passes props _down_ using unidirectional data flow. If a function is passed downward as a prop, the parent can still access it.

Understandably, this concept can be confusing at first. Let's make it more concrete by describing how we will do this in our Help Queue application. We'll do the following:

1. First, we will create a method called `onNewTicketCreation` in the parent `TicketControl` component.
2. Next, we'll pass the `onNewTicketCreation` function to its child `NewTicketForm` component as a prop.
3. Our child `NewTicketForm` component has a function called `handleNewTicketFormSubmission` which correctly gathers user inputs from a form. We will add `onNewTicketCreation` to the `handleNewTicketFormSubmission` as a callback.
4. Form data will be passed into the `onNewTicketCreation` callback via its parameters.
5. The parent `TicketControl` component will have access to that data, which it can then use to add a new ticket to our `mainTicketList`.

Now that we've covered unidirectional data flow, we're ready to implement this new functionality. Don't worry if it still doesn't make sense. This is a new and complex concept for React beginners — the best way to learn and internalize how this works is to actually write the code — and then continue to practice working with unidirectional data flow until the underlying concepts begin to click.