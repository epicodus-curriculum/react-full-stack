Up to this point, we haven't discussed adding CSS styles to a React application. There are many different ways of adding styles to React applications, including using external stylesheets and creating CSS objects. In this lesson, we'll discuss using external stylesheets for CSS styling. Later in this course section, we will discuss styling with CSS objects. We will also cover other approaches in future weeks.

## Using External Stylesheets in React
---

When we create a new application with create-react-app, two CSS files are automatically generated: `index.css` and `App.css`. These files are imported into `index.js` and `App.js` respectively. Why have separate stylesheets for both `index.js` and `App.js`? Well, the idea here is that any styles we want to apply globally should go into `index.css` while styles we just want to apply to `App.js` should go into `App.css`.

From a readability perspective, this can be helpful, but there is a big problem with this approach. When our React application is compiled, all of the CSS files in our application will be minified into a single CSS file. Even though we can separate our CSS into separate files, ultimately, it's almost exactly the same as breaking out a single global stylesheet into many smaller files — which isn't great at all. To make matters worse, if a child component has a similarly named CSS rule (say, for `h1`) as a parent component, the child component's rule will override the parent component's rule — **even though the child component's rule isn't imported into the parent component**. That creates even more of a problem!

For that reason, we recommend extreme caution when using external stylesheets with React. Here are the guidelines we suggest:

* If your application will have any global style rules, put them in `index.css`.
* If you plan to use stylesheets for individual components (which we don't recommend), make sure that classes and ids very specifically pinpoint elements in that component.
* Some animations and pseudo class selectors (like `hover`) can't be implemented with recommended practices like CSS objects — so stylesheets may be an acceptable option in these use cases.

So what do we recommend doing if stylesheets are generally not a best practice? CSS objects and using stylized components are both good ways to go. We will cover both approaches in future lessons. However, for now, we want to focus on learning the basics of React, not styling our applications. If you want to do some basic styling over the next few class sessions, a global stylesheet is enough.