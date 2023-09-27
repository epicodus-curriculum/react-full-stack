So far, we've used JavaScript to build browser applications. When we want to try out snippets of code, we've used the console in Chrome Developer Tools or an online development environment like Codepen.

If you are a full-time student, you have experience using a REPL (Read - Evaluate - Print - Loop) — either with C# or Ruby. A REPL is a tool we can use to evaluate code in the terminal. With JavaScript, we can use Node to run our code in the terminal. Node is a runtime environment just like the browser. The difference is that the browser is a client-side environment while Node is a server-side environment. To distill this a little further, JavaScript in the browser is "front end" code while JavaScript using Node is "back end" code.

Both Node and Chrome use the V8 JavaScript engine. The V8 JavaScript engine is an open-source project developed by Google and written in C++. It uses just-in-time (JIT) compiling to translate JavaScript into code our machines can understand. This makes the V8 engine extremely fast, which is part of the reason Node is a popular solution for server-side projects.

We will not cover Node in depth in this course. However, we can use Node both as a REPL (Read - Evaluate - Print - Loop) and as a tool to run our JavaScript programs. (If Node is not installed on your personal machine yet, see the [installation instructions here](https://new.learnhowtoprogram.com/intermediate-javascript/setting-up-javascript/installing-nodejs)).

To use Node as a REPL, simply type `node` in the Terminal.

This will bring up a command line prompt:

```js
>
```

Now we can type in JavaScript code and the REPL will evaluate it. Here's an example:

```js
> "Node".concat(" is server-side.");
'Node is server-side.'
```

We can type the `Tab` key to get a list of Node commands. Some of these commands are JavaScript methods and data types that we've used before. Others are for using server-side Node.

Finally, to exit the REPL, hold down the Ctrl and c keys twice.

The Node REPL can be helpful for trying out code.

We can also run a JavaScript file using the `node` command. Let's move the code above into a file. We will also need to add a `console.log()` in order for our code to print to the command line:

<div class="filename">js-code.js</div>

```js
console.log("Node".concat(" is server-side."));
```

Now we can run our script using Node:

```js
$ node js-code.js
Node is server-side.
```

Note that the string characters won't show up in the Terminal.

It may seem odd to use `console.log()` statements to print commands to the Terminal. However, `console.log()` happens to use a method called `process.stdout.write()` under the hood. `stdout` is short for **standard output**. Standard input is for data streaming into a program while standard output is for data streaming out of a program. It is not specific to Node or JavaScript.

Alternatively, if we already have Node open, we can add a require statement to add a script. (This might be useful if we were experimenting with several scripts at once.)

```js
> require('./js-code.js')
```

Note that we must include the relative path `./` for this to work.

You may choose to use Node, the Chrome console or an online code editor like Codepen to experiment with functional code. At the very least, we recommend trying out the Node REPL.
