---

title: "You wouldn’t test a car once it’s fully assembled, why do that to your app?"
publishDate: 2023-08-16
---

One of the things that I have seen over my career is how often we see organisations see quality and testing as a feature that happens just before you ship with minor tweaks.

This is very different to how cars are made. Cars, like websites, are made from components. But… can we safely say that web components we use are tested to the same level? Or even half the level that car components are?

There are a number of similarities in how cars are built and how software is built. There are humans moving things about and there are a robots doing the heavy lifting. In software, our robots are IDEs, linters, CIs, and even the ML that helps us in our development environments.

Unfortunately, the way that we develop and test software is not to the same level. I remember when I was in university I was told that software doesn’t need to be tested as rigorously as machinery that can kill people. And while that is mostly true, there is one thing that is very different is our view of what is good enough testing.

I have seen a lot of companies that feel that there should be a QA department writing tests and the “engineering” teams should be writing code. This has led to a weird situation that tertiary education does not teach students how to test. This leads students thinking that testing is beneath them which then leads to people in the industry think that testing is a second class citizen. This means that SDETs are software developers. It’s the same as when I see Front End Engineers being put down by back end engineers.

Since they are treated as second class citizens we tend to see a number of problems creep into how they develop their tests. We all know that we *should* try to follow the testing pyramid but what generally happens is we get what I call the Testing Christmas tree as seen below.

![test_christmas_tree.png](/img/test_christmas_tree.png)

So, how do we go about improving this and having fewer UI tests if that’s the problem. Well, that’s not problem per se, it’s how people are approaching the problem. We’ve had component testing for a while now and people are using it but are they doing enough or even testing the right thing? Below is a video I created recently that shows how the “best practise” doesn’t catch issues, and people are unaware until it really catches them out in production.

{{< youtube h5ct9Plqpjk>}}

So, how do we improve this?

The simple answer is to make sure that we are testing things in the right manner at the right time. There is no silver bullet. We should not be pushing to have 1 type of testing and developers need to remember that the QA team does not assure quality. They are just there to highlight your errors in your code. Quality is everyones responsibility.

Make sure that you split up your tests so that you get to a state that computers are waiting on people not people waiting on computers.
