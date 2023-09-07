Over the next several lessons, we will learn about reducers and use them to create all the CRUD functionality we'll need in our Help Queue. In this lesson, we'll start the project set up process. We will:

* Open up our Help Queue project.
* Learn how testing works with `create-react-app`.
* Create the folder structure for our reducers and reducer tests.
* Learn about naming conventions for testing.

Take note that we don't need to actually install Redux in our project yet — that's because our reducers and tests are just plain JavaScript. So setup in this lesson is focused on adding folders to our project.

Later on, we'll be integrating our reducers with Redux in our Help Queue application, so we'll start building off that project starting now. You may either use your own repo or use the following repo, which contains all the code from the React Fundamentals course section: 

---
**[<i class="glyphicon glyphicon-folder-open"></i>  Starter Project for Help Queue](https://github.com/epicodus-lessons/react-help-queue-starter-project)**

Note that the remainder of this lesson can be applied both to the Help Queue project and to setting up new projects that will use Redux.

### Testing in create-react-app

Fortunately, `create-react-app` is ready for us to start writing tests right out of the box: **`create-react-app` uses Jest**! However, this isn't apparent from the `package.json` file that `create-react-app` generates, which has the following script for the `test` command:

```json
"test": "react-scripts test"
```

Remember that `react-scripts` takes care of our configuration under the hood so we don't have to add any packages like Babel. 

Be careful if you do choose to modify the `test` script. For instance, the following will not work:

```json
// This will not work!

"test": "jest"
```

Even though `create-react-app` is using Jest under the hood, it is doing so with a specific configuration (which includes Babel). If we just run the `jest` command, it will do so without Babel and everything else we need to run our tests. As you may recall from our JavaScript course, Jest uses Node `require` statements and doesn't recognize `import` statements without help from Babel. This is just one of many little details that `create-react-app` takes care of for us!

`create-react-app` does not come with Redux. This makes sense — Redux is a separate state management library and smaller React applications won't need it. There's no need to bloat `create-react-app` with packages that we might not need.

Over the next handful of lessons, we will just be building and testing our first reducer. Since it is just plain JavaScript, we actually don't need to use Redux yet, so we won't add it until we actually plan to incorporate Redux into our Help Queue project.

### Setting Up a Folder Structure

Let's set up a folder structure for our reducers and tests. Open your Help Queue app, and do the following:

* Create a directory called `__tests__` inside `src`. 
* Then add a directory to `__tests__` called `reducers`.
* Finally, add a directory called `reducers` to `src`. (This is **different** from our other `__tests__/reducers` directory — one holds our reducer tests while the other holds the actual reducers.)

When you've completed the above steps, your file structure for these directories should look like this:

```
src
  |__  reducers
  |__  __tests__
        |__  reducers
```

We will be using this structure for our tests and reducers for every project we create that uses Redux.

### Test File Naming Conventions

We will also append `.test.js` to the name of all of our test files. This way, Jest will properly be able to find our tests. For example, in the next lesson we'll create our first test file, which will be named `ticket-list-reducer.test.js`. There's two things to note about test file naming conventions:

* We use hyphens to separate words, not underscores or other special characters. 
* The name of the file should correspond to the name of our reducer. In turn, the reducer file name should clearly communicate what the reducer handles. In the `ticket-list-reducer.test.js` example, we can tell this is a test file for a reducer that handles our ticket list.

Up next, we'll write a test for our first reducer — and start building the reducer itself.
