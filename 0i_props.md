In this lesson, we'll learn how to pass data between React components. For now, this data will be hard-coded, not dynamic, because we aren't quite ready to work with state yet.

Our Ticket component currently looks like this:

<div class="filename">src/components/Ticket.js</div>

```jsx
import React from "react";

function Ticket(){
  const name = "Thato";
  const name2 = "Haley";
  return (
    <React.Fragment>
      <h3>3a</h3>
      <h3>{name} and {name2}</h3>
      <p><em>Firebase entries not saving!</em></p>
      <hr/>
    </React.Fragment>
  );
}

export default Ticket;
```

Currently, the names on this ticket are hard-coded in our `Ticket.js` component. We are going to change this so that all ticket info is passed from `Ticket.js`'s parent (TicketList) down to `Ticket.js`. This will help prepare us for working with state. Generally, we will want our state to live in one place and be the *single source of truth*. Instead of each `Ticket.js` storing its own data (which wouldn't be a single source of truth), we'll have a parent component store all of the ticket data — that way, our state will be stored in one place.

React components accept properties (known as **props**) passed down from a parent. Because React components are functions, these props are actually just a special kind of argument.

### Passing Props

We can pass props down to a child component using JSX tags. Let's update the `TicketList.js` component so it can pass props to its child `Ticket.js` component:

<div class="filename">src/components/TicketList.js</div>

```js
import React from "react";
import Ticket from "./Ticket";

function TicketList(){
  return (
    <Ticket
      location="3A"
      names="Thato and Haley"
      issue="Firebase will not save record!"/>
  );
}

export default TicketList;
```

We've added three props here: `location`, `names` and `issue`. It's common to put each prop on a separate line. While this isn't required, it makes it easier to read our code so we can see what's being passed down to child components.

### Accessing Props

Next, let's update our `Ticket` so that it uses the props from its parent `TicketList` component.

<div class="filename">src/components/Ticket.js</div>

```js
import React from "react";

function Ticket(props){
  return (
    <React.Fragment>
      <h3>{props.location} - {props.names}</h3>
      <p><em>{props.issue}</em></p>
      <hr/>
    </React.Fragment>
  );
}

export default Ticket;
```

Notice we've also added `props` as an argument to the `Ticket` component function's method signature (`function Ticket(props)`) to indicate it should now accept props. Remember that our components are just functions. All we're doing now is passing an argument (`props`) into our `Ticket` function.

As always, **JSX JavaScript expressions must be wrapped in curly braces.** Content inside curly braces will be evaluated instead of literally rendered. Because `props` is an object, we access its properties just like we would with any other object. For instance, we access the ticket's location with `props.location`.

### Props Are Read-Only

React components aren't just functions — they are pure functions. As we know from our functional programming course section, pure functions don't have side effects and don't alter state.

We need to follow these same rules when we are working with props. We will **never** alter the value of props because this would alter the state of our application and break a cardinal rule of pure functions: no side effects.

For that reason, it's very important to remember that **props are read-only**.

### The Power of Props

So what is the point of doing this? We're lifting data out of the `Ticket` component and passing it down from `Ticket List`, but isn't this just an extra step? Well, let's add a second `Ticket` to `TicketList` and see.

<div class="filename">src/components/TicketList.js</div>

```js
...
function TicketList(){
  return (
    <React.Fragment>
      <Ticket
        location="3A"
        names="Thato and Haley"
        issue="Firebase will not save record!"/>
      <Ticket
        location="4B"
        names="Sleater and Kinney"
        issue="Prop types are throwing an error."/>
    </React.Fragment>
  );
}
...
```

All we've done here is add a second ticket, this time with different props. The magic of this is what we *didn't* have to write: all of the code for another `Ticket`. In the next lesson, we'll learn how to leverage looping to make this process even DRYer so we can create any number of `Ticket`s.



