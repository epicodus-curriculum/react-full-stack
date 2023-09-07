**Note:** While you are not expected to incorporate any of the concepts from the homework on date-fns into your independent project, it's important to understand how the component lifecycle works. Pay close attention to the material in this lesson!

---

In this lesson, we'll learn about the lifecycle of a React component. We'll also add a timer to our Help Queue to see how lifecycle methods work.

The React lifecycle is a series of methods that is always called in a certain order. We can use these lifecycle methods to call our own methods at a very specific time during a component's lifecycle. We've actually used the most common lifecycle methods before: `constructor()` and `render()`.

We can roughly break the component lifecycle down into three stages:

* **Mounting** refers to the stages where a component is instantiated and then added to the DOM. It includes the following common lifecycle methods, which are called in order:

  * **`constructor()`**: We've used this method extensively. This is where we specify any properties the component should have such as local state.
  * **`render()`**: We're also familiar with this method, which is used to render elements. It's the only lifecycle method that's required in a class component.
  * **`componentDidMount()`**: We haven't used this method before. `componentDidMount()` is invoked after a component has finished inserting all DOM nodes. The React documentation recommends setting up subscriptions during this lifecycle method. We will use this method to instantiate a timer to keep track of our Help Queue.

* **Updating** is a stage that can happen multiple times during a component's lifecycle. For instance, this stage would occur each time a user increments a counter.
  * **`render()`**: Not surprisingly, `render()` gets called again. That way, any DOM nodes that have changed can be refreshed.
  * **`componentDidUpdate()`**: If we have a method that we want to call any time the component updates, we could do so here. Be careful, though — it may be called many times and this isn't necessarily very efficient.

* **Unmounting** occurs when the component is being removed from the DOM. It only has one method:

  * **`componentWillUnmount()`**: We can use this method to perform any cleanup such as unsubscribing or canceling a timer. We will utilize this method to cancel the timer in our own Help Queue.

It's important to understand the component lifecycle and the order of these lifecycle methods. There are a number of other lifecycle methods listed in the [React documentation](https://reactjs.org/docs/react-component.html) but they are not commonly used. However, we recommend taking a look at the docs to get a better sense of the full lifecycle. Each lifecycle method is well documented if you want more information.

If we review the lifecycle methods above, there are three new lifecycle methods we haven't used before: `componentDidMount()`, `componentDidUpdate()` and `componentWillUnmount()`. We will use the first and the third in our Help Queue application.

### Using a Timer to Demonstrate Lifecycle Methods

Before we move on, let's implement a simple timer in our Help Queue application. We'll add all three of the lifecycle methods we just learned. We'll also incorporate much (but not all) of the code below in the next lesson as well.

<div class="filename">src/components/TicketControl.js</div>

```js
...
// Add the following code after the constructor.

componentDidMount() {
    this.waitTimeUpdateTimer = setInterval(() =>
      this.updateTicketElapsedWaitTime(),
    1000
    );
  }

  // We won't be using this method for our Help Queue update — but it's important to see how it works.
  componentDidUpdate() {
    console.log("component updated!");
  }

  componentWillUnmount(){
    console.log("component unmounted!");
    clearInterval(this.waitTimeUpdateTimer);
  }

  updateTicketElapsedWaitTime = () => {
    console.log("tick");
  }
```

When our component is mounted, `componentDidMount()` will be called. That will trigger the following code: 

```js
this.waitTimeUpdateTimer = setInterval(() =>
  this.updateTicketElapsedWaitTime(),
1000
);
```

This code should be relatively familiar from Intermediate JavaScript. The `setInterval()` method is a function. The function call itself includes two parts: the code to be executed and the delay between each interval. In this case, the interval is set to 1000 milliseconds — a single second.

We set the value of the code to be executed to another function called `this.updateTicketElapsedWaitTime()`. We _could_ call all the code we want to execute inside `componentDidMount()` but that wouldn't be very clean. All we want to do is have a timer here. Updating the UI of the queue to reflect the elapsed time should be a separate concern.

We set the value of the `setInterval()` function to `this.waitTimeUpdateTimer`. We make it a property of the component so that we can use it in other functions. This way, we can actually cancel the timer when we need to.

We will keep this method for our Help Queue application — though we will change the interval to 60000 ms so the queue updates every minute — just like the one at Epicodus.

Next, we call `componentDidUpdate()`. We add a `console.log("component updated!")` so we can see when this method is called in our component. We won't be using `componentDidUpdate()` in our Help Queue — but it's still helpful to see how this lifecycle method works.

Then we call `componentWillUnmount()`. This method will be called when our component is cleared from the UI. It includes the following code:

```js
console.log("component unmounted!");
clearInterval(this.waitTimeUpdateTimer);
```

The `console.log()` will tell us when the component is about to be unmounted. Then we need to call `clearInterval()` to actually clear the timer. If we hadn't saved the timer inside `this.waitTimeUpdateTimer`, we wouldn't have a way to clear our interval.

Finally, we have the following method, which just logs a "tick" to the console.

```js
updateTicketElapsedWaitTime = () => {
  console.log("tick");
}
```

This method is triggered each second in our `setInterval()` function. Right now, it's not doing much. However, this method will be responsible for communicating with Redux in the next lesson.

Now let's run `npm start` and open the console. Try adding, updating and closing a few tickets. Note the following:

* `componentDidMount()` is called once, when we run the application. At that point, the timer is triggered and `tick` is logged to the console every second.
* Each time we add, update, or close a ticket, `component updated!` is logged to the console. As we can see, the `componentDidUpdate()` method is triggered each time a change is made to the UI. The timer has no effect on it. If we execute code that doesn't affect the UI, `componentDidUpdate()` won't be triggered. This makes sense, because the component _itself_ (eg, its UI) isn't updated.
* `componentWillUnmount()` is never called. If we think about the structure of our application, this makes sense. `componentWillUnmount()` is called inside `TicketControl.js`. However, this component is essentially a container determining which child component should be rendered — the ticket list, a ticket form, or the ticket detail. `TicketControl.js` is always there and is never unmounted. On the other hand, all the various child components such as ticket list and our form component are unmounted each time they are closed. Why bother to add code to `componentWillUnmount()` at all then? Well, if our application were to expand and `TicketControl` could be unmounted, we'd want this code to trigger. We are writing code that is both proactive (thinking about future expansion) and defensive (trying to avoid future bugs).

Now that we have a general sense of how these lifecycle methods work, we are ready to implement time elapsed functionality in our Help Queue. We'll do that in the next lesson.