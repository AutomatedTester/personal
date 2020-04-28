---
title: "How Selenium Works: Episode 7 - Driver Executables"
publishDate: 2020-04-29
---

So this episode is going to be slightly different to previous episodes as this question came up this week. Why do browser vendors and the Selenium Project ship a driver executable? And Why do I need to update it with every browser release?

## Why do I need to download something?

This is one of the biggest pain points I have seen people struggle with when using Selenium. The majority of the people who struggle are those who do not have a lot of experience programming. So why do we download this executable?

In [Episode 1]({{< ref "/blog/how-selenium-works-transport">}}) I mentioned Selenium under the hood speaks a [REST-ish API]({{< ref "/blog/how-selenium-works-transport#http-and-rest-ish-really">}}). 

The driver you are downloading is an HTTP server for the client libraries to be able to speak to.

## So... Why not just get the client bindings to speak whatever is native to the browser?

Unfortunately, as I mentioned in [Episode 1]({{< ref "/blog/how-selenium-works-transport">}}), doing that increases the amount of work each binding maintainer needs to do and potentially limits who can write the bindings.

It also means any setup required for the browser needs to be done by the bindings. If you're using a Chromium-based browser or Firefox, the bindings will need to know where to find the browser executable normally, as well as make sure the profile directory is created properly. It also puts the onus of fixing any breaking changes the browser vendors do to their startup setups on to each of the binding maintainers.

Trust me, the Selenium Project used to do that and every browser release day meant a mad scramble. We would start running tests in an attempt to make sure nothing was broken. We would fix the problems and then release. This herculean effort by core contributors meant we were limited in adding new features.

Not having to scramble and making sure it was simple for client binding maintainers to keep up to date was a great push for creating the [WebDriver Specification](https://w3c.github.io/webdriver/).

## What about automatic downloads from the Selenium Project?

Well... This is something we have wanted to implement in the past but we haven't had time. Fortunately, there have been other projects that have come along and made this work easier. In the future, we are likely to try to incorporate one of these projects or we might look to see about implementing something ourselves. 

This will be something we will try look into once we have shipped Selenium 4.