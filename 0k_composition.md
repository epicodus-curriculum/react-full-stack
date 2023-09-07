Functional programming favors **composition**, not inheritance. In fact, composition is an extremely important computer science concept and it is commonly applied in object-oriented programming as well.

With composition, we **compose** the functionality of more complex objects. Instead of focusing on what an object _is_, we focus on what an object can _do_.

To take a functional approach, we'll bypass classes altogether. We'll create a modular `eat()` function which we can include in objects that need it. There is no need to store that `eat()` function in a class. As a result, the method will be loosely coupled, more flexible and easier to reuse.

Let's create a method that will allow any kind of animal to eat:

```js
const canEat = function(creature) {
  const obj = {
    eat: function(food) {
      return `The ${creature} eats the ${food}.`
    }
  }
  return obj;
}
```

Here we are creating a function literal that returns an object. The object itself has a single property called `eat` which stores a function. Note that the function stored inside the `eat` property is a closure because it "closes over" variables from both the inner and outer function. Because of that, the inner function will have access to the value of `creature` from the outer function even after the outer function has been called and completed.

Let's give a cat the ability to eat:

```js
> const cat = canEat("cat");
```

We create a new `cat` variable which will store the object returned from the `canEat("cat")` function. Now the argument `"cat"` will be stored inside the `eat` function.

Our cat variable is now an object that has the following property:

```js
{
  eat: function(food) {
        return `The cat eats the ${food}.`
      }
}
```

Now our cat can eat food:

```javascript
> cat.eat("salmon");
'The cat eats the salmon.'
```

In the example above, our code doesn't care _what_ the object is. It's focused on what the object can _do_, which is eat.

We can easily use this functionality with a fish now — no need to inherit from a restrictive class like `Mammal`:

```javascript
> const salmon = canEat("salmon");
> salmon.eat("insects")
'The salmon eats the insects.'
```

What if we want to add more functionality to our cat? For instance, we should give animals the ability to sleep, too. 

It might be tempting to refactor our function like this:

```js
const canDoThings = function(creature) {
  const obj = {
    eat: function(food) {
      return `The ${creature} eats the ${food}.`
    },
    sleep: function() {
      return `The ${creature} sleeps.`
    }
  }
  return obj;
}
```

In the example above, we add an additional property that provides sleep functionality. This will work as expected — but be careful! Our function just became less modular and reusable. What if we wanted to add a strange new alien species that never sleeps? Or a robot that never sleeps but needs to eat fuel? The function above is no longer very flexible for odd use cases. And while these use cases may seem odd, it's very important to keep our functions small, modular and reusable. As our applications grow in size, we can't necessarily predict how they will change. The best we can do is keep our code flexible and open-ended.

Instead, we'll have two separate methods:

```js
const canEat = function(creature) {
  const obj = {
    eat: function(food) {
      return `The ${creature} eats the ${food}.`
    }
  }
  return obj;
}

const canSleep = function(creature) {
  const obj = {
    sleep: function() {
      return `The ${creature} sleeps.`
    }
  }
  return obj;
}
```

Now that we have two methods, how can we assign them both to a single cat object? Well, let's start by making a **function factory** for creatures. We can use this to assign both of these methods to _any_ creature that needs to eat and sleep.

```js
const sleepingEatingCreature = function(name) {
  let state = {
    name
  }

  return { ...state, ...canEat(state), ...canSleep(state) };
}
```

In the example above, we create a function literal that contains a variable called `state`. We can think of state as being a "snapshot" of the creature's properties and functions. While we could call this variable something else, state is a commonly used term and one that we'll see frequently in React. This variable only has one property: the name of the creature (in our case, it will be a cat). We could also add additional properties here if needed.

Note that we are also using `let` instead of `const`. This is because the `state` of the object will be modified. But isn't this a big no-no in functional programming? It is, and the technique above can more accurately be called **object composition** because we are combining elements of composition with object creation. We will discuss state further in the next lesson.

Next, we use the spread operator to merge the three objects together. Remember that both the `canEat()` and `canSleep()` functions return objects. Using these techniques, we can merge any number of objects, which also means that we can compose as many pieces of additional functionality as we need. Note that we could also use `Object.assign()` here instead of the spread operator.

We will need to make a small update to our other methods because we are passing in entire objects to both the `canEat()` and `canSleep()` methods:

```js
const canEat = function(creature) {
  const obj = {
    eat: function(food) {
      return `The ${creature.name} eats the ${food}.` // Small update
    }
  }
  return obj;
}

const canSleep = function(creature) {
  const obj = {
    sleep: function() {
      return `The ${creature.name} sleeps.` // Small update
    }
  }
  return obj;
}
```

We need to specify the property of the object, not the object itself.

Now we can create any kind of sleeping and eating creature. Our application has no opinions on what kind of creature can do these things because we aren't using inheritance.

```js
> const platypus = sleepingEatingCreature("platypus");
```

Our `platypus` object should look like this:

```js
platypus {
  name: 'platypus', 
  eat: function(food) {
    return `The ${creature.name} eats the ${food}.`
  }, 
  sleep: function() {
    return `The ${creature.name} sleeps.`
  }
}
```

A platypus can eat and sleep just like other mammals. However, if we need to add a modular method so a platypus can lay eggs, that would be easy to do. We could also reuse that method for birds and any other creatures that lay eggs. We can't do that with classical inheritance!

One further thing: we can refactor our code to use arrow notation. We've omitted arrow notation up to this point because it makes the code appear even more abstract. Here's how our new functions look with arrow notation:

```js
const canEat = (creature) => ({
  eat: (food) => {
    return `The ${creature.name} eats the ${food}.`
  }
});

const canSleep = (creature) => ({
  sleep: () => {
    return `The ${creature.name} sleeps.`
  }
});

const sleepingEatingCreature = (name) => {
  let creature = {
    name
  }

  return { ...creature, ...canEat(creature), ...canSleep(creature) };
};
```

Note that when we use arrow notation with a function that returns a single object, we don't need to add a return statement. Instead, we can take advantage of the **implicit return**, which is when we can omit a return statement in a function. This cleans up our code further but it does make it a bit more abstract and unfamiliar. While it's important to be able to use and read arrow notation, for now it's fine to use the syntax that feels most comfortable.

In this lesson, we learned how to use composition to make fully functioning objects out of smaller functions. These smaller functions determine what an object can _do_, not what an object _is_. With a handful of functions, we could create creatures from the far reaches of the animal kingdom, from glow worms to playpuses to whales. We could also pick and use which functions are necessary for each object, whether that's `swim()`, `layEggs()`, `fly()`, or something else.
