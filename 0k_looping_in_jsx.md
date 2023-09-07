Currently, our application only has two hard-coded tickets. However, this isn't how our Help Queue should actually work. A functioning, production-ready application should contain a dynamic list of tickets. In this lesson, we'll cover looping through content in JSX.

## Looping in JSX
---

First, let's create an array of tickets in _TicketList.js_:

<div class="filename">src/components/TicketList.js</div>

```js
import React from 'react';
import Ticket from './Ticket';

const mainTicketList = [
  {
    names: 'Thato and Haley',
    location: '3A',
    issue: 'Firebase won\'t save record. Halp.'
  },
  {
    names: 'Sleater and Kinney',
    location: '4B',
    issue: 'Prop types are throwing an error.'
  },
  {
    names: 'Imani & Jacob',
    location: '9F',
    issue: 'Child component isn\'t rendering.'
  }
];

function TicketList(){
  ...
...
```

In the future, this list will come from a database or a Redux store. (We'll be exploring both approaches in the next two course sections.) For now, we'll store hard-coded tickets inside our `TicketList` component. Note that we use `const`, not `let`. Remember that props are read-only and that we can't change them.

Next, we'll add a loop to render a Ticket component for each entry in `mainTicketList`. In JSX, we use the `map()` function for loops.

<div class="filename">TicketList.js</div>

```js
...
    return (
      <React.Fragment>
        <hr/>
        {mainTicketList.map((ticket, index) =>
          <Ticket names={ticket.names}
            location={ticket.location}
            issue={ticket.issue}
            key={index}/>
        )}
      </React.Fragment>
    );
...
```

As we can see here, `map()` loops through our `mainTicketList`. On each iteration, it creates a new `Ticket` with props corresponding to one of the tickets in `mainTicketList`.

There are a few other important things to note:

* We add `index` as an argument to our `map()` function. `map()` can take an optional `index` argument if we need access to how many times our loop has iterated.
* We add a `key` value to each ticket which corresponds to the current `index`.

Why bother to include the `index` and create a unique `key` value? If we don't, our code will run correctly but we'll get the following error in the console: _Warning: Each child in an array or iterator should have a unique "key" prop._ This error is pretty clear. Each ticket should have a unique "key" prop.

Having unique keys makes our application more efficient because it helps React differentiate between similar components so it can identify which have been updated, added, or removed from the list during its virtual DOM reconciliation.

Our tickets don't have a unique ID value (at least not yet), which is why we are using the item's array index for now. If our tickets already had unique IDs, we wouldn't need to pass in `index` as an argument to `map()` — we'd just use the unique IDs as keys. The keys just need to be unique so React can manage reconciliation effectively.

Now we can run our application and see that all our tickets are correctly populating!

### Additional Resources

If you'd like a more technical explanation about how React uses these unique `key` props, check out the ["Recursing on Children" section of the React Reconciliation Documentation](https://facebook.github.io/react/docs/reconciliation.html#recursing-on-children).
