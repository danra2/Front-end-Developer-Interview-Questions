# JS Questions:

* Explain event delegation

DOM event delegation is a mechanism of responding to ui-events via a single common parent rather than each child, through the magic of event "bubbling" (aka event propagation).

Event bubbling provides the foundation for event delegation in browsers. Now you can bind an event handler to a single parent element, and that handler will get executed whenever the event occurs on any of its child nodes (and any of their children in turn). This is event delegation. 

### Event bubbling: 

When an event is triggered on an element, the following occurs:

> The event is dispatched to its target  EventTarget and any event listeners found there are triggered. Bubbling events will then trigger any additional event listeners found by following the EventTarget's parent chain upward, checking for any event listeners registered on each successive EventTarget. This upward propagation will continue up to and including the Document.

[reference](https://stackoverflow.com/questions/1687296/what-is-dom-event-delegation)


* Explain how `this` works in JavaScript

In most cases, the value of this is determined by how a function is called. It can't be set by assignment during execution, and it may be different each time the function is called. ES5 introduced the bind method to set the value of a function's this regardless of how it's called, and ES2015 introduced arrow functions which don't provide their own this binding (it retains the this value of the enclosing lexical context).

[reference](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/this)

* Explain how prototypal inheritance works

dynamic and prototype-based

Inheritance. JavaScript only has one construct: objects. Each object has a private property which holds a link to another object called its prototype. That prototype object has a prototype of its own, and so on until an object is reached with null as its prototype. By definition, null has no prototype, and acts as the final link in this prototype chain.

Nearly all objects in JavaScript are instances of Object which sits on the top of a prototype chain.

```js
function doSomething(){}
doSomething.prototype.foo = "bar"; // add a property onto the prototype
var doSomeInstancing = new doSomething();
doSomeInstancing.prop = "some value"; // add a property onto the object
console.log( doSomeInstancing );

//result
{
    prop: "some value",
    __proto__: {
        foo: "bar",
        constructor: ƒ doSomething(), //when declaring a function, generate a constructor property in its prototype
        __proto__: {
            constructor: ƒ Object(),
            hasOwnProperty: ƒ hasOwnProperty(),
            isPrototypeOf: ƒ isPrototypeOf(),
            propertyIsEnumerable: ƒ propertyIsEnumerable(),
            toLocaleString: ƒ toLocaleString(),
            toString: ƒ toString(),
            valueOf: ƒ valueOf()
        }
    }
}
```

To check whether an object has a property defined on itself: hasOwnProperty().

It is not enough to check whether a property is undefined. The property might very well exist, but its value just happens to be set to undefined.

be aware of the length of the prototype chains in your code and break them up if necessary to avoid possible performance problems. 

Further, the native prototypes should never be extended unless it is for the sake of compatibility with newer JavaScript features.

[reference](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Inheritance_and_the_prototype_chain)

* What do you think of AMD vs CommonJS?

CommonJS's a project to define a common API and ecosystem for JavaScript. One part of CommonJS is the Module specification. Node.js and RingoJS are server-side JavaScript runtimes, both of them implement modules based on the CommonJS Module spec.

AMD (Asynchronous Module Definition). RequireJS is probably the most popular implementation of AMD. AMD is generally more used in client-side (in-browser) JavaScript development due to this.

However, you can use either module spec in either environment - for example, RequireJS offers directions for running in Node.js and browserify is a CommonJS Module implementation that can run in the browser.

CommonJS compliant:
Taken from Addy Osmani's book.

```js
// package/lib is a dependency we require
var lib = require( "package/lib" );

// behavior for our module
function foo(){
    lib.log( "hello world!" );
}

// export (expose) foo to other modules as foobar
exports.foobar = foo;
```

AMD compliant:

```js
// package/lib is a dependency we require
define(["package/lib"], function (lib) {

    // behavior for our module
    function foo() {
        lib.log( "hello world!" );
    }

    // export (expose) foo to other modules as foobar
    return {
        foobar: foo
    }
});

// Somewhere else the module can be used with:

require(["package/myModule"], function(myModule) {
    myModule.foobar();
});

```

[stackoverflow](https://stackoverflow.com/questions/16521471/relation-between-commonjs-amd-and-requirejs)

[whyamd](https://requirejs.org/docs/whyamd.html)

* Explain why the following doesn't work as an IIFE: `function foo(){ }();`.
  * What needs to be changed to properly make it an IIFE?

An IIFE (pronouced as ‘iffy’) is an abbreviation for Immediately Invoked Function Expression. Purpose of using an IIFE is to maintain code inside of a local scope. 

* What's the difference between a variable that is: `null`, `undefined` or undeclared?
  * How would you go about checking for any of these states?

undefined is a variable that has been declared but no value exists.

null is a value of a variable and is a type of object.

undeclared variables is a variable that has been declared without ‘var’ keyword.

testVar = ‘hello world’;

as opposed to

var testVar = ‘hello world’;

When former code is executed, undeclared variables are created as global variable and they are configurable (ex. can be deleted).

* What is a closure, and how/why would you use one?

A closure is the combination of a function bundled together (enclosed) with references to its surrounding state (the lexical environment). 

To use a closure, simply define a function inside another function and expose it. To expose a function, return it or pass it to another function. => The inner function will have access to the variables in the outer function scope, even after the outer function has returned.

Use cases:

(1) the primary mechanism used to enable data privacy. You can’t get at the data from an outside scope except through the object’s privileged methods (any exposed method defined within the closure scope).

(2) to create stateful functions whose return values may be influenced by their internal state

(3) In functional programming, closures are frequently used for partial application (a function that takes a function with multiple parameters and returns a function with fewer parameters) & currying. 

[reference](https://medium.com/javascript-scene/master-the-javascript-interview-what-is-a-closure-b2f0d2152b36)

* Can you describe the main difference between a `forEach` loop and a `.map()` loop and why you would pick one versus the other?

forEach() — executes a provided function once for each array element. returns undefined

map() — creates a new array with the results of calling a provided function on every element in the calling array.

* What's a typical use case for anonymous functions?

Anonymous Functions are function expressions rather than the regular function declaration which are statements. Function expressions are more flexible. We can assign functions to variables, object properties, pass them as arguments to other functions, and even write a simple one line code enclosed in an anonymous functions.

[reference](https://medium.com/@rlynjb/js-interview-question-what-s-a-typical-use-case-for-anonymous-functions-54cf547b2a0e)

* How do you organize your code? (module pattern, classical inheritance?)

### Module Pattern

In the past, I used Backbone for my models which encourages a more OOP approach, creating Backbone models and attaching methods to them.

The module pattern is still great, but these days, I use React/Redux which utilize a single-directional data flow based on Flux architecture. I would represent my app's models using plain objects and write utility pure functions to manipulate these objects. State is manipulated using actions and reducers like in any other Redux application.

### Classical Inheritance

I avoid using classical inheritance where possible. When and if I do, I stick to these [rules](https://medium.com/@dan_abramov/how-to-use-classes-and-sleep-at-night-9af8de78ccb4).

(1) Resist making classes your public API. 

(2) Don’t inherit more than once. Inheritance can be handy as a shortcut, and inheriting once is um-kay, but don’t go deeper. The problem with inheritance is that the descendants have too much access to the implementation details of every base class in the hierarchy, and vice versa. 

(3) Don’t make super calls from methods. If you override a method in a derived class, override it completely. 

(4) Don’t expect people to use your classes. Even if you choose to provide your classes as a public API, prefer duck typing when accepting inputs. 

(5) Learn functional programming. It will help you not think in classes, so you won’t be compelled to use them even though you know their pitfalls.

[ref](https://github.com/yangshun/front-end-interview-handbook/blob/master/questions/javascript-questions.md)

* What's the difference between host objects and native objects?

Native objects are objects that are part of the JavaScript language defined by the ECMAScript specification, such as String, Math, RegExp, Object, Function, etc.

Host objects are provided by the runtime environment (browser or Node), such as window, XMLHTTPRequest, etc.

[ref](https://stackoverflow.com/questions/7614317/what-is-the-difference-between-native-objects-and-host-objects)

* Difference between: `function Person(){}`, `var person = Person()`, and `var person = new Person()`?

This question is pretty vague. My best guess at its intention is that it is asking about constructors in JavaScript. Technically speaking, function Person(){} is just a normal function declaration. The convention is to use PascalCase for functions that are intended to be used as constructors.

var person = Person() invokes the Person as a function, and not as a constructor. Invoking as such is a common mistake if it the function is intended to be used as a constructor. Typically, the constructor does not return anything, hence invoking the constructor like a normal function will return undefined and that gets assigned to the variable intended as the instance.

var person = new Person() creates an instance of the Person object using the new operator, which inherits from Person.prototype. An alternative would be to use Object.create, such as: Object.create(Person.prototype).

[ref](https://github.com/yangshun/front-end-interview-handbook/blob/master/questions/javascript-questions.md)


* What's the difference between `.call` and `.apply`?

Both .call and .apply are used to invoke functions and the first parameter will be used as the value of this within the function. However, .call takes in comma-separated arguments as the next arguments while .apply takes in an array of arguments as the next argument.

[reference](https://github.com/yangshun/front-end-interview-handbook/blob/master/questions/javascript-questions.md)

* Explain `Function.prototype.bind`.

The bind() method creates a new function that, when called, has its this keyword set to the provided value, with a given sequence of arguments preceding any provided when the new function is called. [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_objects/Function/bind)

It is most useful for binding the value of this in methods that you want to pass into other functions. This is frequently done in React components. [ref](https://github.com/yangshun/front-end-interview-handbook/blob/master/questions/javascript-questions.md)

* When would you use document.write()?

document.write() writes a string of text to a document stream opened by document.open(). When document.write() is executed after the page has loaded, it will call document.open which clears the whole document (<head> and <body> removed!) and replaces the contents with the given parameter value. Hence it is usually considered dangerous and prone to misuse.

There are some answers online that explain document.write() is being used 

(1) in analytics code 

(2) when you want to include styles that should only work when JavaScript is enabled. (to replace a style.display = none, because onload event comes too late)

(3) It is even being used in HTML5 boilerplate to load scripts in parallel and preserve execution order

* What's the difference between feature detection, feature inference, and using the UA string?

### Feature Detection

Feature detection involves working out whether a browser supports a certain block of code, and running different code depending on whether it does (or doesn't), so that the browser can always provide a working experience rather crashing/erroring in some browsers. For example:

```js
if ('geolocation' in navigator) {
  // Can use navigator.geolocation
} else {
  // Handle lack of feature
}
```
Modernizr is a great library to handle feature detection.

### Feature Inference

Feature inference checks for a feature just like feature detection, but uses another function because it assumes it will also exist, e.g.:

```js
if (document.getElementsByTagName) {
  element = document.getElementById(id);
}
```

This is not really recommended. Feature detection is more foolproof.

### UA String

This is a browser-reported string that allows the network protocol peers to identify the application type, operating system, software vendor or software version of the requesting software user agent. It can be accessed via navigator.userAgent. 

However, the string is tricky to parse and can be spoofed. For example, Chrome reports both as Chrome and Safari. So to detect Safari you have to check for the Safari string and the absence of the Chrome string. Avoid this method.

[ref](https://github.com/yangshun/front-end-interview-handbook/blob/master/questions/javascript-questions.md)

* Explain Ajax in as much detail as possible.

Ajax (Asynchronous JavaScript + XML), while not a technology in itself, describes a "new" approach to using a number of existing technologies together. 

(1) standards-based presentation using XHTML and CSS;

(2) dynamic display and interaction using the Document Object Model;

(3) data interchange and manipulation using XML and XSLT;

(4) asynchronous data retrieval using XMLHttpRequest;

(5) and JavaScript binding everything together.

Although X in Ajax stands for XML, JSON is used more than XML nowadays because of its many advantages such as being lighter and a part of JavaScript.

[MDN](https://developer.mozilla.org/en-US/docs/Web/Guide/AJAX)

At the start of the session, the browser loads an Ajax engine, for both rendering the interface the user sees and communicating with the server on the user’s behalf. The Ajax engine allows the user’s interaction with the application to happen asynchronously — independent of communication with the server. So the user is never waiting around for the server to do something.

![](http://adaptivepath.org/uploads/archive/images/publications/essays/ajax-fig1.png)

[ref](http://adaptivepath.org/ideas/ajax-new-approach-web-applications/)

* What are the advantages and disadvantages of using Ajax?

### Advantages

Better interactivity. New content from the server can be changed dynamically without the need to reload the entire page.

Reduce connections to the server since scripts and stylesheets only have to be requested once.

State can be maintained on a page. JavaScript variables and DOM state will persist because the main container page was not reloaded.

Basically most of the advantages of an SPA.

### Disadvantages

Dynamic webpages are harder to bookmark.

Does not work if JavaScript has been disabled in the browser.

Some webcrawlers do not execute JavaScript and would not see content that has been loaded by JavaScript.

Basically most of the disadvantages of an SPA.

* Explain how JSONP works (and how it's not really Ajax).

JSONP (JSON with Padding) is a method commonly used to bypass the cross-domain policies in web browsers because Ajax requests from the current page to a cross-origin domain is not allowed.

JSONP works by making a request to a cross-origin domain via a <script> tag and usually with a callback query parameter. When you use a script tag, the domain limitation is ignored, but under normal circumstances, you can't really do anything with the results, the script just gets evaluated.

Enter JSONP. When you make your request to a server that is JSONP enabled, you pass a special parameter that tells the server a little bit about your page. That way, the server is able to nicely wrap up its response in a way that your page can handle.

For example, say the server expects a parameter called "callback" to enable its JSONP capabilities. Then your request would look like:
```
http://www.example.net/sample.aspx?callback=mycallback
```
Without JSONP, this might return some basic JavaScript object, like so:
```js
{ foo: 'bar' }
```
However, with JSONP, when the server receives the "callback" parameter, it wraps up the result a little differently, returning something like this:
```js
mycallback({ foo: 'bar' });
```
As you can see, it will now invoke the method you specified. So, in your page, you define the callback function:
```js
mycallback = function(data){
  alert(data.foo);
};
```
And now, when the script is loaded, it'll be evaluated, and your function will be executed.

one major issue with JSONP: you lose a lot of control of the request. For example, there is no "nice" way to get proper failure codes back. As a result, you end up using timers to monitor the request, etc, which is always a bit suspect. 

[ref](https://stackoverflow.com/questions/2067472/what-is-jsonp-all-about/2067584#2067584)

### CORS

These days (2015), CORS is the recommended approach vs. JSONRequest. 

Cross-origin resource sharing. The CORS standard describes new HTTP headers which provide browsers and servers a way to request remote URLs only when they have permission. 

```js
function createCORSRequest(method, url) {
  var xhr = new XMLHttpRequest();
  if ("withCredentials" in xhr) {

    // Check if the XMLHttpRequest object has a "withCredentials" property.
    // "withCredentials" only exists on XMLHTTPRequest2 objects.
    xhr.open(method, url, true);

  } else if (typeof XDomainRequest != "undefined") {

    // Otherwise, check if XDomainRequest.
    // XDomainRequest only exists in IE, and is IE's way of making CORS requests.
    xhr = new XDomainRequest();
    xhr.open(method, url);

  } else {

    // Otherwise, CORS is not supported by the browser.
    xhr = null;

  }
  return xhr;
}

var xhr = createCORSRequest('GET', url);
if (!xhr) {
  throw new Error('CORS not supported');
}
```

[CORS tutorial](https://www.html5rocks.com/en/tutorials/cors/)

* Have you ever used JavaScript templating?
  * If so, what libraries have you used?

Nowadays, you can even use ES2015 template string literals as a quick way for creating templates without relying on third-party code.

const template = `<div>My name is: ${name}</div>`;
However, do be aware of a potential XSS in the above approach as the contents are not escaped for you, unlike in templating libraries.


* Explain "hoisting".

Variables declared or initialized with the var keyword will have their declaration "moved" up to the top of the current scope, which we refer to as hoisting. However, only the declaration is hoisted, the assignment (if there is one), will stay where it is.

Note that the declaration is not actually moved - the JavaScript engine parses the declarations during compilation and becomes aware of declarations and their scopes. 

```js
// var declarations are hoisted.
console.log(foo); // undefined
var foo = 1;
console.log(foo); // 1

// let/const declarations are NOT hoisted.
console.log(bar); // ReferenceError: bar is not defined
let bar = 2;
console.log(bar); // 2
```

### functions

Function declarations have the body hoisted, while the function expressions (written in the form of variable declarations) only has the variable declaration hoisted.

[ref](https://javascriptweblog.wordpress.com/2010/07/06/function-declarations-vs-function-expressions/)
[ref2](http://www.adequatelygood.com/JavaScript-Scoping-and-Hoisting.html)
[ref3](https://github.com/yangshun/front-end-interview-handbook/blob/master/questions/javascript-questions.md#explain-how-jsonp-works-and-how-its-not-really-ajax)

* Describe event bubbling.

Bubbling principle: When an event happens on an element, it first runs the handlers on it, then on its parent, then all the way up on other ancestors. Event bubbling is the mechanism behind event delegation.

* Describe event capturing.

The standard DOM Events describes 3 phases of event propagation:

Capturing phase – the event goes down to the element.

Target phase – the event reached the target element.

Bubbling phase – the event bubbles up from the element.

![](https://javascript.info/article/bubbling-and-capturing/eventflow@2x.png)

for a click on <td> the event first goes through the ancestors chain down to the element (capturing), then it reaches the target, and then it goes up (bubbles), calling handlers on its way.

### Usage

There are two possible values for the optional last argument of addEventListener:

If it’s false (default), then the handler is set on the bubbling phase.

If it’s true, then the handler is set on the capturing phase.

* What's the difference between an "attribute" and a "property"?

When writing HTML source code, you can define *attributes* on your HTML elements. Then, once the browser parses your code, a corresponding DOM node will be created. This node is an object, and therefore it has *properties*.

When a DOM node is created for a given HTML element, many of its properties relate to attributes with the same or similar names, but it's not a one-to-one relationship. There are several properties that directly reflect their attribute (rel, id), some are direct reflections with slightly-different names (htmlFor reflects the for attribute, className reflects the class attribute), many that reflect their attribute but with restrictions/modifications (src, href, disabled, multiple), and so on. 

```html
<input id="the-input" type="text" value="Name:">
```
The value property reflects the current text-content inside the input box, whereas the value attribute contains the initial text-content of the value attribute from the HTML source code.

[stackoverflow](https://stackoverflow.com/questions/6003819/what-is-the-difference-between-properties-and-attributes-in-html)

* Why is extending built-in JavaScript objects not a good idea?

Imagine your code uses a few libraries that both extend the Array.prototype by adding the same contains method, the implementations will overwrite each other and your code will break if the behavior of these two methods is not the same.

The only time you may want to extend a native object is when you want to create a polyfill, essentially providing your own implementation for a method that is part of the JavaScript specification but might not exist in the user's browser due to it being an older browser.

[ref](https://github.com/yangshun/front-end-interview-handbook/blob/master/questions/javascript-questions.md#explain-how-jsonp-works-and-how-its-not-really-ajax)

* Difference between window load event and document DOMContentLoaded event?

The DOMContentLoaded event is fired when the initial HTML document has been completely loaded and parsed, without waiting for stylesheets, images, and subframes to finish loading.

window's load event is only fired after the DOM and all dependent resources and assets have loaded.

[ref](https://developer.mozilla.org/en-US/docs/Web/Events/DOMContentLoaded)

* What is the difference between `==` and `===`?

== is the abstract equality operator while === is the strict equality operator. The == operator will compare for equality after doing any necessary type conversions. 

* Explain the same-origin policy with regards to JavaScript.

Under the policy, a web browser permits scripts contained in a first web page to access data in a second web page, but only if both web pages have the same origin. An origin is defined as a combination of URI scheme, host name, and port number. This policy prevents a malicious script on one page from obtaining access to sensitive data on another web page through that page's Document Object Model.

[wiki](https://en.wikipedia.org/wiki/Same-origin_policy)

* Make this work:
```javascript
duplicate([1,2,3,4,5]); // [1,2,3,4,5,1,2,3,4,5]
```

```js
//answer
function duplicate(arr) {
  return arr.concat(arr);
}
```

* Why is it called a Ternary operator, what does the word "Ternary" indicate?

"Ternary" indicates three, and a ternary expression accepts three operands, the test condition, the "then" expression and the "else" expression.

* What is `"use strict";`? what are the advantages and disadvantages to using it?

'use strict' is a statement used to enable strict mode to entire scripts or individual functions. Strict mode is a way to opt into a restricted variant of JavaScript.

### Advantages:

Makes it impossible to accidentally create global variables.
Makes assignments which would otherwise silently fail to throw an exception.
Makes attempts to delete undeletable properties throw (where before the attempt would simply have no effect).
Requires that function parameter names be unique.
this is undefined in the global context.
It catches some common coding bloopers, throwing exceptions.
It disables features that are confusing or poorly thought out.

### Disadvantages:

Many missing features that some developers might be used to.
No more access to function.caller and function.arguments.
Concatenation of scripts written in different strict modes might cause issues.
Overall, I think the benefits outweigh the disadvantages, and I never had to rely on the features that strict mode blocks. I would recommend using strict mode.

[ref](https://github.com/yangshun/front-end-interview-handbook/blob/master/questions/javascript-questions.md#explain-how-jsonp-works-and-how-its-not-really-ajax)

* Create a for loop that iterates up to `100` while outputting **"fizz"** at multiples of `3`, **"buzz"** at multiples of `5` and **"fizzbuzz"** at multiples of `3` and `5`

https://gist.github.com/jaysonrowe/1592432

* Why is it, in general, a good idea to leave the global scope of a website as-is and never touch it?

Every script has access to the global scope, and if everyone uses the global namespace to define their variables, collisions will likely occur. 

Use the module pattern (IIFEs) to encapsulate your variables within a local namespace.

YUI module pattern: The basic idea is to wrap all your code in a function that returns an object which contains functions that needs to be accessed outside your module and assign the return value to a single global variable.

```js
var FOO = (function() {
    var my_var = 10; //shared variable available only inside your module

    function bar() { // this function not available outside your module
        alert(my_var); // this function can access my_var
    }

    return {
        a_func: function() {
            alert(my_var); // this function can access my_var
        },
        b_func: function() {
            alert(my_var); // this function can also access my_var
        }
    };
})();
```
now to use functions in your module elsewhere, use FOO.a_func(). This way to resolve global namespace conflicts you only need to change the name of FOO.

* Why would you use something like the `load` event? Does this event have disadvantages? Do you know any alternatives, and why would you use those?

The load event fires at the end of the document loading process. At this point, all of the objects in the document are in the DOM, and all the images, scripts, links and sub-frames have finished loading.

(1) A major drawback to window.onload is that it doesn’t work for single page web apps (like Gmail). These web apps only have one window.onload, but typically have several other Ajax-based “page loads” during the user session where some or most of the page content is rewritten. It’s important that this new metric works for Ajax apps. [ref](https://www.stevesouders.com/blog/2013/05/13/moving-beyond-window-onload/)

(2) it is harmful using onload = function(){} rather than other means like DOM L2 methods -- addEventListener (as well as proprietary attachEvent), or intrinsic event attributes — <body onload="...">

assigning to undeclared variable actually creates a property on a Global Object. Global variables declared locally are hard to maintain and generally cause confusion.

In MSHTML DOM, When undeclared assignment happens in IE, an obscure error is thrown if identifier is named as id or name of one of the elements in a document.

Causes referenceError in strict mode.

[ref](http://perfectionkills.com/onloadfunction-considered-harmful/)

Alternative: The DOMContentLoaded event is fired when the initial HTML document has been completely loaded and parsed, without waiting for stylesheets, images, and subframes to finish loading. It is an incredibly common mistake to use load where DOMContentLoaded would be much more appropriate.

* Explain what a single page app is and how to make one SEO-friendly.

Traditionally, the browser receives HTML from the server and renders it. When the user navigates to another URL, a full-page refresh is required and the server sends fresh new HTML to the new page. This is called server-side rendering.

However, in modern SPAs, client-side rendering is used instead. The browser loads the initial page from the server, along with the scripts (frameworks, libraries, app code) and stylesheets required for the whole app. When the user navigates to other pages, a page refresh is not triggered. The URL of the page is updated via the HTML5 History API. New data required for the new page, usually in JSON format, is retrieved by the browser via AJAX requests to the server. The SPA then dynamically updates the page with the data via JavaScript, which it has already downloaded in the initial page load. This model is similar to how native mobile apps work.

### The benefits:

The app feels more responsive and users do not see the flash between page navigations due to full-page refreshes.
Fewer HTTP requests are made to the server, as the same assets do not have to be downloaded again for each page load.
Clear separation of the concerns between the client and the server; you can easily build new clients for different platforms (e.g. mobile, chatbots, smart watches) without having to modify the server code. You can also modify the technology stack on the client and server independently, as long as the API contract is not broken.

### The downsides:

Heavier initial page load due to the loading of framework, app code, and assets required for multiple pages.
There's an additional step to be done on your server which is to configure it to route all requests to a single entry point and allow client-side routing to take over from there.
SPAs are reliant on JavaScript to render content, but not all search engines execute JavaScript during crawling, and they may see empty content on your page. This inadvertently hurts the Search Engine Optimization (SEO) of your app. However, most of the time, when you are building apps, SEO is not the most important factor, as not all the content needs to be indexable by search engines. To overcome this, you can either server-side render your app or use services such as Prerender to "render your javascript in a browser, save the static HTML, and return that to the crawlers".

[ref](https://github.com/yangshun/front-end-interview-handbook/blob/master/questions/javascript-questions.md#explain-how-jsonp-works-and-how-its-not-really-ajax)

* What is the extent of your experience with Promises and/or their polyfills?

A promise is an object that may produce a single value sometime in the future: either a resolved value or a reason that it's not resolved (e.g., a network error occurred). A promise may be in one of 3 possible states: fulfilled, rejected, or pending. Promise users can attach callbacks to handle the fulfilled value or the reason for rejection.

Some common polyfills are $.deferred, Q and Bluebird but not all of them comply with the specification. ES2015 supports Promises out of the box and polyfills are typically not needed these days.

[ref](https://medium.com/javascript-scene/master-the-javascript-interview-what-is-a-promise-27fc71e77261)

Promises following the spec must follow a specific set of rules:

(1) A promise or “thenable” is an object that supplies a standard-compliant .then() method.

(2) A pending promise may transition into a fulfilled or rejected state.

(3) A fulfilled or rejected promise is settled, and must not transition into any other state.

(4) Once a promise is settled, it must have a value (which may be undefined). That value must not change.

* What are the pros and cons of using Promises instead of callbacks?

### Pros

Avoid callback hell which can be unreadable.
Makes it easy to write sequential asynchronous code that is readable with .then().
Makes it easy to write parallel asynchronous code with Promise.all().

### Cons

Slightly more complex code (debatable).
In older browsers where ES2015 is not supported, you need to load a polyfill in order to use it.

* What are some of the advantages/disadvantages of writing JavaScript code in a language that compiles to JavaScript?

CoffeeScript, Elm, ClojureScript, PureScript, and TypeScript.

### Advantages:

Fixes some of the longstanding problems in JavaScript and discourages JavaScript anti-patterns.

Enables you to write shorter code, by providing some syntactic sugar on top of JavaScript, which I think ES5 lacks, but ES2015 is awesome.

Static types are awesome (in the case of TypeScript) for large projects that need to be maintained over time.

### Disadvantages:

Require a build/compile process as browsers only run JavaScript and your code will need to be compiled into JavaScript before being served to browsers.

Debugging can be a pain if your source maps do not map nicely to your pre-compiled source.

Most developers are not familiar with these languages and will need to learn it. There's a ramp up cost involved for your team if you use it for your projects.

Smaller community (depends on the language), which means resources, tutorials, libraries, and tooling would be harder to find.

IDE/editor support might be lacking.

These languages will always be behind the latest JavaScript standard.

Developers should be cognizant of what their code is being compiled to — because that is what would actually be running, and that is what matters in the end.

[ref](https://github.com/yangshun/front-end-interview-handbook/blob/master/questions/javascript-questions.md#what-are-some-of-the-advantagesdisadvantages-of-writing-javascript-code-in-a-language-that-compiles-to-javascript)

* What tools and techniques do you use debugging JavaScript code?

React and Redux

- React Devtools

- Redux Devtools

Vue

- Vue Devtools

JavaScript

- Chrome Devtools

- debugger statement

- Good old console.log debugging


* What language constructions do you use for iterating over object properties and array items?

### For objects:

- for loops - for (var property in obj) { console.log(property); }. However, this will also iterate through its inherited properties, and you will add an obj.hasOwnProperty(property) check before using it.

- Object.keys() - Object.keys(obj).forEach(function (property) { ... }). Object.keys() is a static method that will lists all enumerable properties of the object that you pass it.

- Object.getOwnPropertyNames() - Object.getOwnPropertyNames(obj).forEach(function (property) { ... }). Object.getOwnPropertyNames() is a static method that will lists all enumerable and non-enumerable properties of the object that you pass it.

### For arrays:

- for loops - for (var i = 0; i < arr.length; i++). The common pitfall here is that var is in the function scope and not the block scope and most of the time you would want block scoped iterator variable. ES2015 introduces let which has block scope and it is recommended to use that instead. So this becomes: for (let i = 0; i < arr.length; i++).

- forEach - arr.forEach(function (el, index) { ... }). This construct can be more convenient at times because you do not have to use the index if all you need is the array elements. There are also the every and some methods which will allow you to terminate the iteration early.

Most of the time, I would prefer the .forEach method, but it really depends on what you are trying to do. for loops allow more flexibility, such as prematurely terminate the loop using break or incrementing the iterator more than once per loop.
[ref]()

* Explain the difference between mutable and immutable objects.
  * What is an example of an immutable object in JavaScript?

once string value is created, it can never change. In fact, no string methods change the string they operate on, they all return new strings. The reason being that strings in javascript are immutable.

in case of array or objects one can alter the original value, leading to the result that they are mutable.

  * What are the pros and cons of immutability?


  * How can you achieve immutability in your own code?

The best way to always create a new object is with following notation:

const obj = {}
Reason being that as the objects are mutable, one can easily alter it and add properties to it later on. While const ensures that the variable obj does not get re-declared to some other object accidentally later on in the code.

[ref](https://medium.com/nodejs-tips/mutable-immutable-in-javascript-988cc5c1f9a3)

* Explain the difference between synchronous and asynchronous functions.

* What is event loop?
  * What is the difference between call stack and task queue?

The event loop is a single-threaded loop that monitors the call stack and checks if there is any work to be done in the task queue. If the call stack is empty and there are callback functions in the task queue, a function is dequeued and pushed onto the call stack to be executed.

Philip Robert's talk on the Event Loop

[ref](https://github.com/yangshun/front-end-interview-handbook/blob/master/questions/javascript-questions.md)

* Explain the differences on the usage of `foo` between `function foo() {}` and `var foo = function() {}`

The former is a function declaration while the latter is a function expression. The key difference is that function declarations have its body hoisted but the bodies of function expressions are not (they have the same hoisting behavior as variables).

[ref](https://github.com/yangshun/front-end-interview-handbook/blob/master/questions/javascript-questions.md)

* What are the differences between variables created using `let`, `var` or `const`?

Variables declared using the var keyword are scoped to the function in which they are created, or if created outside of any function, to the global object. var allows variables to be hoisted. 

let and const are block scoped, meaning they are only accessible within the nearest set of curly braces (function, if-else block, or for-loop).


[ref](https://github.com/yangshun/front-end-interview-handbook/blob/master/questions/javascript-questions.md)

* What are the differences between ES6 class and ES5 function constructors?

inheitance is different

```js
// ES5 Function Constructor
function Student(name, studentId) {
  // Call constructor of superclass to initialize superclass-derived members.
  Person.call(this, name);

  // Initialize subclass's own members.
  this.studentId = studentId;
}

Student.prototype = Object.create(Person.prototype);
Student.prototype.constructor = Student;

// ES6 Class
class Student extends Person {
  constructor(name, studentId) {
    super(name);
    this.studentId = studentId;
  }
}
```


[ref](https://github.com/yangshun/front-end-interview-handbook/blob/master/questions/javascript-questions.md)

* Can you offer a use case for the new arrow `=>` function syntax? How does this new syntax differ from other functions?

* What advantage is there for using the arrow syntax for a method in a constructor?

* What is the definition of a higher-order function?

A higher-order function is any function that (1) takes one or more functions as arguments, which it uses to operate on some data, and/or (2) returns a function as a result. 


[ref](https://github.com/yangshun/front-end-interview-handbook/blob/master/questions/javascript-questions.md)

* Can you give an example for destructuring an object or an array?

Destructuring is an expression available in ES6 which enables a succinct and convenient way to extract values of Objects or Arrays and place them into distinct variables.

Array destructuring

```js
// Variable assignment.
const foo = ['one', 'two', 'three'];

const [one, two, three] = foo;
console.log(one); // "one"
console.log(two); // "two"
console.log(three); // "three"

// Swapping variables
let a = 1;
let b = 3;

[a, b] = [b, a];
console.log(a); // 3
console.log(b); // 1
```

Object destructuring
```js
// Variable assignment.
const o = { p: 42, q: true };
const { p, q } = o;

console.log(p); // 42
console.log(q); // true
```

[ref](https://github.com/yangshun/front-end-interview-handbook/blob/master/questions/javascript-questions.md)

* ES6 Template Literals offer a lot of flexibility in generating strings, can you give an example?

* Can you give an example of a curry function and why this syntax offers an advantage?

* What are the benefits of using `spread syntax` and how is it different from `rest syntax`?

ES6's spread syntax is very useful when coding in a functional paradigm as we can easily create copies of arrays or objects without resorting to Object.create, slice, or a library function. 

ES6's rest syntax offers a shorthand for including an arbitrary number of arguments to be passed to a function. It is like an inverse of the spread syntax, taking data and stuffing it into an array rather than unpacking an array of data, and it works in function arguments, as well as in array and object destructuring assignments.

```js
function addFiveToABunchOfNumbers(...numbers) {
  return numbers.map(x => x + 5);
}
```

[ref](https://github.com/yangshun/front-end-interview-handbook/blob/master/questions/javascript-questions.md)

* How can you share code between files?

This depends on the JavaScript environment.

On the client (browser environment), as long as the variables/functions are declared in the global scope (window), all scripts can refer to them. Alternatively, adopt the Asynchronous Module Definition (AMD) via RequireJS for a more modular approach.

On the server (Node.js), the common way has been to use CommonJS. Each file is treated as a module and it can export variables and functions by attaching them to the module.exports object.

ES2015 defines a module syntax which aims to replace both AMD and CommonJS. This will eventually be supported in both browser and Node environments.


[ref](https://github.com/yangshun/front-end-interview-handbook/blob/master/questions/javascript-questions.md)

* Why you might want to create static class members?

Static class members (properties/methods) are not tied to a specific instance of a class and have the same value regardless of which instance is referring to it. Static properties are typically configuration variables and static methods are usually pure utility functions which do not depend on the state of the instance.

[ref](https://github.com/yangshun/front-end-interview-handbook/blob/master/questions/javascript-questions.md)