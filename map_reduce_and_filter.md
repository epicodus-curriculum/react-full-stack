JavaScript provides some built-in methods that are commonly used in functional programming. In fact, we already have experience with `Array.prototype.map()`, one of the most popular of these methods.

We'll start by taking a look at why `map()` is so useful in functional programming. Then we'll cover `reduce()` and `filter()`, two other extremely popular methods used in functional programming.

## `map()`
---

Let's say we want to take all the numbers in an array and multiply them by 2. We'll quickly compare how we can solve this problem using both a loop and `map()`. This is review from Introduction to Programming:

```js
const numArray = [1, 2, 3, 4, 5];
let doubledArray = [];
numArray.forEach(function(element) {
  doubledArray.push(element * 2);
});
doubledArray;
```

In the example above, we use `forEach()` to double each element in an array. From a functional perspective, there are several issues:

* We must initialize an empty array and then store the new values in this array. As a result, we are mutating an array instead of creating a new constant. This is something we want to avoid.
* The example above is imperative instead of declarative. We are telling the computer exactly what to do (imperative) instead of telling the computer the end result we want (declarative).

Now let's look at `map()`:

```js
const numArray = [1, 2, 3, 4, 5];
const doubledArray = numArray.map(function(element) {
  return element * 2;
});
doubledArray;
```

In the example above, we don't mutate an array. Instead, we save mapped `numArray` into a new constant.

The example above is declarative, not imperative. We don't explicitly tell the computer to initialize a new array, loop through an array of numbers, double each element in the array, and then push those doubled numbers into the initialized array. Instead, we just tell the computer that we want a new array with each value doubled.

We can make `map()` even more concise with arrow functions:

```js
const doubledArray = numArray.map (e => e * 2);
```

Remember that with arrow functions we can take advantage of implicit return so we don't need to explicitly state `return`.

We also make the variable more abstract (`e` instead of `element`), which makes this even more concise and abstracted.

**Use `Array.prototype.map()` whenever you want to modify every element in an array.**

## `reduce()`
---

We can use `Array.prototype.reduce()` to reduce an array to a single element. One of the most common usages is to sum an array:

```js
const numArray = [3, 7, 5];
const summedArray = numArray.reduce(function(currentValue, element) {
  return element + currentValue;
}, 0);
```

In the example above, reduce takes two arguments. The first is the `currentValue` of the `reduce()` function. The second is the `element` of the array. Finally, note the `0` after the function itself. This is an initial value that we can optionally provide. That way, `reduce()` knows what the `currentValue` will start with. Note that if we wanted to multiply or divide, this initial value would be `1`. Otherwise, we'd be multipling or dividing by zero!

While this functionality is very helpful, we can use `reduce()` to do so much more. For example, we can use `reduce()` to do things like find the longest or shortest string in an array. However, we won't provide a code snippet for that here. Instead, let's look at a more complex example.

Let's say we have a group of friends that are planning on spending a weekend at the ocean. We have a data set that lists each friend's name as well as an array of the three things each friend most wants to do on the coast:

```js
const friends = [
  {
    name: "Jasmine",
    wantToDo: ["hike", "go out to eat", "swim"]
  },
  {
    name: "Ada",
    wantToDo: ["play games", "hike", "cook meals"]
  },
  {
    name: "Desmond",
    wantToDo: ["sleep", "swim", "play games"]
  },
  {
    name: "Andres",
    wantToDo: ["hike", "swim", "play games"]
  }
];
```

Next, we want to determine which `wantToDo`s are most popular — that will help this group of friends determine how to spend the weekend.

How could `reduce()` possibly be helpful for this? Well, `reduce()` is extremely helpful for manipulating data. In the example above, it would be difficult to find the most popular `wantToDo`s based on the structure of this data set. After all, the data we want is nested pretty deeply: we are looking for a series of arrays that are contained within the property of a series of objects which are in turn contained in an array.

Fortunately, we can use `reduce()` to turn our array of objects into an array that contains all of our `wantToDo`s:

```js
const toDos = friends.reduce(function(array, friend) {
  return array.concat(friend.wantToDo);
}, []);
```

In the snippet above, our initial condition is an empty array `[]`. `reduce()` will iterate through each `friend` and return their `wantToDo` property, which will be concatenated into our `array`. Think of this as being like summing an array of numbers. However, in this case, instead of adding numbers, we are adding arrays to our original array. Because we are using `concat`, which will add two arrays together into one larger array, our final result will be a series of arrays reduced into one single larger array:

