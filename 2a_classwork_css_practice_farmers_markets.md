**Goal:** Practice adding local state to your application. For these projects, it is enough to add local state that allows users to toggle between components. However, you may wish to experiment with local state in other ways as well.

## Warm Up
---

* What is the difference between local state and shared state?
* What is conditional rendering and how is it useful in React?
* Why do we need to bind functions in React? What's the alternative to explicitly binding them?


## Code
---

### Adding Local State to Help Queue

Work along with the homework to add local state to your Help Queue application.

Next, use conditional rendering to allow users to see the following three pages in sequence before reaching the ticket form:

* "Have you gone through all the steps on the Learn How to Program debugging lesson?"
* "Have you asked another pair for help?"
* "Have you spent 15 minutes going through the problem documenting every step?"

### Farmer's Market Circuit

Avery's Organics is a mid-sized farm in Northern Oregon that grows organic produce and sells it at farmers markets throughout town. Since Avery's is at a different market almost every day, they've started a website to show customers which market they will be at on a given day.

Avery's also grows different crops in different seasons. They'd like to display what produce is available during which months on their site too.

Using React and all other tools we've covered so far, create a website that depicts this information. **The data you'll use is at the bottom of this page, in the "Data" section of this lesson.** Also, take time to construct your entire environment from scratch. It's important to practice these fundamentals before we increase the complexity of our projects later in this course section!

Try using local state to toggle between different days. Note that we can use conditional rendering for as many conditions as we want — including all seven days of the week!

Once again, make sure to plan your application first and include a component diagram in your README.

## Peer Code Review
---

* Components are used to create modular UI elements.
* propTypes define data types for all component props.
* Local state has been implemented and works correctly.
* Application is well planned and includes a component diagram.
* Application works as expected.

# Data

Here's the two sets of data Avery's Organics farm would like displayed on their site:

### Market Schedule

```js
const marketSchedule = [
{
    day: "Sunday",
    location: "Lents International",
    hours: "9:00am - 2:00pm",
    booth: "4A"
},
{
    day: "Monday",
    location: "Pioneer Courthouse Square",
    hours: "10:00am - 2:00pm",
    booth: "7C"
},
{
    day: "Tuesday",
    location: "Hillsboro",
    hours: "5:00pm - 8:30pm",
    booth: "1F"
},
{
    day: "Wednesday",
    location: "Shemanski Park",
    hours: "10:00am - 2:00pm",
    booth: "3E"
},
{
    day: "Thursday",
    location: "Northwest Portland",
    hours: "2:00pm - 6:00pm",
    booth: "6D"
},
{
    day: "Saturday",
    location: "Beaverton",
    hours: "10:00am - 1:30pm",
    booth: "9G"
}
];
```

### Seasonal Produce

Notice this second set of data includes a nested array. This means you'll likely need to create a JSX loop _within_ a JSX loop to display this information. We haven't explicitly covered this in our lessons. Try your best to apply both your general knowledge of programmatic looping, and your new knowledge about JSX looping to determine a solution.

