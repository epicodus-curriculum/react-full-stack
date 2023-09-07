So far, the form component in our Help Queue application just contains placeholder data. We'll need an actual form to add tickets to the queue. In this lesson, we'll create a form that collects the value of different fields by taking advantage of `event.target`. Then, over the next several lessons, we'll learn how to take advantage of unidirectional data flow and shared state so our form correctly adds tickets to the queue.

## Adding a Form
---

We'll start by replacing the placeholder text in the `return()` of our `NewTicketForm` component with an actual form:

<div class="filename">NewTicketForm.js</div>

```javascript
...

function NewTicketForm() {

  ...

  return (
    <React.Fragment>
      <form onSubmit={handleNewTicketFormSubmission}>
        <input
          type='text'
          name='names'
          placeholder='Pair Names' />
        <input
          type='text'
          name='location'
          placeholder='Location' />
        <textarea
          name='issue'
          placeholder='Describe your issue.' />
        <button type='submit'>Help!</button>
      </form>
    </React.Fragment>
  );
}

...

```

Notice that our form has a new type of event handler called `onSubmit`. This is similar to when we added an `onClick` event handler when we learned how to toggle local state. The difference is that `onSubmit` triggers when the submit button of a form is clicked.

Our `onSubmit` handler will trigger the function `{handleNewTicketFormSubmission}`. We haven't written that function yet, but we will soon. Note that we are calling `handleNewTicketFormSubmission` a function, not a method. This component isn't a class component so it has functions, not methods that are called on the instance of a class. That also means we'll be using the `function` keyword, unlike with class components.

## Adding an Event Handler to Our Form
---

Now that we have a form in place, we need a `handleNewTicketFormSubmission` function. This function will be triggered when the form is submitted.

Let's add that function just after the lines of code where we instantiated our form field variables:

<div class="filename">NewTicketForm.js</div>

```js
...

function NewTicketForm(){

  function handleNewTicketFormSubmission(event) {
    event.preventDefault();
    console.log(event.target.names.value);
    console.log(event.target.location.value);
    console.log(event.target.issue.value);
  }
...
```

Once again, note that we have to use the `function` keyword here. We can't just use ES6 class syntax to define our functions because this function is inside another function, not inside a class.

We need to add `event.preventDefault()` to our form just as we have in the past. The default behavior of an HTML submit button is to submit data and refresh the page. We don't want it to refresh the page so we prevent the default behavior.

Finally, we'll `console.log()` the values of our fields. Note that we are taking advantage of `event.target`. `event.target` gives us access to the event that was just fired. In this case, we just had a submit event. We can actually grab the values that came from that submit event. Specifically, we can grab the values based on their `name` property. We just need to call `event.target.[input-field-name-goes-here].value`.

Now if we run `npm start`, we will see that the fields from our form are properly logged in the console.

In the next lesson, we'll learn about unidirectional data flow. Then, in the lesson after that, we'll learn how we can get our form data to its parent `TicketControl` component, which can actually handle state.