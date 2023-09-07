One common and powerful functional programming technique is called **recursion**. Recursion is simply a function that calls itself. In this lesson, we will talk about how we can use recursion instead of loops such as `forEach` or `for`. Knowing how to write a recursive function is an extremely important part of coding. It is also a common whiteboard question in a technical interview. Even if you aren't asked to write a recursive function in a technical interview, you can solve a problem that involves using a loop both with a traditional loop _and_ recursively — a great way to show potential employers that you know your stuff!

## Writing a Recursive Function
---

Let's start by looking at a very simple example of a recursive function. This function will increment a counter 3 times. We could easily do this with a `for` loop like this:

```javascript
let counter = 0
for (let i = 0; i < 3 ; i++ ) {
  counter += 1
}
```

Once this loop is complete, the value of `counter` will be `3`.

Now let's solve this same problem recursively:

```js
const incrementCounter = (counter) => {
  if (counter >= 3) {
    return counter;
  } else {
    console.log(counter);
    return incrementCounter(counter + 1);
  }
}

incrementCounter(0);
```

Our recursive function includes two conditionals. The first includes what is called the **base case**. This is the final condition where our function will stop calling itself. If we don't have a base case, a recursive function will call itself forever.

The second conditional includes the recursion itself. Each time this conditional is reached, the function will call itself. The argument we pass in is `counter + 1` because we want the new value of the counter to be incremented by one.

We've also added a `console.log()` so you can see the incrementing value in the console. Now, if you put this recursive function in the terminal, you'll see each value logged when you call `incrementCounter(0);`.

However, be careful! Recursive functions should always include one more thing: a **termination case**. This case determines what the function should do if something goes wrong. For instance, if we were to pass a string into this function as it's currently written, the function will create an infinite loop because the function will just concatenate another `1` to the string each time it's called.

Let's add a termination condition now:

```javascript
const incrementCounter = (counter) => {
  // This is the termination condition.
  if (isNaN(counter)) {
    return;
  }
  if (counter >= 3) {
    return counter;
  } else {
    console.log(counter);
    return incrementCounter(counter + 1);
  }
}
```

As we can see, we added a conditional to see if `counter` is `NaN`. 

To recap, there are three main parts to a recursive function:

* **Base case**: The final condition of a successfully called recursive function.
* **Termination case**: A conditional that's called if something goes wrong. This prevents an infinite loop.
* **Recursion**: This part of the function is where the magic happens and the function calls itself.

There's another very important aspect of recursive functions that can be difficult to understand at first. Each time we call `incrementCounter()` again, we are still in the process of calling the previous function. When we are calling the functon for the third time, both the first and second function call are still in process.

```js
incrementCounter() {
  // This call will complete last.
  return incrementCounter() {
    // This call will complete second.
    return incrementCounter() {
      // This call will complete first.
    }
  }
}
```

Note the order that the functions above will be called in. This is because all functions in process go on the JavaScript stack. The stack is a place where all the actions that need to be completed are stored. The JS stack works on the principle of **LIFO**, which means "last in, first out." This is why the innermost function in the example above will complete first while the outermost function will complete last.

Let's look at another implementation of a recursive function. In the process, the concept of LIFO should become clearer. We'll create a function that recursively does the same thing as `reverse()`. When we input a word into our function, it will be returned to us backwards.

```js
const recurseReverse = (string) => {
  if (string === "") {
    return "";
  } else {
    return recurseReverse(string.substring(1)) + string[0];
  }
}
```

In the example above, our `recurseReverse()` function takes a `string` as an argument. If the `string` is equal to `""`, it will return `""`. This is our base case. Otherwise, it will call `recurseReverse()` again. We will pass `string.substring(1)) + string[0]` into our function. In the example above, the `substring()` method will return all characters in the string except for the first (denoted by the `1` passed in as an argument).

Let's see what will happen if we call `recurseReverse("fern")`.

We call our function:

```javascript
string = "fern"
```

We reach our return statement and call `recurseReverse()` again:

```javascript
recurseReverse("ern") + "f";
```

This is **not** the same as returning `"ern" + "f"`, so don't get tripped up here. Also, remember that we still haven't completed any of our functions calls yet — we are still recursively calling the function and adding a letter to the end each time.

We call it yet again:

```javascript
recurseReverse("rn") + "e";
```

The third time through, we get this result:

```javascript
recurseReverse("n") + "r";
```

The next time, we will get the following:

```javascript
recurseReverse("") + "n";
```

This alerts our function to stop recursing and to return the string `""`.

We now have multiple nested function calls. Our innermost function call will return first, followed by the second-innermost function call, and so on until we reach the outermost function call. Each function is returning a letter or an empty string like this:

```js
recurseReverse() {
  return "f";
  recurseReverse() {
    return "e";
    recurseReverse() {
      return "r";
      recurseReverse() {
        return "n";
        recurseReverse() {
          return "";
        }
      }
    }
  }
}
```

Because the JavaScript stack is "last in, first out," the letters will be concatenated in this order:

```javascript
"" + "n" + "r" + "e" + "f"
```

Because we reached our base case (an empty string), our function stops calling itself and returns an empty string. If we didn't have that base case, our function would keep calling itself until the maximum callstack is exceeded.

In short, "fern" is spelled backwards.

### Tail Call Optimization

If you accidentally create an infinite loop, you'll get the following error: `Maximum call stack size exceeded`. This is because there is a limit to how large the stack can be.

The way JavaScript handles the call stack is a problem when it comes to using recursive functions. This is because the stack keeps getting bigger and bigger until the base case is reached. For more complex equations, the stack will get too big and we'll get an error. Even if the stack doesn't get big enough to throw an error, a recursive function will still be less performant for this reason.

Functional languages deal with this issue by using what is called **tail call optimization**. With tail call optimization, the return value of a function is computed instead of allocating the entire function to the stack. This saves memory and makes recursive functions much more efficient. ES6 adds functionality for tail call optimization, but unfortunately it doesn't work with browsers — and it's not clear if browsers will ever add support for this. Node, on the other hand, can utilize tail call optimization.

There is an alternative that is beyond the scope of this lesson — writing what is called a **trampoline function**. A trampoline function wraps a recursive function in a loop and breaks it down so each function isn't heaped on the stack.

At this point, we are ready to start writing our own recursive functions. The principle of LIFO should also be clear. Writing recursive functions is hard at first — keep practicing! When you need to iterate in your applications during this course section, try using recursion in addition to the usual suspects such as `map()`.
