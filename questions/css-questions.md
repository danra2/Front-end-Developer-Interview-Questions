# CSS Questions:

* What is CSS selector specificity and how does it work?

Simple selectors

Attribute selectors

Pseudo-selectors Pseudo-classes (based on the element's state), e.g. ::after

Pseudo-elements (based on relative position of elements), e.g. :first-of-type, :hover

Combinators, e.g. '+'

Multiple selectors (apply a single set of declarations to all the elements selected by those selectors)


* What's the difference between "resetting" and "normalizing" CSS? Which would you choose, and why?

CSS resets aim to remove all built-in browser styling. Standard elements like h1 - h6, p, strong, em end up looking exactly alike, having no decoration at all. You're then supposed to add all decoration yourself.

[Normalize.css](http://nicolasgallagher.com/about-normalize-css/) aims to make built-in browser styling consistent across browsers. Elements like h1 - h6 will appear bold, larger et cetera in a consistent way across browsers. You're then supposed to add only the difference in decoration your design needs.

* Describe Floats and how they work.

The float CSS property specifies that an element should be placed along the left or right side of its container, allowing text and inline elements to wrap around it. The element is removed from the normal flow of the web page, though still remaining a part of the flow (in contrast to absolute positioning).

```css
/* Keyword values */
float: left;
float: right;
float: none;
float: inline-start;
float: inline-end;

/* Global values */
float: inherit; /*Get the property from the parent element.*/
float: initial; /*The default value for the property (the browser default).*/
float: unset; /*Acts as either inherit or initial. It’ll act as inherit if the parent has a value that matches, or else it will act as initial.*/
```

* Describe z-index and how stacking context is formed.

## Scenarios

A stacking context is formed, anywhere in the document, by any element in the following scenarios:
```
Root element of document (HTML).

Element with a position value "absolute" or "relative" and z-index value other than "auto".

Element with a position value "fixed" or "sticky" (sticky for all mobile browsers, but not older desktop).

Element that is a child of a flex (flexbox) container, with z-index value other than "auto".

Element with a opacity value less than 1 (See the specification for opacity).

Element with a mix-blend-mode value other than "normal".

Element with any of the following properties with value other than "none":
transform, 
filter, 
perspective, 
clip-path, 
mask / mask-image / mask-border

Element with a isolation value "isolate".

Element with a -webkit-overflow-scrolling value "touch".

Element with a will-change value specifying any property that would create a stacking context on non-initial value (see this post).

Element with acontain value of "layout", or "paint", or a composite value that includes either of them.

```

## In summary

Stacking contexts can be contained in other stacking contexts, and together create a hierarchy of stacking contexts.

Each stacking context is completely independent from its siblings: only descendant elements are considered when stacking is processed.

Each stacking context is self-contained: after the element's contents are stacked, the whole element is considered in the stacking order of the parent stacking context.

* Describe BFC (Block Formatting Context) and how it works.

It is the region in which the layout of block boxes occurs and in which floats interact with other elements.

A block formatting context is created by at least one of the following:

```
the root element or something that contains it

floats (elements where float is not none)

absolutely positioned elements (elements where position is absolute or fixed)
inline-blocks (elements with display: inline-block)

table cells (elements with display: table-cell, which is the default for HTML table cells)

table captions (elements with display: table-caption, which is the default for HTML table captions)

anonymous table cells implicitly created by the elements with display: table, table-row, table-row-group, table-header-group, table-footer-group (which is the default for HTML tables, table rows, table bodies, table headers and table footers, respectively), or inline-table 

block elements where overflow has a value other than visible

display: flow-root

elements with contain: layout, content, or strict

flex items (direct children of the element with display: flex or inline-flex)

grid items (direct children of the element with display: grid or inline-grid)

multicol containers (elements where column-count or column-width is not auto, including elements with column-count: 1)

column-span: all should always create a new formatting context, even when the column-span: all element isn't contained by a multicol container (Spec change, Chrome bug).

```

* What are the various clearing techniques and which is appropriate for what context?

Clear is sister property of float's.

Clear has four valid values as well. (1) Both is most commonly used, which clears floats coming from either direction. (2) Left and Right can be used to only clear the float from one direction respectively, less commonly used. (3) None is the default, which is typically unnecessary unless removing a clear value from a cascade. (4) Inherit would be the fifth, but is strangely not supported in Internet Explorer.

* How would you approach fixing browser-specific styling issues?

After identifying the issue and the offending browser, use a separate style sheet that only loads when that specific browser is being used. This technique requires server-side rendering though.

Use libraries like Bootstrap that already handles these styling issues for you.

Use autoprefixer to automatically add vendor prefixes to your code.

Use Reset CSS or Normalize.css.

[references](https://github.com/yangshun/front-end-interview-handbook/blob/master/questions/css-questions.md)

* How do you serve your pages for feature-constrained browsers?
  * What techniques/processes do you use?

Graceful degradation - The practice of building an application for modern browsers while ensuring it remains functional in older browsers.

Progressive enhancement - The practice of building an application for a base level of user experience, but adding functional enhancements when a browser supports it.

Use caniuse.com to check for feature support.

Autoprefixer for automatic vendor prefix insertion.

Feature detection using Modernizr.

Use CSS Feature queries

[references](https://github.com/yangshun/front-end-interview-handbook/blob/master/questions/css-questions.md)


* What are the different ways to visually hide content (and make it available only for screen readers)?

'display:none' makes content inaccessible by screen readers. 

These techniques are related to accessibility (a11y).

visibility: hidden. However, the element is still in the flow of the page, and still takes up space.
width: 0; height: 0. Make the element not take up any space on the screen at all, resulting in not showing it.
position: absolute; left: -99999px. Position it outside of the screen.
text-indent: -9999px. This only works on text within the block elements.
Metadata. For example by using Schema.org, RDF, and JSON-LD.
WAI-ARIA. A W3C technical specification that specifies how to increase the accessibility of web pages.
Even if WAI-ARIA is the ideal solution, I would go with the absolute positioning approach, as it has the least caveats, works for most elements and it's an easy technique.

[references](https://github.com/yangshun/front-end-interview-handbook/blob/master/questions/css-questions.md)

* Have you ever used a grid system, and if so, what do you prefer?
* Have you used or implemented media queries or mobile specific layouts/CSS?
* Are you familiar with styling SVG?
* Can you give an example of an `@media` property other than `screen`?
* What are some of the "gotchas" for writing efficient CSS?
* What are the advantages/disadvantages of using CSS preprocessors?
  * Describe what you like and dislike about the CSS preprocessors you have used.
* How would you implement a web design comp that uses non-standard fonts?
* Explain how a browser determines what elements match a CSS selector.
* Describe pseudo-elements and discuss what they are used for.
* Explain your understanding of the box model and how you would tell the browser in CSS to render your layout in different box models.
* What does ```* { box-sizing: border-box; }``` do? What are its advantages?
* What is the CSS `display` property and can you give a few examples of its use?
* What's the difference between inline and inline-block?
* What's the difference between the "nth-of-type()" and "nth-child()" selectors?
* What's the difference between a relative, fixed, absolute and statically positioned element?
* What existing CSS frameworks have you used locally, or in production? How would you change/improve them?
* Have you played around with the new CSS Flexbox or Grid specs?
* Can you explain the difference between coding a web site to be responsive versus using a mobile-first strategy?
* Have you ever worked with retina graphics? If so, when and what techniques did you use?
* Is there any reason you'd want to use `translate()` instead of *absolute positioning*, or vice-versa? And why?
