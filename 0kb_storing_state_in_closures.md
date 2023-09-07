Now that we've had some practices working with closures, it's time to explore another cool thing we can do with closures. We can use them to store basic info about state.

Let's take a look at how we can use a closure to store basic state. We'll create a function that increments a counter by 1 to demonstrate how this works:

```js
const counterFunction = () => {
  let counter = 0;
  return () => {
    counter ++;
    return counter;
  }
}
```

In the example above, the outer function `counterFunction()` stores a variable `counter` which is set to `0`. Note that we use `let` here because we will be modifying the value of the counter.

Our `counterFunction()` function returns an anonymous function that increments the value of `counter` and then returns its value. (We won't worry about the fact that we aren't mutating this in a functional way since this is merely for demonstration purposes.) The inner function has access to the `counter` variable due to **lexical scope**. Lexical scope means that an inner function has access to the variables of any outer functions that surround it.

Now we can do the following:

```js
const incrementer = counterFunction();
```

If we check the value of `incrementer` in the console, we'll see that it stores our inner function:

```js
() => {
  counter ++;
  return counter;
}
```

We can't see the value of `counter` but we know that `incrementer` has access to it due to lexical scope. So what happens if we call this function multiple times?

```javascript
> incrementer()
1
> incrementer()
2
> incrementer()
3
```

Each time we call `incrementer()`, it modifies the value of `counter`. Since `incrementer()` is inside the outer function's lexical scope, it remembers the value of `counter`. However, the outer function isn't being called again so `counter` never falls out of scope (nor is it reassigned) regardless of how many times we call in the inner function.

What would happen if we created another incrementer and then called our first incrementer again?

```javascript
> const incrementerTwo = counterFunction();
> incrementerTwo()
1
> incrementerTwo()
2
> incrementer()
4
```

`incrementerTwo` creates a _new_ lexical scope that doesn't affect the value of `counter` in `incrementer`. In other words, we could store an indefinite number of counters in functions using our closure.

In the next lesson, we'll build a small application that will use a closure to store state.
