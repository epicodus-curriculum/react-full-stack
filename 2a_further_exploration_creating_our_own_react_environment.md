We've used create-react-app to build React applications because it's easy to get started and we can bypass the additional work of setting up our own webpack configuration. create-react-app comes with a lot of tools right out of the box and we can always add additional libraries as needed.

However, create-react-app has its drawbacks. As developers, we have less control over our environment. The inner workings of our environment remain opaque.

We have two options if we want to customize our environment ourselves:

* We can build a webpack environment from scratch and build our React application manually.
* We can take an application built with create-react-app (either a new one or one we've already been working on) and run the `npm run eject` command, then configure it further.

### Building an Environment from Scratch

In general, developers want to avoid spending too much time building out an environment — instead, we'd rather be coding. However, there are plenty of valid reasons to have our own custom React environment. Many React developers prefer not to use create-react-app, which is difficult to customize. In this case, the best approach is to do something similar to what we did in Intermediate JavaScript — create a basic webpack environment and then reuse it for our projects, adding libraries only when they're required. 

At a minimum, a custom environment will need React, a basic webpack configuration, and Babel, including Babel presets for JSX. At that point, you can then build out your configuration to use other plug-ins and loaders as needed. The process isn't much different from the webpack set up we've done in the past (other than needing to use specific Babel presets to transpile JSX to vanilla JavaScript).

There are many tutorials online for creating a React environment from scratch. However, you already have the tools to set up a custom environment from Intermediate JavaScript. If you want to build your own environment, you can follow along with lessons from [Intermediate JavaScript](https://www.learnhowtoprogram.com/intermediate-javascript/test-driven-development-and-environments-with-javascript/introduction-to-webpack) — or find a tutorial online specific to webpack environments with React. You can also eject a create-react-app application to get a sense of what libraries you may or may not need.

at the very least, you'll need the following [Babel preset](https://babeljs.io/docs/en/babel-preset-react) to transpile JSX code.

### Ejecting from create-react-app

We learned about the `npm run eject` command during the React Fundamentals course section. If you haven't had the chance yet, now would be a good time to eject an application made with create-react-app to see what's going on under the hood. This is a great opportunity to get a better sense of the tools at our disposal.

There are several situations where it's useful to use `npm run eject`:

* Right after a project is built with create-react-app — and with the intention of learning more about what create-react-app offers
* At a point in a project's development where we have no choice but to eject in order to build our project further

In general, it's not a good idea to eject an application because there's no going back. In the first use case listed above, we have nothing to lose — we're just trying to learn more about create-react-app and different external libraries.

In the second use case, we've reached a point of no return and we simply can't use create-react-app anymore. Even then, we should carefully consider whether there's a smaller tweak that we can use so we don't need to eject.

If you plan to build your own custom environment using webpack, we don't recommend building a new application with create-react-app and then immediately altering it. You'll still end up with all the bloat from create-react-app — and fewer benefits of streamlining your own environment. Instead, consider building your environment from scratch while using an ejected create-react-app application as a reference point. You'll have a sense of which libraries to use and which versions work best together.

Once you have a bare bones development environment that correctly transpiles JSX, includes a development server, and other features you might consider necessary (such as linting), either start a new repo or a new branch for more custom configurations. Eventually, you'll end up with one and possibly more configurations tailor-made to specific use cases.

There's a final option that combines a custom environment with create-react-app. We can fork `react-scripts` and create our own custom configuration. This option is a bit more advanced so we won't cover it in detail here — but there are numerous tutorials online that describe how to do it.

Even if you plan to continue using create-react-app, we recommend tinkering with a custom environment. Many workplaces will have custom environments. If you have a better understanding of creating this kind of environment for React now, it'll be one less thing to potentially overwhelm you at an internship or new job.