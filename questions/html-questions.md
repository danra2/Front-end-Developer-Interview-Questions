# HTML Questions:

* What does a `doctype` do?

Basically, the DOCTYPE describes the HTML that will be used in your page.

Browsers also use the DOCTYPE to determine how to render a page. Not including a DOCTYPE or including an incorrect DOCTYPE can trigger quirks mode. The kicker here is that quirks mode in Internet Explorer is quite different from quirks mode in Firefox (and other browsers), meaning that you'll have a much harder job trying to ensure your page works consistently in all browsers if pages are rendered in quirks mode than you will if they are rendered in standards mode.

* How do you serve a page with content in multiple languages?

code-level language information: from “lang” attributes to Document Type Definitions (DTD). Lang attribute on html tag doens't allow multiple languages. Some web editing programs create these attributes automatically, and therefore they aren’t very reliable when trying to determine the language of a webpage.

HTTP header: 
```
HTTP/1.1·200·OK
Date:·Sat,·23·Jul·2011·07:28:50·GMT
Server:·Apache/2
Content-Location:·qa-http-and-lang.en.php
Vary:·negotiate,accept-language,Accept-Encoding
TCN:·choice
P3P:·policyref="http://www.w3.org/2001/05/P3P/p3p.xml"
Connection:·close
Transfer-Encoding:·chunked
Content-Type:·text/html; charset=utf-8
Content-Language:·en
```

Like the meta element with the http-equiv attribute set to Content-Language, the value of the HTTP header can be a comma-separated list of language tags

### Geo -targeting

buy country-specific top-level domains (TLD) for all the countries you plan to serve. Users will identify your site as a local one they are more likely to trust. On the other hand, expensive and hard to update and maintain. 

Subdomains: Put the content of every language in a different subdomain. For our example, you would have en.example.com, de.example.com, and es.example.com.

Subdirectories: Put the content of every language in a different subdirectory. This is easier to handle when updating and maintaining your site. For our example, you would have example.com/en/, example.com/de/, and example.com/es/.

Server location (through the IP address of the server) is frequently near your users. However, some websites use distributed content delivery networks (CDNs) or are hosted in a country with better webserver infrastructure, so we try not to rely on the server location alone.

### Language -targeting

If you want to reach all speakers of a particular language around the world, you probably don't want to limit yourself to a specific geographic location. 

Content-organization: 

* What kind of things must you be wary of when design or developing for multilingual sites?

Use different URLs for different language versions

Make sure the page language is obvious: We don’t use any code-level language information such as lang attributes, or the URL.

Let the user switch the page language. If you have multiple versions of a page: (1) Consider adding hyperlinks to other language versions of a page. That way users can click to choose a different language version of the page. (2) Avoid automatic redirection based on the user’s perceived language. These redirections could prevent users (and search engines) from viewing all the versions of your site.


Use language-specific URLs: It’s fine to use localized words in the URL, or to use an Internationalized Domain Name (IDN). However, be sure to use UTF-8 encoding in the URL (in fact, we recommend using UTF-8 wherever possible) and remember to escape the URLs properly when linking to them.

* What are `data-` attributes good for?

Any attribute on any element whose attribute name starts with data- is a data attribute. 
```html
<article
  id="electriccars"
  data-columns="3"
  data-index-number="12314"
  data-parent="cars">
...
</article>
```
### JavaScript Access

You can read out via a dataset property.

To get a data attribute through the dataset object, get the property by the part of the attribute name after data- (note that dashes are converted to camelCase).


```js
var article = document.getElementById('electriccars');
 
article.dataset.columns // "3"
article.dataset.indexNumber // "12314"
article.dataset.parent // "cars"
```

### CSS Access

As data attributes are plain HTML attributes, you can even access them from CSS. 

(1) with attr() function.

```css
article::before {
  content: attr(data-parent);
}
```

(2) Attribute Selectors
```css
article[data-columns='3'] {
  width: 400px;
}
article[data-columns='4'] {
  width: 600px;
}
```

* Consider HTML5 as an open web platform. What are the building blocks of HTML5?

