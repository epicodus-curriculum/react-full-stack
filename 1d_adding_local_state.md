We're ready to add local state to our application. We'll start by adding two new components to `src/components`:

`NewTicketForm.js`: This will be our form;
`TicketControl.js`: This will be the parent component for `NewTicketForm.js` and `TicketList.js`.

Let's add some placeholder code to our `NewTicketForm` component:

<div class="filename">src/components/NewTicketForm.js</div>

```js
import React from "react";

function NewTicketForm(props){
  return (
    <React.Fragment>
      <h3>This is a form.</h3>
    </React.Fragment>
  );
}

export default NewTicketForm;
```

Because we are only worried about our local state right now (and toggling between two different components), we won't worry about the particulars of the form just yet.

Next, let's add the skeleton of our `TicketControl` component. Remember that this must be a class-based component to handle state.

<div class="filename">src/components/TicketControl.js</div>

```js
import React from 'react';
import NewTicketForm from './NewTicketForm';
import TicketList from './TicketList';

class TicketControl extends React.Component {

  constructor(props) {
    super(props);
    this.state = {};
  }

  render(){
    return (
      <React.Fragment>
      </React.Fragment>
    );
  }

}

export default TicketControl;
```

We briefly covered state components over the weekend homework. We recommend quickly reviewing the [React Components](/react/react-fundamentals/react-components) lesson if the code above doesn't look familiar. We will add more code to this component shortly. Since this component will be a parent to `NewTicketForm` and `TicketList`, we need to make sure we import both.

Next, let's make a small update to our `App` component. It needs to render the `TicketControl` component now, not the `TicketList` component.

<div class="filename">src/components/App.js</div>

```js
import React from "react";
import Header from "./Header";
import TicketControl from "./TicketControl";

function App(){
  return ( 
    <React.Fragment>
      <Header />
      <TicketControl />
    </React.Fragment>
  );
}

export default App;
```

We no longer import the `TicketList` component — we import `TicketControl` instead. Our `App` component has also been updated to display the `TicketControl` component instead of `TicketList` as well.

**Note that if we run our application right now, it will not render any tickets.** This is expected — our `TicketControl` component doesn't render anything yet.

Next, let's add some state to our `TicketControl` component. Remember that our component can have one of two possible states:

* `TicketList` is showing and `NewTicketForm` is hidden;
* `NewTicketForm` is showing and `TicketList` is hidden.

What do we want the default local state to be? A list of tickets or a form? Well, if we are reproducing the Epicodus Help Queue, the default state should show the `TicketList` component and hide the `NewTicketForm` component. Our new state should reflect that:

<div class="filename">src/components/TicketControl.js</div>

```js
...

  constructor(props) {
    super(props);
    this.state = {
      formVisibleOnPage: false
    };
  }

...
```

We simply add a key-value pair to our `this.state` object. The default state of our application should be `formVisibleOnPage: false`.

We can add as many key-value pairs as we need to `this.state`. Just make sure each key-value pair is inside the `{}` and that each key-value pair is separated by a comma.

Now that we have a default state, we need a way to change it. We'll also need to use conditional rendering to determine which component should be showing.

In the next lesson, we'll add conditional rendering to our component. Then, in the lesson after that, we'll learn how to update state with events. After those two lessons are complete, we will have working functionality to toggle between our ticket list and our placeholder form.
