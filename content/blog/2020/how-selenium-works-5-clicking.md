---
title: "How Selenium Works: Episode 5 - clicking"
publishDate: 2020-03-25
---

After an interaction on the last weekend of January 2020, on a Selenium Issue where someone said “why can’t you just…” after I explained the issue I thought that I would start explaining commands in Selenium WebDriver and why we landed on the design that we have today.

I will repeat this on every page of the series but a lot, an annoying amount sometimes, of thinking goes into how every little bit of Selenium works. 

In this episode, we are going to look at what goes into clicking. For simplicity, we are only going to be looking at the work that goes into doing `element.click()`.

## You have an element you want to click, now what?

So you have [found an element]({{< ref "/blog/2020/how-selenium-works-4-finding-elements">}}) and you need to click on it. Selenium will go through the following steps before it even does the click.

### Check Element is still on the page.

When we were [finding an element]({{< ref "/blog/2020/how-selenium-works-4-finding-elements#what-happens-when-it-finds-the-element">}}) the browser would have returned a representation of that element. All subsequent calls to that element need to be checked that the element is still on the page. If it is not you will get a `StaleElementReferenceException`.

### Check the element is visible

Once we know the element is still on the page we need to make sure that the element [is displayed]({{< ref "/blog/2020/how-selenium-works-3-isdiplayed">}}). This is important as we would never expect a user to click on an element that is not visible. Since a user would never click there we need to make sure that Selenium can't click here too. If it is not displayed you will receive an `ElementNotVisibleException`.

### Scroll to the element

Next, we need to make sure that we can scroll to the element. To have all the events fire properly we are going to need to have the element in the [viewport](https://developer.mozilla.org/en-US/docs/Glossary/Viewport). If the element is not in the viewport after trying to scroll to it, the browser will return a `ElementNotInteractableException`.

### Check if the element is interactable

The browser will now check if the element that will receive clicks in the area that we're telling it is the same element as we have passed in. If it looks like it's not the element that will receive the click the browser will throw a `ElementClickInterceptedException`.

### Firing some events off

Now we're ready to fire off some events. We have a special case for `<option>` to make sure that it's parent element that receives the events.

We then calculate the centre of the element and start sending the events. You can read which events we send in the [WebDriver Specification](https://w3c.github.io/webdriver/#element-click).

### What about navigation?

We need to tell the browser to watch for navigation happening thanks to the click. If a click does cause a navigation we apply the same logic if we were [navigating]({{< ref "/blog/2020/how-selenium-works-2-navigation">}}).

## Further Reading

You can read the [WebDriver Specification](https://w3c.github.io/webdriver/#element-click) for more details