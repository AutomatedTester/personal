---

title: "Accessibility Testing is important and easy to do!"
publishDate: 2023-11-02
---

In today's digital age, creating websites and web applications that are accessible to all is not just a best practice; it's a legal requirement in many regions. Accessibility, often denoted as "a11y", ensures that websites are usable by individuals with disabilities. As an aside, a11y is short for "accessibility" since it starts with `a` and ends in `y` and the word is 11 characters long.

To achieve this, automated testing tools like Nightwatch.js can be invaluable. In this blog post, we'll explore how Nightwatch.js can be used to perform accessibility testing against a website, helping you make your web content inclusive for everyone. In the [EU there are laws](https://en.wikipedia.org/wiki/European_Accessibility_Act) coming into affect in 2025 that makes companies liable for not having an accessible website or application.

## Accessibility Testing is easy

Having been working on Nightwatch for a while we wanted to make sure that Accessibility testing was:

- First class citizen
- As easy as you can make it!

If you don’t have Nightwatch installed you can do that by doing the following:

```jsx
npm init nightwatch@latest
```

Once you had answered the questions then you can then start writing a11y tests which look like this.

```jsx
browser.axeInject()
  .axeRun('swap with a locator for something on the page')

```

You can do this in a normal test like

```jsx
it('Nightwatch website page has accessible headers', function(browser) {
    browser.axeInject()
      .axeRun('body');
});
```

or if you are doing component testing it would look like

```jsx
export const A11yCheckBoxes = Object.assign(
    () => <CheckBoxes name='A11y' />, {
    test: async (browser, { component }) => {
        browser.axeInject()
            .axeRun('div')
    }
})
```

You can see it in action

{{< youtube Iv_QYguxYDQ >}}

## Analyzing the Results

Once the test runs, Nightwatch.js generates a detailed report highlighting accessibility issues, including WCAG (Web Content Accessibility Guidelines) violations and suggestions for improvements.

It's crucial to review and address these issues, ensuring that your website complies with accessibility standards. Common issues may include missing alt text on images, improper use of ARIA attributes, and keyboard navigation problems.

## Conclusion

Using NightwatchJS for accessibility testing is a powerful way to ensure your website is inclusive for all users with little to no effort. By automating the process, you can catch and address accessibility issues early in your development cycle, saving time, resources, and ensuring that your web content is compliant with accessibility standards. Accessibility isn't just a good practice; it's a fundamental aspect of a user-friendly and legally compliant website.
