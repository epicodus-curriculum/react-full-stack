In this lesson, we'll cover adding static assets such as images to a React application that uses `create-react-app`. You aren't required to add images for any independent projects. However, you might want to make an application look nicer with images and other graphics, especially any applications that you plan to include in your portfolio.

In general, adding images to a React application is pretty easy. However, there are a few important gotchas which we'll cover in this lesson.

Let's add an image to our Help Queue. You can pick an image of your choice or simply follow along with this lesson later when you plan to add images to an application.

We've added a picture of tickets to our application as the image below shows:

![Header component has a tickets image.](https://learnhowtoprogram.s3.us-west-2.amazonaws.com/React/Week-1-React-2019/tickets-image.png)

It's not a stylistically impressive site — but it'll do for learning purposes.

In MVC applications such as Rails or .NET, it's common to use a public folder for static assets like images. For .NET, that folder is `wwwroot`. Traditionally, Rails applications used a `public` directory, though Rails now uses a different directory for static assets. Based on that, it may seem like we should do the same with a React application that uses `create-react-app`. After all, `create-react-app` automatically creates a `public` directory in the top level of a `create-react-app` project — and to make matters more confusing, it automatically ships with several image files.

However, we should _not_ use the `public` directory for most images. This is clear if we look at the `create-react-app` documentation on [Using the Public Folder](https://create-react-app.dev/docs/using-the-public-folder/). According to this doc:

> Note that we normally encourage you to import assets in JavaScript files... This mechanism provides a number of benefits.

The biggest benefit is that webpack will minify and bundle our assets as long as we are within the module system. But what does it even mean to be within the module system? Well, what does webpack bundle in a `create-react-app` application (or even a basic JS application that uses webpack)? The `src` directory. In order to benefit from bundling, our images need to be stored in the `src` directory. We definitely want that benefit because it will make our code more efficient in the browser.

There's another huge benefit of storing files such as images in the `src` directory. Let's say we use the `public` directory for an image but that image is missing. We'll just get a broken image link in our application. It's really easy to miss a broken image or another missing file because our application won't throw any errors. We likely won't notice unless we look at the application in the browser. However, when these files are stored in `src` and combined with `import` statements, they will throw errors in compilation so we can fix them immediately.

In fact, if we try to use images stored in the `public` directory, `create-react-app` won't even let us. We'll get the following error: `Relative imports outside of src/ are not supported.`. There are ways around this — and there are special use cases when a developer might need to use the `public` directory for static assets. After all, that's why the directory is there. However, we don't have any special use cases — we just want to add images to our application.

So instead of using the `public` directory, create an `img` directory (or a similarly named directory) in `src` instead. Go ahead and save any photos you want to use in that directory. For the image above, we've saved an image called `tickets.png` in a directory called `img`.

Next, let's consider where we want to render this image. We should apply the same best practices to rendering images as we do to rendering other code. In this case, we'll make it part of our header and put it in `Header.js`.

Next, we need to import the file just as we'd import any other component. Here's how we can add it to `Header.js`:

<div class="filename">components/Header.js</div>

```js
import React from "react";
import ticketsImage from "./../img/tickets.png";

function Header(){
  return (
    <React.Fragment>
      <h1>Help Queue</h1>
      <img src={ticketsImage} alt="An image of tickets" />
    </React.Fragment>
  );
}

export default Header;
```

Just as with other default imports, we can call the thing we are imported whatever we want — as always, we should be very clear on the name. We've called this `ticketsImage`. We could just call it `tickets`, though that might get confusing later with all of our similarly named components.

Next, we need a standard `<img>` tag. This has all of the attributes of a typical HTML `<img>` tag — and as always, we should add an `alt` attribute to make our site more accessible to users with disabilities. The key difference is that we use curly braces to render our image inside the `src` attribute.

And that's all there is to adding an image to a site! Just make sure you follow the best practices outlined in this lesson and save your images in a directory inside `src`. For more information on adding images with `create-react-app`, see [Adding Images, Fonts, and Files](https://create-react-app.dev/docs/adding-images-fonts-and-files/).
