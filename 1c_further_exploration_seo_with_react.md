Up to this point, we've focused entirely on creating small applications without thinking much about deployment. Even when we do have a chance to deploy our work, we aren't thinking about web traffic. More likely, we're deploying as part of our learning experience — and hopefully to share our project with friends, family, and potential employers.

However, most real world applications are concerned about increasing the number of users that visit their site. A key way to increase visitor traffic is to incorporate **SEO** (**search engine optimization**).

Search engine optimization seeks to increase visitor traffic through search engines such as Google or Bing. For instance, if we do a Google search for "Portland online code school," we'll get a list of results that includes Epicodus.

According to [research](https://www.brafton.com/news/95-percent-of-web-traffic-goes-to-sites-on-page-1-of-google-serps-study/), 95% of all user searches only involve the first page of search results. For that reason, it's really important for websites to end up on that first page of user searches. Unlike paid advertising, web searches are free — and an essential way for sites to generate more users.

SEO is a huge topic — and it's well beyond the scope of this lesson. This lesson will focus on the issues that React applications have with SEO — and how we can address them. If you're interested in learning about SEO in general, we recommend checking out a more in-depth source [like this one](https://moz.com/learn/seo).

### React and SEO

React applications are often SPAs (single page applications) with components that render dynamically based on user activity. This makes React extremely fast, but it's not ideal for the web crawlers that index sites. There are several reasons for this:

**React applications have only one URL.** Because many React sites are SPAs, they only have one URL along with a number of virtual pages. Web crawlers may not be able to reach all the virtual pages, resulting in a lower search index.

**Because there is only one URL — and one entry point — every virtual page has the same meta data.** We can add meta tags to our HTML pages to improve SEO. When users navigate to a page, they won't see the meta content because it's not visible. However, this content is visible to web crawlers — which often use it to create a description of the site in a search engine. Meta content is also used to determine search engine rankings as well.

**React is all about dynamic JavaScript.** Most web crawlers can handle JavaScript — but not necessarily all JavaScript. For that reason, crawlers might not be able to access some content that users can — and this content won't be indexed.

There are several things we can do to improve the SEO of a React application. You're encouraged to explore these further on your own. 

### Isomorphic React

A standard React application is client-side, not server-side. The application is downloaded as a bundle on the client machine. However, if the user has JavaScript disabled (or a crawler has issues with JavaScript content), the site won't render properly. An isomorphic React application adds server-side rendering to a React application. If there are issues with the client-side copy (such as JavaScript being disabled), the server-side copy of the application is switched in. This can help ensure that web crawlers are able to index all of a site's content.

Server-side rendering can also help with another significant issue — sharing site content with others.

Let's say you want to share a virtual "page" of a React application with a friend. Without React Router, we can't do that. There is only one URL and we can only share that URL — not the exact configuration of clicks and user interaction we completed to arrive at the virtual page.

Adding routes with React Router doesn't solve this issue. We are still downloading a bundle in our browser and all routes from React Router are stored there. If we tried to send a link to a friend — for instance, _my-awesome-react-app.com/bio_ — our friend won't be able to open that link. There will be a 404 error.

We can solve this problem by using React Router to create server-side rendering — in other words, we need to build an isomorphic React application. 

Web crawlers will also be able to crawl these URLs, improving our application's SEO.

For more information on this topic, see the [Server Rendering](https://reactrouter.com/en/v6.3.0/guides/ssr) documentation at React Router. You will also likely need to find a tutorial to set up an isomorphic React application.

Previously, create-react-app did not support server rendering. Now, you can configure a create-react-app application to work with libraries to render your app's content server-side. To learn more, visit the create-react-app documentation titled [Pre-Rendering Into Static HTML Files](https://create-react-app.dev/docs/pre-rendering-into-static-html-files/).

### Title and Meta Tags

Even if we don't create an isomorphic React application, we can still use React Router to create separate URLs. Ideally, each URL should have its own meta tags. This will allow search engines to do a better job indexing our site — and not just the entry point index page but other pages on our site as well.

create-react-app provides some help with this. See the [Title and Meta Tags](https://create-react-app.dev/docs/title-and-meta-tags/) documentation that create-react-app provides.

There are also other tools for creating title and meta tags. The most popular is probably [react-helmet](https://github.com/nfl/react-helmet), which was developed by the National Football League. react-helmet allows us to write our title and meta tags inside a `<Helmet>` element like this:

```js
...
<Helmet>
  <title>React Portfolio Site</title>
  <meta name="description" content="An awesome portfolio site" />
  <meta name="keywords" content="React, JavaScript, Firestore, Epicodus">
  <meta name="author" content="Epicodus Student">
</Helmet>
...
```

We haven't discussed meta tags in the past, but there are different tags such as `name` and `content`. `name` is the type of meta content (such as a description or keywords). `content` is self-explanatory — and web crawlers can take this information and use it to determine how highly a site should be rated.

### React Frameworks

There are a handful of React frameworks that provide server-side rendering. These frameworks provide a whole toolset that can make bootstrapping React applications that much easier... that is, after you are past the learning curve. 

Here are some of the most popular React frameworks for web development: 

* [Gatsby.js](https://www.gatsbyjs.com/)
* [Next.js](https://nextjs.org/)
* [Remix](https://remix.run/)

We highly recommend researching these frameworks to learn about the tradeoffs and benefits of each.

These are just a few approaches to improving SEO. Having an understanding of how SEO works — and how to improve it in a React application — is a valuable skill. Whether you're working for a small or large company, a development agency, or even creating side projects for friends and family members — these little details are essential to providing free advertising and traffic to websites.