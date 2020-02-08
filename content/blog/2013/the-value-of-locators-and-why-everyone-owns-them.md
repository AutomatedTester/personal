---
title: The value of locators and why everyone "owns" them
date: 2013-02-26
--- 

The other day I got asked if I knew of a tool that would notice changes to IDs of elements and update Selenium tests accordingly because there was an incurred maintenance cost in updating these all the time because the test will fail.

The tl;dr; is there isn't a tool and I don't think there should ever be one

Why?

HTML documents are not complex things, far from it, so when we change them we should think about how this is going to impact everything that hangs off a page.

If a designer or front end engineer changes the ID of elements then they go around updating the JavaScript references to that element and then update any CSS that works with those elements!

The non-obvious thing that they tend to forget is the test automation like, Selenium, that uses that element too!

Why? Well 9/10 times its because they don't have any ownership in the process! They don't submit tests nor do they run tests so they don't see the problem.

Now you are probably going to say "We use XPath/ CSS/ Names because (insert excuse here)!"

The point is still the same, your testing process and your development process are silo'ed and you're feeling the affects of this! Just remember everyone involved has a stake in the final product and QA aren't gate keepers for the product. 