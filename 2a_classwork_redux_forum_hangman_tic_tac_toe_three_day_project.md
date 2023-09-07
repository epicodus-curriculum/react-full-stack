**Goals:** Spend the rest of the course section building out a fully-functional Redux project. For full-time students, this will be a three-day project. For part-time students, this will be a multi-day project.

Start by taking the first class session(s) to focus on understanding Redux state management and following the flow of data between React and Redux. Practice combining reducers.

Then take the next class session(s) to add action creators and constants.

Then, take the remaining class session(s) to put the finishing touches on your project and consider experimenting with lifecycle methods.

## Warm Up
---
Use these warm ups after you've read the corresponding homework for each topic. You may work through these warm ups each class session or over time until you've completed them all.

### Combining Reducers

* What does Redux's `combineReducers()` do? When should we use it?
* How might we determine when an application needs multiple reducers?
* When we combine reducers with `combineReducers()`, what do we need to do to ensure that we are able to access all state slices in our React application?

### Action Creators and Constants

* What are action creators and how can they help us clean up our code?
* What is the benefit of using constants instead of strings for action types?
* Walk through how Redux reducers, actions, and stores all work together to manage application state.

### Lifecycle Methods

* What is a lifecycle method?
* List the most used lifecycle methods (in the order they are called) and what we might use them for.

## Code
---

First, follow along with the Help Queue lessons throughout the course section to make sure you understand key concepts. Once you are done, work on your project of choice.

You will be working with the same pair for this multi-day project. There are four options for your project.

### Redux Help Queue

Follow along with the Help Queue lessons throughout the course section to make sure you understand key concepts. Once you are done, work on your project of choice.

### Redux Vote-Based Discussion Forum

Sites like [Reddit](https://www.reddit.com/r/aww/), [HackerNews](https://news.ycombinator.com/newest) and others offer a collection of different pages or forums where users may post news, pictures, and other information around a certain topic. Other users can then upvote or downvote content. The more upvotes an item receives, the higher it's displayed on the list. Recreate a basic subreddit and/or vote-based discussion board using React and Redux. Here are some user stories to get you started:

* As a user, I want to enter content into a form and submit to create a new post.
* As a user, I want my new posts to include a timestamp. And I want to see when other listings were posted, too.
* As a user, I want to upvote posts I particularly enjoy.
* As a user, I want to downvote posts I don't like, or find inappropriate.
* As a user, I'd like posts with the most upvotes to appear higher on the page. (We haven't **explicitly** covered this in our curriculum, but here's a hint: You can complete logic before the `return` statement of a `mapStateToProps()` method!)

#### Further Exploration

* As a user, I want to click a post to view additional details. For now simply hide/show some message content. (Hint: You could create a `currentlySelectedPost` Redux state key, and alter the value based on which post the user is selecting.)

### Redux Word Puzzle

Create a React and Redux application that allows users to play a word guessing game, where users have to guess the word in 6 (or so) tries. Users should be presented with a number of blank spaces corresponding to the number of letters in a word. Each round, users should guess a letter: 

* if the letter is in the word, the letter should show up on the webpage
* if the letter is not in the word, the number of allowed guesses counts down by 1. 

Once the number of guesses reaches 0, the game is over. If the user guesses the word before the number of guesses runs out, they win the game.

Make sure to:

* Provide a form that the player can use to make a guess. The app should tell them if it's right or wrong, and keep track of how many guesses they have left.
* On the game page, display the letters they have guessed correctly so far, like: `b _ b b _ _`
* Display the letters the player has guessed incorrectly, and how many guesses they have left.
* Make sure to display messages telling the user whether they've guessed correctly or incorrectly. Then a "Game Over" message if they're out of guesses.
* Don't allow duplicate guesses. If the user tries to guess the same letter twice they should be told to try again.

Bonus functionality:

* Try to change the color of an element on your game page based on whether the guess was right or wrong. Play around with changing the layout of your template file based on whether a guess is right or wrong, and whether or not the game is over.
* Make the game two-player! Create a new form page where one user can choose a word for the other user to guess.
* Try stylizing this game like [Wordle](https://wordplay.com/).

### Redux Tic-Tac-Toe

**Note:** Please be aware that this prompt is more challenging than the Word Puzzle prompt. Stick with the first prompt if you are still getting the hang of Redux.

Create a React and Redux application that allows users to play tic-tac-toe. Begin by following [this highly-recommended tutorial from the React documentation](https://facebook.github.io/react/tutorial/tutorial.html) that walks through the creation of a _non_-Redux tic-tac-toe game.

Even though you already know how to make non-Redux React apps, take your time and pay careful attention to each step of this tutorial. The tutorial includes shortcuts and pieces of functionality we haven't covered such as creating helper methods to dynamically render JSX.

Once you've successfully completed the tutorial, add Redux and React-Redux to your tic-tac-toe game and refactor the application to rely solely on Redux for all application state management.

### Redux Minesweeper

If you finish creating tic-tac-toe before the end of the course section, try creating a Minesweeper game in Redux and React.

Not sure how to tackle this? Check out [James Meyers' Minesweeper Codepen](https://codepen.io/FullR/pen/qNbRGz). Don't just copy this code.  Take the time to analyze it and see if you can figure out how it works. This is something you'll do a lot at future internships and jobs. Next, try creating your own Minesweeper application based on what you learned.   

## Peer Code Review
---

* Application state is successfully managed by Redux.
* All Redux reducers are pure functions.
* Actions are successfully dispatched to specify changes to state.
* Reducers receive and handle actions to correctly mutate the store's state.
* Jest unit tests are included for any Redux reducers and tests pass.
* Previous objectives and React best practices are followed
* Reducers are combined when necessary.
