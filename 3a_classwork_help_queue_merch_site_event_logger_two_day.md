**Goal:** Over the next class sessions, practice building a React site that has both local and shared state. Add full CRUD functionality to your application. Make sure you take the time to carefully plan out your application — including where shared state should go and all components should live. As always, take the time to plan out your application and draw a component diagram. Don't forget to include the diagram in your project README.

## Warm Up
---

* What is unidirectional data flow? Why is it important?
* What is a UUID? How are they useful?
* How can a method in a state component have access to data in child components if data can only flow down, not up?


## Code
---

**Make sure that you get practice with CREATE, READ, and UPDATE functionality with the project you decide to build over the next few class sessions.**

### Help Queue Continued

Follow along with the coursework to add full CRUD functionality to the Help Queue. We recommend revisiting the Help Queue project to add this functionality as you continue to learn more from the upcoming homework.

### Merch Site

Build a website for selling merchandise for a band, author, sports team, or any other purveyor that interests you.

A user should be able to do the following:

* Create, Read, Update and Delete items in the store. Items should have fields for `name`, `description`, and `quantity` (along with any other fields you wish to add).
* Increase or decrease the `quantity` of an item in the store. For instance, if a user clicks "Buy", the `quantity` will decrease by one. If a user clicks "Restock", it will increment by a specified number.
* When the `quantity` of an item is reduced to `0`, the item should say "Out of Stock". A user should not be able to reduce the `quantity` of an item below `0`.

#### Further Exploration

 * Create a `Cart` component. When a user clicks "Buy", the specified item should be added to the cart.
 * A user should be able to view and remove items from the cart.
 * Create a widget that shows the number of items in the cart. This widget should be updated when items are added to the cart.

### Event Logger

Nope, this isn't a site for `console.log()`. Create a site for logging a specified type of real-world event. For instance, it could be a site for birdwatchers to log sightings of birds, celebrity watchers to log sightings of pop stars, or board game players to log their plays of games.

Users should be able to do the following:

* Add full CRUD functionality for types of events to be logged. For instance, in a bird-watching application, a user might add "Spotted Sandpiper" to indicate a viewing of that kind of bird.
* Allow users to increment the event by one when there is a viewing, a play of the game, or so on. Users should also be able to decrement an event (for instance, if they made a mistake).

### Further Exploration

* Create a dynamic component that shows the total number of events logged so far. The component should show in the top right corner of every page and should aggregate every different kind of event — so if there are 3 "Spotted Sandpiper" viewings and 4 "Red-Winged Blackbird" viewings total, the aggregator might read "7 total viewings."

## Peer Code Review
---

* Application effectively uses local and shared state.
* Application was well-planned and utilizes unidirectional data flow.
* propTypes define data types for all component props.
* Application has working CRUD functionality.
* Application is well planned and includes a component diagram.
