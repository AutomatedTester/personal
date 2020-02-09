---
title: "How Selenium Works: Episode 2 - Navigation"
publishDate: 2020-02-13
---

After an interaction on the last weekend of January 2020, on a Selenium Issue where someone said “why can’t you just…” after I explained the issue I thought that I would start explaining commands in Selenium WebDriver and why we landed on the design that we have today.

I will repeat this on every page of the series but a lot, an annoying amount sometimes, of thinking goes into how every little bit of Selenium works. 

In this episode we are going to look into the sheer amount of work that goes into navigation. 

{{< highlight python >}}
from selenium import webdriver

driver = webdriver.Firefox()
driver.get("https://www.theautomatedtester.co.uk")
{{< /highlight >}}

We can see above... it looks easy enough right... no!

Actually, it leads me to a pet peeve in interviews. If someone ever asks you to ever describe what happens in a browser when you type a URL and press enter there is a high probability that they have no real idea what goes into navigation. Anyway... back to Selenium and how we navigate.

# `driver.get`

Once we send the request over the [transportation layer]({{< relref "/blog/how-selenium-works-transport">}}) the real fun starts. We need to figure out where we want to go and tell the browser to start going there for us. Unfortunately this will lead to the first of the many problems for automators.

## Certificates

If you have ever worked in major corporate companies where technology is not the core then you will understand the pain that a development team might have to endure. Now double that pain and you're starting to get an idea of the pain that testers and automators have to deal with.

One of these pains is certificates. Companies are cheap and will do self-signed certificates and other monstrosities. Especially in the early days of Selenium when there wasn't services like [Let's Encrypt](https://letsencrypt.org/). And even now, most Developers and QA teams rarely have access to change configurations on their test environments or on their CI servers. We needed to come up with a way to circumvent the certificates. (_This is one of the first reasons why Selenium is seen as a security risk_)

So each of the browsers will allow the poorly configured certificates through when we automate. It's because if we didn't a lot of testers/developers would never be able to test their sites. 

Now... that we go round the first problem we need to move on to getting the page to load.

## Loading

Once we have got round the certificates we get the page to load. Luckily we don't need to do anything more than the equivalent of 

{{< highlight javascript >}}
location = "https://www.theautomatedtester.co.uk"; 
// or
window.location.href = "https://www.theautomatedtester.co.uk";
{{< /highlight >}}

## Done... 

When Selenium has "finished" a command it will return. So, we just need to wait for the page to be finished loading. To be honest, what does _"finished loading"_ even mean.

In the browser it will fire a number of different events. We will know when the page has been shown and then tell you what it's [`readyState`](https://developer.mozilla.org/en-US/docs/Web/API/Document/readyState). Selenium will check all of that and it will also wait for the [`DOMContentLoaded`](https://developer.mozilla.org/en-US/docs/Web/API/Window/DOMContentLoaded_event).

Then there is the problem if you're on the page and you try navigate to an anchor on the page. Let's look at the following example.

{{< highlight python >}}
from selenium import webdriver

driver = webdriver.Firefox()
driver.get("https://www.theautomatedtester.co.uk")
driver.get("https://www.theautomatedtester.co.uk#someAnchor")
{{< /highlight >}}

Oh look, none of the page loading events are going to be fired! Yay? Stupid browser is being efficient and will just scroll to the anchor. This means that we can't solely depend on events coming out of the page.

No, so we need to have specialised code for this case and what "done" means in this case. If we get this wrong it's going to create a lot of unstable tests. Unstable tests make people grumpy and we don't want grumpy developers, there are enough of them in the world without unstable tests.

Once we have done those checks, and you can manipulate how we look at those events, if you want it to load quicker, using the [Page Load Strategies](https://w3c.github.io/webdriver/#dfn-page-loading-strategy). This is more of a power user feature so I wouldn't worry about them now but they affect how quickly the navigation command 

## What about JavaScript Frameworks and navigation

This is where there is "fun". A lot of frameworks will do a lot of loading after their initial load on to the page. If you have ever worked on a single page app, or have ever used a single page app, you will have seen a lot of items render as the data comes in. Unfortunately it does mean that you can't just rely on the navigation command returning. You will have to add a `WebDriverWait` command to your code, as shown below, to make sure your test is in the right state before going off to do what it needs to.

{{< highlight python >}}
from selenium import webdriver
from selenium.webdriver.support.wait import WebDriverWait

driver = webdriver.Firefox()
driver.get("https://www.theautomatedtester.co.uk")
element = WebDriverWait(driver, 10).until(lambda x: x.find_element_by_id(“someId”))
{{< /highlight >}}


## Conclusion

When loading a page, don't always rely on Selenium returning when the page has finished. If you need to look at an element on the page then do that. Just be aware of how the JavaScript on the page could mutate the page after the initial downloads are done.


## Further Reading

* [Navigation Specification Prose](https://w3c.github.io/webdriver/#navigate-to)
* [Page Load Strategies](https://w3c.github.io/webdriver/#dfn-table-of-page-load-strategies)