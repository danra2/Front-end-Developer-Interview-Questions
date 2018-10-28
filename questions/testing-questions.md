# Testing Questions:

* What are some advantages/disadvantages to testing your code?

Advantages

- Prevent regression errors (bugs that occur repeatedly)

- Ensure there is no missing part to change when refactoring

- Tests can be used as specs

- Test process can be shared amongst team members

Disadvantages: there are rarely disadvantages

- It may or may not take more time to prototype.

* What tools would you use to test your code's functionality?

For JavaScript, the basic assert module is quite enough for simple tests. But when it gets a big and production-level project, it is a better idea to set a test framework. For backend, I usually use Mocha. For frontend, Mocha can still be used with headless browsers such as PhantomJS. There are also other famous tools like Selenium for browser tests.

[ref](https://github.com/utatti/Front-end-Developer-Interview-Questions-And-Answers/blob/master/answers/testing-questions.md)

* What is the difference between a unit test and a functional/integration test?

- Unit testing is the practice of testing small pieces of code, typically individual functions, alone and isolated. If your test uses some external resource, like the network or a database, it’s not a unit test.

- A functional test is on a particular feature and check if it generates a proper output for a provided input. E2E testing, or browser testing. In practice with web apps, this means using some tool to automate a browser, which is then used to click around on the pages to test the application.

While you can have hundreds of unit tests, you usually want to have only a small amount of functional tests. This is mainly because functional tests can be difficult to write and maintain due to their very high complexity. They also run very slowly, because they simulate real user interaction on a web page, so even page load times become a factor.

- An integration test checks if each part(or unit) of code works well together and results in combination functions correctly. While unit tests are isolated from other components, integration tests are not. For example, a unit test for database access code would not talk to a real database, but an integration test would.

[reference](https://codeutopia.net/blog/2015/04/11/what-are-unit-testing-integration-testing-and-functional-testing/)

* What is the purpose of a code style linting tool?

linting is a kind of static analysis.