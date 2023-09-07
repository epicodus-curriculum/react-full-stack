In this lesson, we'll cover **conditional rendering**. Conditional rendering is exactly what it sounds like — using a conditional to determine what content should be rendered. Conditional rendering is a great example of local state and it's very common in dynamic applications. It's really also just mostly plain old JavaScript.

We'll only need to update the `render()` method in our `TicketControl` component in this lesson. Because this is the first class component we are building, a quick refresher: class components always need to have a `render()` method. The code we put inside the `render()` method determines the content that will be rendered for the user.

Let's add a condition to the `TicketControl` component now:

<div class="filename">src/components/TicketControl.js</div>

```js
...

class TicketControl extends React.Component {

  constructor(props) {
    super(props);
    this.state = {
      formVisibleOnPage: false
    };
  }

  render(){
    let currentlyVisibleState = null;
    if (this.state.formVisibleOnPage) {
      currentlyVisibleState = <NewTicketForm />
    } else {
      currentlyVisibleState = <TicketList />
    }
    return (
      <React.Fragment>
        {currentlyVisibleState}
      </React.Fragment>
    );
  }

}

...
```

First, we create a variable called `currentlyVisibleState` and set it to `null` because we haven't determined which component should be rendered yet.

Next, we can access any property in `this.state` with dot notation just as we would with any other JavaScript object. If `this.state.formVisibleOnPage` is true, the `currentlyVisibleState` will be set to our `NewTicketForm` component. Otherwise, our `currentlyVisibleState` will be set to our `TicketList` component.

Note that this code is just JavaScript, **not** JSX. We can use plain old JavaScript outside of our `return()` statement. We only need to use JSX and curly braces for evaluation inside our `return()`. We do set the value of `currentlyVisibleState` to React components, but this is just like setting the value of a variable to any other HTML element.

Finally, in our `return()` statement, we use JSX curly braces to evaluate which component should be rendered.

That's all there is to it. If we run our application again, our ticket list will be showing because that is the default state of our page. However, we can't toggle anything yet!

In the next lesson, we'll learn how to update our local state with an event.