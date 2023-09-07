Let's use all the concepts we've learned so far to build a simple application. In this application, a user is growing a plant. The plant needs water, soil, and sunshine. The user may increment a plant's values through the functions `hydrate()`, `feed()`, and `giveLight()`.

## An Object-Oriented Approach
---

Before we take a functional approach, let's take a quick look at how we might implement this small program from an object-oriented perspective:

```js
class Plant {
  constructor() {
    this.water = 0;
    this.soil = 0;
    this.light = 0;
  }

  hydrate() {
    this.water ++
  }

  feed() {
    this.soil ++
  }

  giveLight() {
    this.light ++
  }
}
```

To create and hydrate a plant, we'd do the following:

```javascript
> let plant = new Plant();
> plant.hydrate()
> plant
Plant {water: 1, soil: 0, light: 0}
```

The program above is simple and easy to read. However, from a functional perspective, there are several problems with this approach. Let's discuss these issues. As we do, keep in mind that both functional programming and object-oriented programming have many advantages. We are not advocating for one approach over the other — the most important thing is to use the right tool for the job. Many developers incorporate aspects of both object-oriented and functional programming.

### Problems With Our Object-Oriented Approach

From a functional perspective, though, this application is not very flexible and may be prone to bugs. Let's look at why:

**All methods must be called on instances of the `Plant` class.** While these methods are nicely encapsulated, they aren't very reusable. If we wanted instances of other classes to have the same functionality, we'd need to use inheritance — or have repeated code elsewhere. 

From a functional approach, we'll want to use composition instead. That means we won't use a class. It also means that our methods will be more modular and reusable.

**Our methods have side effects.** Remember that pure functions can't have side effects. Our methods definitely have side effects — they are altering the state of a plant object!

**Our methods mutate state.** Not only do our methods have side effects — they are directly mutating state. While this is common in object-oriented programming, it's a no-no in functional programming.

**Our functions don't return anything.** Pure functions also need to return something. However, none of our methods return anything.

So how should we refactor our application to have a more functional approach?

## A Functional Approach
---

Let's start by creating a reusable function that we can use to `hydrate()`, `feed()` and `giveLight()` to a plant. Specifically, we will create a pure function that isn't in a class. This will solve the problems with the methods above all at once. Because it will be pure, it will have:

* No side effects;
* No state mutation;
* A return value.

Let's do that now.

### Abstracting Our Methods Into a Single Function

Let's start by rewriting our `hydrate()` method.

```js
const hydrate = (plant) => {
  return {
    ...plant,
    water: (plant.water || 0) + 1
  }
};
```

In the example above, we create a function literal. The function takes a `plant` as an argument. It will not mutate state. Instead, it will return a new object that represents the plant's new state. We will use the spread operator to return the new state of the plant. The value of `plant.water` will be set to `(plant.water || 0) + 1`. Note that we use the `||` operator here — if an object doesn't contain a `water` property, then `plant.water` will equal `NaN`. This way, the value default to `0` if there is no `water` property.

In theory, the function isn't mutating state because we are returning a new object. (Unfortunately, in reality, JavaScript is still referencing — and mutating — the original object because it creates a shallow clone, not a deep clone. But we'd need to use a special library to ensure we have a deep clone, so we won't worry about that right now!)

Just like that, we've addressed the primary issues with our `hydrate()` function!

We could then do the same thing for our other two functions. For instance, here's a new `feed()` function:

```js
const feed = (plant) => {
  return {
    ...plant,
    soil: (plant.soil || 0) + 1
  }
};
```

This function pretty much looks the same as the previous one. Also, it doesn't seem very reusable. It can only take an object that has a `soil` property and then increment that `soil` property by 1. Because the methods look so similar, we clearly have a chance to refactor here.

Remember that functional programming is an attempt to make our code more abstract and reusable. This is in contrast to object-oriented programming, which is often about making our code more concrete and encapsulated.

