---
title: "Selenium Manager - The best tool from Selenium that you can forget about"
publishDate: 2023-06-07
---

# Selenium Manager - The best tool from Selenium that you can forget about

I am sure that you have heard all the buzz in the Selenium community about Selenium Manager. A tool built into Selenium that by the end of this post you can be safe in the knowledge that you can forget about it.

## So, what is it?

It is an executable that is shipped in all of the driver bindings in all the languages that the Selenium Project maintains that will make sure that you have the driver you need for the browser you are about to use even after the browser updates.

It really is that simple…

*but David, what about Chromium based browsers updating every 20 mins?*

Never fear, it will handle the error about version incompatibilities and download it again and get your tests running. It will check it’s cache every 15 minutes from the last time it did it and update as necessary.

## Why is Selenium only doing it now?

Originally Selenium used to ship drivers where it could in the bindings. Then, after many painful releases of the bindings when browsers released new versions, and after encouragement from Opera, the standard was created. We then removed the drivers from the bindings and left it up to the browser vendors because honestly… it was hard work just keeping the bindings up to date and cleaning up bugs to then also handle browser differences.

This unfortunately meant that we couldn’t control the release cycle of the drivers anymore… and since Selenium has always aimed to be a browser automation tool we just left it up to the users to do the right thing… which didn’t always work out.

The message first time users generally get is a variation on `The path to the driver executable must be set by the ...` which we all agree is a bad experience for new users.

## So why now?

Back when we were talking about Selenium 4 and beyond one of the biggest things that we, as Selenium contributors, wanted to do was to guide Selenium to try be a little bit more “Batteries are included”. The feedback that we received was that for new people Driver Management was quite hard to get right on top of a few other features.

## How’s it built?

As a prototype, I originally wrote it in Rust so that it would be it’s own executable and any project, even those outside of the Selenium Project, could ship it for their bindings and everyone would benefit. Boni Garcia then started from scratch, a wise choice, and built what is shipped today! Why Rust? It’s fast, and can cross compile and is good language for systems applications.

## But what about the other libraries out there that do this?

Well, after my prototype, Boni Garcia the creator of `webdriver-manager` for Java was hired to build Selenium Manager. It’s designed for you to move over. You can delete all references to these types of tools from your projects. E.g. If you’re using `chromedriver` and `geckodriver` as `devDependencies` in a NodeJS project you can delete them, and make sure to update `package-lock.json`.

## Ok… so how do I use it?

Um… just instantiate a driver… like you would normally. No messing around with system properties or environment variables. This is why now you know about it… you can forget about it.

```python
from selenium import webdriver

driver = webdriver.Firefox()
# or
driver = webdriver.Chrome()
```

## This sounds a little too good to be true

But… don’t just take my word for it! Watch Boni Garcia’s talk from Selenium Conference in Chicago this year!

{{< youtube 7MWuXlt6BXE >}}

And Now you can go ahead and forget about it all and just be amazed at how everything Just Works™️