```js
> toDos
["hike", "go out to eat", "swim", "play games", "hike", "cook meals", "sleep", "swim", "play games", "hike", "swim", "play games"]`
```

Now we have a more manageable dataset to work with. Let's use `reduce()` yet again. This time, we will create a `voteTally` object with key-value pairs. Each key will be a `toDo` while the value of the key will be the number of times it shows up in the array. Here's how we can do this:

```js
const toDoTally = toDos.reduce(function(voteTally, toDo) {
  voteTally[toDo] = (voteTally[toDo] || 0) + 1;
  return voteTally;
}, {});
```

In this snippet, our `voteTally` starts with an initial value `{}`. We will be adding key-value pairs to this object. We will set the value of the key in this line: `voteTally[toDo] = (voteTally[toDo] || 0) + 1;`. Note the or (`||`) syntax here. This states that if there is no key `voteTally[toDo]` in the object, then the value of `voteTally[toDo]` should be set to 0. Otherwise, we'll use the value that already exists in the key. Finally, we'll increment the value by 1.

This returns the following:

```js
> toDoTally
{
  "hike:": 3,
  "go out to eat": 1,
  "swim": 3,
  "play games": 3,
  "cook meals": 1,
  "sleep": 1
}
```

Now we have a tally of each `wantToDo` stored in a single object. Finally, we can sort this if we wish:

```js
const mostPopular = Object.entries(toDoTally).sort(function(a,b) { return b[1] - a[1] });
```

We pass an object as an argument into `Object.entries()`, which will return all the key-value pairs in that object in the form of an array. Now we can sort these in descending order. If you aren't familiar with how `sort()` works, check the Mozilla [documentation](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/sort). We won't explain it in depth here because `sort()` mutates the array it is called on, which means it's generally a poor choice for functional programming. However, in the example above, at least we aren't mutating a variable. Nor is `Object.entries(toDoTally)` a previously existing array, which makes it more acceptable here. This returns the following array of arrays:

```js
> mostPopular
[["hike", 3], ["swim", 3], ["play games", 3], ["go out to eat", 1], ["cook meals", 1], ["sleep", 1]]
```

Now we can clearly see that this group of friends would like to spend the weekend hiking, swimming, and playing games.

We can use `reduce()` to manipulate much larger datasets as well. Note that our goal won't always be to come up with a final answer with `reduce()`. Sometimes we might just want to take a dataset and then manipulate it into something more manageable to work with.

At this point, it should be clear that we can use `reduce()` to reduce arrays into any type of object, not just a string or a number. Many of the most powerful applications of `reduce()` involve returning another array or an object.

**Use `Array.prototype.reduce()` whenever you want to "reduce" an array down to a single element.**

## `filter()`
---

We can use `Array.prototype.filter()` any time we want to filter an array or collection based on certain conditions. For instance, let's say we want to filter the following array to only include numbers greater than 10:

```js
const numArray = [7, 14, 32, 8];
const filteredArray = numArray.filter(e => e > 10);
```

In the example above, we simply have to specify the condition (`e > 10`) that we want our final array to have. This is very declarative!

`filter()` is extremely powerful. For instance, we can use it to "search" an array of objects by a specific property. Here's an example. Let's say we want to find all the employees at a company that are developers from the following array of objects:

```js
const employees = [
  {
    name: "Ada",
    role: "developer"
  },
  {
    name: "Tom",
    role: "HR"
  },
  {
    name: "Jasmine",
    role: "developer"
  },
  {
    name: "Hank",
    role: "administrative assistant"
  }
];
```

Now we can use filter to see which employees are developers:

```js
> const developers = employees.filter(e => e.role === "developer");
[ { name: 'Ada', role: 'developer' },
  { name: 'Jasmine', role: 'developer' } ]
```

We simply need to specify which employee has a `role` equal to `"developer"`, which we do with `e.role === "developer"`.

**Use `Array.prototype.filter()` whenever you want to filter an array based on certain conditions.**

### Summary

In this lesson, we explored how we can use three common JavaScript array methods in functional programming. Remember that looping is imperative while the three methods detailed above are declarative. Just as importantly, none of these three methods mutate state. They all return new results, which makes them excellent for immutability.

Try finding use cases in your own applications to apply these three methods. You will also get a chance to whiteboard with these methods in this course section.
