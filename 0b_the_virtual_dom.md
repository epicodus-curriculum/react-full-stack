One of React's most powerful features is its virtual DOM. In this lesson, we'll discuss how the virtual DOM works.

## Document Object Model Review
---

Before we can fully understand React's virtual DOM, we must have a thorough grasp of the standard DOM.

We first discussed the **Document Object Model** (or **DOM**) in Introduction to Programming. The DOM is the browser's interpretation of the HTML it renders. If we inspect an element in an HTML page we've built (by right-clicking in the browser and selecting _Inspect_), we're actually seeing the DOM, not the HTML we wrote.

### DOM vs HTML

We already know what HTML is — a markup language for hypertext. It's a declarative language that allows us to tell the browser about how a page should be structured.

The DOM, on the other hand, is a language-independent model of objects that represent the actual structure of our page.

We can think of HTML as being the blueprint of a house while the DOM is the actual house that the browser builds for us.

When we add JavaScript and use Web APIs in our applications, we can change the DOM so that it no longer matches the HTML.

To continue our blueprint metaphor, think of our house as having switches for turning on lights in each room. Our blueprint represents the house with all the lights turned off, but our DOM can utilize JavaScript code we've written so the lights are turned on (and off again).

### Issues with the Standard DOM

Manipulating the DOM is inefficient and resource-intensive, and unfortunately, the standard DOM was never optimized for creating dynamic user interfaces. 

A modern site like Twitter or Facebook might have thousands of nodes in their HTML DOM, each with the potential to change the DOM in a different way. Well, if these companies decided to just use vanilla JS and Web APIs without further optimization, their code would be slow and inefficient, which means they wouldn't have the userbase and popularity they currently have. In other words, how these websites optimized their code has everything to do with their success.

Because manipulating the DOM is inefficient, the goal of optimization here is to minimize DOM manipulation as much as possible. Well, this is the exact optimization that the React library offers! 

## The Virtual DOM
---

React deals with this issue by creating a virtual DOM. The virtual DOM is React's own internal draft of the DOM. Here's how it works:

### 1. React Creates its own Virtual DOM

React is declarative, which means we only need to declare what our UI should look like. React will handle the rest.

When we create React elements (such as a header or paragraph), we're actually creating new elements in the virtual DOM, not the real DOM.

Creating and updating this virtual DOM is much faster than updating the actual DOM because it doesn't require communication with the browser.

### 2. React Compares the "Actual" DOM to its Virtual DOM

After crafting its own virtual DOM, React then compares it to the "actual" DOM in the browser, noting any differences between the two.

### 3. React Calculates Changes

React then automatically calculates the least number of changes necessary to update the actual DOM to match its virtual DOM. This process is known as **reconciliation**.

### 4.  React Updates Actual DOM

React then updates the real DOM to match the virtual DOM only once. This is much faster than calling the DOM multiple times, making each small change individually.

### 5.  Repeat

This process repeats as the user interacts with the application. React continues to update its own virtual DOM as users complete actions that warrant updates to the user interface.

While it's important to have a basic understanding of how the virtual DOM works, React will take care of the heavy lifting for us and allow us to create very efficient sites with relatively little code.