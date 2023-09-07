Firebase offers much more than just a database solution — the service also provides an authentication solution that's easy to implement.

We'll be adding Firebase authentication to our Help Queue application. Before we do that, though, it would be nice to have a separate sign-in page in our application. We'll put a sign-in link in our header. Then, when a user clicks on that link, they will be taken to that page.

To do this, we will incorporate **client-side routing**. This is different from **server-side routing**, which is how tools like .NET and Rails handle routing. Let's take a quick look at the difference.

## Server-Side Routing
---

Let's think about the simple HTML sites we made in Introduction to Programming. Let's say we decided to host a site (maybe on GitHub, maybe somewhere else). The URL is _www.mywebsite.com_.

Users will view our site in their **clients** (usually a web browser, like Chrome or Firefox). They'll click on links to navigate to different pages on the site. These links depict the URL path of a different location on the site. For instance, we might have a URL like this: _www.mywebsite.com/portfolio_.

When a user clicks a link, the client executes an HTTP `GET` request to retrieve the **resources** at the _portfolio_ **path** from the **server** hosting the site. The server receives this request, gathers the resources being requested (which could be HTML, CSS, JS or other code), and then sends it back to the client in a **response**. The client then uses the contents of the response to render the new page for the user.

Each time a link is clicked, the client sends another request for information. The server receives and processes that request and then sends a response back containing the necessary resources. This ongoing back-and-forth conversation is called a **request-response loop**.

This is also the process that .NET and Rails applications use. It's the traditional way applications have handled routing — but it's not very fast, especially for more interactive sites.

## Client-Side Routing
---

Tools like the React Router library that we'll use follow a pattern called **client-side routing**. As the name implies, this means the client (probably our browser) is responsible for routing, not the server.

Let's imagine that _mywebsite.com_ is a React site that uses client-side routing instead. The user still visits the site by entering a URL or clicking a link in their **client**. The client still sends a request to the website's server to retrieve information about the site. But instead of responding with just the resources for that page, the server responds with a single JavaScript file containing _everything_ for the _entire site_. This file might have a name like _app.bundle.js_.

When the user navigates to a different part of the site such as _www.mywebsite.com/portfolio_), **the client doesn't send another request to the site's server.** Instead, the file that the server initially sent is already in the browser's resources. The client is responsible for locating the content for the new "page" based on the code in the _app.bundle.js_ file.

This means that technically we aren't going to a different page. It just looks that way. This also makes it faster to complete concurrent calls to APIs and databases while rendering a new document because we are no longer making server requests to load and render new pages.

In the next lesson, we'll add client-side routing to our Help Queue application with React Router. Then, once we've added a router and a sign-in page, we'll be ready to add Firebase authentication to our application.