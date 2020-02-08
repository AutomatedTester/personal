---
title: My ideal build, test, and land world
date: 2014-06-05
---

The other week I tweeted I was noticing for that day we had 1 revert push to Mozilla Inbound in every 10 pushes. For those that don't know, Mozilla Inbound is the most active integration repository that Firefox code lands in. A push can contain a number of commits depending on the bug or if a sheriff is handling checkin-needed bugs.

This tweet got replies like, and I am paraphrasing, "That's not too bad", "I expected it to be worse". Personally I think this is awful rate. Why? On that day, only 80% of pushes were code changes to the tree. The bad push and the revert leads to no changes to the tree but still uses our build and test infrastructure. This mean that at best we can(on that day) only be 80% efficient. So how can we fix this?

Note: A lot of this work is already in hand but I want to document where I wish them to go. A lot of the issues are really paper cuts but it can be death by 1000 paper cuts.

## Building

Mach, the current CLI dispatch tool at Mozilla, passes the build detail to the build scripts. It is a great tool if you haven't been using it yet. The work the build peers have done with this is pretty amazing. However I do wish the build targets passed to Mach and then executed were aligned with the way that Chromium, Facebook, Twitter and Google build targets worked.

For example, working on Marionette, if I want run marionette tests I would do |./mach //testing/marionette:test| instead of the current |./mach marionette-test| . By passing in the directory we are declaring what we want explicitly to be built and test.. The moz.build file should have a dependency saying that we need a firefox binary (or apk or b2g). The test task in the moz.build in testing/marionette folder would pick runtests.py and then pass in the necessary arguments ideally based on items in the MozConfig. Knowing the relevant arguments based on the build is hard work involving looking at your history or at a wiki.

Working on something where it has unit tests and mochitests or xpcshell tests? It's simple to just change the task. E.g. ./mach //testing/marionette:test changes quickly to //testing/marionette:xpcshell. Again, not worrying about arguments when we can create sane defaults based on what we just built. I have used testing in my examples because it is simple to show different build targets on the same call.

The other reason declaring the path (and mentioning the dependencies in the same manner) is that if you call |./mach //testing/marionette:test| after updating your repo it will do an incremental build (or a clobber if needed) without you knowing you needed it. Manually clearing things or running builds just to run tests is just busy work again.

## Reviews and Precommit builds/tests

Want a review? You currently either have to use bzexport or manually create a diff and upload it to the bug and set the reviewer. The Bugzilla team are working to stand up review board that would allow us to upload patches and has a gives us a better review tool.

The missing pieces for me are: 1)We have to manually pick a reviewer and 2) that we don't have a pre commit build and test step.

1) I have been using Opera's Critic for reviewing Web Platform Tests. Having the ability to assign people to review changes for a directory means that reviewing is everyones responsibility. Currently Bugzilla allows you to pick a reviewer based on the component that the bug is on. Sometimes a patch may span other areas and you then need to figure out who a reviewer should be. I think that we can do better here.

As for 2) don't necessarily need to do everything but the equivalent of a T-Style run would suffice I'm my opinion. We could even work to pair this down more to be literally a handful of tests that regularly catch bugs or make it run tests based on where the patch was landing.

Why does this matter, we have try that people can use and "my code works" and "it was reviewed, it will compile". Mozilla Inbound was closed for a total of 2 days (48+ hours) in April and 1 day (24hrs) in May. At the moment I only have the data on why the tree was closed, not the individual bugs that caused the failure, but a pre commit step would definitely limit the damage. The pre commit step might also catch some of the test failures (if we had test suites we could agree on for being the smoke test suite) which had Mozilla Inbound closed for over 2 1/2 (61+ Hours) days in April and over 3 days (72+ hours) in May.

## Landing code

Once the review has passed we still have to manually push the code or set a keyword in the bug (checkin-needed) so that the sheriffs can land it. This manual step is just busy work really that isn't really needed. If something has a r+ then ideally we should be queueing this up to be landed. This is minor compared to manual step required to update the bug with the SHA when it has landed, when it is landed we should be updating the bug accordingly. It's not really that hard to do.

Unneeded manual steps have an impact on engineering productivity which have a financial cost that could be avoided.

I think the main reason why a lot of these issues have never been surfaced is there is not enough data to show the issues. I have created a dashboard, only has the items I care about currently but could easily be expanded if people wanted to see other bits of information. The way we can solve the issues above is being able to show the issues. 