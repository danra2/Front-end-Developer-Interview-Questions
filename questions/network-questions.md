# Network Questions:

* Traditionally, why has it been better to serve site assets from multiple domains?

The answer is speed. This speed comes from two main sources:

### Parallelization

If you send 50 requests and each one takes 5 milliseconds for the server to process and respond to, that’s 250 milliseconds.

If you send 50 requests to (a generally ridiculous) 50 servers and each one takes 5 milliseconds to respond, that’s only 5 milliseconds. In this case, the bottleneck would be the client’s ability to send and receive data.

Most of large websites store their static content such as Images, JavaScrips & CSS files to a Content Delivery Network or CDN as deploying your content across multiple servers will make your pages load faster from the user's perspective.

Keeping images and assets on a geographically dispersed CDN allows for faster resource requests by reducing round trip time. 

[ref](https://travishorn.com/why-it-is-better-to-serve-site-assets-from-multiple-domains-972a2bf69d71)

### Domain Sharding

By default, web browsers limit the number of active connections for each domain. When the number of resources to download exceeds that limit, users experience slow page load times as downloads are queued. 

To work around this limitation, developers may split content across multiple subdomains. Since browsers reset the connection limit for each domain, each additional domain allows an additional number of active connections (With domain sharding, the user’s browser connects to two or more different domains to simultaneously download the resources needed to render the web page). This lets the user retrieve files from the same source with greater throughput.

[Quora](https://www.quora.com/Traditionally-why-has-it-been-better-to-serve-site-assets-from-multiple-domains)

[Domain Sharding](https://blog.stackpath.com/glossary/domain-sharding/)

### Reduced header overhead

Many sites have quite a lot of cookies set, these cookies have the purpose of supporting some kind of state.

By putting the static (stateless) resources on a completely different domain you can reduce the size of the http requests. In some cases there are so many cookie that a single http request takes two TCP packets to be transmitted. So having a separate domain is one of the ways to reduce the number of packets to request the various parts of a page.

Other methods with the same goal are merging a lot of images into a single sprite and merging all Javascript into a single file.

[ref](https://webmasters.stackexchange.com/questions/25087/what-is-the-advantage-to-hosting-static-resources-on-a-separate-domain)

* Do your best to describe the process from the time you type in a website's URL to it finishing loading on your screen.

1. You enter a URL into the browser

2. The browser looks up the IP address for the domain name

The first step in the navigation is to figure out the IP address for the visited domain. The DNS lookup proceeds as follows:

- Browser cache – The browser caches DNS records for some time. Interestingly, the OS does not tell the browser the time-to-live for each DNS record, and so the browser caches them for a fixed duration (varies between browsers, 2 – 30 minutes).

- OS cache – If the browser cache does not contain the desired record, the browser makes a system call (gethostbyname in Windows). The OS has its own cache.

- Router cache – The request continues on to your router, which typically has its own DNS cache.

- ISP DNS cache – The next place checked is the cache ISP’s DNS server. With a cache, naturally.

- Recursive search – Your ISP’s DNS server begins a recursive search, from the root nameserver, through the .com top-level nameserver, to Facebook’s nameserver. Normally, the DNS server will have names of the .com nameservers in cache, and so a hit to the root nameserver will not be necessary.

3. The browser sends a HTTP request to the web server

4. The facebook server responds with a permanent redirect

The server responded with a 301 Moved Permanently response to tell the browser to go to “http://www.facebook.com/” instead of “http://facebook.com/”.

5. The browser follows the redirect

The browser now knows that “http://www.facebook.com/” is the correct URL to go to, and so it sends out another GET request

6. The server ‘handles’ the request

The server will receive the GET request, process it, and send back a response.

7. The server sends back a HTML response

The Content-Encoding header tells the browser that the response body is compressed using the gzip algorithm. After decompressing the blob, you’ll see the HTML you’d expect

In addition to compression, headers specify whether and how to cache the page, any cookies to set (none in this response), privacy information, etc.

8. The browser begins rendering the HTML

9. The browser sends requests for objects embedded in HTML

As the browser renders the HTML, it will notice tags that require fetching of other URLs. The browser will send a GET request to retrieve each of these files.

10. The browser sends further asynchronous (AJAX) requests

In the spirit of Web 2.0, the client continues to communicate with the server even after the page is rendered.

[reference](http://igoro.com/archive/what-really-happens-when-you-navigate-to-a-url/)

* What are the differences between Long-Polling, Websockets and Server-Sent Events?

### Long pulling 

#### How it works

Client application (browser) sends a request with event recipient id (here is the user, registered on the page) and current state (the displayed number of unread notification) to the server via HTTP. It creates an Apache process, which repeatedly checks DB until the state is changed in there. When the state eventually changed, the client gets the server response and sends next request to the server.

![](http://dsheiko.com/download//000000123/longpolling-architecture.png)

#### Implementation

JS module sends recipient user id and current state (unread notification number) to the server as XMLHttpRequest. To make possible cross-domain communication, we use JSONP and that means the handler must of the public scope.

Server waits 3 second then check if the updated state matches the given one. If the state has changed in the DB, the server responds, otherwise it repeats the cycle.

### Server-Sent Events

Client (browser) sends a request to the server via HTTP. It creates a process, which fetches latest state in the DB and responds back. Client gets server response and in 3 seconds sends next request to the server. Browser will repeat request every 3 second unless you close the connection

![](http://dsheiko.com/download//000000123/sse-architecture.png)

### Web Sockets

Client notifies web-socket server (EventMachine) of an event, giving ids of recipients. The server immediately notifies all the active clients (subscribed to that type of event). Clients process event when given recipient Id matches the client’s one.

![](http://dsheiko.com/download//000000123/websocket-architecture.png)

[Conclusion](http://dsheiko.com/weblog/websockets-vs-sse-vs-long-polling)

* Explain the following request and response headers:

  * Diff. between Expires, Date, Age and If-Modified-...

  * Do Not Track: request header indicates the user's tracking preference. It lets users indicate whether they would prefer privacy rather than personalized content.

  * Cache-Control: General header, to specify directives for caching mechanisms in both requests and responses.

  * Transfer-Encoding: Response header, Specifies the the form of encoding used to safely transfer the entity to the user. e.g. chunked

  * ETag: It is a validator, a unique string identifying the version of the resource. Conditional requests using If-Match and If-None-Match use this value to change the behavior of the request.

  * X-Frame-Options: Indicates whether a browser should be allowed to render a page in a <frame>, <iframe> or <object>

[ref](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers)

* What are HTTP methods? List all HTTP methods that you know, and explain them.

GET: requests a representation of the specified resource, retrieve data.

POST: sends data to the server. The type of the body of the request is indicated by the Content-Type header.

PUT: creates a new resource or replaces a representation of the target resource with the request payload. Idempotent: calling it once or several times successively has the same effect (that is no side effect), where successive identical POST may have additional effects, like passing an order several times.

DELETE: deletes the specified resource. Idempotent.

PATCH: applies partial modifications to a resource. Not idempotent.

(below not included in RESTful)

HEAD: requests the headers that are returned if the specified resource would be requested with an HTTP GET method (ignore the body of a GET request). Such a request can be done before deciding to download a large resource to save bandwidth, for example.

CONNECT: starts two-way communications with the requested resource. It can be used to open a tunnel.

OPTIONS: to describe the communication options for the target resource. The client can specify a URL for the OPTIONS method, or an asterisk (*) to refer to the entire server.

Examples: (1) To find out which request methods a server supports, one can use curl and issue an OPTIONS request. (2) In CORS, a preflight request with the OPTIONS method is sent, so that the server can respond whether it is acceptable to send the request with these parameters.

TRACE: performs a message loop-back test along the path to the target resource, providing a useful debugging mechanism.