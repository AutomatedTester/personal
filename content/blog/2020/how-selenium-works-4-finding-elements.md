---
title: "How Selenium Works: Episode 4 - Finding Elements"
publishDate: 2020-03-10
---

After an interaction on the last weekend of January 2020, on a Selenium Issue where someone said “why can’t you just…” after I explained the issue I thought that I would start explaining commands in Selenium WebDriver and why we landed on the design that we have today.

I will repeat this on every page of the series but a lot, an annoying amount sometimes, of thinking goes into how every little bit of Selenium works. 

In this episode we are going to look at how `findElement` works. We need to be able to find elements before we can interact with them.

## Finding an element is easy right?

Well, yes if people designed their applications for testability over anything else! Unfortunately we live in a world where most of the time people think about quality and testing as an after thought.

Anyway... so let's look at the different ways we can look them up.

### Search Types

This part is simple, Selenium offers the same methods for searching as you can find when interacting with the DOM with JavaScript. If we can look up things via [`document.querySelectorAll(aQuery);`](https://developer.mozilla.org/en-US/docs/Web/API/Document/querySelectorAll) then it will be searchable via `find_element` or `find_elements`. This is searchable from the `WebDriver` object or from the `WebElement` object. If you do `element.find_element(...)` that is equalivent to doing `element.querySelectorAll(aQuery)` which sets the starting point, or the root, for searching from that element.

Below is an example of how to use `find_element`:

{{< highlight python >}}
from selenium import webdriver
from selenium.webdriver.common.by import By

driver = webdriver.Firefox()
driver.get("https://www.theautomatedtester.co.uk")
driver.find_element(By.ID, "someID")
driver.find_element(By.CSS_SELECTOR, "#someID")
driver.find_elements(By.CSS_SELECTOR, "#someID")
driver.find_element(By.NAME, "someName")
driver.find_elements(By.NAME, "someName")
driver.find_element(By.CLASS_NAME, "someName")
driver.find_elements(By.CLASS_NAME, "someName")
driver.find_element(By.TAG_NAME, "someName")
driver.find_elements(By.TAG_NAME, "someName")
{{< /highlight >}}

We can also look up elements on the page by [XPATH](https://developer.mozilla.org/en-US/docs/Web/XPath).

We can do the following

{{< highlight python >}}
from selenium import webdriver
from selenium.webdriver.common.by import By

driver = webdriver.Firefox()
driver.get("https://www.theautomatedtester.co.uk")
driver.find_element(By.XPATH, "//li")
{{< /highlight >}}

We can also look up links based on the visible text. Visible text uses the [`isDisplayed` algorithm]({{< ref "/blog/2020/how-selenium-works-3-isdiplayed">}}) to work out what people can and can't read. Selenium will collect all the `<a></a>` tags on the page, get the visible text, and then find the first element that has that text.

{{< highlight python >}}
from selenium import webdriver
from selenium.webdriver.common.by import By

driver = webdriver.Firefox()
driver.get("https://www.theautomatedtester.co.uk")
driver.find_element(By.LINK_TEXT, "some text")
driver.find_element(By.PARTIAL_LINK_TEXT, "text")
{{< /highlight >}}

## What happens when it finds the element?

When we are able to find an element we keep track of it in a Map. The Map will contain a representation of a element using [uuid](https://en.wikipedia.org/wiki/Universally_unique_identifier). The map allows us to look up elements when we get them back from a Selenium test and make sure that element is actually [connected](https://dom.spec.whatwg.org/#connected) to the DOM. If the element is not connected to the DOM and you try use it you will get a [`StaleElementReferenceException`](https://w3c.github.io/webdriver/#dfn-stale-element-reference).

## and when it doesn't find the element or elements

There are 2 things that can happen. 

If you look for only 1 element you will get an exception like:

{{< highlight python >}}
from selenium import webdriver
from selenium.common.exceptions import NoSuchElementException
from selenium.webdriver.common.by import By

driver = webdriver.Firefox()
driver.get("https://www.theautomatedtester.co.uk")
try:
    driver.find_element(By.LINK_TEXT, "some text")
except NoSuchElementException:
    pass
{{< /highlight >}}

If you are looking for more than 1 element you will get an empty list:

{{< highlight python >}}
from selenium import webdriver
from selenium.common.exceptions import NoSuchElementException
from selenium.webdriver.common.by import By

driver = webdriver.Firefox()
driver.get("https://www.theautomatedtester.co.uk")
elements = driver.find_elements(By.CSS_SELECTOR, "myShadowElements")

assert len(elements) == 0

{{< /highlight >}}


## Further reading

You can read further in the [Element Retrieval](https://w3c.github.io/webdriver/#element-retrieval) section of the [WebDriver Specification](https://w3c.github.io/webdriver/)
