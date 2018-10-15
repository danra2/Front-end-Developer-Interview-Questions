# HTML Questions:

* What does a `doctype` do?

Basically, the DOCTYPE describes the HTML that will be used in your page.

Browsers also use the DOCTYPE to determine how to render a page. Not including a DOCTYPE or including an incorrect DOCTYPE can trigger quirks mode. The kicker here is that quirks mode in Internet Explorer is quite different from quirks mode in Firefox (and other browsers), meaning that you'll have a much harder job trying to ensure your page works consistently in all browsers if pages are rendered in quirks mode than you will if they are rendered in standards mode.

* How do you serve a page with content in multiple languages?

code-level language information: from “lang” attributes to Document Type Definitions (DTD). Lang attribute on html tag doens't allow multiple languages. Some web editing programs create these attributes automatically, and therefore they aren’t very reliable when trying to determine the language of a webpage.

HTTP header: 
> HTTP/1.1·200·OK
> Date:·Sat,·23·Jul·2011·07:28:50·GMT
> Server:·Apache/2
> Content-Location:·qa-http-and-lang.en.php
> Vary:·negotiate,accept-language,Accept-Encoding
> TCN:·choice
> P3P:·policyref="http://www.w3.org/2001/05/P3P/p3p.xml"
> Connection:·close
> Transfer-Encoding:·chunked
> Content-Type:·text/html; charset=utf-8
> Content-Language:·en

Like the meta element with the http-equiv attribute set to Content-Language, the value of the HTTP header can be a comma-separated list of language tags

###Geo -targeting

buy country-specific top-level domains (TLD) for all the countries you plan to serve. Users will identify your site as a local one they are more likely to trust. On the other hand, expensive and hard to update and maintain. 

Subdomains: Put the content of every language in a different subdomain. For our example, you would have en.example.com, de.example.com, and es.example.com.

Subdirectories: Put the content of every language in a different subdirectory. This is easier to handle when updating and maintaining your site. For our example, you would have example.com/en/, example.com/de/, and example.com/es/.

Server location (through the IP address of the server) is frequently near your users. However, some websites use distributed content delivery networks (CDNs) or are hosted in a country with better webserver infrastructure, so we try not to rely on the server location alone.

###Language -targeting

If you want to reach all speakers of a particular language around the world, you probably don't want to limit yourself to a specific geographic location. 

Content-organization: 

* What kind of things must you be wary of when design or developing for multilingual sites?

Use different URLs for different language versions

Make sure the page language is obvious: We don’t use any code-level language information such as lang attributes, or the URL.

Let the user switch the page language. If you have multiple versions of a page: (1) Consider adding hyperlinks to other language versions of a page. That way users can click to choose a different language version of the page. (2) Avoid automatic redirection based on the user’s perceived language. These redirections could prevent users (and search engines) from viewing all the versions of your site.


Use language-specific URLs: It’s fine to use localized words in the URL, or to use an Internationalized Domain Name (IDN). However, be sure to use UTF-8 encoding in the URL (in fact, we recommend using UTF-8 wherever possible) and remember to escape the URLs properly when linking to them.

* What are `data-` attributes good for?



* Consider HTML5 as an open web platform. What are the building blocks of HTML5?
* Describe the difference between a `cookie`, `sessionStorage` and `localStorage`.
* Describe the difference between `<script>`, `<script async>` and `<script defer>`.
* Why is it generally a good idea to position CSS `<link>`s between `<head></head>` and JS `<script>`s just before `</body>`? Do you know any exceptions?
* What is progressive rendering?
* Why you would use a `srcset` attribute in an image tag? Explain the process the browser uses when evaluating the content of this attribute.
* Have you used different HTML templating languages before?
