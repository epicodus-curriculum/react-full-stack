Because we are focusing on teaching the concepts of functional programming, we will not include tests in the lessons. However, you are still expected to write tests for all of your functions. We can still use Jest in exactly the same way whether our code is functional or object-oriented. That means the tests themselves will not change significantly from how we wrote them in Intermediate JavaScript. Focus on the following process:

* **Start by writing a test for the smallest unit of behavior you can.** In this section, this will usually mean checking to see if an expected output occurs when a function is called. In functional programming, **a function _always_ returns the same output**. We'll discuss this more in a future lesson. This actually makes functional programs _easier_ to test than object-oriented programs.

* **Verify that the test fails.** The function should be correctly called but the expected output won't be returned because we haven't added code to the function body yet.

* **Get the test passing.** This is when you'll use the functional programming techniques we learn throughout this section.

So why aren't we including webpack, testing, and Jest with this section's lessons if we still expect you to write tests? Well, at this point in the program, you are almost ready to continue onto an internship or a future career. It's important to be able to retrieve skills you've learned in the past and refresh them as needed. **This is an essential skill for developers.** You will learn many things throughout your career — and you will forget many, too. There will not always be a tutorial to help you along the way as there often is with Learn How To Program.

We see the transition back to JavaScript as a valuable opportunity to apply a skill you've learned in the past to a new way of doing things. In this case, we are applying a tool we've learned for testing object-oriented JavaScript — but this time we'll be using it to test functional JavaScript code. This is another essential skill for developers — applying a tool you've used in a past familar setting to new, unfamiliar code.

Finally, we want the lessons in this section to focus entirely on functional programming. Functional programming is challenging to learn for new developers. Cluttering up the lessons with extraneous steps on incorporating Jest, writing tests, and adding webpack will detract from the key concepts in this section — especially since you've already learned how to use these tools!

You may need to review lessons on test-driven development, using Jest, and setting up webpack. You are also welcome to use your repositories from Intermediate JavaScript to set up a boilerplate environment with webpack and Jest. Having good reference projects in GitHub can be very helpful when you need to revisit concepts you haven't used for a while. If you need to refer back to Intermediate JavaScript lessons to set up webpack and Jest, these links will be helpful:

* We used the example project called "Shape Tracker" to demonstrate how to use TDD with Jest:
  * [Shape Tracker](https://github.com/epicodus-lessons/section-5-shape-tracker)
* The following lessons review TDD and how to set up Jest and Babel: 
  * [TDD Review](https://new.learnhowtoprogram.com/intermediate-javascript/test-driven-development-and-environments-with-javascript/tdd-review)
  * [Red Green Refactor Workflow](https://new.learnhowtoprogram.com/intermediate-javascript/test-driven-development-and-environments-with-javascript/red-green-refactor-workflow)
  * [Setting Up Jest](https://new.learnhowtoprogram.com/intermediate-javascript/test-driven-development-and-environments-with-javascript/setting-up-jest)
  * [Setting Up Babel](https://new.learnhowtoprogram.com/intermediate-javascript/test-driven-development-and-environments-with-javascript/setting-up-babel)
* The following lessons demonstrate using TDD to build out business logic in the Shape Tracker project:
  * [TDD with Jest: Testing the Triangle() Constructor](https://new.learnhowtoprogram.com/intermediate-javascript/test-driven-development-and-environments-with-javascript/tdd-with-jest-testing-the-triangle-constructor)
  * [TDD with Jest: Testing the Triangle.prototype.checkType() Method](https://new.learnhowtoprogram.com/intermediate-javascript/test-driven-development-and-environments-with-javascript/tdd-with-jest-testing-the-triangle-prototype-checktype-method)
* The following lessons explain best practices and additional tools we can implement to improve our testing experience with Jest:
  * [Testing Best Practices](https://new.learnhowtoprogram.com/intermediate-javascript/test-driven-development-and-environments-with-javascript/testing-best-practices)
  * [Expanding our Testing Tools: Adding Setup and Teardown](https://new.learnhowtoprogram.com/intermediate-javascript/test-driven-development-and-environments-with-javascript/expanding-our-testing-tools-adding-setup-and-teardown)
  * [Improving Test Reports: Adding Test Coverage Information](https://new.learnhowtoprogram.com/intermediate-javascript/test-driven-development-and-environments-with-javascript/improving-test-reports-adding-test-coverage-information)

You may also want to update your ESLint configuration to include functional programming rules. If so, we recommend the eslint-plugin-fp library:

* [npm page for eslint-plugin-fp](https://www.npmjs.com/package/eslint-plugin-fp)
* [GitHub page for eslint-plugin-fp](https://github.com/jfmengels/eslint-plugin-fp)
