---
title: "Hello, #30DaysOfPlaywright"
date: 2021-12-01T10:22:54-05:00
draft: false
toc: true
tags: [general, playwright, 30Days]
---


## Let's Learn About Testing!

It's December 1! The new year is just over 30 days away so it's the perfect time to make a learning resolution, get started on it now - and then check it off the very first day of the New Year! So what do _you_ want to learn this year? 

I've decided to invest this month exploring a topic that fascinates me - **end-to-end testing for modern web apps**. I want to go from [learning core concepts and tools](https://developer.mozilla.org/en-US/docs/Learn/Tools_and_testing), to implementing efficient cross-browser testing strategies in my web app development workflows. And I want to do this using [Playwright](https://playwright.dev) by default.

So join me as I begin my [#30DaysOfPlaywright](../about/#my-30daysofplaywright-quickstart) learning journey - and let's explore the tools, API and best practices, one test scenario at a time! 


## What is Playwright?

[Playwright](https://playwright.dev) an [open source](https://github.com/microsoft/playwright) framework for Web Testing and Automation that enables **cross-browser testing** and **web automation** across [Chromium](https://www.chromium.org/Home), [Firefox](https://www.mozilla.org/en-US/firefox/new/) and [WebKit](https://webkit.org/), *with a single API*. 

Want to get a rapid introduction to Playwright and fundamental concepts? Watch this excellent 45-minute "Introduction to Playwright test runner" from [Andrew Lushnikov](https://twitter.com/alushnikov) of the Playwright team - and look out for a future blog post that dives into some of these fundamental concepts in more detail.

{{< youtube JjhY2aFBTTk >}}


## Why Cross-Browser Testing?

A key benefit of web apps over platform-specific apps is in their _reach_. Users can access web apps simply by discovering and accessing the related link (URL) from any modern browser, on a variety of device form factors (mobile, desktop, embedded, TV) with no added effort.

The challenge is that users expect a _consistent_ experience (user interface and interaction behaviors) across the diverse browser and device platforms, despite inherent differences in their supported capabilities.

[Cross-Browser Testing](https://developer.mozilla.org/en-US/docs/Learn/Tools_and_testing/Cross_browser_testing/Introduction) makes sure your application works as expected across a reasonable subset of modern web browsers so that the _designed user experience lives up to the reality of user expectations_. This can include testing for criteria in performance, accessibility, responsiveness and data integrity - across browsers, and across operating systems (for a given browser).


## Why Test Automation?

As app developers, we are likely familiar with writing and executing unit and integration tests on a manual basis, as part of our developer workflows. But _scaling_ the testing strategy becomes challenging with increasing test frequency (e.g., system has more feature commits or bug fixes) or test granularity (e.g., need to validate against more browser and device platforms).

Web Automation or [Automated testing](https://developer.mozilla.org/en-US/docs/Learn/Tools_and_testing/Cross_browser_testing/Automated_testing) is the process of using task runners alongside our testing tools, to speed up execution and automate developer workflows to suit the **granularity and frequency** of testing needed - in a reliable and efficient manner.

## Why Playwright?

According to the documentation, Playwright is designed to be [fast, reliable, capable](https://playwright.dev/docs/why-playwright#powerful-automation-capabilities) - and evergreen, with [iterative updates](https://www.youtube.com/channel/UC46Zj8pDH5tDosqm1gd7WTg) that flatten the learning curve. 

From a modern web app development perspective, useful features include:

 * A built-in test runner ([Playwright Test](https://playwright.dev/docs/intro)) with a rich API.
 * A ([Playwright Library](https://playwright.dev/docs/library)) option for use with 3rd party test runners.
 * With [API support for](https://playwright.dev/docs/languages) JavaScript, TypeScript, Python, Java & .NET.
 * Flexibility of use with [headless](https://en.wikipedia.org/wiki/Headless_browser) and [headed](https://playwright.dev/docs/ci#running-headed) testing modes.
 * Browser binaries available for [Linux, MacOS and Windows](https://playwright.dev/docs/library#system-requirements).
 * Device [emulation](https://playwright.dev/docs/emulation) support to validate mobile user experiences.

Over the next 30 days, we'll unpack the [Playwright API](https://playwright.dev/docs/api/class-playwright), exploring relevant tools and examples that put these features to use in a real-world testing context.


## Relevant Resources

Three resources worth bookmarking at the start of this journey:

 * The [Playwright website](https://playwright.dev/) - explore docs and examples.
 * The [Playwright repository](https://github.com/microsoft/playwright) - explore source, submit issues.
 * The [Playwright channel](https://www.youtube.com/channel/UC46Zj8pDH5tDosqm1gd7WTg/videos) - for "What's New In Playwright" updates.

 And don't forget to follow [@playwrightweb](https://twitter.com/playwrightweb) on Twitter for updates. 

## Day 0: Review
  
  * [X] Visit and bookmark [Relevant Resources](#relevant-resources)
  * [X] Star or Watch [Playwright Repo](https://github.com/microsoft/playwright)
  * [X] Watch [Introducing Playwright Test runner](https://www.youtube.com/watch?v=JjhY2aFBTTk)
  * [X] Watch [What's New In Playwright: v1.17](https://www.youtube.com/watch?v=7iyIdeoAP04)


## Day 1: Up Next

The best way to learn something is to take it for a spin with code. In the next blog post we'll dive into how you can [Get Started With Playwright](https://playwright.dev/docs/intro). At the end of that exercise, you should have:

 * Installed the Playwright Test runner
 * Written and executed your first test script
 * Explored headless and headed modes for testing
 * Explored core configuration options
 * Used fundamental concepts like assertions, fixtures and test hooks
 * Explored command-line options

Get a head start exploring that tutorial, then check back for my walkthrough and learnings in the next post.
