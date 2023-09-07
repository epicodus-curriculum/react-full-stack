**State** is an extremely important concept in computer programming. In simple, very general terms, state is anything we are asking a computer to remember. We can think of it as a "snapshot" of the application at any given time. It defines the current conditions of an application.

When we created applications with object-oriented programming, we used constructors to create stateful objects. We also created methods that would allow us to update and do things with these objects. In other words, we regularly mutated state.

However, in functional programming, we want to avoid mutating state. Where possible, our functions should be pure and avoid side effects. We should aim to make our application as immutable as we can. We could also say that our goal is to make the application **stateless**.

While this is a lofty goal, the simple fact is that applications often do need to store state. React applications have both local and application state. Tools like Redux (which are used with React) store more complex state in what is called a **store**.

These tools often use complex patterns to deal with state. For instance, Redux uses the **pubsub pattern**, which in turn is based on the **observer pattern**.

The **observer pattern** is when an object keeps track of dependents (also known as observers) that listen for changes. When the state of the object changes, it notifies the observer. That allows us to write code that triggers when the object changes. The observer pattern is commonly used in computer programming. One of its biggest advantages is that it allows code to be loosely coupled. Instead of having a function call that happens when state changes (which would be tightly coupled), that function call would only happen when an observer is listening for state changes. That function call may be tightly coupled to the observer, but it's separate from the object, which means our code is better separated.

The **pubsub** pattern is more complex and is based on the concept of publishing and subscribing. Instead of observers directly interacting with an object, there is an intermediary in between. An object can post to the intermediary while observers subscribe to it. This way, the observers and the object don't need to know about each other at all.

You do not need to know about these design patterns to be successful at Epicodus, but in your career as a developer, it will be important to understand these complex design patterns and how they work.

Ultimately, using functional programming doesn't mean we can't work with state. However, it does mean that we need to think very carefully about how to incorporate state in a functional program. In more complex programs, we might use the patterns mentioned above to keep our state separate from the rest of our application.

Since we are working with smaller applications this course section, we will cover a simpler way to store state in the next lesson. Instead of storing our state in objects or global variables, we will save state in functions instead.

We will get a chance to explore state further once we get to React. In the next lesson, we will learn how to use closures to store very simple state.
