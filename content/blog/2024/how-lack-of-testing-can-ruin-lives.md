---
title: "How lack of testing can ruin lives - The technology problems behind the Post Office Scandal"
publishDate: 2024-01-09
---
Software testing is a critical phase in the development lifecycle that ensures the reliability and functionality of a program. However, recent scandals involving the Post Office and Fujitsu have shed light on the serious problems that can arise when testing is not conducted rigorously or ethically. In this blog post, we will delve into the issues surrounding the software testing practices in the Post Office and Fujitsu scandal, and the broader implications for the software development industry.

## The Post Office and Fujitsu Scandal

The scandal involving the Post Office and Fujitsu revolves around the Horizon IT system, which was introduced in 1999 to manage financial transactions across thousands of post offices in the United Kingdom. The system, developed by Fujitsu, was intended to streamline operations and enhance efficiency. However, over the years, numerous postmasters were accused of financial discrepancies and even faced legal actions, leading to ruined reputations and financial ruin for over 800 people.

It was later revealed that software glitches within the Horizon system were responsible for the inaccuracies in financial records. Shockingly, despite the evidence of software errors, the Post Office and Fujitsu continued to deny any wrongdoing for years. This highlights a significant problem in the software testing process – the lack of transparency and accountability.

Since a lot of this story happened over 2 decades we have to think about how testing, and even continuous integration was just a new space when the HorizonIT software was initially being built.  Computer Weekly was the original group to publish the [story](https://www.computerweekly.com/news/366538096/Post-Office-scandal-cover-up-a-dark-chapter-in-government-corporate-and-legal-history).

## Implications for the Software Development Industry

The Post Office and Fujitsu scandal serve as a wake-up call for the software development industry. It emphasizes the importance of rigorous and independent testing to ensure the integrity of software systems. It’s really simple yet people still struggle with this. Do the following:

- Set up testing from the start. Ideally this should be done with TDD since if you have never seen the test fail, how can you truly trust it.
- Follow the testing pyramid as much as possible and remember quality is everyones job
- Set up continuous integration systems
- Setup telemetry systems. [OpenTelemetry](https://opentelemetry.io/) is a good choice here, and you can even get [Selenium Server to use it](https://www.selenium.dev/blog/2021/selenium-4-observability/).

The Post Office and Fujitsu scandal serves as a stark reminder of the consequences of inadequate software testing. It highlights the need for a fundamental shift in the industry's approach to testing – one that prioritizes independence, transparency, and comprehensive evaluation. By learning from these mistakes and implementing robust testing practices, the software development industry can regain trust and ensure the delivery of reliable and secure software solutions.
