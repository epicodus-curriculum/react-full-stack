**Goal:** Either continue with the project you began at the start of the course section or begin a new multi-day project:

*  If you are continuing with the same project from the start of the course section, the goal is to build a polished project that you are proud to put in a portfolio. 
* If you are beginning with a new project, continue to reinforce what you've learned about using React with Firebase services and other libraries.

## Warm Up
---

* What React technologies are you interested in exploring further? What are your strengths and weaknesses so far in terms of learning React?
* What should we consider when making a decision whether to add an external library to a project?

## Code
---

### Help Queue with Firestore

Work through the homework and update the Help Queue application to use the following:

* Routing with react-router, 
* Firebase authentication and authorization, 
* date-fns and Firestore server timestamps to add a wait time to each ticket.

### Firestore Portfolio Site

Create a portfolio site where a user can add `projects`, `skills` and a `bio`. None of these properties should be hard-coded. Instead, there should be a UI where the admin can sign into a dashboard and update these properties. Add an option for a user to sign in — but not sign up. You can create a user manually in Firestore and update the database rules so only the "admin" can make changes.

### Choose Your Own Adventure / Dungeon Crawler

Create a choose your own adventure or dungeon crawler game using Firestore to store adventure "scenes" or dungeon "rooms." This is more challenging than it sounds and it's important to think carefully about how data will be structured. How can you associate two scenes with each other, or two rooms? Once you've figured out a data structure, focus on building a simple text-based implementation. If you have the time, you can always work on a GUI after the basics are complete.

## Peer Code Review
---

* Application correctly uses Firestore for data storage.
* Application uses hooks for component state and lifecycle events; there are no class components.
* Data is organized in a logical manner.
* All previous objectives are met.