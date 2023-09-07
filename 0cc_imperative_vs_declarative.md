Up to this point, we've mostly used an **imperative** style of programming. **Imperative programming** is when we tell our program exactly what we want to do and how we want it done. This means explicitly stating all the steps needed to get to an end result.

This is in contrast to the **declarative** style of programming, which is preferred when we write functional code. **Declarative programming** is when we tell our program what the end result should be and then let our program decide the best way to achieve this result.

Let's use an example to demonstrate the differences between these two techniques. Let's say we are leaving instructions so our friend can do their laundry.

Here's how we'd do this from an imperative perspective:

1. Open the washing machine.
2. Add clothes.
3. Add one cup of laundry detergent.
4. Close the washing machine.
5. Set the water temperature to warm.
6. Turn the knob to start.
7. Pull the start knob to begin washing.

Here's how we'd do the exact same thing from a declarative perspective:

1. Wash the clothes.

In the first example, we give very specific instructions on how clothes should be washed. Our program will follow each step, which will lead to the desired end result — clean (but not dry) clothes. In the second example, we simply tell the program what to do — wash the clothes — and let our program handle the steps it takes to reach that result.

We've actually written declarative code before. For instance, when we write HTML, we don't describe where each individual pixel on the screen should be placed. Instead, we describe what the end goal is. When we create an `<input>` field, we're saying "place a form input here." We aren't saying "create a box roughly x pixels high and y pixels wide here, add a cursor if the user clicks on it, and then make sure the box has functionality to accept user text input." We only have to describe the end goal, not every single step required to reach that end goal.

We've also used declarative programming in JavaScript. `map()` is an excellent example.

From an imperative perspective, we could double each element in an array like this:

```js
const originalArray = [1,2,3];
let doubledArray = [];
originalArray.forEach(function(element) {
  const doubledElement = element * 2;
  doubledArray.push(doubledElement);
});
doubledArray;
```

Here we instruct our code to create an empty array called `doubledArray`, iterate over each element in our `originalArray`, double each of those elements, and then push them into `doubledArray`.

With `map()`, however, we let JavaScript handle most of the work:

```js
const originalArray = [1,2,3];
const newArray = originalArray.map(function(element) {
  return element * 2;
});
```

Here, we simply tell the program to complete a transformation (`map()`) that doubles every element by 2. We don't state every step explicitly. As a result, our code is cleaner and easier to read.

While imperative code is sometimes necessary, declarative code is widely considered more reusable, readable, and easier for programmers to collaborate on. We will favor this style of coding when we build functional programs.
