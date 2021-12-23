---
title: "#006 - Tool Talk: Hello, Codegen"
date: 2021-12-22T17:48:15-05:00
draft: false
toc: true
tags: [30Days, tooling, codegen, playwright ]
---

> Welcome to **Day 6** of [#30DaysOfPlaywright](https://dev.to/nitya/series/15755)!! 

![Banner Image: 10 things to know about the CLI](https://nitya.github.io/learn-playwright/images/006-banner.png)


---

## Today's Resources

* [Command Line](https://playwright.dev/docs/cli) - Playwright CLI commands and options
* [Test Generator](https://playwright.dev/docs/codegen) - Generate test scripts from user actions
* [Recording Videos](https://playwright.dev/docs/videos) - Capture video walkthrough for post-mortem
* [Capturing Screenshots](https://playwright.dev/docs/screenshots) - Capture page screenshot for post-mortem
* [Emulating Devices](https://playwright.dev/docs/test-configuration#emulation) - Test for mobile web, browser contexts
* [Test Fixtures](https://playwright.dev/docs/intro#using-test-fixtures) - Create isolated browser environment for test
* [Locators](https://playwright.dev/docs/api/class-locator) - View to selector-matched elements in page
* [Auto-Wait](https://playwright.dev/docs/navigations#wait-for-element) - Make navigation more reliable (without timeout)

## Today's Objectives

In our [last post](https://nitya.github.io/learn-playwright/005-command-line/) we continued the series of "Tool Talk" posts by looking at the Playwright [Command Line](https://playwright.dev/docs/cli) tools and options. 

Today we'll do a deep-dive into one key tool you can launch from CLI: [`codegen`](https://playwright.dev/docs/codegen), the _test authoring_ tool that allows you to generate test scripts directly from user interactions on a page! We'll learn:

 * What `codegen` does and what command-line options we can use with it.
 * Why it is useful to experienced and beginner testers alike.
 * How to use `codegen` to generate a test script for a user workflow.
 * How to run the generated test script - and ways to customize its usage.
 * Resources to explore more on your own.

Let's dive in!

---

## 1. What is `codegen`?

Playwright's [codegen](https://playwright.dev/docs/codegen) tool helps you author tests _out of the box_ without you having to write the script manually. Instead, Playwright generates the test script code based on user interactions with the page. 

Here's a sneak peek at my first attempt to use this to generate a test script for my [demo app](https://bit.ly/recipes-for-aj). We'll dive into details in just a bit.

{{< twitter 1470444215515725836 >}}



## 2. Why is `codegen` useful?

With one command, developers can _launch_ a browser, _open_ to a specific page, and _record_ user actions with it in a way that automatically _generates test scripts_ for that workflow. 

This provides a number of benefits:

 * **Time Efficiency**. _Auto-generated scripts_ provide a solid foundation to build upon, saving time a tester would otherwise need to hand-craft tests for each action.
 * **Code Efficiency**. Codegen attempts to find _the most resilient text-based selectors_ - those that remain reliable even with changes in implementation of tested features.
 * **Customizability**. Once the base script is generated, it's easy to adapt it to use other Playwright features like [recording sessions](https://playwright.dev/docs/videos), [capturing screenshots](https://playwright.dev/docs/screenshots) or [emulating environments](https://playwright.dev/docs/test-configuration#emulation) - for test debugging & coverage.

As a beginner, there's one added benefit - _learning by example_! Want to know how to write _reliable_ tests or workflows for a given sequence of user interactions? Auto-generate the test scripts and then analyze the output to get a sense of how Playwright APIs and features can be put to use in practice.

## 3. How do I use `codegen`?

Let's look at the `--help` pages for `codegen` - review the [last post](https://nitya.github.io/learn-playwright/005-command-line/#7-playwright-cli-review-options) to see what each option represents and how it can be used.

{{< gist nitya 44799d683e74c3e5bb14ce56c6de9311 >}}

Based on the above, we know the _codegen_ command-line tool can do the following - we'll try some of these out in practice next.
 * save generated scripts to a file.
 * record/save session traces (for post-mortem analysis).
 * pick a language for script (JS, Python, C#, Test).
 * load/store state (e.g., cookies, tokens) for reuse.
 * specify a browser (or distribution) for test.
 * emulate mobile browsers (with device descriptors).
 * emulate context (geo, timezone, lang, color-scheme).


## 4. Demo: Let's Codegen!

Let's set the stage for this exercise! I have a [recipes app](https://bit.ly/recipes-for-aj) and I want to test an interaction workflow where a user:
 * visits the [website](https://green-stone-0ef96ef10.azurestaticapps.net/)
 * searches for recipe: term "appetizer" (with 0 results). 
 * searches for recipe: term "drinks" (with at least 1 result).
 * clicks on first result ("view recipe")
 * scrolls through page, clicks print icon (new page)
 * closes printable page (returns to recipe page)
 * clicks home

Let's use `codegen` to create the test script and save to file with this command:
```
$ npx playwright codegen --output recipes-search.spec.js https://bit.ly/recipes-for-aj
```

You can see the video walkthrough below:

{{< youtube XX2aSSHXuJw >}}

Below is a screenshot from that demo that highlights three things:
 * The target website is rendered in the default chrome browser (left)
 * The actions are recorded in a Playwright Inspector window (right)
 * The last user action (click in search bar) is highlighted (left) - showing the _selector_ used by codegen to create the corresponding test action (right)

{{<figure src="https://nitya.github.io/learn-playwright/images/006-codegen-demo.png" title="Screenshot of Playwright `codegen` demo run" >}}

Notice that you can hover over any element on the page (under _codegen_) to see the relevant _selector_ that Playwright would use, for targeting actions on it.

## 5. Code: Let's Review!

Here's a copy of the test script generated by that walkthrough. Try running the script yourself and see if you get a different output!

{{< gist nitya c71e6e6fb13bd67a78e8a13546c5f7bd >}}

Here are a few concepts shown, that are worth understanding:

 * **Test Fixtures:** (Line 3) `{{ page }}` is a [test fixture](https://playwright.dev/docs/intro#using-test-fixtures) that provides an isolated page context for this test run. If the test script is used with a configuration file defining multiple projects we can run them independently (in parallel) for efficiency, without needing browser restarts.
 * **Auto-wait:** (Line 6) navigating to a URL (with `goto`) will [_auto-wait_]((https://playwright.dev/docs/navigations#wait-for-element)) for the page to fire the `load` event before proceeding (removing the need to guess-timate and set timeouts) making test workflows more reliable.
 * **Locators:** (Line 9) `page.click('[placeholder=.."]')` illustrates [locators](https://playwright.dev/docs/api/class-locator) - representing a view to _selector_-matching elements on the page, on which operations (like _click_) can be performed. Locators are strict - operations will throw an error if more than one element matches the defined selector.
 * **Handling Popups:** (Line 31) shows how to [handle new page opens](https://playwright.dev/docs/pages#handling-popups) - e.g., from `target="_blank"` links. These return a new page reference that can be used to monitor events and handle actions related to that page.

## 6: Execute: Run the Test

Now that we have a generated test script, how do we use it? Let's try using the generated `recipes-search.spec.js` file as follows:

```js
$ npx playwright test recipes-search.spec.js --browser=chromium
Error: recipes-search.spec.js: JavaScript files must end with .mjs to use import.
```

Hmm .. that gave me a file naming error - let's rename the script file to use a `.mjs` extension and try this again - and let's throw in the `--debug` flag so we can launch the Playwright Inspector and `step through` the script interactively.

```js
$ npx playwright test recipes-search.spec.mjs --browser=chromium --debug     

Running 1 test using 1 worker

     [chromium] › recipes-search.spec.mjs:3:1 › test
```

**SUCCESS!!** You should notice that the script execution steps through the same sequence of user actions you had previously done, to generate that script. Now, you can automate this test with different options (e.g., browser types, device types, browser parameters etc.)

## 7: Demo: Different languages

Let's try adapting the `codegen` command to create the test script in a different language (`python`). Try running this command yourself - see how the output differs from the one before. Here's a peek at my [generated Python test script](https://gist.github.com/nitya/3885645ac80242775a924f3a20436136) for this demo if useful.

```
$ npx playwright codegen --output recipes-search.spec.py --target=python https://bit.ly/recipes-for-aj
```

## 8: Demo: Emulate devices

What happens if the user is visiting my site from a mobile browser? How does this alter the test script generation?. Let's use `codegen` with an emulated device ("Pixel 5") targeting a mobile Chrome browser - and compare outputs. Try this command out yourself:

```
$npx playwright codegen --output recipes-search.spec.mjs --device "Pixel 5"  https://bit.ly/recipes-for-aj
```

Here's a screenshot of what that looks like - note that the user experience is already different, requiring an additional click to open the menu that displays the search bar, before it can be populated with the desired search term.

{{<figure src="https://nitya.github.io/learn-playwright/images/006-codegen-pixel5.png" title="Screenshot of Playwright `codegen` demo using Pixel 5 emulated device" >}}

## 9: Demo: Customize it!

Once you have the basic script created, you have two ways to build on that script for a richer testing experience:

 * **Functionality** | Edit the generated test file to add/modify tests - it helps to know the [Playwright API](https://playwright.dev/docs/api/class-playwright) and features.
 
  * **Consistency** | Use [Configuration files](https://playwright.dev/docs/test-configuration) to create multiple projects (diverse targets, fixtures) and test in parallel.

## 10: What's Next?

In the previous post, we identified these key CLI commands for deep dives - we've completed two of them so far, so let's keep going with the _Tool Talk_ series and explore one more: Test Reporters! Until the next time! (_exit, stage left_)

 * `show-trace` - explore [Trace Viewer](https://playwright.dev/docs/trace-viewer) | previously [done](https://nitya.github.io/learn-playwright/004-trace-viewer/) ✅
 * `codegen` - explore [Test Generator](https://playwright.dev/docs/codegen) | done today ✅
 * `show-report` - explore [Test Reporters](https://playwright.dev/docs/test-reporters) 
 * `test` - explore [Playwright Test](https://playwright.dev/docs/test-cli) 
 * `test --debug` - explore [Playwright Inspector](https://playwright.dev/docs/inspector) + [Debugging Tools](https://playwright.dev/docs/debug)