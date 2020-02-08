---
title: Do you trust a test that you have never seen fail
date: 2014-04-28
---

Recently David Heinemeier Hansson ([dhh](https://twitter.com/dhh)) wrote a blog post called "[TDD is dead, long live testing](http://david.heinemeierhansson.com/2014/tdd-is-dead-long-live-testing.html)". He describes how the TDD world has got mean spirited and perhaps the use of the technique was to break down the barriers of automated testing and regression testing but that is no longer the case. (I agree with this a little but there are a lot of angry people out there)

He then declares that he has had enough and declares that he does not write tests first and is proud of it.

This post has obviously had mixed reviews from people. A lot of the consultant types that I follow on twitter who are rather large advocates of TDD have said dhh shouldn't really be saying things like this. Their arguments have been around how hard it is to test rails applications because the underlying architecture doesn't allow it (Which distracts from the argument in my opinion!). The thing that David describes briefly is that we should not follow things dogmatically which is sound advice that everyone should follow. It's the same with best practises, don't follow them unless they make sense.

The amount of people who then started shouting from the roof tops that they don't test first and were proud of it grew quite quickly. I find this sentiment quite worrying. Why?

## How do you trust a test that you have never seen fail?

If you have hundreds of thousands of tests that are run when you commit code you want there to be a very high probability that any regression will be caught. Writing tests after you have written the code can lead to a lot of tests that may never fail which take up huge amounts of resource that costs money. At the beginning of the year Mozilla would use ~200 computing hours per push to Mozilla Inbound (the main tree that holds the Firefox code). Thats A LOT of resource to be wasting on a test that you are not sure will ever catch anything. For what it is worth there a lot of tests in the Mozilla tree that may never catch anything but its hard to find them :(.

I know that you can't always write a test first, there are times where it is quite hard to do that, but making sure you have faith in your tests so that they actually do what you expect is the most important thing you can do.

dhh does say that not everything can be unit tested and I think this is the crux of his issue. He is, and a lot of other people, are getting hung up on labels for tests in my opinion. This then puts them off writing tests first. I have been a big fan of the way that [Google labels their tests](http://googletesting.blogspot.co.uk/2011/03/how-google-tests-software-part-five.html). This removes the dogmatic beliefs in TDD and describes them in how much work they are doing. This is the way we should be doing it!!!

So if you are going to not write a test first make sure that you are writing tests that you can trust and that your colleagues can trust too! 