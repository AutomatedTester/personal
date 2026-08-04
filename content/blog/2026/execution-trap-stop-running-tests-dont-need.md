---
title: "The Execution Trap: Stop Running Tests That Don't Need Running"
publishDate: 2026-06-19
---

We've all been there. You update a small piece of text or fix a tiny CSS bug. You push the code, and then... you wait. Your CI pipeline decides that this minor tweak means it needs to run your entire end-to-end regression suite.

It's slow, it's expensive, and honestly, it's a sledgehammer to crack a nut.

If you tuned into Selenium Conference this year, you might have caught Simon Stewart's talk. He hit on a topic that every team struggling with slow deployments needs to hear: we need to stop running things just because they are there.

## The "Run Everything" Problem

Over the years, we've been conditioned to run all our tests on every commit. Why? Because we do it out of fear. We're terrified that a minor change to the homepage will somehow break the payment gateway. So, we build these massive safety nets.

But as your application scales, that safety net turns into a bottleneck. Your build starts taking 45 minutes or an hour. Developers switch context to something else. By the time a test failure actually surfaces, they've completely forgotten what they were working on.

Testing isn't about achieving absolute perfection; it's about managing risk. If your automation suite takes an hour to give feedback, it's not managing risk, it's actively slowing your team down.

## Removing the Magic: Smart Test Selection

The absolute fastest test in your suite is the one you don't run at all.

Instead of mindlessly brute-forcing the entire suite on every single PR, we need to get smarter about understanding the impact of code changes.

Here is the fundamental approach Simon discussed:

- **Understanding your dependencies**: You need to map out exactly which parts of your application each test actually touches.
- **Impact analysis**: When a developer modifies a file, your pipeline looks at what changed and traces it back to your tests.
- **Run only what matters**: If someone updates the backend API for user profiles, your pipeline knows to only queue the profile tests. It skips the frontend shopping cart tests entirely because there's no logical way they were affected.

This isn't some futuristic, AI-powered magic. It's just using your codebase's dependency graph to make informed engineering decisions rather than relying on fear.

By only running the tests that matter for a given commit, you drop your execution time from hours to minutes. You stop burning cash on unnecessary cloud infrastructure, and your developers get feedback while the code is still fresh in their minds.

Stop treating your automation suite like a blunt instrument.

If you want to understand exactly how to implement this and get out of the execution trap, I highly recommend watching Simon "Mavi" Stewart's full talk from Selenium Conf this year.
