## Declaring Prop Types

While our props are working correctly, in a more complex application they can become prone to bugs. For instance, we might find ourselves passing the wrong type of data to a child component. At that point, it can become difficult to trace where the bug is coming from — especially if data is passed down through many components. Many other languages feature strict typing, which forces developers to declare the data type of variables when they are declared. JavaScript, for better or worse, is a loosely typed language and does not enforce such declarations.

For this reason, it's a best practice to assign strict data types to props with a special `propTypes` property. When we assign strict data types, we are simply stating that a variable (in this case, a property) **must** be of a certain type. If they aren't, our application will show a warning in the JavaScript console. We will always assign strict data types to all of our properties. **You will also be expected to use PropTypes for all properties on independent projects.**

### Using PropTypes

We can use PropTypes out of the box with `create-react-app`. Let's add PropTypes to our `Ticket` component now.

<div class="filename">src/components/Ticket.js</div>

```javascript
import React from "react";
import PropTypes from "prop-types";

function Ticket(props){
  return (
    <React.Fragment>
      <h3>{props.location} - {props.names}</h3>
      <p><em>{props.issue}</em></p>
      <hr/>
    </React.Fragment>
  );
}

Ticket.propTypes = {
  names: PropTypes.string,
  location: PropTypes.string,
  issue: PropTypes.string
};

export default Ticket;
```

First, we must always `import PropTypes from "prop-types";` if we plan to use PropTypes in a component. It's much more efficient to only import this dependency where we need it instead of having it available globally. However, that means we need to remember to import any dependencies a component needs.

We've also added the following code near the bottom of our `Ticket` component (just above the `export` statement):

<div class="filename">src/components/Ticket.js</div>

```js
Ticket.propTypes = {
  names: PropTypes.string,
  location: PropTypes.string,
  issue: PropTypes.string
};
```

When we declare `PropTypes`, we will always do so with the following format:

```js
Ticket.propTypes = {

};
```

All `PropTypes` in the component will be stored inside this object literal. Note that we state `Ticket.propTypes` — it's easy to make a mistake here and state `Ticket.PropTypes` instead, but that won't work. It makes sense in this context because `propTypes` is a property of `Ticket` so it shouldn't be capitalized.

Each prop inside the object literal is declared like this:

```js
propertyName: PropTypes.propertyType
```

As we can see, this is a key-value pair. All prop types are key-value pairs. The name of the property (shown as `propertyName` in the example above) is the key while the actual property type is the value. Note that `PropTypes` is capitalized here. This is because we are using the `PropTypes` library we've imported.

The capitalization of `PropTypes` (or lack thereof) is often confusing for beginners, so if you are having a problem getting `PropTypes` to work, double-check your syntax. When it's a property of the component, it should be lower camel case (`Ticket.propTypes`). When it's referring to the library we are importing, it should be upper camel case (`names: PropTypes.string`).

We can also make a property required by adding `isRequired`:

<div class="filename">src/components/Ticket.js</div>

```js
...

Ticket.propTypes = {
  names: PropTypes.string.isRequired,
  location: PropTypes.string.isRequired,
  issue: PropTypes.string
};

...
```

### More PropTypes

As seen in the [PropTypes Library Documentation](https://github.com/facebook/prop-types), there are many different types of propTypes. Here's how to define a few of the most common:

```javascript
MyExampleComponent.propTypes = {
  exampleArray: PropTypes.array,
  exampleBoolean: PropTypes.bool,
  exampleFunction: PropTypes.func,
  exampleNumber: PropTypes.number,
  exampleObject: PropTypes.object,
  exampleString: PropTypes.string,
  exampleSymbol: PropTypes.symbol,
  exampleReactElement: PropTypes.element,
}
```

We can also declare more complex property types as well. For instance, we can declare that a prop is an array full of a specific type of entries:

```javascript
...
 exampleArrayOfNumbers: PropTypes.arrayOf(PropTypes.number),
 exampleArrayOfStrings: PropTypes.arrayOf(PropTypes.string),
...
```

We can also declare that a prop is an instance of a class:

```javascript
...
  exampleClassTypeProp: PropTypes.instanceOf(ExampleClassName),
...
```

To learn more about the `prop-types` library and the available types, check out the [documentation](https://github.com/facebook/prop-types).
