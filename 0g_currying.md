**Currying** is an essential functional programming concept named after the mathematician Haskell Curry.

When we **curry** a function, we take a function with multiple arguments and then rewrite it as a series of functions, each with one argument. A function with only one argument is also known as an **unary function**.

Let's use currying to write a function to display our favorite (and not-so-favorite) things.

Here's how the uncurried function might look:

```js
function aThingIMaybeLike(howMuchILikeIt, thing, reason) {
  return `I ${howMuchILikeIt} ${thing} because ${reason}.`;
}
```

We could then call it like this:

```js
aThingIMaybeLike("really like", "functional programming", "it's cool");
```

This will return `"I really like functional programming because it's cool."`

We can curry this function by creating a series of nested anonymous functions inside the `aThingIMaybeLike()` function:

```js
function aThingIMaybeLike(howMuchILikeIt) {
  return function(thing) {
    return function(reason) {
      return `I ${howMuchILikeIt} ${thing} because ${reason}.`;
    }
  }
}
```

Each of these functions now take a single argument. In order to use this code, we need to do the following:

```js
aThingIMaybeLike("really like")("functional programming") ("it's cool")
```

In the snippet above:

* Our outer function `aThingIMaybeLike(howMuchILikeIt)` takes `"really like"` as an argument. When the function is invoked, it calls the first inner function `function(thing)`.
* Our first inner function `function(thing)` is then invoked with the second argument `"functional programming"`. It, too, returns a function: the innermost anonymous `function(reason)`.

* Finally, our innermost function returns the string `"I really like functional programming because it's cool."`

However, what's the point? Didn't we just write additional code to essentially do the same thing?

In the short term, yes. However, our curried function has additional powers: it's both more reusable and more flexible.

For instance, we can do the following:

```js
const thingsThatBugMe = aThingIMaybeLike("do not like");
```

Now we can call this with the inner two arguments:

```javascript
thingsThatBugMe("global variables")("they are a code smell");
> 'I do not like global variables because they are a code smell.'
thingsThatBugMe("functions with side effects")("they break code");
> 'I do not like functions with side effects because they break code.'
```

We have additional flexibility to do this with multiple arguments as well:

```js
const reasonILoveCoding = aThingIMaybeLike("love")("coding");
```

If we try this out in the REPL, we'll see we can call our new function with just a single argument.

```javascript
> reasonILoveCoding("it is fun");
'I love coding because it is fun.'
> reasonILoveCoding("I enjoy problem-solving");
'I love coding because I enjoy problem-solving.'
```

Let's briefly return to our original function that takes three arguments:

```js
function aThingIMaybeLike(howMuchILikeIt, thing, reason) {
  return `I ${howMuchILikeIt} ${thing} because ${reason}.`;
}
```

This method may be fewer lines but it doesn't have nearly as much flexibility as our curried version. It has no reusability while we were able to use our curried function to create new functions that use 1, 2 or 3 arguments.

Over the next few class sessions, try writing unary functions. Because each function should take only one argument, you will need to curry functions that would otherwise take multiple arguments. Note that you won't always be able to create an unary function. However, if a function takes too many arguments, that may also be a sign that it's trying to do too much.

Currying is another complex concept that often takes some time to absorb. Don't worry — you will get more practice and we will cover more use cases in upcoming lessons.