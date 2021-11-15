---
title: "Learn Playwright"
date: 2021-10-06T16:24:44-04:00
draft: false
---

Welcome to my learning journey into end-to-end testing for modern web apps using [Playwright](https://playwright.dev/). Want to join me - here's what you need to know:


## What is Playwright?

[Playwright](https://playwright.dev/) is an Node.js library that enables **cross-browser** **web test automation** across [Chromium](https://www.chromium.org/Home), [Firefox](https://www.mozilla.org/en-US/firefox/new/) and [WebKit](https://webkit.org/), *with a single API*. 

 * The project is [open source](https://github.com/microsoft/playwright).
 * It supports [headless](https://en.wikipedia.org/wiki/Headless_browser) mode across all browsers.
 * It runs on Linux, MacOS and Windows platforms.
 * The API is [available](https://playwright.dev/docs/languages) in JavaScript, TypeScript, Python, Java and .NET.


## What is Cross-Browser Testing?

_A key benefit to developing web apps is their reach._ Users can discover them easily (shared links or URLs) and get access to the latest versions by default (no need to update apps). 

_This also means web apps need to support diverse platforms_. Users can access web apps from different browsers (Safari, Opera, Chrome, Edge, Firefox, Brave etc.) on different device form factors (mobile, desktop, embedded, TV) -- and expect the a rich and consistent experience.

[Cross-Browser Testing](https://developer.mozilla.org/en-US/docs/Learn/Tools_and_testing/Cross_browser_testing/Introduction) is _the practice of making sure that the web sites and web apps you create work across an acceptable number of web browsers._ This includes checking how your app performs given diverse browser platform capabilities (APIs), supported device types (mobile, desktop, embedded, TV) and inclusive operation (accessibility etc.)

## What is Web Automation?

Running tests **manually** across the diverse browser platforms and testing conditions is not a scalable or effective approach - especially since tests will need to be run repeatedly, to check status with new feature releases or bug fixes.

Web Automation or [Automated testing](https://developer.mozilla.org/en-US/docs/Learn/Tools_and_testing/Cross_browser_testing/Automated_testing) uses tasks runners with relevant testing tools, to speed up and automate your testing workflows at the granularity and frequency you desire. Playwright provides its own built-in test runner (called [Playwright Test](https://playwright.dev/docs/intro)) and integrates easily with [popular third party runners](https://playwright.dev/docs/test-runners/) using the [Playwright Library](https://playwright.dev/docs/library).


## Getting started with Playwright

Check out the [Getting Started](https://playwright.dev/docs/intro/) section of the documentation for now. I'll update this section with my own posts and other resources that I found helpful, as I explore this further - keep an eye on the [#30Days](../tags/30days/) tag for more.
