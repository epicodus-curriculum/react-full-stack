React is a JavaScript library used to create highly dynamic views. In this lesson, we'll explore its history, who uses it, and the functionality and benefits it offers.

## History
---

React was developed at Facebook in 2011 for use in their newsfeed area. After continued issues with long load times, they needed to develop something fast and scalable enough to deal with extremely heavy traffic — [Facebook reported](https://newsroom.fb.com/company-info/) 1.28 billion daily active users in March 2017 alone.

In 2013, React was made open source. Since then, it has become extremely popular. Sites ranging from Instagram to AirBnB use it.

## What is React?
---

What exactly is React? At its core, it's a **library** for developing the view layer. A library focuses on one piece of functionality and that's exactly what React does. It isn't concerned with the back-end of an application. Instead, it manages how user interfaces look and behave.

React is also **highly dynamic**. This means it can handle views that need to change a lot. React allows us to quickly and dynamically update the UI without reloading pages. Any application or site that requires frequent updates is a great candidate for React.

## Who Uses React?
---

Here are a few companies, products, websites, and apps that use React's library:

* [Angi](https://www.angieslist.com/)
* [AirBnB](https://www.airbnb.com/)
* [Asana](https://asana.com/)
* [BBC](http://www.bbc.com/)
* [BleacherReport](http://bleacherreport.com/)
* [DeviantArt](http://www.deviantart.com/)
* [Ebay](http://www.ebay.com/)
* [Everlane](https://www.everlane.com/)
* [Facebook](https://www.facebook.com/)
* [GlassDoor](https://www.glassdoor.com)
* [IMDb](http://www.imdb.com/)
* [Imgur](http://imgur.com/)
* [Instagram](https://www.instagram.com/)
* [Intuit](https://www.intuit.com/)
* [Khan Academy](https://www.khanacademy.org/)
* [Lyft](https://www.lyft.com/)
* [Netflix](https://www.netflix.com/)
* [New York Times](https://www.nytimes.com/)
* [OKCupid](https://www.okcupid.com/)
* [Patreon](https://www.patreon.com)
* [Paypal](https://www.paypal.com/us/home)
* [Pinterest](https://www.pinterest.com/)
* [Reddit](https://www.reddit.com/)
* [SeatGeek](https://seatgeek.com/)
* [Tesla](https://www.tesla.com/careers/)
* [Twitch](https://www.twitch.tv/)
* [Venmo](https://venmo.com/)
* [Weather Underground](https://www.wunderground.com)

And many, many more!  

## Why Use React?
---

React also has many other benefits as well, including:

### Component-Based

React is component-based, which assists in organizing even the most complex user interfaces into small packages of dedicated functionality. We can also reuse components to keep projects organized and DRY.

### Declarative

React components are **declarative**. As we discussed in the last course section, declarative programming is when we write code that describes the intended end result instead of writing every single step required to reach that result. Writing declarative code saves us time because we don't need to explicitly state every step needed to reach the end result. Declarative code is also easier to read and understand.

### Data Model Synchronization

Updating user interfaces to reflect changing application data is one of the most difficult challenges that web developers face on a day-to-day basis. React includes built-in functionality to automatically synchronize our data models with our user interface. That means when we update a piece of data in our application (called state), we can code our user interface to automatically update to reflect that change.

### The Efficient Virtual DOM

React utilizes a **virtual DOM**. We'll explore what this is and how it works in the next lesson. This feature allows us to interact with the DOM more efficiently and with much less code than other libraries and frameworks.  

### Easier to Create Single Page Applications

For a long time, all websites were multi-page applications. Many still are, including future MVC applications we will eventually create with .NET in the next course. Whenever we request to see information or complete an action, we navigate to a new page. For example, check out the way a user interacts with the multi-page site [kiva.org](https://www.kiva.org):

![kiva-multi-page-app-example](https://learnhowtoprogram.s3.us-west-2.amazonaws.com/React/kiva-multi-page-app-example.gif)

Each time we request to see different information, we navigate to a new page entirely.

Now many popular sites like Gmail, Facebook, Instagram, and Twitter are **single-page apps**. They load a single HTML page and then dynamically update that page as necessary. This creates a smoother user experience due to the reduced number of page refreshes.

For instance, check out a user on Instagram's single-page application can navigate another user's profile. Notice how it differs from the experience above:

![instagram-spa-example](https://learnhowtoprogram.s3.us-west-2.amazonaws.com/React/instagram-spa-example.gif)

The user can complete multiple actions without navigating to a new page, reloading, or refreshing. The page the user is already on is simply updated dynamically.

However, despite the fewer number of pages, creating and maintaining single-page applications is considerably more difficult than making traditional multi-page sites. The DOM needs to be updated constantly. The data and the user interface must constantly be synchronized even though making DOM updates is an inefficient process.

React makes creating these modern single-page applications much easier due to its virtual DOM and data model synchronization capabilities.

### JSX

React uses a special syntax called **JSX** that allows developers to mix HTML with JavaScript. While not mandatory, developers report that JSX makes developing in React much easier. Nearly all React applications use JSX syntax. We'll use JSX, too.

### Support

Facebook maintains React. It is a large, established company with the resources to support and maintain React (and its documentation) into the foreseeable future. Our applications will be more stable if we use well-supported tools.

### Library Benefits

Because React is just a view library, developers have tremendous flexibility to build out other parts of their applications as they see fit.

### React Native

React developers can explore building mobile-friendly applications with [React Native](https://reactnative.dev/). Both React and React Native follow the same design patterns. Once you have a strong foundation in React, you'll be that much more prepared to build mobile-friendly.
