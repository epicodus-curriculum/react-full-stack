So far, we've focused on refactoring our React Help Queue application to incorporate Redux. However, what should we do if we want to build a React application with Redux from the ground up?

Well, that's a bit of a trick question. Over the last several sections, we've actually built out our Help Queue following the best practices we should use to build a React-Redux application from scratch. In this lesson, we'll go over those best practices so you can apply them for this section's projects.

Before we start, a quick reminder: building React applications that use Redux is hard. You've just started learning Redux — so be patient with yourself. Like many other topics we cover at Epicodus, we have to cover these concepts quickly to give you as much breadth in your knowledge as possible during your time as a student. It's totally normal if you need a lot more practice with React and Redux before it really clicks. That means it might not fully click until _after_ you graduate from Epicodus. That's okay. Like we said, this stuff is hard.

### 1. Plan Your Component Tree

You might be really excited to start writing and testing reducers but that's not the first thing you'll do when you build an application from scratch. We should always start by planning our component tree and drawing a diagram. We discussed this in React Fundamentals with [Planning a React Application](https://www.learnhowtoprogram.com/react/react-fundamentals/planning-a-react-application). In that lesson, we provided a link to an article in the official React docs called [Thinking in React](https://reactjs.org/docs/thinking-in-react.html). That article doesn't even mention Redux — and yet we should follow _all_ of the steps from that article _before_ we start incorporating Redux. If you're feeling a bit hazy about that article, we recommend reading it again.

In Intermediate JavaScript, we really focused on writing (and testing) business logic first before building a UI. For that reason, the approach we should take with a React/Redux application might still feel a bit counter-intuitive. Instead of writing and testing our business logic first (the reducers), we need to start with the UI.

### 2. Build a Static Site

This is exactly what we did in React Fundamentals. Once we planned the basic structure of our application, we built out a static site that included all of our components. By the way, this _doesn't_ mean graphic design and making it look pretty. Don't worry about adding lots of CSS and content. This should be a bare-bones static site. 

By the time you're done, it should include all the components and just enough content so you can see that the components are all working together correctly. If your site looks nice, you probably spent too much time on this step! This would also be a good time to add props and prop types. However, if you still aren't sure how your props will look yet because you haven't thought about state, it's okay to wait until the next section.

### 3. Time to Think About State!

Once you've completely built out a static site, you're ready to think about state. In [Thinking in React](https://reactjs.org/docs/thinking-in-react.html), this step is described as _Identify The Minimal (but complete) Representation Of UI State_. The key words here are _minimal_ and _complete_. You don't want to clutter your application with too much state. Also, you want to have a good plan for all the state your application needs. It's no fun to realize later that you've forgotten an essential piece of state — only to discover that adding it in results in a painful refactor.

At this point, take the time to write out the state your application needs. Be thorough. 

For instance, one of the project prompts in this section is the word guessing game we call Word Puzzle. For the Word Puzzle game, players are supposed to guess a word, letter by letter, within about 6 or so guesses. For the UI, the word is hidden, but we tell the player how many letters are in it. Each time the player guesses a correct letter, the UI is updated with that correct letter.

For Word Puzzle, it's not enough to just write _add letters to game board_. There's a lot more to the UI than that. Here are a few steps:

* Generate a word.
* Generate blanks (or spaces or lines) based on the number of letters in the word.
* If a player guesses correctly, fill in the blanks that should be that letter.
* If a player guesses incorrectly, subtract one from the number of available guesses.

Next, we need to differentiate between application state and local state. Generally, if we want to follow best practices closely, we'd let React handle local state and we'd let Redux handle application state. However, in this section we encourage you to practice using Redux as much as possible. That means it's fine if you use Redux to handle all state including local state. You will still need to think about where that state should live and how it should flow through your application regardless of what kind of state it is.

If you haven't added props and prop types, now's the time to do it. You know how the state should look in your application. Don't forget that props are read-only! Even though we haven't added state yet, you should know where the state should live and you should have any `Control` components in place — and you should also figure out how components will deal with shared state as needed. This might involve some time spent thinking and brainstorming with your pair. With more challenging coding problems, you'll often have to spend extra time with your hands off the keyboard. That means planning and discussing the pros and cons of different approaches.

### 4. Test and Write Reducers

This is where we diverge from [Thinking in React](https://reactjs.org/docs/thinking-in-react.html). That's because we aren't thinking in React anymore. We're thinking in Redux! At this point, we should've identified all the state our application will need.

We can apply our TDD principles to start with the simplest behaviors possible. In the case of our Word Puzzle game, a reducer should be able to take the word that the player is supposed to guess and return some basic information about it. For instance, if we input a six-letter word into a `SHOW_BLANKS` action, the action might start by returning six blanks (or spaces or lines). Gradually, we'd need to think about how our action will handle which blank should be letters. We'd need to write several tests and updates to our action.

You may be able to test and write all of your reducers at this point — or you might realize later that you need to add additional actions. Regardless, you should always start with a test. Test the simplest behavior possible. Repeat this process with another test and the next simplest behavior until the action is fully tested and functioning correctly. Remember, you can apply this process throughout your career to break down a difficult problem into more manageable parts.

Don't even think about incorporating Redux into your application until you have some fully tested reducers and actions that you're ready to incorporate. Your tests and reducers should be airtight. If not, your application could end up a total mess — and it'll be much harder to find what's causing any bugs. 

### 5. Add Redux to Application

Only now are we ready to add Redux to the application. You can follow along with this Help Queue lesson again to walk through each step: [React with Redux: Adding Redux to React: Part 2](https://www.learnhowtoprogram.com/react/react-with-redux/adding-redux-to-react-part-2). Also, don't forget to install react and react-redux if they aren't already in your `package.json` file:

```javascript
$ npm install redux@4.2.0 react-redux@8.0.2
```

While the lesson linked above walks through refactoring the Help Queue project, all the same steps apply to your new application. If you've following all the steps outlined in this lesson, it's the same process that we've used to build out the Help Queue application. In the [React with Redux: Adding Redux to React: Part 2](https://www.learnhowtoprogram.com/react/react-with-redux/adding-redux-to-react-part-2) lesson, we wire up Redux to the `TicketControl.js` component. At this point, you likely have a `Control` component or another component that's handling most or all of the state in your application.

### 6. Be Ready to Debug... Don't Pull Your Hair Out!

Okay, we're serious about the debugging part. Hopefully you won't get so frustrated that any hair is at risk. But there are a lot of parts working together here so frustration _is_ common — especially if things aren't working as you expected. 

At the very least, you should feel good about your reducers and actions — they should be well-tested, after all. More likely, you might run into issues with communication between components. If you haven't familiarized yourself with React Developer Tools yet, this is a good time to practice. Don't forget about this [interactive tutorial for using React Developer Tools](https://react-devtools-tutorial.now.sh/) which you've hopefully already checked out at this point. We also recommend checking out the [Redux DevTools Extension](https://github.com/zalmoxisus/redux-devtools-extension) as well.

Once again, be patient with yourself if you get to this point and things just... aren't... working. We'll say it again. React with Redux is hard. The experience you'll need to feel fully comfortable with React and Redux is likely considerably more than the time we have to cover it at Epicodus. Don't forget you have your whole career to get better at these tools and any others that you might need to learn for future jobs.
