# Performance Questions:

* What tools would you use to find a performance bug in your code?

Performance Testing tool - To check if the application is able to cope up with excessive traffic incoming. Example - Apache JMeter

Cross Browser Testing tool - Helps you in finding bugs of a website across a variety of thousands of browsers running on real OS and different devices. Example - LambdaTest - You can also find bugs in RWD (Responsive Web Design) with their automated screenshots.

VAPT tools- Vulnerability Assessment & Penetration Testing tools - To help you identify the soft spots in your website’s security. Example - Wireshark, NMAP etc.

Bug Tracking Tools - Jira, Asana, Microsoft VSTS helps you to maintain a neat and clean library of all the bugs you found so you could collaborate and organize better with your teammates. Note that LambdaTest provides integrations to these 3 and many other bug tracking platform.

[Quora](https://www.quora.com/What-are-the-tools-used-for-finding-bugs-in-web-applications-websites)

* What are some ways you may improve your website's scrolling performance?

CSS can play a big part in scroll performance degradation. 

To witness where things are causing slowdowns on your site, open up Chrome dev tools - rendering - paint flashing. Now when you scroll around, green blocks you see are repaints which are expensive and thus can slow down perceived performance of your site. 

CSS Performance tricks

(1) no unnecessary tag identifiers, (get rid of too specific identifiers)

(2) no ancestors, otherwise will perform searches right to left, which is expensive
```css
html div tr td{
    //first look at tens of thousands of td's, then all of those with an ancestor tr, and all the way up
    ...
}
```

(3) no universal selectors
```css
* {
    display: block
}
```

(4) no chaining: better be more specific, because search takes longer
```css
.small.private.icon{
    width: 30px
}
.small-private-icon{
    width: 30px
}
```

(5) order of importance (when you do chain)
```css
.foo.bar{

}
//indexed in m_classRules under the key .foo
// if .foo matches 10k elements then you are overloading that index, and search longer. Swapping to .bar.foo will improve the performance

#foo.bar{

}
//better than .bar#foo
```

(6) no unqualifeid selectors

(7) Remove unnecessay htmls (e.g. nested divs)

(8) replace html tags that match to less CSS

(9) use similar styles, refactor css into reusable components

(10) preprocessor: creating really long ancestors and chaining. max indent 3 levels when using sass

[CSS performance](https://vimeo.com/54990931)

* Explain the difference between layout, painting and compositing.

Layout: Once the browser knows which rules (CSS & JS) apply to an element it can begin to calculate how much space it takes up and where it is on screen. 

Painting: the process of filling in pixels. It involves drawing out text, colors, images, borders, and shadows, essentially every visual part of the elements. The drawing is typically done onto multiple surfaces, often called layers.

Compositing: Since the parts of the page were drawn into potentially multiple layers they need to be drawn to the screen in the correct order so that the page renders correctly. This is especially important for elements that overlap another.

[ref](https://developers.google.com/web/fundamentals/performance/rendering/?hl=en)
