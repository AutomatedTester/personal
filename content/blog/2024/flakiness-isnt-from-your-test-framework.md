---
title: "Flakiness isn't from your test framework"
publishDate: 2024-05-08
---

This week I saw Filip Hric share [a post](https://www.linkedin.com/feed/update/urn:li:activity:7193270830271762432/) from Gleb Bahmutov, ex-principal engineer for Cypress, explaining that the way cypress works and not using transport layers like playwright or WebDriver based frameworks makes its tests less flaky.

That’s 100% not the reason your tests are flaky. I’m not going to lie, this shocked me that Gleb would say this as I’ve always thought of him as a good engineer after seeing his work on Cypress. The reason your tests are flaky is more down to how you interpret the UI and how the browser runs the code, or to put in a different way, you’re thinking about your synchronous steps of a test while they’re running in an asynchronous way. **This mismatch leads to flakiness**.

Now add in different browsers interpreting the code in different ways leads to more flakiness. This is why some frameworks don’t want to or can’t support different browsers.

## Single threadedness of JavaScript

Cypress runs in the page that’s being tested. That means Cypress is hemmed in by the [same-origin policy](https://developer.mozilla.org/en-US/docs/Web/Security/Same-origin_policy). It injects what it needs to the page. The problem is this means CPU has to swap in commands from different tasks. This *could* lead to less flakiness but it’s because it’s running much slower. Since there is no guarantee of ordering of commands between tests and the front end, the reduced flakiness is pure chance.

Selenium, when [Jason Huggins](https://twitter.com/hugs) created it, used this technique for automating the browser and Selenium moved away from it when we merged with WebDriver. Hugs have been calling this out for ever. It’s also the reason why you can’t do basic things like trusted events, iframes, or navigating between different [origins](https://developer.mozilla.org/en-US/docs/Glossary/Origin).

{{< x user="hugs" id="1177619507927490560" >}}

Driving the browser from inside to outside, where outside is your webpage, is always going to give you a more realistic testing experience.

## Transport layers

The transport layer for speaking to the browser doesn’t affect the flakiness. Its main benefit is scalability. If you need your tests to run in the same browser as the runner then you struggle to scale. Since Selenium’s main transport system is based off HTTP we know it’s highly scalable. Cypress tests are less scalable because it wanted to do everything on the browser.

### but what about CDP?

What about it? It’s a chromium based protocol. Playwright uses it, Puppeteer uses it, and this might shock you, Selenium uses it. This is firstly how Edgedriver and chromedriver speak to the browser. So if you’ve used chromedriver at any point in the last 8+ years you’ve used CDP.  Selenium can also speaks directly to the browser using CDP for some commands. We see this with their network intercept API and logging APIs are examples. Now, webdriverio also supports WebDriver and puppeteer through these APIs. So if we follow [Gleb’s post](https://glebbahmutov.com/blog/cypress-vs-other-test-runners/), we need to move *everything* down to playwright/puppeteer part of his diagram.

The one downside to having to rely on CDP is you limited to chromium specific APIs. These are not stable by design which the Chromium team will tell you. It’s the reason why chromedriver/edgedriver needs to be updated with each browser release. Fortunately, [Selenium Manager can auto update](https://www.theautomatedtester.co.uk/blog/2023/selenium-mananger/) your drivers for you without you needing to worry. If you're not using Selenium Manager you will have to update all your dependencies which you would have to do with Playwright or puppeteer. If you’re using the Selenium event driven APIs then you will have to update your selenium dependency.

As mentioned, it does limit us to chromium specific APIs which is why Selenium is working with Google, Apple, Mozilla, and a few other little companies to bring about the new Webdriver-bidi spec. When WebDriver-BiDi is out to all browsers then the requirements for updating with the browser will drop like they did with Firefox and geckodriver.

The [puppeteer team is supporting WebDriver-BiDi](https://developer.chrome.com/blog/webdriver-bidi/). A debug protocol is not the best for automation as it relies heavily on browser state. It’s great for debugging but not automation. Since this work is happening in the open, we are happy for the playwright and cypress team to come collaborate with us.

## How do we solve flakiness then?

Auto-waiting, minimizing what your tests are doing, and root cause analysis of failures. Webdriver.io, Nightwatch, playwright, puppeteer, and cypress have the auto waiting. All at different levels. There is some opinions that differ on what should be waited for but they are aiming for the same end result.

Selenium has had auto waiting in a minimal wait with the explicit and implicit waits. People can struggle with them. These waits are opinionated like the above. So... we're down to which opinions we like. So... if you're learning a tool for a CV... well it's just an opinion that's different. How to do web testing, and understanding it, that is the super power you need.

*As an aside, when I was at Mozilla, automattic came to us saying they wanted puppeteer support as they were dropping selenium because of flakiness... and then then the flakiness was still happening with puppeteer*

Flakiness has been around for a long time. [Simon Stewart](https://twitter.com/shs96c) wrote a [good blog post about this problem 15 years ago](https://testing.googleblog.com/2009/06/my-selenium-tests-arent-stable.html) and how to solve some of them. This is not a new problem and someone telling you their framework will solve it is lying.

Unfortunately, there will still be some flakiness… and it’s not your fault.

## Front end frameworks hate testers

These frameworks make flakiness equal when it comes to testing.

Browsers have had to put a lot of effort to improving the speed of rendering and painting to handle these because of the constant moving in and out of the DOM. These asynchronous changes to the DOM  the way your tests run. In selenium you’ll hit a `StaleElementReference`. These are painful but auto waiting can solve it for you easily. Batteries included systems, like NightwatchJS, webdriverio, playwright, and cypress try make how this works opaque to the end user.

### Asynchronocity is hard

As I alluded to above, we don’t think of tests in the same way we think of rendering front end tests. When we’re wanting to take data off a server and render it, it’s all happening asynchronously due to how JavaScript and fetch APIs work. The move to server side rendering again is not going to solve this either… it’s just moving the heavy lifting around. Yes with promises we can write code that looks synchronous but that’s just for where that bit of code is. A slow response from a server can impact our tests. Having your tests and front end competing in the same thread won’t make your tests less flaky other than by sheer luck.

## Finally… work in the same world as your users

So is there any benefit to running your tests in Electron? Well… how many of your users use your site with electron? Probably the same amount as would use playwright-Firefox or playwright-WebKit. It’s a number that is very close to 0.

I’ve talked about this many times about having your tests work. It’s important to have a number of tests running in the environments where our users are. I’ve got examples in my talk from last year. Different environments, like mobile versus desktop, can also lead to different reasons for flakiness.

{{< youtube Mo6LmFGrtxY >}}

So… when picking a framework, pick one that works well for you. It should be able to do a [Login Test](https://www.theautomatedtester.co.uk/blog/2023/the-login-test/) easily. If it can’t then it shouldn’t be used.
