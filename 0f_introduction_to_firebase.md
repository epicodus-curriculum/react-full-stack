Firebase is a cloud-based NoSQL database solution offered by Google. In this lesson, we'll give some background on Firebase and Firestore, the realtime database we'll be using. Then, in the next lesson, we'll discuss NoSQL in detail — including how it's different from SQL.

As we now know, React is not an opinionated library. React can work equally well with any kind of database, whether that's SQL, NoSQL, or another solution. And while it works well with Firebase, there are many other cloud-based solutions that work with React.

However, Firebase is widely considered the most popular and well-maintained backend data service provider. It also works well with React applications.

## Backends-as-a-Service (BaaS)
---

Firebase is a realtime, cloud-based storage provider. What exactly does that mean? **Cloud-based** means it exists online, or "in the cloud." **Realtime** means we can see database changes immediately in our online dashboard.

Firebase is also a **Backend-as-a-Service**, often abbreviated as **BaaS**. This is part of an application's backend (like a database) managed and provided by an online service (like Firebase). Note that BaaS providers often provide more than just cloud storage. For example, Firebase also provides:

* Analytics
* Authentication
* Performance monitoring
* Messaging
* Hosting
* And more!

To see a full list of Firebase offerings, visit [https://firebase.google.com/](https://firebase.google.com/).

You'll almost certainly encounter other BaaS solutions in your future career. Other examples of backends-as-a-service include:

* [Amazon Web Services (AWS)](https://aws.amazon.com/products/frontend-web-mobile/) is one of the largest BaaS providers. AWS offers tools for authentication, database, analytics, file storage, hosting, and more.

* [Auth0](https://auth0.com/) is a user management platform with single sign-on, multifactor authentication, and other security features.

* [Backendless](https://backendless.com/) advertises itself as a "complete" backend. It includes user auth, data persistence and file storage, messaging, and other business logic.

These are just a few common BaaS solutions, but by no means not an exhaustive list.

### Benefits of BaaS

There are many benefits to using a BaaS, most notably:

* **Decreased development time.** Using an existing, thoroughly tested outside tool means we don't have to write and test that code ourselves. This can decrease development time and cost.

* **Smaller learning curve.** The learning curve to use a BaaS is usually lower than constructing the same features from scratch.

* **Scalability**. If we wrote our own backend from scratch and our application exploded in popularity overnight, we would experience issues if we didn't construct our code to handle heavy traffic. Big services like Firebase and AWS are already designed to handle high traffic.

* **Multiple features in one.** Many BaaS providers offer more than one service. For instance, Firebase offers a database, user authentication, push notifications, and more. As soon as one of these BaaS features is integrated, it's often easier to integrate additional features from the same service.

### Drawbacks of BaaS

However, there are also downsides to a BaaS solution:

* **Lack of control**. If another service writes your back end, you can only make customizations the service allows. Your codebase is vulnerable to changes and bugs that may occur in the BaaS.

* **Cost**. Tools like Firebase are often free for small applications. However, they charge for more premium tools, increased traffic, and other services. Depending on the circumstances, this may not be cheaper than creating a custom backend for a company.

* **Stability**. You may risk losing your backend if a BaaS provider closes doors. This happened to users of a service called Parse, which closed its doors and left its users scrambling to find other solutions.

While there are some drawbacks to using BaaS, the positives generally outweigh the negatives — which is a big part of the reason services like Firebase and AWS are so popular. Ultimately, we see several major benefits of learning Firebase:

* Learning about and understanding the concept of BaaS is an essential part of having a successful career in tech;
* NoSQL is a common type of database used in the industry and it's important to know how a NoSQL database works;
* Learning about NoSQL can help us better understand the advantages and disadvantages of SQL;
* Firebase is a popular tool in its own right and a nice addition to a developer's resume.

### Firebase Versus Firestore

Firebase is the name of the BaaS we are using. It's also the name of one of two realtime database solutions we can incorporate in our projects. The Firebase Realtime database stores all data in a large JSON object. The downside of this approach is that it's more difficult to query this database.

Fortunately, Firebase also provides another database option called Firestore. This is also a realtime cloud-based database solution — and it's the one Firebase recommends for the majority of users. One of the biggest advantages of Firestore is that it includes functionality that makes it easier to query our database.

So remember that we are using Firebase as a BaaS but we are using Firestore as our database solution.