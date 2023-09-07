In the last lesson, we added a button with an event handler as well as a function that triggered a change in local state.

We also mentioned that we need to use arrow notation for our `handleClick()` method. In this lesson, we'll explain how method binding works and why we need it in our application.

Let's take another look at our `handleClick()` method. We'll make a small update to our code to intentionally break it. Specifically, we'll remove the arrow notation from `handleClick()` so it looks like this:

```js
  handleClick(){
    this.setState(prevState => ({
      formVisibleOnPage: !prevState.formVisibleOnPage
    }));
  }
```

If we run our application with `npm start` and then click the "Add Ticket" button, we'll get the following error:

```javascript
TypeError: Cannot read property 'setState' of undefined
```

Here is the problem. React uses the `render()` method to create elements in the DOM with event handlers. Let's revisit the code we use to create our button's event handler.

```jsx
<button onClick={this.handleClick}>{buttonText}</button>
```

React will use this code to create a button that has a click handler attached. This button lives in the DOM and has no knowledge of its original context (the class where the `handleClick()` method was created). When a function loses its context (or doesn't have a specific context to begin with), `this` can be one of two things:

* If JavaScript isn't in `strict` mode, `this` refers to the global window.
* If JavaScript is in `strict` mode, `this` becomes undefined.

`strict` mode is exactly what it sounds like — a stricter mode of JavaScript. (For more information, see the [documentation on strict mode](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Strict_mode)). We haven't explicitly used it in the past but ES6 classes automatically use `strict` mode, which means that if a function loses its context, it will be `undefined`.

In order to fix this problem in our application, we have to "bind" our function so it "remembers" what `this` should be. Traditionally, React used JavaScript's `bind()` method to solve this problem. Here's how we'd need to modify our code to work with `bind()`:

<div class="filename">src/components/TicketControl.js</div>

```js
class TicketControl extends React.Component {

  constructor(props) {
    super(props);
    this.state = {
      formVisibleOnPage: false
    };
    this.handleClick = this.handleClick.bind(this); //new code here
  }

  handleClick() {
    this.setState(prevState => ({
      formVisibleOnPage: !prevState.formVisibleOnPage
    }));
  }

...
```

Here's the specific line of code that binds our function:

```js
this.handleClick = this.handleClick.bind(this);
```

The code in the line above states that whenever `this.handleClick` is called, it should have the current context of `this` bound to it. Because this line of code is inside the constructor, `this` is an instance of the class itself, which is exactly what we need. That way, when `this.handleClick` is called in the DOM, our application still knows what `this` should be.

Fortunately, we don't need to use `bind` anymore because arrow functions automatically bind functions to their lexical scope. That means we can do the following:

```js
  handleClick = () => {
    this.setState(prevState => ({
      formVisibleOnPage: !prevState.formVisibleOnPage
    }));
  }
```

Because this method uses arrow notation, `this` is automatically bound to its current lexical scope, which is an instance of the class itself. Once again, that's exactly what we need — and takes less code!

This concept of binding may seem confusing at first. The ways in which `this` changes context can be confounding even for experienced developers. You do not need to have an advanced understanding of this concept to use React, though. In fact, you don't really need to worry about this concept at all as long as you use arrow functions so any methods you write are properly bound so that callbacks keep their context in the DOM.

There's one other important thing to note: this issue with binding functions has nothing to do with React. It's a JavaScript thing. Having a better understanding of binding `this` will improve your understanding of JavaScript fundamentals.