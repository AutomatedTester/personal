---
title: "Py.Test and -XDist Plugin"
publishDate: 2011-02-14
---

The other day I was working on trying to find a decent way to start parallelising our Selenium Tests within Mozilla. One thing to know is that the team I am in, WebQA, does the QA work for all of the Mozilla Web properties.

The thing that people don't probably realise is that Mozilla has a lot sites. Lots and lots of sites so we need to make sure that we run our Selenium tests really quickly. The next thing that we need to do is to make sure that we can report on all our tests. I have a QA Director that likes to have a look at the test reports every so often so metrics are very important.

So what have we gone for to make sure we get awesome parallelism and good metrics? Well we had been using Nose as our test runner because it has good test discoverability and comes with a nice Xunit reporting plugin. Nose has an awesome ability to run tests in parallel with its Processes plugin.

Unfortunately these two plugins don't work together. The XML file is created for the results but its empty which means when the CI server picks it up it thinks something is wrong and goes red. This is not ideal because "cry wolf" situations on a CI are always bad.

On Selenium we use Py.Test to run our tests because it too has really good discoverability of tests. It has a really good way to store the results into to XUnit style report that CI can use. This is obviously a good thing! The thing I personally like about Py.Test is the verbose messaging when a test fails but thats just me!

Next I went to play with moving the tests to run in Parallel. The -XDist plugin has a nice way to run tests in parallel and handles the scheduling of tests so that they run in Parallel. It then send the details, by the looks, back to Py.Test and suddenly we have some metrics.

So if you ever want to contribute Selenium tests to Mozilla, we're open source remember!!!, you will ideally need to install those 2 packages so you know it works beautifully! Keep your eyes open for details of an upcoming Test Day!