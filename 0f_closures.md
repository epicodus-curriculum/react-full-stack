In this lesson, we'll cover another concept central to functional programming: **closures**.

Closures are a challenging concept at first. However, understanding what they are and how they work is essential to your development as a coder.

In fact, closures are so important that prominent JavaScript developer and author Eric Elliott describes the concept as the "$40,000 question." According to Elliott, if a potential employee can't answer questions about closures, it could cost them 40k in salary — or even the job. Check out his post on [closures](https://medium.com/javascript-scene/master-the-javascript-interview-what-is-a-closure-b2f0d2152b36) for more.

## Closures
---

A closure is an inner function that has access to variables from an outer function. In JavaScript, closures are created every time a function is created, at function creation time.

Here's an example. We'll create a `welcome()` function with a custom salutation and name. Try putting this example in the console.

```javascript
function welcome(salutation) {
  return function(yourName) {
    return `${salutation}! Nice to meet you, ${yourName}!`
  }
}
```

The `welcome()` function is the outer function. It takes an argument (a specific salutation) and then returns an anonymous inner function. The inner function also takes an argument: `yourName`.

When the anonymous inner function is called, it will return a string that includes both the `salutation` and `yourName`. In other words, the inner function has access both to the variable declared inside it (`yourName`) and the one declared in the outer function (`salutation`).

We've now created a closure, but how do we use it? We need to assign it to a variable so it can be used elsewhere:

```javascript
const heyThere = welcome("Hey there");
```

So what exactly is happening here? We're calling `welcome("Hey there");` and _returning_ the inner function, which is stored inside the variable `heyThere`. We can see this by checking the value of `heyThere` in the console:

```javascript
function(yourName) {
  return `${salutation}! Nice to meet you, ${yourName}!`
}
```

The return value of the outer function is the inner function. In other words, the `heyThere` variable is just _referencing_ the outer function. To actually call the inner function, we need to add `()`. So let's call the `heyThere` function in the console and see what happens:


```javascript
> heyThere()
"Hey there! Nice to meet you, undefined!"
```

Let's set aside the `undefined` for a moment. The key takeaway here is that the inner function still has access to `salutation`, which was declared in the outer function. The variable `yourName` is still `undefined` because the inner function takes `yourName` as an argument, so we need to pass it in.

We can pass in a value for `yourName` by passing in an arugment to the `heyThere` function, like this:

```js
> heyThere("Joe")
"Hey there! Nice to meet you, Joe!"
```

This may not seem very useful, but our original function is very flexible. We can use it with other greetings:

```javascript
> const spanishGreeting = welcome("Buenos días!");
> spanishGreeting("Joe");
"Buenos días! Nice to meet you, Joe!"
```

This reusability is a large part of what makes closures so powerful. We will explore this further when we discuss currying in the next lesson.

As we can see in the examples above, our inner function has access to variables in both the inner and outer function. Even after the outer function has been called, the inner function continues to have this access. This is where the term closure comes from: the ability of the function to _close over_ the variables, keeping them in the function's scope. In many languages (C being an example), variables are destroyed as soon as the outer function is completed. As a result, these languages can't use this powerful functionality.

Let's look at one more example of a closure. This time, we will create a function that we can reuse to multiply a number by different values.

```js
const multiplier = (numberToMultiplyBy) => {
  return (numberToMultiply) => {
    return numberToMultiplyBy * numberToMultiply;
  }
}
```

Now we can do the following:

```js
const doubler = multiplier(2);
const tripler = multiplier(3);
const quadrupler = multiplier(4);
```

Once again, let's go into how this works. With `doubler`, we pass in `multiplier(2)`. The `doubler` variable now stores a function that looks like this:

```js
(numberToMultiply) => {
  return 2 * numberToMultiply;
}
```

You can check this out for yourself in the console. This works because the inner function retains the value of the argument passed into the outer function — which in turn is stored in the `doubler` variable. This inner function will be invoked when we call `doubler()`.

`tripler` and `quadrupler` work in exactly the same way. The difference is that we are passing in a different argument to the outer function, which is being stored in the inner function.

So `tripler` stores the following information: 

```js
(numberToMultiply) => {
  return 3 * numberToMultiply;
}
```

Meanwhile, `quadrupler` looks like this:

```js
(numberToMultiply) => {
  return 4 * numberToMultiply;
}
```

Closures can be a very confusing concept at first. However, we've actually used closures before. Consider this example:

```js
function howManyEvenNumbers(userInputArray) {
  let instances = 0;
  userInputArray.forEach(function(element) {
    if (element % 2 === 0) { 
      instances++; // we have access to `instances` thanks to closures
    }
  });
  return instances;
}

howManyEvenNumbers([2,3,4,5,6,7]);
// returns 3
```

The callback passed to `Array.prototype.forEach()` is an example of a closure. The inner, anonymous callback function has access to the variables declared in the outer function, `howManyEvenNumbers()`.

If you do get asked about closures in an interview, remember to mention that callbacks are an example of an inner function having access to an outer function's scope. However, not all callbacks are examples of closures. Consider this example:

```js
const myLogger = () => { 
  console.log(myNum) // we're hoping that `myNum` will be defined
}

const myCounter = (functionParam) => {
   const myNum = 1;
   functionParam(); // not a closure! Just a callback
}

myCounter(myLogger);
// error! myNum is undefined 
```

In this case, since `myLogger()` was not defined inside of `myCounter()`, it doesn't have access to any of the variables declared in `myCounter()`.

Ultimately, closures are a powerful technique in both functional programming and programming in general. In the next lesson, we will see how we can combine closures with currying to create reusable, modular code and **function factories**, which are essentially functions we can use to churn out many other helpful functions.

Closures are also commonly used for enclosing private data, a use case we won't be exploring in depth. To learn more, see the [Mozilla documentation on closures](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures).

By the way, if you still don't understand closures after reading this lesson, don't worry. We will get more practice working with them over upcoming lessons. This is also one of those concepts that usually doesn't click at first — there's a reason that this is one of the things that separates junior and midlevel developers!
