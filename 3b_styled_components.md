Up to this point, we've covered only two basic approaches to styling React applications:

* External stylesheets
* CSS objects

While we generally don't recommend using external stylesheets with React, CSS objects are a very solid approach. Another very popular approach in the React community is to use an external library called `styled-components`.

We can add `styled-components` to a project with the following `npm` command:

```
$ npm install styled-components
```

The `styled-components` library has a unique approach to styling that makes it very easy for us to scope styles to specific elements.

Here's an example. Let's say we want the "Help Queue" header in our Help Queue application to have special styles attached to it. We can do the following with the `styled-components` library:

<div class="filename">src/components/Header.js</div>

```js
...
import styled from 'styled-components';

const HelpQueueHeader = styled.h1`
  font-size: 24px;
  text-align: center;
  color: white;
  background-color: purple;
`;

function Header(){
  return (
    <React.Fragment>
        <HelpQueueHeader>
          Help Queue
        </HelpQueueHeader>
        <ul>
          <li>
            <Link to="/">Home</Link>
          </li>
          <li>
            <Link to="/sign-in">Sign In</Link>
          </li>
        </ul>
    </React.Fragment>
  );
}

export default Header;
```

We start by importing `styled from 'styled-components';`. Next, we create a variable that will hold our styles:

```js
const HelpQueueHeader = styled.h1`
  font-size: 24px;
  text-align: center;
  color: white;
  background-color: purple;
`;
```

In this case, we wanted a styled `h1`. `styled` will always come before the styled element — think of it as being a property on `styled`.

Next, we put the actual CSS rules inside of backticks.

Finally, we wrap the element we want to stylize in tags with the same variable name:

```js
...
<HelpQueueHeader>
  Help Queue
</HelpQueueHeader>
...
```

If we run our application in the browser, our styles will be applied! Best of all, they will be scoped only to the code contained inside our tags.

You may wonder why our variable is capitalized (`HelpQueueHeader`). Well, it's convention to capitalize component names — and `styled-components` is actually taking our styles and turning them into a component called `HelpQueueHeader`, hence the name of the library.

Let's look at one more example. Let's say that we want to wrap all of the code in our `Header` component in a style. We could do the following:

<div class="filename">Header.js</div>

```js
...

const StyledWrapper = styled.section`
  background-color: orange;
`;

function Header(){
  return (
    <StyledWrapper>
      ...
    </StyledWrapper>
  );
}

...
```

We create a `StyledWrapper` that we wrap around all the JSX code in our return statement. We don't even need to use a React fragment anymore because our code is already wrapped in our new stylized component.

This only scratches the surface of what we can do with `styled-components`. We recommend using this library for styling going forward.

For more information on `styled-components`, check out the library's [excellent documentation](https://styled-components.com/docs).
