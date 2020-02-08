---
title: Could CSS3 be making sites that are not testable?
date: 2013-02-06
--- 

Could CSS 3, while is a great thing for the Internet and for web developers, be making websites that are extremely hard to automate?

As most of you know, in Selenium WebDriver we try an emulate what elements that a user can interact with. This means that we do a lot of DOM walking and gathering important little bits about the CSS on each of the elements to make sure that they are visible. You can see the details of what we do in the Determining Visibility section of the W3C Browser Automation Specification. Unfortunately every so often we get a situation where people start raising bugs about their Selenium WebDriver script with respect to allowing or allowing interaction with elements.

Recently we have started getting issues with how we handle CSS transforms. The first bug report came from the Web QA Team creating tests for Firefox OS. They were automating the Music app and noticed that tests were saying that an element was visible when it wasn't. I recreate the issue and make a minimised test case. You can see the element is miles away from the rest of the DOM below in the minimised test case below. 

![CSS transform has moved element way off the page](/img/css_transform_to_nowhere.jpeg)

Marionette, the future of FirefoxDriver and what we use to automate Firefox OS, thinks that is visible. So down the slippery slope we go, add in support CSS Transforms, because that is the root cause for this bug. Now if you are wondering how complex transforms can get have a look at this [MDN article](https://developer.mozilla.org/en-US/docs/CSS/transform-function). It's just Algebraic transforms, no big deal right? But it can be when you start thinking in terms of cascading. Parent, or even great great grandparent element of the element we need to interact with could have the style applied. Now we need to walk the DOM from the element we want all the way back to body.

NOTE: Selenium has to walk the DOM a few times during various parts of the visibility checks which on large pages can be very expensive (therefore slow!). I am looking at you in particular large table element! We do the same when getting the text of elements

So while we are doing all of this work we used to assume that anything that mutates the element, like moving it across the page or changing its visibility, is going to happen in JavaScript and since the visibility code for Selenium is written in JavaScript, we will be blocking the JavaScript thread. CSS 3 has created things that have meant that we don't need JavaScript anymore to do animations

That means working with Selenium on something that is pure CSS animations might not work as you expect. So next time you decide to have a play with CSS 3 and it's awesomeness, have a play with Selenium at the same time and see how we fare. I suspect there are a lot of cases we don't cater for and we should! 