So how can we refactor this method to take in and alter `soil`, `water`, or `light`? We could do the following:

```js
const changePlantState = (plant, property) => {
  return {
    ...plant,
    [property]: (plant[property] || 0) + 1
  }
}
```

Now we are passing in both a `plant` and the specific `property` that we want to change. Note that we can use square brackets to pass the value of a variable into an object key or property. This is a piece of functionality from ES6.

Now we can call this function like this:

```javascript
> let plant = { soil: 0, light: 0, water: 0 }
> changePlantState(plant, "soil")
{soil: 1, light: 0, water: 0}
```

And just like that, we've reduced three functions into one.

While this is a good first step in terms of refactoring, we can do more to make this function reusable and flexible. Here's how our function is still limited:

* It's still specific to plants when we could technically use it to increment any property of any object by 1. We can rename the variables to be more abstract.
* Why should we limit ourselves to just incrementing a plant property by 1? If we were going to turn this into a game, we'd probably want ways to increment (or decrement) different properties in different ways. Our function would be much more flexible if it could do this.
* Finally, this function takes multiple arguments, so it might be nice to turn this into an unary function with just one argument. (Hint: we will need to curry our function to do this.)

Let's handle this one step at a time.

First, we'll make our variables more abstract:

```js
const changeState = (state, prop) => {
  return {
    ...state,
    [prop]: (state[prop] || 0) + 1
  }
}
```

Now our function is no longer limited to just working with plants. It could increment any property of state by 1. Note that we call the property passed in `prop`. `prop` or `props` is a common name for this variable and you'll see it frequently in React.

Our function is gradually improving but it could still be a lot better. Why would we want to limit ourselves to just incrementing a property by 1? Let's refactor our function again. Now it will also accept a `value`:

```js
const changeState = (state, prop, value) => ({
  ...state,
  [prop] : (state[prop] || 0) + value
})
```

This is a very small change but it makes our function even more flexible. In the process, though, we've created another problem. We went from having an unary function with just one argument to a function with three arguments.

How can we solve this problem, too? It's time to curry this function! Our outer function will take just one argument. This outer function will return a second function which takes another argument. Finally, this inner function will return yet another inner function that will take the final argument. Here's how our curried function looks:

```js
const changeState = (prop) => {
  return (value) => {
    return (state) => ({
      ...state,
      [prop] : (state[prop] || 0) + value
    })
  }
}
```

You may be wondering if there's any value in currying this function. Don't we still have to pass in three arguments? Well, there is a method to our madness — or rather, a function to our funny business. Now we can create some function factories!

Note that `prop` is passed into the outer function, then `value` is passed to the inner function, and finally, `state` is passed to the innermost function. We could pass in those arguments in any order we like. However, our current setup isn't accidental. We can now use this function to make some smaller, more specific functions. Here are some examples:

```js
const feed = changeState("soil");
const hydrate = changeState("water");
const giveLight = changeState("light");
```

We just used our function to easily create more specific functions for each kind of property. We could add 5 to the soil of a plant by doing the following:

```js
feed(5)(plant)
```

We could theoretically drill down and get even more specific:

```js
const blueFood = changeState("soil")(5)
const greenFood = changeState("soil")(10)
const yuckyFood = changeState("soil")(-5)
```

Now we can do the following with a plant:

```js
blueFood(plant)
```

This will increase a plant's food level by 5.

None of this flexibility would've been possible without currying!

We've now incorporated the following:

* Our function is pure, does not mutate state, and has no side effects;
* Our function is unary and takes only one argument;
* Our function uses currying, which allows us to reuse it as a function factory;
* Our function takes advantage of closures (because we wouldn't be able to curry without it);
* Our function is sufficiently abstracted that it could be used with other types of objects that could be incremented or decremented as well.

That's a lot of extra power that we didn't have with our object-oriented methods!

At this point, we have a solid start on our first functional application. However, we still don't have any way to store state. In the next lesson, we'll use closures to store state in our application.
