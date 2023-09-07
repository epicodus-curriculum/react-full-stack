**Immutability** is an essential part of functional programming. In fact, some developers would argue that immutability is the most important aspect of functional programming — and that we can't call code functional if it isn't immutable.

But what is immutability? If something is immutable, it cannot change. Conversely, if something is mutable, it can be changed. Breaking this down further, this means that we can't change values in variables, objects, or even arrays. Instead, we need to create a new variable, object, or array.

Let's start with a simple example. From an object-oriented approach, it's okay to do the following:

```js
let x = 1
x = 2 + 1
```

In the example above, we use `let` because the value of `x` will change. We then modify the value of `x` by adding another number to it. This variable is mutable because it changes.

From a functional approach, we'd need to do something different:

```js
const x = 1
const newX = 2 + x
```

Note that we are using `const` instead. When writing in a functional style, we will use `const`, not `let`.

Instead of assigning a new value to `x` like we did in the first example, we create a new variable and assign the new value to it. Now both variables are immutable because neither can change.

Many developers argue that functional programming is preferred over object-oriented programming in part because of this focus on immutability. Think about it this way. If we see `let x` in a large codebase, we know that `x` is probably going to change somewhere — and maybe even in many places. As a result, it's potentially unreliable — what if `x` is the value we expect in one part of the code but then mutates into something unexpected elsewhere in the code? That could lead to a bug.

However, when we see `const x`, we can be assured that we know what the value of `x` is. It will not change elsewhere in the code. It becomes easier to communicate our intentions to other developers and to prevent bugs.
