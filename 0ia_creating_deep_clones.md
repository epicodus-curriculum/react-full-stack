You will not be expected to create deep clones of objects this course section. However, it's still helpful to know how we might go about creating a deep clone instead of a shallow clone.

As we discussed in the lesson on the spread operator, there isn't native support for deep clones in JavaScript. That being said, there's still a way to do it. It involves combining two JSON methods like this:

```js
const objectCopy = JSON.parse(JSON.stringify(originalObject));
```

In the example above, we serialize the object into a JSON string. Then we deserialize it back into an object. This breaks the reference to the original object.

This is a bit hacky, however. For one, it doesn't look very pretty. It also doesn't work very well with large objects — and if the `JSON.stringify()` method doesn't properly serialize everything inside the object, then some information will be lost.

The better solution for deep clones is to use an external library. There are multiple options, so we'll just mention a few:

[Lodash](https://lodash.com/) is a popular utility library that includes a `clonedeep()` function that can be used like this:

```js
const clonedeep = require('lodash.clonedeep');

const objectCopy = clonedeep(originalObject);
```

You can include just the Lodash `clonedeep` function in a project with the following command:

```javascript
npm i lodash.clonedeep
```

Lodash also has a lot of additional functionality as well. For instance, it has a function that can be used to automatically curry functions with multiple arguments. If you find that you want to experiment with other Lodash functionality, you can install it like this instead:

```javascript
npm i lodash
```

In this case, you'd import `clonedeep` like this:

```js
const clonedeep = require('lodash/clonedeep');
```

Another very popular tool that focuses on deep clones is [immutability-helper](https://github.com/kolodny/immutability-helper), which includes an `update` function that creates deep clones. We can use it like this:

```js
const update = require('immutability-helper');

const objectCopy = update(originalObject, {prop: "property to update" });
```

`update()` takes two arguments. The first is the object to be copied. The second is any property that needs to be updated in the copy. This library is commonly used with React but can also be used with vanilla JavaScript.

Note that we used `require` statements above — which works well for Node command line applications. You can also use `import` statements to use these libraries as well.
