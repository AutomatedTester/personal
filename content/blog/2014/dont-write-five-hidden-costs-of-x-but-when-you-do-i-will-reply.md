---
title: Don't write "Five Hidden Costs of X" but when you do I will reply
date: 2014-03-12
---

Recently I was shown that Telerik did a "[Five Hidden Costs of Selenium](http://blogs.telerik.com/jimholmes/posts/14-03-07/five-hidden-costs-of-selenium)". I knew straight away from the title that this was purely a marketing document targeting teams with little to no automation skills to do automation. For what it is worth, if you want to do automation you should really hire the right engineers for the job.

My offence with the article is not that its wrong, there are a few items I disagree with which are documented below, but with it trying to sell snake oil or silver bullets. So let's even up the argument a bit. Note I am only comparing the WebDriver parts since if it were purely Selenium IDE vs Teleriks tool then I think it would be fair comments.

# No Build/Scheduling Server

Telerik say we don't have those items and we don't. We don't want to be working on them since there are some awesome open source products with many years worth of engineering effort in them that you can use. These are free and allow a great amount freedom of customisation. They also work really well if you have hybrid systems as part of your test. Have you seen that ThoughtWorks has open sourced Go which is a great product from people who have been doing continuous integration for nearly, if not more, than a decade. Don't want to host it yourself, because managing servers is a hidden cost in all worlds, then look at the huge amount of Continuous Integration as a Service companies out there.

# Execution Agents/Parallel Running

It says this is a 3rd party plugin which is not true. The Selenium server has a remote server system built in and if the correct arguments are passed in it can become a grid with a central hub managing which nodes are being used. This is called Selenium Grid.

The one thing, from the documentation, is that you have to host all these nodes yourself. Does it create hermetic environments to run against when scheduling? Hermetic environments is something that each core developer would want and if we can't give it then its not worth releasing. There are Infrastructure as a Service companies that WebDriver tests can be hooked up to so you don't need to maintain all the infrastructure yourself. The infrastructure costs can be quite expensive if you're a smallish team, using a service here can help. Unfortunately Telerik don't offer execute nodes as a service so you'll have to manage them yourself.

Also, its fine that nUnit doesn't support parallel execution, get your scheduling server to run a task for each browser and these tasks could be run in parallel.

# Reporting

This is best done by the continuous integration server as part of the build and test. These take the output from the tasks they have told them to execute and then report. Having this as a selling point in marketing documentation feels like it is just targeting the untrained staff.

# Multi-Browser Support

This is where you would think we would be even but Telerik is stuck to desktop browsers. WebDriver, due to its flexible transport mechanism (it's like we thought about it or something, means anyone can implement the server side and control a browser and all languages just work. We recently saw that with [Blackberry creating an implementation](http://devblog.blackberry.com/2014/02/selenium-support-in-blackberry-10/) for their devices. We have [Selendroid for Android](http://selendroid.io/) and [iOS-Driver for iOS](http://ios-driver.github.io/ios-driver/). Mobile is the future so only supporting the major desktop browsers is going to limit your future greatly.

Jim also mentioned that you would need to build a factory and teach it to get things running against multiple projects. You do have to but here is a [link](https://code.google.com/p/selenium/source/browse/dotnet/test/common/Environment/) to the Selenium projects' way of handling it. We need to run our tests in multiple environments and we do it pretty well. This is just a one time sunk cost.

# Maintenance of tests

I might be pulling a Telerik here but having tests look like the following 

![screenshot of telerik in use but with weird locators](/img/telerik-features.jpg)

Being able to code as an automation engineer is crucial. Being able to write good tests is useful too! I am biased but Mozilla has some really good examples that show good maintainable tests. Tests are trivial to write and update since they invested in a good pattern for tests. Record and playback tools have never had the ability to write maintainable tests and for them to be using meaningful API names. Also, it hampers (as in no one will take you seriously) your career calling yourself an automation engineer and only using record and playback tools.

Now beware of the snake oil that is being offered by vendors and for all that is holy... if you want to do automation don't do record and replay. I, and my peers, will not even let you past a phone screen if you don't show enough knowledge about coding and automation.

Also if Telerik was thinking straight they would be wrapping webdriver and then they would get everything that is happening in the webdriver world. Knowing your tests will always work in the browser no matter what platform (including mobile) is a huge selling point. And its standards based, feels like a no-brainer but i am obviously biased. 