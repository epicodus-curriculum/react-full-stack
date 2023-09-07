In this lesson, we will learn how to use the [React Redux](https://github.com/reactjs/react-redux) library, which contains the official bindings for adding Redux to a React project.

**Bindings** are libraries that help two languages, tools, or technologies integrate with one another. They usually do this by "wrapping" logic and functionality from one technology into functions that are easier to call in the second technology.

The React Redux library wraps Redux functionality into methods and components that are easier to use in React. This library is "official" because the React core team endorses the tool.

###  Documentation

Even though the library is published by React [on GitHub](https://github.com/reactjs/react-redux), its documentation is [located here, alongside Redux documentation](https://redux.js.org/basics/usage-with-react).

### Features and Functionality

We will be using the following tools from the React Redux library:

  * The `<Provider>` component;
  * The `connect()` function;
  * The `dispatch()` function (you can probably already guess what this one handles); 
  * The `mapStateToProps()` function. 
  
Let's learn about each of these tools before integrating them into our Help Queue project.

#### `<Provider>` Component

We can use a `<Provider>` component to make our Redux store available to any components that are nested inside the `<Provider>` component. All of the `<Provider>` component's children will get access to the `connect()` method.

If we make the `<Provider>` component the parent of our application's component tree, the entire application will be able to access the Redux store via the `connect()` method.

In other words, we can look at the `<Provider>` component as extending Redux's functionality through out application. This is the glue that gives a React application access to a Redux store.

#### `connect()`

As long as a component has access to Redux via a parent `<Provider>` component, it has access to the `connect()` function. `connect()` takes a component as an argument and then returns the component with additional functionality. Most notably, it adds a `dispatch()` method to the component as well as the ability to define a `mapStateToProps()` method.

After we import the `connect()` function into a component, we will then pass the entire component into `connect()` as an argument. This triggers React Redux to rewrite the component class to include the necessary Redux store functions in its props. Essentially, this method automatically outfits our components with all necessary logic to communicate with the Redux store.

`connect()` also acts as an intermediary between a component and the store — the component itself will never directly communicate with the store. This makes our code more modular and loosely coupled.

#### `dispatch()`

We already know what `dispatch()` does — it allows us to dispatch an action. This is the case for both Redux and for the React Redux library. The code for dispatching an action looks very similar in both libraries. 

#### `mapStateToProps()`

`mapStateToProps()` can make our lives much easier as developers. As its name implies, it matches state items from the Redux store to their corresponding React props. In order to use it, we define a `mapStateToProps()` function that matches Redux state to React props like this:

```javascript
function mapStateToProps(state) {
  return {
          reactProp1: state.reduxStateItem1,
          reactProp2: state.reduxStateItem2,
          reactProp3: state.reduxStateItem3
        }
}
```

As seen in the example above, `reactProp1` in our component would map to the `reduxStateItem1` property in our store's state.

This allows us to quickly retrieve and display Redux-managed data in our React UI.

Now that we've learned a little about the React Redux library, we are ready to combine React and Redux together in our Help Queue application.
