In the last lesson, we briefly covered local and shared state. The plan we made for our static Help Queue is a good start but we will need to make some changes to its structure. That means rethinking some parts of our application and creating a new diagram.

![This diagram shows a few changes to the structure of our application, including a control component to determine which child component should be displayed.](https://learnhowtoprogram.s3.us-west-2.amazonaws.com/React/Week-1-React-2019/help-queue-with-ticket-control-updated.jpg)

The main addition here is a component called `TicketControl`. Our application will be an SPA (single-page application) that shows the `TicketList` component. However, when a user clicks the "Add Ticket" button, the `TicketList` component will be hidden and the user will see the `NewTicketForm` component instead.

That means both the `TicketList` component and the `NewTicketForm` component need to have the same parent — but only one of the components will be showing at a time.

In order to toggle between these two components, `TicketControl` will need to have local state to determine which of the following states the page should be in:

* `TicketList` showing and `NewTicketForm` hidden;
* `NewTicketForm` showing and `TicketList` hidden.

We will take care of toggling between these components (our local state) before we start building our shared state. However, it's important to have our plan in place so let's take a look at which of these components will need to share state:

* `TicketList` will need access to the main list of tickets so it can read and display them;
* `NewTicketForm` will need access to the main list of tickets so it can ensure new tickets can get passed into that main list.

So where should this shared state go? Fortunately, this is a simple question to answer. Both components have the same parent. `TicketControl` is the lowest common ancestor to which we can lift our application state.

If our plan isn't fully clear yet, use this page as a reference as we follow along with upcoming lessons. 