[cheatsheet](https://www.wpkube.com/html5-cheat-sheet/)

(1) more semantic text markup

HTML5 brings precision to the sectioning and heading features, allowing document outlines to be predictable and used by the browser to improve the user experience.

(2) new form elements

The <form> element will define where and how to send the data thanks to the action attribute and the method attribute.

To name the data in a form you need to use the name attribute on each form widget that will collect a specific piece of data. Example:

```html
<form action="/my-handling-form-page" method="post"> 
  <div>
    <label for="name">Name:</label>
    <input type="text" id="name" name="user_name" />
  <div>
  <div>
    <label for="mail">E-mail:</label>
    <input type="email" id="mail" name="user_email" />
  </div>
  <div>
    <label for="msg">Message:</label>
    <textarea id="msg" name="user_message"></textarea>
  </div>

  ...

```

<fieldset> element create groups of widgets that share the same purpose, for styling and semantic purposes. a <legend> element just below the opening <fieldset> tag formally describes the purpose of the <fieldset>. Many assistive technologies will use the <legend> element.

<label> formal way to define a label for an HTML form widget.

(3) video and audio

video/audio elements with controls, supporting multiple formats

(4) new javascript API

WebGL

(5) canvas and SVG

### canvas

<canvas> is an HTML element which can be used to draw graphics via scripting. Unlike SVG, <canvas> only supports one primitive shape: rectangles. All other shapes must be created by combining one or more paths.

```js
var canvas = document.getElementById('tutorial');

if (canvas.getContext) {
  var ctx = canvas.getContext('2d');
  // drawing code here
} else {
  // canvas-unsupported code here
}
```

Another powerful feature of the new canvas Path2D API is using SVG path data to initialize paths on your canvas. This might allow you to pass around path data and re-use them in both, SVG and canvas.

### Drawing text

### SVG

scalable vector graphics

### WebGL

a JavaScript API for rendering interactive 3D and 2D graphics within any compatible web browser without the use of plug-ins. WebGL does so by introducing an API that closely conforms to OpenGL ES 2.0 that can be used in HTML5 <canvas> elements.


(6) new communication API

WebSocket API: open a two-way interactive communication session between the user's browser and a server. With this API, you can send messages to a server and receive event-driven responses without having to poll the server for a reply.

WebRTC (Web Real-Time Communications): is a technology which enables Web applications and sites to capture and optionally stream audio and/or video media, as well as to exchange arbitrary data between browsers without requiring an intermediary. The set of standards that comprises WebRTC makes it possible to share data and perform teleconferencing peer-to-peer, without requiring that the user install plug-ins or any other third-party software.


(7) geolocation API

The Geolocation API allows the user to provide their location to web applications if they so desire. 

```js
if ("geolocation" in navigator) {
  /* geolocation is available */
  navigator.geolocation.getCurrentPosition(function(position) {
    do_something(position.coords.latitude, position.coords.longitude);
    });
} else {
  /* geolocation IS NOT available */
}

```

(8) web worker API

Web Workers is a simple means for web content to run scripts in background threads. The worker thread can perform tasks without interfering with the user interface. 

传统页面中（HTML5 之前）的 JavaScript 的运行都是以单线程的方式工作的，虽然有多种方式实现了对多线程的模拟（例如：JavaScript 中的 setinterval 方法，setTimeout 方法等），但是在本质上程序的运行仍然是由 JavaScript 引擎以单线程调度的方式进行的。在 HTML5 中引入的工作线程使得浏览器端的 JavaScript 引擎可以并发地执行 JavaScript 代码，从而实现了对浏览器端多线程编程的良好支持。

(9) new data storage

### Web Storage interfaces

Storage object: Allows you to set, retrieve and remove data for a specific domain and storage type (session or local.)

Window: extended with two new properties — Window.sessionStorage and Window.localStorage, and a Window.onstorage event handler that fires when a storage area changes (e.g. a new item is stored.)

StorageEvent: The storage event is fired on a document's Window object when a storage area changes.

### IndexedDB API

IndexedDB is a low-level API for client-side storage of significant amounts of structured data, including files/blobs. Unlike SQL-based RDBMSes, which use fixed-column tables, IndexedDB is a JavaScript-based object-oriented database.

IndexedDB lets you store and retrieve objects that are indexed with a key. You need to specify the database schema, open a connection to your database, and then retrieve and update data within a series of transactions.

(10) online/offline events

You need to know when the user comes back online, so that you can re-synchronize with the server.

You need to know when the user is offline, so that you can queue your server requests for a later time.

* Describe the difference between a `cookie`, `sessionStorage` and `localStorage`.

## Cookies:

We can set the expiration time for each cookie
The 4K limit is for the entire cookie, including name, value, expiry date etc. 

The data is sent back to the server for every HTTP request (HTML, images, JavaScript, CSS, etc) - increasing the amount of traffic between client and server.

## LocalStorage:

Web storage can be viewed simplistically as an improvement on cookies, providing much greater storage capacity (5MB)

The data is not sent back to the server for every HTTP request (HTML, images, JavaScript, CSS, etc) - reducing the amount of traffic between client and server.

The data stored in localStorage persists until explicitly deleted. Changes made are saved and available for all current and future visits to the site. It works on same-origin policy, data stored will only be available on the same origin.

Same-origin: Two pages have the same origin if the protocol, port (if one is specified), and host are the same for both pages.

## sessionStorage:

It is similar to localStorage.

Changes are only available per window (or tab in browsers like Chrome and Firefox). The data is available only inside the window/tab in which it was set.

Like localStorage, it works on same-origin policy.

* Describe the difference between `<script>`, `<script async>` and `<script defer>`.

## Normal execution <script>

Default behavior of the <script> element. Parsing of the HTML code pauses while the script is executing. For slow servers and heavy scripts this means that displaying the webpage will be delayed.

## Deferred execution <script defer>

Delaying script execution until the HTML parser has finished. 

A positive effect of this attribute is that the DOM will be available for your script. However, since not every browser supports defer yet, don’t rely on it!

## Asynchronous execution <script async>

Don’t care when the script will be available. 

HTML parsing may continue and the script will be executed as soon as it’s ready. I’d recommend this for scripts such as Google Analytics.

* Why is it generally a good idea to position CSS `<link>`s between `<head></head>` and JS `<script>`s just before `</body>`? Do you know any exceptions?

## positioning CSS in head:

It prevents the flash of unstyled contents. Placing at the top allows the page to render progressively which improves user experience. Putting stylesheets near the bottom of the document prohibits progressive rendering in many browsers, including Internet Explorer. (Some browsers block rendering to avoid having to repaint elements of the page if their styles change.) 

## positioning JS before body

Scripts in the other hand are placed in the bottom to allow the HTML to be parsed and displayed to the user first while scripts are downloaded and executed.


* What is progressive rendering?

techniques used to render content for display as quickly as possible.

It used to be much more prevalent in the days before broadband internet but it's still useful in modern development as mobile data connections are becoming increasingly popular (and unreliable!)

Examples of such techniques :

(1) Lazy loading of images where (typically) some javascript will load an image when it comes into the browsers viewport instead of loading all images at page load.

(2) Prioritizing visible content (or above the fold rendering) where you include only the minimum css/content/scripts necessary for the amount of page that would be rendered in the users browser first to display as quickly as possible, you can then use deferred javascript (domready/load) to load in other resources and content.

* Why you would use a `srcset` attribute in an image tag? Explain the process the browser uses when evaluating the content of this attribute.

···html
<img srcset="elva-fairy-320w.jpg 320w,
             elva-fairy-480w.jpg 480w,
             elva-fairy-800w.jpg 800w"
     sizes="(max-width: 320px) 280px,
            (max-width: 480px) 440px,
            800px"
     src="elva-fairy-800w.jpg" alt="Elva dressed as a fairy">
···

srcset defines the set of images we will allow the browser to choose between, and what size each image is.

sizes defines a set of media conditions (e.g. screen widths) and indicates what image size would be best to choose, when certain media conditions are true

* Have you used different HTML templating languages before?

EJS, Handlebars, Jade
