We can use animation for a wide range of effects such as guided focus, loading wheels, visualizations, and for improving a user's experience, among other things.

If we look at notable React applications we can see this interactivity in action. For instance, check out Airbnb's site. Notice how the menu options slide in and out in a when hovering over the logo. Another example is the loading animations at the bottom of the page:

<img alt="animations-on-airbnb-site" src="https://learnhowtoprogram.s3.us-west-2.amazonaws.com/React/airbnb-animations-example.gif" width="90%">

Subtle effects like this can make a huge impact on user experience. Loading animations let users know that more information is coming and hopefully provide a pleasant transition.

The online store Everlane is also built with React. Let's take a look at some of the animations on their splash page:

<img alt="animations-on-everlane-site" src="https://learnhowtoprogram.s3.us-west-2.amazonaws.com/React/everlane-animations.gif" width="90%">

Links and other clickable content change color when hovered over, adding a feeling of interactivity and responsiveness. Menus and navigation options slide in and out of views and images in the carousel also slide gently in and out. This gives the site a professional, polished look — and also encourages the user to have a pleasant experience.

In this lesson, we'll discuss some of the animation tools at our disposal. Most of these tools can also be used with React Native applications.

### React Transition Group

**Transitions** are just what they sound like — a type of animation that changes as components enter and leave the DOM.

The React core team has created an add-on called [React Transition Group](https://github.com/reactjs/react-transition-group/tree/v1-stable). This library specializes in performing CSS transitions. The library works by providing access to a built-in `<ReactCSSTransitionGroup>` component.

For more information, check out [Animation Add-Ons](https://reactjs.org/docs/animation.html) in the React documentation.

### react-spring

The [react-spring](https://www.react-spring.io/) library is easy to implement and even incorporates its own custom React hooks. This is a great library to start with. Check out some [examples](https://codesandbox.io/examples/package/react-spring) of react-spring animations. react-spring currently provides five custom hooks. More information on these hooks can be found in the docs starting [here](https://react-spring.io/hooks/use-chain). Each hook also has a demo for it.

### Framer Motion

Framer Motion describes itself as a "a production-ready animation and gesture library." Motion divides its functionality into animations, [gestures](https://www.framer.com/api/motion/gestures/), and variants. "Gestures" refer to animations related to hovering, dragging, and tapping. We can think of these animations as responding to the gestures we make with a mouse or touch screen. The library also provides [examples](https://www.framer.com/api/motion/examples/) of Framer Motion animations.

### React-Motion

[React-Motion](https://github.com/chenglou/react-motion) was one of the inspirations for react-spring. Check out the GitHub documentation, which includes some examples of animations.

Similar to React Transition Group, React Motion provides pre-made components. We can pass props to these components to help us create animations. The primary built-in components are: `<Motion/>`, `<StaggeredMotion/>` and `<TransitionMotion/>`.

If you'd like to explore React Motion further, see the [documentation](https://github.com/chenglou/react-motion).

