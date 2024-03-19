---
title: "The Pitfalls of Code Coverage."
publishDate: 2024-03-19
---

t sIn software development, metrics play a crucial role in pretty much everything. These can be from how people are using our products to how engineers build and test their applications.

One such metric that often garners attention is code coverage. While code coverage can provide insights into the extent to which a codebase has been tested. Unfortunately, and what feels like an unpopular opinion, it is a metric that is relied on a little too much as it doesn’t offer much help in terms of quality of your applications.

In this post, we'll delve into the pitfalls of code coverage and why it should be approached with caution.

## False Sense of Security

Code coverage measures the percentage of code lines or branches that are executed by tests. I’ve seen so many places setting that the minimum amount of lines that should be covered is 80%. However, high code coverage doesn't necessarily equate to high quality testing. Developers may inadvertently write tests that only cover the surface-level functionalities, leaving critical edge cases untested. This can lead to a false sense of security, where developers believe their code is well-tested when, in reality, it's not.

## Quality Over Quantity

Focusing solely on achieving a high code coverage percentage can incentivise developers to write tests that merely increase coverage without necessarily improving the quality of testing. Writing tests that are meaningful, targeted, and thorough is paramount for ensuring the reliability of software. Code coverage, therefore, should not be the ultimate goal but rather a tool to complement other testing strategies. Ideally code reviews or pair programming should hopefully limit this as having another person review them gives another perspective.

## Blind Spots

Code coverage metrics often overlook certain aspects of code quality, such as error handling, performance, and security vulnerabilities. A codebase with high coverage may still contain critical flaws that are not captured by tests as developers could be writing tests to specifically increase coverage without adequately verifying the correctness of the code. My favourite example of this is where someone [wrote the most inaccessible site and got 100% score on Lighthouse](https://www.matuzo.at/blog/building-the-most-inaccessible-site-possible-with-a-perfect-lighthouse-score/)

## Maintenance Challenges

As software evolves, maintaining high code coverage becomes increasingly challenging. Refactoring code, adding new features, or making architectural changes can impact existing tests and require significant effort to update them accordingly. Focusing solely on maintaining a high code coverage percentage can hinder the development process.

In conclusion, while code coverage can be a valuable tool for assessing test completeness, it's essential to recognize its limitations and use it accordingly. Diversity of a team can lead to a more holistic approach to testing, encompassing various techniques such as unit testing, integration testing, and exploratory testing, is necessary for ensuring software quality. By prioritising meaningful tests, focusing on quality over quantity, and considering the broader context of development, teams can mitigate the dangers associated with code coverage as a metric.