```js
const availableProduce = [
  {
      month: "January",
      selection: [
        "Apples",
        "Hazelnuts",
        "Pears",
        "Garlic",
        "Mushrooms",
        "Onions",
        "Potatoes",
        "Turnips"
      ]
  },
  {
      month: "February",
      selection: [
        "Apples",
        "Hazelnuts",
        "Pears",
        "Garlic",
        "Mushrooms",
        "Onions",
        "Potatoes"
      ]
  },
  {
      month: "March",
      selection: [
        "Apples",
        "Hazelnuts",
        "Pears",
        "Rhubarb",
        "Garlic",
        "Mushrooms",
        "Onions",
        "Potatoes"
      ]
  },
  {
      month: "April",
      selection: [
        "Apples",
        "Hazelnuts",
        "Pears",
        "Rhubarb",
        "Asparagus",
        "Garlic",
        "Lettuce",
        "Mushrooms",
        "Onions",
        "Potatoes"
      ]
  },
  {
      month: "May",
      selection: [
        "Apples",
        "Hazelnuts",
        "Pears",
        "Rhubarb",
        "Asparagus",
        "Cauliflower",
        "Garlic",
        "Lettuce",
        "Potatoes",
        "Radishes"
      ]
  },
  {
      month: "June",
      selection: [
        "Apples",
        "Hazelnuts",
        "Pears",
        "Rhubarb",
        "Blackberries",
        "Cherries",
        "Raspberries",
        "Strawberries",
        "Asparagus",
        "Broccoli",
        "Cauliflower",
        "Kohlrabi",
        "Lettuce",
        "Mushrooms",
        "Potatoes",
        "Radishes",
        "Squash"
      ]
  },
  {
      month: "July",
      selection: [
        "Apples",
        "Hazelnuts",
        "Pears",
        "Rhubarb",
        "Apricots",
        "Blackberries",
        "Blueberries",
        "Cherries",
        "Melons",
        "Nectarines",
        "Peaches",
        "Raspberries",
        "Strawberries",
        "Tomatoes",
        "Beets",
        "Broccoli",
        "Brussel Sprouts",
        "Cabbage",
        "Carrots",
        "Cauliflower",
        "Cucumber",
        "Eggplant",
        "Garlic",
        "Green Beans",
        "Kohlrabi",
        "Lettuce",
        "Mushrooms",
        "Potatoes",
        "Radishes",
        "Squash",
        "Turnips"
      ]
  },
  {
      month: "August",
      selection: [
        "Apples",
        "Apricots",
        "Blackberries",
        "Blueberries",
        "Cherries",
        "Melons",
        "Nectarines",
        "Peaches",
        "Pears",
        "Plums",
        "Raspberries",
        "Rhubarb",
        "Strawberries",
        "Tomatoes",
        "Beets",
        "Broccoli",
        "Brussel Sprouts",
        "Cabbage",
        "Carrots",
        "Cauliflower",
        "Corn",
        "Cucumber",
        "Eggplant",
        "Garlic",
        "Green Beans",
        "Kohlrabi",
        "Lettuce",
        "Mushrooms",
        "Onions",
        "Peas",
        "Peppers",
        "Potatoes",
        "Radishes",
        "Squash",
        "Turnips"
      ]
  },
  {
      month: "September",
      selection: [
        "Apples",
        "Blueberries",
        "Grapes",
        "Melons",
        "Peaches",
        "Pears",
        "Plums",
        "Raspberries",
        "Tomatoes",
        "Broccoli",
        "Brussel Sprouts",
        "Cabbage",
        "Carrots",
        "Cauliflower",
        "Corn",
        "Cucumber",
        "Eggplant",
        "Garlic",
        "Green Beans",
        "Kohlrabi",
        "Lettuce",
        "Mushrooms",
        "Onions",
        "Peas",
        "Peppers",
        "Potatoes",
        "Radishes",
        "Squash",
        "Turnips"
      ]
  },
  {
      month: "October",
      selection: [
        "Apples",
        "Grapes",
        "Hazelnuts",
        "Melons",
        "Peaches",
        "Pears",
        "Tomatoes",
        "Broccoli",
        "Brussel Sprouts",
        "Cabbage",
        "Carrots",
        "Cauliflower",
        "Corn",
        "Cucumber",
        "Eggplant",
        "Garlic",
        "Green Beans",
        "Kohlrabi",
        "Lettuce",
        "Mushrooms",
        "Onions",
        "Peas",
        "Peppers",
        "Potatoes",
        "Pumpkins",
        "Radishes",
        "Squash",
        "Turnips"
      ]
  },
  {
      month: "November",
      selection: [
        "Apples",
        "Hazelnuts",
        "Pears",
        "Broccoli",
        "Carrots",
        "Cauliflower",
        "Garlic",
        "Mushrooms",
        "Onions",
        "Potatoes",
        "Squash",
        "Turnips"
      ]
  },
  {
      month: "December",
      selection: [
        "Apples",
        "Hazelnuts",
        "Pears",
        "Broccoli",
        "Carrots",
        "Cauliflower",
        "Garlic",
        "Mushrooms",
        "Onions",
        "Potatoes",
        "Turnips"
      ]
  }
];
```
