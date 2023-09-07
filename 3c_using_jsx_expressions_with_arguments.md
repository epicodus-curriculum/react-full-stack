Before we continue, let's discuss an important gotcha related to evaluating JavaScript functions in JSX curly braces.

Let's look at an example. If we were to attach an `onClick` handler to a JSX div, it might look something like this:

```jsx
<div onClick={ doAThing }>Click This Button To Do A Thing</div>
```

`doAThing` is a callback so it doesn't have parens. Let's say we were to add parens:

```jsx
<div onClick={ doAThing() }>Click This Button To Do A Thing</div>
```

This will not have the intended effect. Now `doAThing` will be invoked immediately when the DOM is updated — instead of waiting for a click event as it should.

We've seen this kind of behavior before. Here's an example of a function where we might want to pass another function in as a callback:

```js
function thisTakesACallbackAsAnArgument(thisIsAFunction) {
  const argumentToFunction = // some computed value
  return thisIsAFunction(argumentToFunction);
}
```

In the example above, `thisIsAFunction` should take an argument — but we don't know what that argument should be until we calculate the variable `argumentToFunction`. We can't pass in `thisIsAFunction()` as an argument to the outer function because it will be invoked immediately. Instead, because `thisIsAFunction` is a first-class citizen, we can pass it around like a variable until we are ready to invoke it by adding parens (in this case, parens that include the argument `argumentToFunction`).

The example above is contrived — but it's similar to what's going on when we evaluate functions and methods inside JSX curly braces. Since these functions are usually connected to an event handler, we don't want them to be invoked immediately. We want them to wait until a user does something.

However, what if we need to pass an argument to a JavaScript function in curly braces? Let's say that `doAThing` needs to take `someOtherThing` as an argument. We can't do the following because it will be invoked immediately:

```jsx
<div onClick={ doAThing(someOtherThing) }>Click This Button To Do A Thing</div>
```

We only want this function to be invoked on a click event so we need to do the following:

```jsx
<div onClick={ () => doAThing(someOtherThing) }>Click This Button To Do A Thing</div>
```

`() => ` is an anonymous arrow function with no parameters. You may wonder how in the world this will solve our problem. Well, let's take a look at a simpler example:

```js
const hey = () => "Hey there!"
```

If we check the value of `hey` in the console, it's `() => "hey there"`. The `hey` variable is storing our anonymous function.

To invoke it, we need to do the following:

```js
hey()
"Hey there!"
```

In other words, the `() =>` syntax is just another way of creating a function literal.

Let's look at another example:

```js
function heyThere(name) {
  return `Hey ${name}!`
}

const dontInvokeYet = () => heyThere("Jasmine")
const invokeNow = heyThere("Jasmine")
```

Try this out in the console. Our `heyThere` function needs to take a `name` as an argument now — so we have to add parens to the function.

`dontInvokeYet` stores the function (because we use `() => `). We can invoke it later by calling `dontInvokeYet()`. The value of the `dontInvokeYet` variable is `() => heyThere("Jasmine")`

`invokeNow` will call the function immediately and store it in the `invokeNow` variable. The value of the `invokeNow` variable is `"Hey Jasmine!"`

Applying this to our JSX example, we want the value of `onClick` to be set to a function that should be evoked later, not now.

So while the syntax may look a little strange at first, remember that it's just JavaScript, not React. We always need to make sure that any event handlers being evaluated with JSX are invoked later (when an event is triggered), not immediately (when the component is rendered). React can only do this because JavaScript functions are first-class citizens. It's another way in which React leverages the functional programming power of JavaScript.