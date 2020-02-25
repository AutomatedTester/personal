---
title: "How Selenium Works: Episode 3 - isDisplayed"
publishDate: 2020-02-27
---

After an interaction on the last weekend of January 2020, on a Selenium Issue where someone said “why can’t you just…” after I explained the issue I thought that I would start explaining commands in Selenium WebDriver and why we landed on the design that we have today.

I will repeat this on every page of the series but a lot, an annoying amount sometimes, of thinking goes into how every little bit of Selenium works. 

In this episode we are going to look at the vast amount of work that goes into `isDisplayed()`. This work is used in interaction commands so it is good to get an understanding of how this all works and how other commands use it. From a technical point of view, this is my favourite command and how it works.

## Why do we care if it is "displayed"?

So, let's start with understanding why we would even want to care if the element is displayed or not. 

Firstly, testing is one the biggest use cases for Selenium so when people do an interaction on the page. Unfortunately, web pages are designed for showing information and not necessarily designed for interactive web pages. As we move to a more interactive web we needed a tool that could at least try workout if elements are visible. 

There are two main cases that we will try see if an element is visible. When we call the `element.is_displayed()` for looking at an element and when we do interactions.

_NOTE_

_Selenium has two type of commands when it comes to interactions. The methods found on `WebElement` are of the "do what I mean" style: selenium tries to do make what you intended to happen when you `type` or `click`. The methods associated with `Actions` are of the "do what I say" style --- these commands will do exactly what you tell them to, without attempting to interpret what you're actually trying to do._

## How does it work?

First of all, we need to understand that `element.is_displayed()` needs to work without having to make the page scroll. This is important as we don't want to be moving the page unnecessarily.

From there we need to have a look at the element. We need to start looking at the CSS that is on the element. 

First, let's see if the element would still be part of the accessibility tree. We don't look in the accessibility tree, we just look at some scenarios where they won't be there and they won't influence any positioning of elements on the page. We don't look in the accessibility tree as this can be an extremely expensive operation in the browser. 

An easy scenario like that is when there is `display: none` on a element like below.

{{< highlight css >}}
#idOfElement {
    display: none;
}
{{< /highlight >}}

This will result in nothing being added to the [accessibility tree](https://developer.mozilla.org/en-US/docs/Learn/Accessibility/What_is_accessibility) and won't influence the positioning of elements. 

Next we need to look and see if we can actually scroll to the element if that is necessary. We are just going to test it as we don't want to actually scroll. This is important for elements that have been transformed to somewhere else on the page. This means that we can't always rely on looking at the DOM tree. I discussed this in a [previous post in 2013]({{< ref "/blog/2013/could-css3-be-making-sites-that-are-not-testable">}}). 

From there we need to do a few checks on items like `<input>`, `<option>`, `<optgroup>` and `<map>`. `<option>` and `<optgroup>`  items are "hidden" unless you click on them we assume they are visible... well mostly.

Once we have done these tests we need then recursively walk the DOM and redo all the tests until we reach the [`documentElement`](https://developer.mozilla.org/en-US/docs/Web/API/Document/documentElement).

### Why the recursion?

Well unfortunately due to CSS we don't always know at what level of the DOM. We need to check each node along the way back. It does mean that `isDisplayed` can be a little slow to run but it gives us a good approximation if the element would be visible.

## Can't we just ask the browser if an element is visible?

Unfortunately no. Browsers when working out what to render they build up display lists and then send these over to the window manager for it to do all the heavy lifting. Browsers have in the past never worried too much about how efficiently the display list was generated. The window manager will try make the display list more efficient and then render. Great!

Now, the thing with display lists is they only tell the window manager what to render for the viewport. The viewport is the area of the page that you can see when you are using the browser. We want `isDisplayed()` to tell us if an element **_will be_** displayed. 

We also get into the realms of partially obscured elements. How do we know if an element is truly visible or just a small part is visible. Here we could use some web APIs like [`document.elementFromPoint(x, y);`](https://developer.mozilla.org/en-US/docs/Web/API/DocumentOrShadowRoot/elementFromPoint). Again, this would only tell us about elements that are in the viewport and then there are cases where it an element might be covering another but clicks go to the "hidden element".

I go into this in a little more detail in my Selenium Conf London 2016 talk. You can watch the [video](https://www.youtube.com/watch?v=hTa1KI6fQpg). 

Unfortunately, no one wants to specify how this would work in a browser so until then we need to have our own way of doing these calculations.

## Further reading

Normally I would put a link to the WebDriver spec here for this command but you can read the [appendix](https://w3c.github.io/webdriver/#element-displayedness) which tells drivers what end point to add.

You can also go down the rabbit hole and look at the [code](https://github.com/SeleniumHQ/selenium/blob/master/javascript/atoms/dom.js#L573-L616) that does all this work.