---
title: The tale of Selenium bug 141
date: 2013-06-26
---

The selenium project has for a while now wanted to create a library that allows user emulation in the browser. The project has done a reasonable job at this so far with respect to this. We check if items are in the DOM, we check the visibility of items and we normalise text from the browser, amongst our other amazing talents!

Our default position is we enforce the idea of emulation. "How would a user do this action?" is the main question we ask ourselves! So when a bug that doesn't meet that we tend not to implement it. One of these times is bug 141 - WebDriver lacks HTTP response header and status code methods. This is one of the most contentious bugs in the Selenium repository. People see Selenium as more than browser automation library, they see it as a web testing framework. Yes, Selenium RC was like this but then again Selenium RC also had ~140 methods hung off one object. We made a mistake in the past, let's try not to repeat it! Anyway, I digress, in a web testing framework world there may be times where there are use cases where you need to find out data from the response header and status codes.

So is this a feature we want to add to WebDriver? No. "But David, we do!?!?!?" I hear you cry! The use case that comes up regularly is I want to know if a server is returning a 404 or 500 when I access a page. Why is there a need to start a browser to find this out? When I asked this question on twitter, my friend and fellow committer Kevin Menard said that it could be useful to to see the page response on failure and some pages can only be accessed with a real browser. The use cases that Kevin had is you want to check that the endpoint that you originally hit might might change based on the user agent passed in or there might be a site that follows the single page idea and breaks push state and therefore most of the web semantics. I admit Kevin is right with those use cases. However its only looking at a small part of the issue and there are a lot of corner cases. The major one for me is watching XHRs. What if one of your XHR returns a 500 and you get an error? Should WebDriver show you that information as well? I am sure you can see how quickly this can escalate. If you start worrying about all requests, and you should if you are worried about the initial request to a page, then you want the browser to capture all of this information and return it in a standard format. There just happens to be a format called Http Archive (HAR).

So does the project take the task of creating a mechanism for extracting a HAR from the browser? That could be near impossible on browsers that don't readily share network usage information. So we go from WebDriver being a browser automation framework for user emulation to browser automation framework for user emulation and networking information gathering. That might be a little obtuse but its meant to be.

While Selenium's main use case is for test automation, is not solely designed for that. We want to be the best at browser automation. Proxies are really good at managing network information and reporting on it in standard formats. Examples are fiddler and browsermob-proxy. And you can even hook them into Selenium WebDriver! Like the following:
{{< highlight python >}}
from browsermobproxy import Server
server = Server("path/to/browsermob-proxy")
server.start()
proxy = server.create_proxy()

from selenium import webdriver
profile  = webdriver.FirefoxProfile()
profile.set_proxy(proxy.selenium_proxy())
driver = webdriver.Firefox(firefox_profile=profile)


proxy.new_har("google")
driver.get("http://www.google.co.uk")
proxy.har # returns a HAR JSON blob

server.stop()
driver.quit()

{{< /highlight >}}

Yes it might be a little verbose to use them but if you really want to see that type of information then you should do it properly otherwise the real errors, that you originally wanted, will go missing.

Putting in half a solution into Selenium is just going to create scope creep and a large number of bugs because its not doing the "right" thing. The "right" thing is far too subjective for it to be done correctly. 