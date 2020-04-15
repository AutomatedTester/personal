---
title: "How Selenium Works: Episode 6 - sendKeys"
publishDate: 2020-04-16
---

After an interaction on the last weekend of January 2020, on a Selenium Issue where someone said “why can’t you just…” after I explained the issue I thought that I would start explaining commands in Selenium WebDriver and why we landed on the design that we have today.

I will repeat this on every page of the series but a lot, an annoying amount sometimes, of thinking goes into how every little bit of Selenium works. 

In this episode, we are going to look at what goes into clicking. For simplicity, we are only going to be looking at the work that goes into doing `element.send_key("a string")`.

## You have an element you want to send some text, now what?

So you have [found an element]({{< ref "/blog/2020/how-selenium-works-4-finding-elements">}}) and you need to send text to it. Selenium will go through the following steps before it even sends any text.

### Check Element is still on the page.

When we were [finding an element]({{< ref "/blog/2020/how-selenium-works-4-finding-elements#what-happens-when-it-finds-the-element">}}) the browser would have returned a representation of that element. All subsequent calls to that element need to be checked that the element is still on the page. If it is not you will get a `StaleElementReferenceException`.

### Check the element is visible

Once we know the element is still on the page we need to make sure that the element [is displayed]({{< ref "/blog/2020/how-selenium-works-3-isdiplayed">}}). This is important as we would never expect a user to click on an element that is not visible. Since a user would never click there we need to make sure that Selenium can't click here too. If it is not displayed you will receive an `ElementNotVisibleException`.

### Scroll to the element

Next, we need to make sure that we can scroll to the element. To have all the events fire properly we are going to need to have the element in the [viewport](https://developer.mozilla.org/en-US/docs/Glossary/Viewport). If the element is not in the viewport after trying to scroll to it, the browser will return an `ElementNotInteractableException`.

### Check if the element is interactable

The browser will now check if the element that will receive our text can receive it. For example, we will look if the element might be `disabled`. If it looks like the element can't receive the text, the browser will throw an `ElementNotInteractableException`.

### Process Text String

The next step is we need to process the string that has come in and split it according to [grapheme text clusters](http://www.unicode.org/reports/tr29/). We do this as Selenium has assigned some values in the [unicode private use area](https://en.wikipedia.org/wiki/Private_Use_Areas) or PUA for some of the keys. Below is a sample
{{< highlight python >}}
    TAB = '\ue004'
    CLEAR = '\ue005'
    RETURN = '\ue006'
    ENTER = '\ue007'
    SHIFT = '\ue008'
{{< /highlight >}}

This allows people to send through combinations of keys like below.

{{< highlight python >}}
    from selenium.webdriver.common.keys import Keys
    element.send_keys("I" + Keys.SPACE + "love " + Keys.SHIFT + "cheese")
    # This writes "I love CHEESE"
{{< /highlight >}}

### Firing some events off

Now we're ready to fire off some events. The browser will have gotten the string, so in the case above it would have received `I\ue00dlove \ue008cheese`. The browser will split this into an array and process each character. When it hits one of the PUA it will have logic that needs to apply. For example, with the `Keys.SHIFT` it will set the shift property on characters that follow.

It will fire off a list of events into the event loop so that it will come across as a [trusted event](https://dom.spec.whatwg.org/#dom-event-istrusted). And when done, it will return to the Selenium script.

### What about navigation?

We need to tell the browser to watch for navigation happening thanks to the click. If a click does cause a navigation we apply the same logic if we were [navigating]({{< ref "/blog/2020/how-selenium-works-2-navigation">}}).

### Exceptions

#### File Inputs

There is one element that will be an exception and that is the `<input type=file>` element. When this element is being interacted with the browser will try to check if the string being passed in is a path to a file. If it is not, the browser will error and pass that back to the Selenium script. When using Selenium Grid, Selenium will encode the file you want to upload as a base64 string, send it through the grid, and then let the grid update the file path that will be used.

Selenium will also try to ignore some of the visibility checks. This is because File inputs can't be styled so people do CSS tricks to have a nicer looking element on the page and hide the actual `<input type=file>`.

#### IME Keyboards

Send keys do not engage the IME keyboards on the operating system. Selenium assumes a US Keyboard for everything. Fortunately, Send Keys does allow you to send through Unicode characters if you need to type kanji or other languages that would require an IME keyboard. You might not get any special events from the IME keyboard.

## Further Reading

You can read the [WebDriver Specification](https://w3c.github.io/webdriver/#element-send-keys) for more details on what the browser does for send keys.
