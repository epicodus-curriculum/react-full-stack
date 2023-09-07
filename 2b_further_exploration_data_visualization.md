Data visualization is the process of creating a visual representation of information. Data visualization can make data really pop. It's a tool for making information more digestible, immersive, and generally interesting.

In this lesson, we'll cover three of the most popular data visualization libraries. You may wish to build a portfolio piece or capstone process around data visualization — or you may wish to add a small interactive chart to make your application stand out further. At the very least, you should have a basic understanding of what these libraries do — data visualization is a valuable job skill — and it's also a great way to differentiate your skill set from other junior developers applying for the same jobs.

After giving a brief overview of three data visualization tools, we'll provide a recommended structure for exploring these tools further on your own.

### D3.js

[D3.js](https://d3js.org/) is the most popular and well-known library for data visualization. Note that we are making a small but important distinction here: while D3.js is excellent for data visualization, it can do many other things as well. In fact, trying to learn D3.js all at once is too overwhelming — and unnecessary as well. For this reason, if you choose to use D3.js, focus on what you want your data visualization to do instead of what D3.js _can_ do — because there will be a lot of D3.js functionality you won't need for your use case.

D3.js was created by Mike Bostock, who used it to create visualizations at the New York Times before moving on to found his own company Observable. You can take a look at some of his New York Times visualizations [here](https://bost.ocks.org/mike/). These are powerful examples of what we can do with data visualization.

You can also see excellent examples of D3.js at work [here](https://observablehq.com/@d3/gallery).

Combining D3.js with React applications can be challenging. This is because both React and D3.js manipulate the DOM — and both have their own ways of doing things.

There are numerous tutorials online to combine React and D3.js — and making a vanilla JavaScript application with D3.js is also an option if you're determined to explore D3.js but don't want to combine it with React.

Another option to consider is the [React D3 library](https://react-d3-library.github.io/), which compiles D3.js code into React components.

Ultimately, while D3.js has a steep learning curve — and takes a little extra work to combine with React — it has a huge community, lots of plugins, and lots of online tutorials and support. It's by far the most popular library for data visualization for a reason.

### Chart.js

If complex data visualization seems too overwhelming but you're interested in adding a few charts to your application, [Chart.js](https://www.chartjs.org/) is a great option. As its name implies, Chart.js is great for adding charts including line, bar and pie charts among others. These charts go inside what's called a "canvas." Chart.js can also be used for other data visualization needs as well. While it's not quite as popular as D3.js, it's easier to implement. You can see examples of charts and other visualizations made with Chart.js [here](https://www.chartjs.org/samples/latest/).

There's also a helpful library called [react-chartjs-2](https://github.com/jerairrest/react-chartjs-2) that provides a React wrapper for adding Chart.js visualizations to React applications. This library makes it much easier to implement Chart.js. However, we recommend trying to integrate charts into React without this library first. It's not too difficult — and it will provide some interesting problem-solving in terms of getting multiple libraries to work together.

### three.js

While [three.js](https://threejs.org/) isn't as widely used as either D3.js or Chart.js, it's still very popular. It also has an interesting approach to rendering objects. We start with a scene (which is equivalent to a canvas in Chart.js). Then we can use cameras to determine how we will view the objects we create. We use a renderer to actually render the scene. We use mesh and materials to fill in these objects. It's also relatively straightforward to integrate three.js with React. We recommend starting with [Creating a Scene](https://threejs.org/docs/index.html#manual/en/introduction/Creating-a-scene) in the documentation. See if you can figure out how to place the object created in this basic tutorial in a React application. There are walk-throughs online that demonstrate how to do this.

In addition, there's also the well-supported [react-three-fiber](https://github.com/react-spring/react-three-fiber) library, which allows developers to use three.js with React. It was developed by the maker of react-spring, which we will discuss in the next lesson on UI animations.

Check out [examples](https://threejs.org/examples/) of three.js at work.

### Getting Started

Getting started with data visualization can be overwhelming. If you choose to explore this topic further, we recommend following the steps below. You don't have to complete all of the steps — even just doing the first one or two can give you some valuable layperson's knowledge on these tools.

1. Take a look at the documentation. You don't need to read extensively yet — just get a feel for the library and take a quick look at any tutorials. Do you see anything you want to explore further? During this step, you should get a general sense of what the library does and how it does it. (For instance, what exactly is a canvas or a scene?) We recommend taking a look at the documentation for all three tools mentioned above (and perhaps others that interest you), so you can get a sense of which one you'd most like to work with.

2. Take a look at the library's provided examples. If you already have a specific visualization in mind for a project, do you see anything in the examples that could get you started? If you don't have a visualization in mind yet, do you see anything that inspires you that you might want to recreate? If so, you'll be able to use the source code as a reference for your own exploration.

3. By this point, you've likely chosen a specific library to work with. Complete a beginner tutorial. Don't worry about incorporating React yet. At this point, we just want to get a better understanding of how the library works. That likely means building a small vanilla JavaScript project, which is completely fine.

4. If you're not ready to get started with React yet, you may even want to create a proof of concept using just vanilla JavaScript. Try to build out your visualization without using React yet. An interesting data visualization using vanilla JavaScript can still be the basis for something you might want to share with potential employers.

5. Once you're ready to get started with React, start with the most basic implementation you can. If you've completed a basic tutorial or completed a proof of concept for a data visualization, it's time to try integrating this into a React project created just for this purpose. You're still in the learning phase here — the goal at the moment is to determine how React and the visualization library in question work together.

6. Finally, you should have a good enough understanding to add visualizations to a more complex project. As always, start simple — even a small visualization can really make your project pop.

While it may not be necessary to complete all of these steps, especially if you find an excellent external tutorial that provides instructions on combining a visualization library with React, there are many benefits to doing a deeper dive. It's an opportunity not just to learn more about a data visualization library but also more about how React itself works.