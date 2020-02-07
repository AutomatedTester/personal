---
title: WebDriver Specification - Have you read it lately?
date: 2015-06-09
---

A lot of work has gone into the [WebDriver Specification](http://w3c.github.io/webdriver) this year. The methods in there have had a major make over to make them more specific in the steps that are required as well as having the relevant links. Go have a read of it and feel free to raise bugs against it, we will be updating it quite regularly. You can see all the work that is happening on [Github](https://github.com/w3c/webdriver). We do everything via [pull requests](https://github.com/w3c/webdriver/pulls) so you can read things before they land.

My team have also been working hard at making sure that our implementation is following the specification and are making some great leaps with it. I will be releasing a development version of the python bindings soon that use the httpd, like InternetExplorerDriver and ChromeDriver, to drive the browser. Currently our httpd only works against Nightly but there is a merge to Aurora happening soon when we will be sending out links for you to start playing with it all. I am actually looking forward to some of the feedback that we get about it. 