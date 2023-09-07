When we program in a functional style, the functions we write must always be **pure**. Let's take a look at what makes a **pure function**.

* **Given a specific input, a pure function always returns the same output.**

Here's a very basic example:

```js
function addOne(num) {
  return num + 1;
}
```

In the example above, no matter how many times we pass in the value `7` as an argument, the function will always return `8`.

In contrast, we can't have a function that returns a random value:

```js
function guessMyNumber(num) {
  const myNumber = Math.floor(Math.random() * Math.floor(10) ) + 1;
  if (num === myNumber) {
    return "You guessed right!"
  } else {
    return "That wasn't it."
  }
}
```

The function above takes a number as an argument. We then use `Math.random()` to generate a number between 1 and 10. If the random number matches the number passed in as an argument, the function returns `"You guessed right!"` Otherwise, the function returns `"That wasn't it."`

This function isn't pure because we can get different results with the same input. For example, if we input `7`, sometimes we'll get one result and sometimes we'll get the other.

* **A pure function always returns an output.** We've already implied this, but let's make it explicit here: a pure function has to return something. A pure function can't always return the same output for a given input if it doesn't return anything at all!

* **A pure function can not have side effects.** A side effect is when a function changes something in the application outside of the function. Pure functions should only return a single thing. They should not alter anything else in the application.

We wrote many functions in JavaScript that had side effects. For instance, UI functions that change something in the DOM have side effects:

```js
function addWord(word) {
  document.getElementById("typing-box").append(word);
}
```

The function above appends a word to the DOM, which means it's altering something outside of the function. In fact, the function above is entirely side effects — there is no return value. It's definitely not a pure function!

* **Pure functions cannot rely on external variables or state.** If a pure function could rely on external variables or state, then this could affect the return value of the function. Let's use pig dice as an example. Let's assume we have the following function inside a `Player` class:

```js
class Player {

  constructor() {
    this.totalScore = 0;
  }

  tallyScore(roundScore) {
    this.totalScore = this.totalScore + roundScore;
    return this.totalScore;
  }
}
```

In the example above, when a player object is instantiated, it begins with a `totalScore` of `0`. Then, when we call the `tallyScore()` method on the player, the player's `totalScore` will be incremented. Because the `totalScore` changes throughout the game, this function can return different results with the same input. For instance, if we input `7`, the function will return `33` if the player's `totalScore` was already `26`, but it would return `7` if the player didn't have any points yet.

It should also be clear that the `tallyScore()` function has side effects because it alters external state — yet another reason this function isn't pure.

The same problems occur with global variables. For example:

```js
let score = 0;

function incrementScore(points) {
  score += points;
  return score;
}
```

We learned in Introduction to Programming that this kind of code can be problematic. In a large code base, it could be very difficult for developers to know which functions are changing those global variables and how those global variables in turn are affecting the rest of the application. The application would be hard to test and have a higher potential for bugs.

The code snippet above breaks most rules for a pure function. It can return different values even if the input is the same, it relies on external application state, and it has side effects.

### Benefits of Pure Functions

Pure functions are one of the basic tenets of functional programming, but why are they beneficial? Why does it matter if a function returns a consistent value, doesn't have side effects and so on?

* **Pure functions are easier to test.** One input always returns the same output. If our tests pass, we know our code is probably good to go. Our pure functions will not alter other parts of the codebase, which means we can keep our tests simple instead of needing to account for other factors such as changing state.

* **What You See Is What You Get.** When we look at a pure function, we know the function has no side effects and can't be altered by external state. For that reason, we can look at a pure function in isolation without needing to know about the rest of the application.

* **Pure functions result in fewer bugs.** If a pure function is easier to test, easier to reason about, and works independently of the rest of the code base, we can more easily isolate and prevent bugs. This is especially important in a larger codebase.

We must always write pure functions when using functional programming. In fact, even when we take an object-oriented perspective, we can benefit from writing pure functions.
