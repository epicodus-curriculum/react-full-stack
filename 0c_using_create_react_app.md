Some developers build React applications from scratch. This involves creating custom webpack configurations, adding Node packages, and customizing Babel to fit the needs of the project. While this is a valid approach to building React applications, we've had plenty of practice with webpack, Node packages, and Babel during the Intermediate JavaScript course.

Fortunately, we can use `create-react-app`, a tool that will set up our project for us. `create-react-app` uses tools like webpack behind the scenes but we will not need to customize our environment ourselves.

`create-react-app` comes with a default configuration right out of the box. This will make developing applications with React much more convenient. However, keep in mind that we won't be able to customize our configuration without **ejecting** our application, a process we will cover in just a moment.

While more complex applications may need custom configurations, `create-react-app` will cover all our needs and allow us to focus on learning React, not get bogged down in configuring our environment.

## Using `create-react-app`
---

We can create our first `create-react-app` with the following command:

```javascript
npx create-react-app help-queue
```

Note that we are using the `npx` command, not the `npm` command. `npx` is automatically bundled with recent versions of npm. The `npm` command is used for **managing** packages while the `npx` command actually **executes** those packages. Because we want to execute the `create-react-app` package, `npx` is the right command here. 

Another nice thing about the `npx` command is that it can automatically execute packages that haven't been installed yet, by going and fetching the information on the npm registry. So, there's no need to download a package before directing Node to use it.

In the case of `create-react-app`, npx will first ask permission to download the package, to which you should enter 'y'.

