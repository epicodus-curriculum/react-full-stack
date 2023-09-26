In 2015, Facebook created React Native, an open source version of React meant for mobile development. This allowed developers to build truly cross-platform applications with JavaScript. iOS and Android apps for Facebook, Instagram, Skype, [and more](https://facebook.github.io/react-native/showcase.html) are built with React Native. Many companies that already use React for websites often opt to use React Native for their mobile apps because there is enough similarity between the two libraries to share code.

This lesson will provide a quick introduction to the basics of React Native, including how it differs from React. If you wish, you may use React Native for your capstone project — and you are also welcome to add a React Native front end to your project in this course section.

### React vs. React Native

While React Native is very similar to React, there are notable differences.

* **React Native is a framework.** This is in contrast to React, which is a library. This means React Native comes with everything we need to get started.

* **Applications run on a mobile device or emulator.** We can't test our applications in the browser because React Native apps are meant to be run on mobile devices. We must either launch them on physical mobile devices (which isn't great for development) or an **emulator**, which is a computer program that mimics a mobile device. We can use [Expo](https://expo.io/) to run React Native applications on our personal machines. 

* **React Native doesn't use web elements**. We can't use HTML tags in React Native applications. HTML is designed for browsers, not mobile applications. However, React Native contains prebuilt components for a variety of different purposes. Many of these are perfectly functional replacements for HTML elements. For instance, React Native's `<Text>` is similar to a `<p>` tag. 

* **We need to consider device operating systems when working with React Native**. We can create sites in React and safely assume they'll look and function similar in most browsers. However, when creating sites with React Native, we need to consider whether they'll be used on iOS or Android devices — or even both. Because each operating system is different, we can't assume that an application will look and function in exactly the same way on each device. However, if you decide to build a React Native application, don't worry about this just yet. Build an application that uses your device's current OS. Just keep in mind that tutorials and resources for Android React Native won't be exactly the same as the resources for iOS apps. 

### Setting Up a Development Environment

There are two options for setting up a React Native environment. Expo is recommended if you're new to React Native. React Native CLI is recommended for more experienced mobile developers but it's also an option as well. If you use a Mac, you need to make sure XCode is up to date to install React Native CLI. Alternatively, you can use Android Studio for developing with Android.

For instructions on setting up Expo (or React Native CLI), see [Environment Setup](https://reactnative.dev/docs/environment-setup).

### Similarities Between React and React Native

As we've already pointed out, there are many similarities between React and React Native. Because of this, we've already made significant progress towards learning how to build React Native applications.

Here are some key similarities:

* They both use function and class components. The syntax for components in React Native is the same.
* Both use JSX as well as curly braces for expressions.
* They both use props.
* State is used in a similar way — and so are hooks if we want to incorporate state in function components.

For more information on these similarities, see the [React Fundamentals](https://reactnative.dev/docs/intro-react) section of the React Native documentation.

If you're interested in building a React Native application for this course section's project or your capstone, start with the React Native [documentation](https://reactnative.dev/docs/getting-started), which includes basic tutorials. Even if you don't plan to build with React Native right now, we still recommend looking over the documentation. That way, you still have some familiarity with React Native if you apply for a job where it's used — or get asked questions about a related topic in an interview.