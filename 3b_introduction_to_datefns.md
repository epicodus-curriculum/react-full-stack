**Note:** You are not expected to incorporate any of the concepts from the homework on the date-fns library for the independent project.

---

Over the next several lessons, we will add a key piece of functionality to our Help Queue. Users will be able to see how long a ticket has been open, just like with the Epicodus Help Queue.

In this lesson, we'll learn about [date-fns](https://date-fns.org/), a modern JavaScript date utility library used for managing time and dates.

Then, in the next lesson, we'll learn about a React component's lifecycle — and how to write lifecycle methods.

Finally, we'll put all the pieces together and add this functionality to our Help Queue project.

### date-fns

date-fns is a popular JavaScript library for working with dates and time. We can use date-fns to manipulate and parse time — which is exactly what we'll use it for.

We'll start by adding it to our project:

```javascript
npm install date-fns@2
```

The [documentation for date-fns](https://date-fns.org/docs/Getting-Started) is extensive, and there are many helper functions available. We recommend checking it out — there are many use cases where it can add valuable functionality to an application.

When we import date-fns into a component where we needed it, we'll need to import the specific tool we want to use. We'll use [the `formatDistanceToNow` helper function](https://date-fns.org/v2.29.1/docs/formatDistanceToNow), which we'll import with the following import statement:

```js
import { formatDistanceToNow } from 'date-fns';
```

Then we can use that helper in our code as needed. This is how we'll use this function:

```js
formatDistanceToNow(new Date(), {
  addSuffix: true
});
```

In our case, we are only interested in seeing how much time has elapsed since a ticket was added to the queue, and this is what `formatDistanceToNow()` does. If a ticket was put in seven minutes ago, the output from date-fns will be `7 minutes ago`. We can remove the suffix `ago` by calling not passing in the second argument to the helper function:

```js
formatDistanceToNow(new Date());
```

Also notice that our first argument makes use of the JavaScript `Date` object. A JavaScript-formatted date is all that `formatDistanceToNow()` needs to work! When we call the `Date` constructor, it will return the date and time, with the time zone matching what our computer is set to. 

For our particular use case in our Help Queue, this is all we need to know about date-fns. We need to be able to create track when a ticket was initially created and then we need to check to see how much time has passed since the ticket was put in.

Now that we know a little about date-fns, we're ready to take a closer look at the lifecycle of React components — as well as lifecycle methods we can use to automatically update a React component. We'll do that in the next lesson.