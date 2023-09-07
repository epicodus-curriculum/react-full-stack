So far each of our tickets in the `TicketList` component use a key set to the index of a `map()` function. While this works as a temporary solution, it's not a good practice in a real world application. Instead, each of our tickets should have its own unique ID.

Unique IDs are important for many reasons. In larger database-backed applications, they are an essential way to differentiate between records. However, they are also useful even in React applications that don't use databases. Using the index of an iterator function isn't a reliable way to ensure that each record in a React application has a unique key. In fact, using the index as a key can potentially make our code less efficient and even break our application.

Remember that React relies on unique keys in order to efficiently reconcile the virtual DOM with the actual DOM. We should do everything we can to make sure those keys are always unique. In addition, unique IDs are helpful for correctly finding a record so it can be updated, deleted, and so on.

Fortunately, create-react-app ships with an industry-standard option: a JS library called **UUID**. The UUID library is designed to quickly generate UUIDs. UUID stands for "universally unique identifier". They're also sometimes referred to as GUID, or "globally unique identifiers".

A UUID has 32 characters. The number of total permutations of a UUID is 2<sup>122</sup>. This is such a large number that every computer application across the world, regardless of language or platform, could use UUID and still have an extremely tiny chance of duplicates. UUIDs are not specific to React, JavaScript, or even web development. They're actually used in everything from operating systems to SQL database keys.

To use the UUID library with create-react-app, we just need to import the following in the file that needs to generate UUIDs:

```javascript
import { v4 } from 'uuid';
```

`v4` refers to the method in the UUID library responsible for creating unique IDs. According to the [UUID Documentation](https://github.com/kelektiv/node-uuid), there are a few available methods: For instance, `v1()` creates an ID based on timestamp while `v5()` uses the object's namespace to generate an ID.

We could then set the ID property of a ticket with a UUID by doing something like the following:

```js
const ticket = {};
ticket.id = v4()
```

The `v4()` function will automatically generate a UUID.

In the next lesson, we'll begin to create our `NewTicketForm` component in which we'll use the UUID library to assign unique IDs to new tickets.