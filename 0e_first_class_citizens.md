In JavaScript, functions are **first class citizens**. But what exactly does this mean and why is this important for functional programming?

Very loosely, this just means we can use a function in the same way we use a variable. In this lesson, we'll go over some of the things we can do in JavaScript that make a function "first class."

## Assigning Functions as Arguments
---

We can pass a function into another function as an argument. We've done this kind of thing before; this is known as a **callback**. Callbacks are extremely common in JavaScript. Try the following code in the Node REPL:

```js
function add(num1, num2) {
  return num1 + num2;
}

function printResult(sum) {
  return `The value of this equation is ${sum}.`
}

printResult(add(5, 7));
```

Our `add()` function simply sums two numbers. Our `printResult()` function simply takes a number and places it in a string. (The example above uses a [template literal](https://www.learnhowtoprogram.com/intermediate-javascript/test-driven-development-and-environments-with-javascript/es6-template-literals).)

We can pass our `add()` function into our `printResult()` function just like we could pass any other variable in as an argument.

While the example above may not seem particularly useful, we could potentially use a method like `printResult()` with many different mathematical functions such as `multiply()`, `subtract()` and so on. This serves two potential benefits:

* It would DRY up our code if we had many mathematical functions and we always wanted to put the result inside a string.
* It creates better separation of logic in our code. One function concerns itself with mathematical equations while the other includes display logic for showing the result to a user.

## Assigning Functions to Variables
---

Because functions are first class, they can be also assigned to variables. Here's an example. Open the Node REPL and type in the following code:

```js
const funkyVariable = function(arg) {
  return arg;
}

funkyVariable("Hello!");
```

In the example above, we store an anonymous function inside `funkyVariable`. We could call `funkyVariable`, but this will just return the function itself. However, we can add parens to a variable storing a function to actually call the function.

This is also known as a **function expression**. A function expression is when an anonymous function (that is, a function that doesn't have a name) is stored inside a variable to be called later.

When would we want to store a function inside a variable? Well, variables are a convenient way to hold information and pass it around. We have the flexibility of a variable (if we call `funkyVariable`) combined with the power of a function (if we call `funkyVariable()`).

## Functions Returning Functions
---

We are used to functions returning values. However, we can also use a function to return another function. A function that returns yet another function is known as a **higher order function**. Functions that take other functions as arguments are also higher order functions as well. Try this example in the Node REPL:

```js
function doAThing() {
  return function() {
    return "A thing was done."
  }
}
```

However, if we call `doAThing()` in the REPL, Node returns `[Function]`. (If we call it in the Chrome console, we'll see that it returns the actual function.) This is because we've called the outer function but we haven't called the inner function yet. To call the inner function, we need to do the following:

```js
> doAThing()()
'A thing was done.'
```

The first set of parens is used to call the `doAThing()` method, which returns a function. The second set of parens calls the inner function.

The example above probably seems a little senseless — why bother to have one function return another? However, this leads to a very powerful programming tool called a **closure**. We will cover closures in the next lesson.

In this lesson, we covered three important ways functions are first-class citizens in JavaScript. While it may not be clear yet why this is useful, JavaScript couldn't be used as a functional language without the techniques described above. Because JavaScript contains this functionality, it has tremendous power not only as an object-oriented language but also as a functional language.
