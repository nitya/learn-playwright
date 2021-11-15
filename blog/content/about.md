---
title: "Learn Playwright"
date: 2021-10-06T16:24:44-04:00
draft: false
---

Welcome to my learning journey into end-to-end testing for modern web apps using [Playwright](https://playwright.dev/). Want to join me? Here's what you need to know:


## What is Playwright?

[Playwright](https://playwright.dev/) is an Node.js library that enables **cross-browser** **web test automation** across [Chromium](https://www.chromium.org/Home), [Firefox](https://www.mozilla.org/en-US/firefox/new/) and [WebKit](https://webkit.org/), *with a single API*. 

 * The project is [open source](https://github.com/microsoft/playwright).
 * It supports [headless](https://en.wikipedia.org/wiki/Headless_browser) and [headed](https://playwright.dev/docs/ci#running-headed) testing modes.
 * It has a built-in [test runner](https://playwright.dev/docs/intro) and [a library](https://playwright.dev/docs/library) for third-party integration.
 * The API is [available](https://playwright.dev/docs/languages) in JavaScript, TypeScript, Python, Java and .NET.
 * It browser binaries work [on Linux, MacOS and Windows](https://playwright.dev/docs/library#system-requirements).


## What is Cross-Browser Testing?

[Cross-Browser Testing](https://developer.mozilla.org/en-US/docs/Learn/Tools_and_testing/Cross_browser_testing/Introduction) is the practice of _making sure that the web sites and web apps you create work across an acceptable number of web browsers._ 

This includes checking how your app performs given the diverse browser platform capabilities (APIs), supported device types (mobile, desktop, embedded, TV) - and ensuring inclusive operation (accessibility etc.)

> Why cross-browser testing?

A key value proposition for web apps is their reach. Users can easily discover web sites and apps simply by finding, sharing or browsing links (URLs) - and they get access to the latest versions by default (no need to _update_ apps).

The challenge though is that users will access web apps from a variety of devices (mobile, desktop, embedded, television, auto) and browser platforms (Safari, Chrome, Edge, Firefox, Opera, Brave etc.) _and expect the same, consistent experience across them all_.

Cross-browser testing is our way of ensuring that our designed user experience lives up to the reality.


## What is Web Automation?

Services like Azure DevOps provide built-in support for  [exploratory and manual testing](https://docs.microsoft.com/en-us/azure/devops/test/overview?view=azure-devops#manual-testing) tools that individuals and teams can use throughout their development workflows. However, as the frequency of testing grows (e.g., with more feature commits or bug fixes), and as the granularity of testing targets increases (e.g., browser and device platforms), we need to ways to **scale** the testing strategy. 

Web Automation or [Automated testing](https://developer.mozilla.org/en-US/docs/Learn/Tools_and_testing/Cross_browser_testing/Automated_testing) uses task runners with testing tools, to speed up and automate your testing workflows at the granularity and frequency you need. Playwright has a built-in test runner ([Playwright Test](https://playwright.dev/docs/intro)) and integrates with [third party runners](https://playwright.dev/docs/test-runners/) using the [Playwright Library](https://playwright.dev/docs/library) if needed.


## Getting started with Playwright

The best resources for beginners is the [Playwright Documentation](). The [Getting Started](https://playwright.dev/docs/intro) guide walks you through the installation and your first test runner execution.

Want to get a sense for what Playwright does without installing anything? Check out [Try Playwright](https://try.playwright.tech/) - an experimental browser-based sandbox with .NET, JavaScript, Python and Java examples.

I'm also documenting my own learning journey - from general web automation (concepts) to Playwright-specific automation (practice) - on this blog. Keep an eye out for an upcoming [#30Days](../tags/30days/) series that organizes posts into a more structured month-of-learning journey.
