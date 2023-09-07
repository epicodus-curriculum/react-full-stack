In this lesson, we will go through the steps to setting up a Firebase account, and setting up project with a web app and Firestore database. In the next lesson, we will integrate our new Firebase project with our Help Queue application.

## Step 1: Set Up A Firebase Account
---

Firebase is cloud-based so all of our data will be stored in the cloud. We'll start by setting up a Firebase account. Note that in order to set up a Firebase account, you do need to set up a Google account if you don't have one already. 

Firebase is free for smaller projects, which gives us the opportunity to explore and integrate projects with Firebase. If we were to build a larger application, however, we'd eventually need to start paying for the service.

Start by navigating to [https://firebase.google.com/](https://firebase.google.com/) in the browser.

Then, click on _Get Started_. This will take you to a sign in page where you can log into a Google account (or create one if needed).

## Step 2: Create a Firebase Project
---

Once you've signed into your Google account, you'll be taken to the Firebase console, which is the homepage for all of your Firebase projects. You can always access the Firebase console later by going to [https://console.firebase.google.com/](https://console.firebase.google.com/). Also, once you are logged into your Google account, there will be a _Go to console_ button on [the Firebase homepage](https://firebase.google.com/) that you can click to get back to the console.

In the console, we will have the option to _Create a Project_. [According to the docs](https://firebase.google.com/docs/projects/learn-more#projects-apps-products), **a Firebase project** is the top-level entity for Firebase. Within one project we can set up databases and different types of apps. For example, one project could have a web app and an Android app that both share the same Firestore database. In our Firebase project, we will create a web app with a Firestore database.

Click on _Create a Project_. When we do this, we'll be taken to a page where we can name our project. We'll call our project `help-queue`, and we'll select the checkbox that confirms that we'll use our project appropriately.

![Firebase screen for naming a project](https://learnhowtoprogram.s3.us-west-2.amazonaws.com/React/Week-4-React-2020/firebase-project-step-1-name.png)

The next step will ask us if we want to use Google Analytics in our project. There's a toggle button _Enable Google Analytics for this project_ that is on. We will click the toggle button to disable Google Analytics, unless you'd like to add it to your own project. It won't affect our project development. If you are planning to build out your project long term, you may want to add this feature.

Clicking continue on the Google Analytics page will prompt Firebase to create your project. Once complete, click continue again to be taken to the homepage of our newly created Help Queue project.

![Success message for creating a Firebase project.](https://learnhowtoprogram.s3.us-west-2.amazonaws.com/React/Week-4-React-2020/firebase-project-is-ready.png)

### Navigating the Help Queue Project Homepage

There are quite a few options from the Help Queue project homepage, and we'll review those now. As we work through the different links and locations, reference the image below. 

* The first thing to note is that we can navigate back to the Firebase console homepage by clicking the drop-down menu called _help-queue_ at the top of the page (circled in black), and selecting the option _see all projects_.
* Next, there's a link to the Firebase docs (circled in black) at the far right of the screen. 
* In the vertical menu to the left of the screen, there's options to set up different Firebase products. The _Build_ tab is highlighted by a green rectangle, and it will have the options for _Firestore Database_ and _Authentication_, which we'll set up later.
* Anytime we navigate away from our Help Queue's homepage, we can always get back to it by clicking on the _Project Overview_ button (circled in green).
* Now, notice the cog symbol to the right of the _Project Overview_ button (circled in red). This is where we'll find our project settings. We'll revisit our project settings as needed to get information about our project's configuration.
* In the middle of the homepage we'll see the message "Get started by adding Firebase to your app". This area gives us options to configure our firebase to connect to a web app, an Android app, or an iOS app. This is something we'll do later in this lesson. The `</>` icon with the black circle represents web apps. 

![The Help Queue project homepage on Firebase, and the various navigation options.](https://learnhowtoprogram.s3.us-west-2.amazonaws.com/React/Week-4-React-2020/firebase-project-homepage-navigation.png)

## Step 3: Set Up Firestore
---

Next, we will set up our Firestore database. For more information on Firestore, check the [Firebase docs on Firestore](https://firebase.google.com/docs/firestore/data-model). We will be covering Firestore in more depth in future lessons.

One the homepage of our Help Queue project, click on the _Build_ tab in the left-hand  menu, and then select _Firestore Database_:

![Click on "Firestore Database" in the "Build" drop-down menu.](https://learnhowtoprogram.s3.us-west-2.amazonaws.com/React/Week-4-React-2020/firebase-click-on-firestore-database.png)

You'll be taken to the following page:

![This image shows the screen for adding Firebase to an application. The button for adding Firebase to a web application is circled.](https://learnhowtoprogram.s3.us-west-2.amazonaws.com/React/Week-4-React-2020/cloud-firestore-click-create-database.png)

Click on the _Create database_ button. 

A popup will appear:

![Click on _Start in Test Mode_.](https://learnhowtoprogram.s3.us-west-2.amazonaws.com/React/Week-4-React-2020/start-in-production-mode.png)

Within this popup, do the following:

* Select _Start in Test Mode_, and then click _Next_.
* On the next page, leave the default form values and click _Enable_ to proceed.
* Then, wait as Firebase creates the database.

When the Firestore database is created, we'll be taken to the Firestore database homepage.

## Step 4: Add Firebase to Our Web App
---

Next, we need to add Firebase to our Help Queue application. First, head back to the Help Queue homepage by clicking the _Project Overview_ button at the top-left of the page. 

Then, click on the `</>` icon from underneath the message "Get started by adding Firebase to your app". In the image below, this icon is circled in black.

![This image shows the screen for adding Firebase to an application. The icon for adding Firebase to a web application is circled.](https://learnhowtoprogram.s3.us-west-2.amazonaws.com/React/Week-4-React-2020/firebase-step-4.png)

When we click on the `</>` button, we'll be taken to a page that reads _Add Firebase to your web app_.

![This image shows the screen for giving our Firebase project a nickname.](https://learnhowtoprogram.s3.us-west-2.amazonaws.com/React/Week-4-React-2020/register-app-web.png)

We'll need to enter an app nickname. We'll call ours `help-queue-web`. The nickname we choose should be something that we can differentiate from other apps that we add to our Help Queue Firebase project. We don't have other apps right now, but this could be a mobile app, or yet another web app. 

We also have an option to set up Firebase hosting. This is a nice thing about Firebase — we can also use it to easily deploy our application! We'll be able to set that up later so we won't do it right now. Go ahead and click on the _Register app_ button.

Finally, we'll be given a script that we will include in our source code to configure and initialize firebase in our application. We're not ready to use this script just yet (we will in the next lesson), but we also don't need to worry about copying down this information. We can access it later via our Help Queue project settings. 

Click on _Continue to console_, which will take us back to the Help Queue project homepage.

Anytime we need to access the firebase configuration script, we can do so by clicking on the gear icon to the right of _Project Overview_ in the upper-left corner of the screen. Then, click on _Project settings_, which will take you to a page which includes the script (make sure to scroll down on that page). See the image below for the location of _Project settings_:

![You can access _Project settings_ in the upper left-hand corner of the Firebase console.](https://learnhowtoprogram.s3.us-west-2.amazonaws.com/React/Week-4-React-2020/firebase-project-settings.png)

At this point, we've finished all the steps for setting up our Firebase project. In the next lesson, we'll integrate Firebase with our Help Queue application. 

Before we move on, let's briefly talk about the read/write rules for our Firestore database.

## Adjusting Firestore Read/Write Rules
---

Let's revisit our database by clicking on the _Firestore Database_, from the _Build_ drop-down menu. We'll see a UI for our database. Here, we'll be able to see our data once we wire up our database to our Help Queue application and start creating tickets.

We can update our database's read/write rules by clicking the _Rules_ tab as pictured in the image below: 

![Image shows tabs for database. The rules tab is circled.](https://learnhowtoprogram.s3.us-west-2.amazonaws.com/React/Week-4-React-2020/firestore-database-rules.png)

These are the current rules:

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /{document=**} {
      allow read, write: if
        request.time < timestamp.date(2021, 11, 19);
    }
  }
}
```

The `timestamp.date` will contain a different date than in the example above. That's because the default rules are configured to expire after 30 days from the time that we initialize our Firestore database. In this example, the database was created on October 20th, 2021. Up until November 19th, 2021, anyone on the internet can view, edit, and delete all data in our Firestore database. After that date, all requests will be denied. 

These are great rules for getting started with Firestore, but we will need to reconfigure them before the 30 days have ended.

We could update our rules to the following:

```javascript
// Note that these rules are not secure and should NEVER be used in production!
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /{document=**} {
      allow read, write: if true;
    }
  }
}
```

However, this would be insecure. We would be updating access so that _anyone_ can modify our database. What's more, there's no expiration for this rule. This is something we should _never_ do in production. 

For development purposes allowing all access may not be as big of a deal — as long as you don't share your database URL. Ultimately, it depends on the security practices of your team and the organization you are working for. 

However, if you end up deploying your Firebase project, you'll want to write rules that protect your data. Here are two great resources to help you learn how to protect your data:

* [Fixing insecure rules](https://cloud.google.com/firestore/docs/security/insecure-rules): for information on fixing insecure rules. 
* [Get started with Cloud Firestore Security Rules](https://firebase.google.com/docs/firestore/security/get-started): for more general information on writing security rules for Firestore.

