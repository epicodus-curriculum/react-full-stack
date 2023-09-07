To wrap-up to the whirlwind of information about context, let's review some of the best practices for using context, the alternatives to context, and a few further exploration opportunities.

## Best Practices with Context
---

**1. You can and should have multiple context providers for different state that needs to be disseminated.** For example, if we wanted to disseminate the current user and the color theme in the Help Queue, those should be in two separate contexts.

**2. Locate your provider as close as possible to the consumer components.** This is a general best practice that may not be possible in some cases. For example, when your consumer components are nested in your component tree and they are on different branches, it often will not be possible to locate your context provider that close to any consumer components. 

**3. Make sure that only the components that need the context data, have it.** You shouldn't make a component a consumer if it doesn't absolutely need to. Keep in mind that anytime the context provider's value changes, all components that consume that context value will re-render. 

**4. Use context judiciously.** Context should be the last tool you reach for, before prop drilling, lifting state up, and composition. Using context in components makes them harder to reuse. How so? Because any consumer component needs to be located downstream of a provider component. So, before you use context, ask yourself if the data that you need to disseminate is really needed on a global or wide scale. Also, before reaching for context, first try using props and composing your components to make passing props easier.

## The Alternatives to Context
---

It's important to understand that the Help Queue doesn't need context to have a light and dark theme that we can toggle in between. Why? The Help Queue is small — with only 10 components, and 4 of those that need the theme data — and we're not planning on expanding its functionality.

What we should do instead is use two big alternatives to context to handle transferring data between components: props and component composition. At this point, we should be well familiar with how props work, but component composition is more nebulous. This is by nature: composition really depends on the needs and structure of your application!

However to make component composition more concrete, we've created a version of Help Queue that has a toggleable light and dark theme and that uses only props and composition to achieve this. You can find it on this branch:

---
**[<i class="glyphicon glyphicon-folder-open"></i>  GitHub Repo for Help Queue with Light/Dark Theme using Props and Composition](https://github.com/epicodus-lessons/react-help-queue-with-context/tree/02_with_props_and_composition)**

We won't walk through all of the changes made in the above branch. Instead, we'll focus on understanding how we've updated the Help Queue to use component composition in `TicketControl` to lower the amount of props we have to pass down from the `TicketControl` component. The main updates have to do with how we render two sets of components:

* `NewTicketForm` which renders `ReusableForm`
* `EditTicketForm` which renders `ReusableForm`

Previously this was the order in which the above components are rendered:

* `TicketControl` imports, passes props to, and renders `NewTicketForm`, and `NewTicketForm` imports, passes props to, and renders `ReusableForm`.
* `TicketControl` imports, passes props to, and renders `EditTicketForm`, and `EditTicketForm` imports, passes props to, and renders `ReusableForm`.

With the update, we've decided to compose our components and their props in `TicketControl`. Now this is the order in which the components are rendered:

* `TicketControl` imports, passes props to, and renders `NewTicketForm` and `ReusableForm`.
* `TicketControl` imports, passes props to, and renders `EditTicketForm` and `ReusableForm`.

Let's look at just one example: how we compose `EditTicketForm` and `ReusableForm` to render both components from `TicketControl`:

```js
...
import EditTicketForm from './EditTicketForm';
import ReusableForm from './ReusableForm';

class TicketControl extends React.Component {
  ...

  ...

  render(){
    let theme = this.props.theme;

    const buttonStyles = { 
      backgroundColor: theme.buttonBackground, 
      color: theme.textColor, 
    }

    let currentlyVisibleState = null;
    let buttonText = null; 
    
    if (this.state.editing ) {      
      // new code below!
      currentlyVisibleState = (
        <EditTicketForm>
          <ReusableForm 
            handleFormSubmission={this.handleEditingTicketInList}
            buttonText="Update Ticket" 
            theme={theme}/>
        </EditTicketForm>
      );
      buttonText = "Return to Ticket List";
    } else if (this.state.selectedTicket != null) {
      ...
    } else if (this.state.formVisibleOnPage) {
      ...
    } else {
      ...
    }

    return (
      <React.Fragment>
        {currentlyVisibleState}
        <button style={buttonStyles}  onClick={this.handleClick}>{buttonText}</button> 
      </React.Fragment>
    );
  }
}

...

export default TicketControl;
```

Notice in the above code snippet that we render both `EditTicketForm` and `ReusableForm` from the `TicketControl` component by composing them together and saving them in the variable `currentlyVisibleState`:

```js
currentlyVisibleState = (
  <EditTicketForm>
    <ReusableForm 
      handleFormSubmission={this.handleEditingTicketInList}
      buttonText="Update Ticket" 
      theme={theme}/>
  </EditTicketForm>
);
```

This is called **inversion of control**, where a component higher up in the tree composes multiple components and either renders them, or saves them in a variable to pass down as a prop to other components. This improves the burden of prop drilling by making it so that we don't have to pass props down through as many levels of components, or so that we can pass fewer props down. 

More specifically, the "inversion of control" here is giving a root component like `TicketControl` more control, rather than leaving component composition to components further down the component tree. [The React docs on context](https://reactjs.org/docs/context.html#before-you-use-context) explains that this is not always the best choice:

> Such inversion, however, isn’t the right choice in every case; moving more complexity higher in the tree makes those higher-level components more complicated and forces the lower-level components to be more flexible than you may want.

As it goes, component composition is always subjective and depends on the needs of your application. In any case, always give component composition a try before you reach for context.

Hopefully this abbreviated example with the Help Queue gives you a better idea of what component composition can look like and how it can ease prop drilling. To see the full context of the above example, check out the GitHub repo we linked to earlier in this lesson. There's a couple things to note about the GitHub repo:

* The link takes you to a branch of the Help Queue app called `02_with_props_and_composition` that implements a light and dark theme with props and component composition. There is no context used in that branch!
* Note that you'll have to learn about the `children` props in [the React docs on composition and containment](https://reactjs.org/docs/composition-vs-inheritance.html#containment) to make sense of the changes made to `NewTicketForm` and `EditTicketForm`.

For another example of component composition, check out the section of the React docs on context that's titled [Before You Use Context](https://reactjs.org/docs/context.html#before-you-use-context). 

## Further Exploration with Context
---

There a few further exploration opportunities to discuss:

**1. For more examples and discussion, check out the docs.**
  
* [Context](https://reactjs.org/docs/context.html)
* [The `useContext` Hook](https://reactjs.org/docs/hooks-reference.html#usecontext)

**2. To learn how to work with multiple contexts, check out the docs.**

* [Consuming Multiple Contexts](https://reactjs.org/docs/context.html#consuming-multiple-contexts)

**3. Explore how to implement a custom provider component and consumer hook with context to DRY up your code.** Kent C. Dodds has a great article called ["How to Use React Context Effectively"](https://kentcdodds.com/blog/how-to-use-react-context-effectively) that shows how to combine the context state management with a custom provider component and abstracts the context consumer logic into a custom hook. [Kent C. Dodds](https://kentcdodds.com/about) is a computer programming educator and has many great articles about React. 

Once you've read the article, try implementing a custom provider component and consumer hook  in the Help Queue or another project.

Optionally, check out the repo branch linked below that demonstrates how to implement the concepts from Kent C. Dodds' article with a custom `useTheme()` hook and a `ThemeProvider` component.

---
**[<i class="glyphicon glyphicon-folder-open"></i>  GitHub Repo for Help Queue with Light/Dark Theme using Custom Provider Component and Consumer Hook](https://github.com/epicodus-lessons/react-help-queue-with-context/tree/03_custom_consumer_hook_and_provider)**