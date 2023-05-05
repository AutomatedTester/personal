---
title: "The Login Test"
publishDate: 2023-05-05
---

# The login test

I have been in the automation space for a very long time and have seen some really amazing ideas and some really awful ideas when it comes to automation frameworks. The one thing that has helped me along the way is figuring out if a framework can handle the “login test”.

Can it log in to your website? Simple enough… right?

Before we start I want to make sure that we all have the same concept of a Login test. It should have the follow characteristics.

- Start from HTTP url. On a stage or production environment it should handle the 302 to HTTPS.
- It should have a username and password  items. This could be done within an iframe or separate window if you’re using a 3rd party service like OKTA
- It should in the positive case it should redirect to a new landing page. In the negative case it should error and show the error that you can access.

Hopefully none of this should be controversial right?

If you’re unsure why the follow should be important to be able to handle let’s break down a high level what is happening in the browser.

## Browser Sandboxes

We all use JavaScript to be able to manipulate the page that we are on so that it can have more of a look and feel of a desktop application. Unfortunately, [bad actors](https://www.merriam-webster.com/dictionary/bad%20actor) learned very quickly they can inject their own JavaScript to do their bidding and steal as much info as possible. Thus started the cat and mouse between security folks in browser vendors and those in the wild.

With CORS, HTTPS, Same site origin policy, mixed-content all in the space, there are loads of areas where just injecting JS into the page to control it will be up against a sandbox of sorts. If you’re ever wondering why it’s hard to do single sign on as part of your testing then one of the above is likely to be blocking your framework. Also, don’t fall for “logging in is an anti-pattern” some groups push to cover up their inefficiencies in their product. Doing it on every test is an anti-pattern. Making sure it works is a normal thing to do

## Trusted Events

When doing a login page, it is important to have defence in depth. We need the server to be able to handle the login `POST` from the browser that the information is correct or if it is incorrect that it doesn’t give away too much info to allow a hack.

On the front end you need to have the usual forms and you need to make sure that have the user inputs are real. In browser nomenclature that means `event.isTrusted` will return `true`. If it returns `false` then you know the clicking or typing has come from someone calling `dispatchEvent`.

If you’re after a use case for this? Say someone creates a phishing page for your site to get logins but doesn’t want to be caught out. They could take your details and then in a hidden iframe be passing the details onto the real site, after storing your login info.

## So what? Why does this all matter?

The main reason this all matters is there are a number of popular test frameworks out there that can’t do the above. They even tell you that the following is an anti-pattern. As mentioned earlier it’s only an anti-pattern if you do it too much.

You can spot these tools by their inability to work with real browsers on desktop or mobile. If you’re not using what your users are using it’s likely to cause bugs that you’re unaware of until it’s possibly too late.

I recently discussed this at Selenium Conf. If you missed it then watch it below

{{< youtube Mo6LmFGrtxY>}}