![Message from npx asking to install `create-react-app`](https://learnhowtoprogram.s3.us-west-2.amazonaws.com/React/npx-install-cra-message.png)

### Versioning Issues

However, there's one important thing to know about `npx`. It will download the latest version of `create-react-app`, as well as `react`, `react-dom`, `react-scripts`, and other packages. This is potentially an issue for two reasons:

* Later versions of `create-react-app` may not be compatible with all the libraries we use.
* The most recent version of `create-react-app` may have a bug that you'll need to find a workaround for.

As of July, 2022, the most recent version of `create-react-app` does in fact have a small bug when we run `$ npm run start`. Take a look:

![Message about deprecated technology after running `npm start` in our `create-react-app` app.](https://learnhowtoprogram.s3.us-west-2.amazonaws.com/React/cra-npm-start-warning-message.png) 

This bug is a warning. While this warning doesn't impact the normal functioning of our application, this does demonstrate what the issues we might run into look like when using the latest technology.

This issue has already seen a lot of discussion in the create-react-app community. Check out [this issue on the create-react-app GitHub repo](https://github.com/facebook/create-react-app/issues/12035) to see how developers have discussed this issue, the solutions they've reported, the related tools they are using, and the general back and forth of discussion. That issue links to other issues and PRs that discuss more solutions. This is truly what being a developer is all about! (If you have the time, I highly suggest reading or skimming through all of the commentary.) 

By the time you read this lesson, the issue may have already been resolved. In any case, we won't worry about addressing it in our applications. If you do find that you can't run your projects as expected, reach out to your instructor to get help.

### Looking at C-R-A Project Files and Folders

Now let's `cd` into our `help-queue` project and open it in the code editor. There aren't too many directories and files, so let's go through them one by one:

**node_modules**: All the packages we need to build a React application have already been installed. This includes both the core React library as well as the ReactDOM library, which allows React to interact with the DOM. It also includes quite a few Babel packages and other packages we won't get into. We won't be able to alter these packages but we can — and will — install more packages in the future.

**public**: This is where we can store public content. It also contains an `index.html` file. This file is a template and we won't be altering it. We've already worked with a template this way using `html-webpack-plugin` in Intermediate JavaScript. That's the exact same thing that's happening here. You can read more about the public directory [here](https://create-react-app.dev/docs/using-the-public-folder/). We won't be adding assets to this directory in this course. You might be wondering whether this is a good place to store images — however, it actually isn't. We'll cover adding images to applications that use `create-react-app` in a future lesson.

Note the following div in our `index.html` file:

<div class="filename">public/index.html</div>

```html
<div id="root"></div>
```

This is where our root React component will be placed. We'll learn more about components in the next lesson — and we will also cover where `root` is coming from later in this lesson.

**src**: `create-react-app` has built a `src` directory for us with some boilerplate code.

  * **`App`**: Let's start with the three `App` files. `App` is the root component of our application. `App.js` contains the JavaScript code for this component while `App.css` contains the CSS code for it. If you take a look at `App.js`, you'll see that it contains code that looks nothing like JavaScript. This code is written in JSX, which we'll cover in more detail soon.
  
    * Finally, we also have an `App.test.js` file for testing our App component. `create-react-app` uses Jest by default. We won't be writing any tests during this course section so don't worry about testing for now.
  
    * We aren't going to use the boilerplate files above — we are more interested in the configuration that `create-react-app` provides — so we will be deleting all three `App` files and starting from scratch when we begin our Help Queue project.

  * **`index.js`**: Next, we have two `index` files — an `index.css` file and an `index.js` file. **`index.js` is the entry point for our application.** This should look familiar from working with webpack. In fact, webpack will work behind the scenes here to recursively include all of our application's files in a bundle. We'll take a longer look at the contents of this file later in this lesson.
  
  * **`index.css`**: This is our global stylesheet. It's not a good practice to use global stylesheets with React applications, though this is where we'd do it. Instead, each component should have its own stylesheet. Because we are focused on building a basic React application this course section, we will not cover styling just yet.

  * **`logo.svg`** can be ignored and should be deleted. This is just a boilerplate image for our React app, and we will never use it. If you want to see how it is used before deleting it, go ahead and start your project's server with `$ npm run start`. The webpage that opens will display the logo.

  * **`reportWebVitals.js`** uses the `web-vitals` library to report on the performance of your React app. We won't be learning how to measure and analyze performance, so this file can be ignored, or optionally deleted. If you delete this file, you'll also have to delete the corresponding import and code in `src/index.js`. To learn more about measuring performance, visit [`create-react-app`'s documentation](https://create-react-app.dev/docs/measuring-performance/).
  
  * **`setupTests.js`** this file is for configuring information about tests to avoid repeating a lot of boilerplate code in our test files. However, this is specifically for testing that React components load as expected, which we won't learn how to do in this course. We'll continue to test business logic only. So, this file can be ignored, or optionally deleted. To learn more about testing components, visit [`create-react-app`'s documentation](https://create-react-app.dev/docs/running-tests/#testing-components).

**.gitignore**: `create-react-app` has  `.gitignore` started for us — and it conveniently ignores all the files we wouldn't want to push to GitHub.

**package.json**: Next, we'll take a look at our `package.json` file. Note there's hardly anything there. That's because most of the dependencies are hidden from view. Only three dependencies are listed:

  * `react`: The core React library;
  * `react-dom`: A library that allows React to interact with the DOM;
  * `react-scripts`: A library that allows us to run `create-react-app` commands.

We also have four npm commands we can run, three which look exactly the same as the scripts we had in Intermediate JavaScript:

  * `start`: Used to build and start our application;
  * `build`: Used to build but not start our application;
  * `test`: Used to run tests;
  * `eject`: Used to eject our application so we can update the configuration.

Note that once we eject an application, we can no longer use it with `create-react-app`. We should only do this if we absolutely need to — or if we'd like to see what's actually going on underneath the hood. Ejecting a React app will expose the base configuration files, the code for each script (such as `start` and `build`) and the full list of dependencies in our `package.json` file. If you are curious or would like to tinker, feel free to do this on your own!

**README.md**: This contains a lot of helpful (but potentially overwhelming) information about using `create-react-app`.

We can run our application by entering the command `$ npm run start` in the root of our project's directory. This will open a page at _http://localhost:3000/_ with the boilerplate code that `create-react-app` provided.

### A Closer Look at `index.js`

Let's take a look at the code in `index.js`. We'll skip the import statements and the `reportWebVitals()` function call.

<div class="filename">src/index.js</div>

```jsx
const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);
```

With this code, we use the `ReactDOM` library to handle inserting our React component tree into the DOM. Remember that React is a UI library that we use in part to create the look (the HTML and layout) of our webpage, so what `ReactDOM` does is handle taking our React code, and inserting it into the webpage. This process is called rendering; we "render" React elements to the DOM.

To understand each step the code above, it may be easier to slightly adjust it:

```jsx
const container = document.getElementById('root');
const root = ReactDOM.createRoot(container);
root.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);
```

Now, let's break this down:

* With the first line of code, we're accessing the HTML in `public/index.html`, and looking for the element with an id of `'root'`. This corresponds to the exact div we noticed earlier:

<div class="filename">public/index.html</div>

```html
<div id="root"></div>
```

* When we access this div element and save it in a variable, we call it `container`, because it will eventually contain all of our React components. In other word, it's the location where our React app will be rendered, and everything inside of this div will be managed by React.

* Next, we call `ReactDOM.createRoot(container);`, passing in our `container` variable. This creates a root DOM node (inside of the div) for React to render all of its components to. 

* Then, we call `root.render()`, which inserts the React components into the DOM; or, in other words "renders" the React components. When we call `root.render()` we must pass in a single element. For example, we could pass in this:

```jsx
root.render(
  <h1>Hello World!</h1>
);
```

* What we do is pass in a React component, nested inside of another React component. This counts as one element:

```jsx
root.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);
```

* The `<React.StrictMode>` component is an optional component that we can wrap around our parent component, `<App />`, to have it perform additional checks for errors in any components nested inside of it. That includes `<App />` and any components nested inside of `<App />`, and so on. Essentially this performs additional error checking on our whole React application, but it doesn't actually print anything to the DOM. (If you're feeling a bit fuzzy about the concept of "components", don't worry, we'll learn exactly what they are in the very next lesson!)  

* The nested component called `<App />` is the parent component for our entire React application. This will contain code that will be printed to the DOM. Note that the syntax `< />` is customary in JSX, which we will be going over more soon.

So, in the end, this code creates the **root DOM node** in which React will render our `App` component. Once again, the `root` variable represents the root DOM node that React uses to render its components, and it is located inside of the div with an id of `'root'`.

Note that **if you are using React 17 or below, the `index.js` configuration will look slightly different**, though it performs the same functionality:

```jsx
import ReactDOM from 'react-dom';
...

ReactDOM.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>,
  document.getElementById('root')
);
```

### How React and webpack Work Together

Now that we've covered all the components in a `create-react-app` project, let's quickly cover how webpack and React work together. This process should be familiar from Intermediate JavaScript, but it's worth a refresher because it's been a while since we've covered this content.

* webpack will recursively manage all of our application's dependencies and bundle them into a single file when we run `npm start`.

* `index.js` is webpack's entry point. 

* `index.js` calls the `root.render()` method, which specifies that the `App` component should be rendered into our `index.html` file's `root` div id.

* `App.js` will be rendered in the `root` div id. It is the parent component for our React application.

As you may have guessed, webpack will do many other things behind the scenes such as compiling our code so the browser can read it. However, because of `create-react-app`, we don't have to worry about any of that. We can focus entirely on learning React.