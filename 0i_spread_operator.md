One of the most popular features of ES6 is the **spread operator**. Spread syntax looks like this: `...`.

There are multiple uses for the spread operator, but we are going to focus on two use cases:

 * Making copies of objects;
 * Merging multiple objects together.

At the end of the lesson, we'll also briefly cover the JavaScript method `Object.assign()`. This method operates in a similar fashion to the spread operator. We'll also cover a few other uses of the spread operator as well.

### Copying Objects

Copying objects is a very common thing to do in functional programming. We will dive into that more in an upcoming lesson on creating deep clones.

Let's take a look at how we can use the spread operator to copy an object. Try this out in the REPL:

```js
const myCat = {
  name: "Murphy",
  age: 1
}

const anotherCat = {...myCat};
```

If we look at the value of `anotherCat`, we will see that it has the same properties as `myCat`.

Let's say we want to make a copy of `myCat` and also update the `age` property in the new object. We can do this:

```js
const myCat = {
  name: "Murphy",
  age: 1
}

const myCatGotOlder = {...myCat, age: 2}
```

In the example above, we create a new object using the spread operator and then specify which properties should be different. If we try the example in the REPL, the value of `myCatGotOlder` is `{ name: 'Murphy', age: 2 }`.

We can also add new properties to the copy of the object as well — they don't have to be properties that existed on the object being copied. Here's an example:

```js
const myCat = {
  name: "Murphy",
  age: 1
}

const myCatGotOlder2 = {...myCat, age: 2, color: "gray and white"}
```

The value of `myCatGotOlder2` will be `{ name: 'Murphy', age: 2, color: 'gray and white' }`.

### Merging Objects

We can also use the spread operator to merge multiple different objects. Let's take a look at an example:

```js
const flagColor1 = {
  color1: "green"
}

const flagColor2 = {
  color2: "gold"
}

const flagColor3 = {
  color3: "black"
}

const jamaicanFlag = {...flagColor1, ...flagColor2, ...flagColor3}
```

Now if we check the value of `jamaicanFlag`, we'll see that it is `{color1: "green", color2: "gold", color3: "black"}`. All three objects have been merged into one! Be careful, however. If several objects have the same property, then the last object that has that property will override the previous value. For instance, let's say that all three of the objects above just had a `color1` property:

```js
const flagColor1 = {
  color1: "green"
}

const flagColor2 = {
  color1: "gold"
}

const flagColor3 = {
  color1: "black"
}

const jamaicanFlag = {...flagColor1, ...flagColor2, ...flagColor3}
```

Now if we type in `jamaicanFlag`, it will return `{color1: "black"}`.

### Object.assign()

The spread operator works very similarly to the JavaScript method `Object.assign()`, which is also used to copy and merge objects. You should be familiar with this method as well — and you may even prefer to use it over the spread operator.

For instance, if we wanted to create a `jamaicanFlag` object using the variables above, we'd do the following:

```javascript
const jamaicanFlag = Object.assign({}, flagColor1, flagColor2, flagColor3);
```

`Object.assign()` can take multiple arguments. The first argument is the object that should be copied or merged. If we wish, we can pass in an empty object `{}` as the first argument, which we do in the example above. Then each additional argument is an object that should be merged into the original. We could've also done the following:

```javascript
const jamaicanFlag = Object.assign(flagColor1, flagColor2, flagColor3);
```

In this example, we omit the empty object `{}`. However, it is fairly common to see `Object.assign()` with an empty object as the first argument.

### Issues with Copying JavaScript Objects

We want to copy objects in functional programming so our code remains immutable. Each time we modify a variable, we are introducing mutability into our application. Copying objects into other constants ensures that each variable is immutable.

There's just one problem: these methods create a **shallow clone** of objects. That means that they don't actually create a new object in memory; they are still referencing the original object. In order for our applications to be truly immutable, we need to create a **deep clone** of objects. Unfortunately, JavaScript doesn't have native support for deep clones. Instead, we need to use an awkward combination of methods or an external library to do that. We will cover this in a future lesson.

### Other Uses for the Spread Operator

While we will be focused on the use cases above during this course section, the spread operator is useful in other ways as well. Here are two more things we can do with the spread operator:

We can use it to combine arrays:

```js
const array = [1,2];
const array2 = [3,4];
const array3 = [...array, ...array2];
array3
[1, 2, 3, 4]
```

We can use it to pass arguments into functions. The example below will pass all arguments from the array into the function — **as separate arguments, not as an array**.

```js
const array = [1,2,3];
spreadArgs(...array);
```

### Conclusion

In this lesson, we covered copying and merging objects with both the spread operator and `Object.assign()`. These JavaScript techniques are very important and are also commonly used in React.

The spread operator also has other uses as well. Check out Mozilla's [spread syntax documentation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax) for more.
