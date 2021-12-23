---
title: "#005 - Tool Talk: Hello, Command Line"
date: 2021-12-20T09:56:45-05:00
draft: false
toc: true
tags: [30Days, tooling, cli, playwright ]
---

> Welcome to **Day 5** of [#30DaysOfPlaywright](https://dev.to/nitya/series/15755)!! 

![Banner Image: 10 things to know about the CLI](https://nitya.github.io/learn-playwright/images/005-banner.png)

I'm back after a short break - let's continue the #30DaysOfPlaywright journey!!

## 1. 📣 #Playwright on dev.to

I've been publishing the [#30DaysofPlaywright](https://dev.to/nitya/series/15755) series on the dev.to platform. And I'm excited to say that I am now also moderating the [#playwright tag](https://dev.to/t/playwright) there. Follow it for new posts from me and the community! 

{{< twitter 1471223653383782400 >}}

_And I have an ask for you!!_

Are you a beginner learning Playwright? Or an experienced tester with insights into how Playwright can be used with existing web frameworks or application scenarios? If so, do consider [writing a dev.to post](https://dev.to/new) and tagging it with `playwright`! I'm monitoring that tag and want to read / amplify your posts!

## 2. Today's Goals

In my [last post](https://nitya.github.io/learn-playwright/004-trace-viewer/) I continued my learning journey by exploring [Trace Viewer](https://playwright.dev/docs/trace-viewer) - a Playwright tool for _post-mortem analysis_ of tracing-enabled test runs.

Today, I want to step back and look at the full spectrum of [Command Line](https://playwright.dev/docs/cli) tools and options available to us for authoring, debugging, and analyzing, our testing runs. Some of these capabilities can also be configured and used from [the Playwright API](https://playwright.dev/docs/api/class-playwright) - something we'll explore in future posts.

For today, I'll focus on three things:
 * CLI _commands_ - what tools can we launch from command line?
 * CLI _options_ - how can we customize tool behaviors for our needs?
 * CLI _examples_ - let's see how these work with practical use cases


## 3. A Visual Cheatsheet

You can browse the [Command Line Tools](https://playwright.dev/docs/cli) documentation in depth - but if you're a visual-spatial learner like me, you might benefit from seeing a big picture before diving into the details. So here's my visual cheatsheet to the various tools and options provided by Playwright CLI. 

I created this from the `--help` screens for Playwright `v1.17.1`. The center yellow rectangle is the starting point for exploration, with arrows pointing to deep-dives into individual commands. Run the highlighted command (in each area) in your terminal to get updated usage notes for _your_ installed Playwright version.

Consider [downloading the hi-res image](https://cloud-skills.dev/gallery/Playwright-01-CommandLine.png) and using it - as a printout or as desktop wallpaper - to give yourself a handy reference to guide our CLI exploration!

![Visual Guide to Playwright Command Line](https://cloud-skills.dev/gallery/Playwright-01-CommandLine.png)


## 4. Playwright CLI: Usage

The [Command Line Tools](https://playwright.dev/docs/cli) documentation page has a great reference with examples, but the best way explore the CLI is interactively. Try this:

```
// Check your installed version of Playwright
$  npx playwright -V
Version 1.17.1

// Get help on Playwright CLI commands available
$npx playwright --help
Usage: npx playwright [options] [command]
...
<truncated for clarity>

// Get help on usage + options for specific CLI command
// Ex: for `open` command
$ npx playwright open --help
Usage: npx playwright open [options] [url]

open page in browser specified via -b, --browser
Options:
...
<truncated for clarity>
```

Check out the visual cheatsheet to see the `--help` outputs for all commands to get a sense of the extent of supported options for each.

## 5. Playwright CLI: Basic Commands

For convenience, here is a simplified table of the main Playwright CLI commands available now (v1.17.1) with links to relevant documentation pages. 

| Commands | Description |
|:---|:---|
| `install` | **installs browsers** needed for this Playwright version  |
| `install-deps` | **installs system dependencies** for supported browsers |
| `open` | **opens the specified page in the default browser** (_cr_ = chromium) |
| `cr` = `open` in chromium <br/> `fr` = `open` in firefox <br/> `wk` = `open` in webkit | **opens the specified page in the specified browser** <br/> (same `open` options) |
| `screenshot` | opens specified page, then <br/> **captures page screenshot**  |
| `pdf` | opens specified page, then <br/> **saves page as pdf** |
| `codegen` | opens specified page, then <br/> **launches [Test Generator](https://playwright.dev/docs/codegen)**, creating test code (script) from user actions. |
| `test` | **launches [Playwright Test runner](https://playwright.dev/docs/test-cli)** using default configuration file (CLI options take priority, if specified) |
| `show-trace` | **launches [Trace Viewer](https://playwright.dev/docs/trace-viewer) (PWA)** for interactive analysis of _trace zipfiles_. |
| `show-report` | **launches [HTML Reporter](https://playwright.dev/docs/test-reporters#html-reporter) webpage** on local server, for test run analysis. | 

The final four commands in this list are likely to find the most use - so I'll explore each in a separate post. Ex: check out the [Trace Viewer](https://nitya.github.io/learn-playwright/004-trace-viewer/) post to learn about `show-trace` and it's usage for post-mortem analysis of test runs.

## 6. Playwright CLI: Try Examples

Let's do a quick run through of a subset of the [command-line examples](https://playwright.dev/docs/cli#open-pages) and use these to understand the Playwright CLI options with a real world context. In the video below, I walk through the examples using [this script](https://nitya.github.io/learn-playwright/files/005-examples-script.txt) - try it out yourself in your local environment.

{{< youtube bWYWyUiQykI >}}

## 7. Playwright CLI: Review Options

For convenience, here's a recap of key options defined for use in the Playwright CLI. Use `--help` with a specific command (or refer to the [visual cheatsheet](https://cloud-skills.dev/gallery/Playwright-01-CommandLine.png)), to learn which options are applicable to each command. 

| Option | Description |
|:--- | :--- |
| `--browser <browserType>` | use a specific browser (options are cr=chromium, ff=firefox, wk=webkit) with default=cr |
| `--channel <distribution>` | for chromium-based browsers - see [channel options](https://playwright.dev/docs/api/class-testoptions#test-options-channel) for chrome, msedge|
| `--device <name>` <br/> `--user-agent <ua-string>` <br/> `--viewport-size <size>`| emulate mobile browser contexts using [supported parameters](https://playwright.dev/docs/emulation) -- see [device descriptors](https://github.com/microsoft/playwright/blob/16ae0af0fd2932efc9cc0e12e86a1b209be62da7/packages/playwright-core/src/server/deviceDescriptorsSource.json) for valid values.|
| `--timezone <time-zone>`<br/> `--lang <lang>`<br/> `--geolocation <lat,long>`<br/>| emulate browser context for [these parameters](https://playwright.dev/docs/emulation) - see [Locale/Timezone](https://playwright.dev/docs/emulation#locale--timezone), [Language tags](https://en.wikipedia.org/wiki/Language_localisation#Language_tags_and_codes), [Geolocation](https://playwright.dev/docs/cli#emulate-geolocation-language-and-timezone) values. |
| `--color-scheme <dark,light>` | emulate context with [dark or light mode](https://playwright.dev/docs/emulation#color-scheme-and-media) |
| `--timeout <timeout>` | set [timeout](https://playwright.dev/docs/test-timeouts) for Playwright test actions to complete, in ms (default "10000")|
| `--save-storage <filename>` <br/> `--load-storage <filename>`| preserve state (e.g., cookies, localStorage) for reuse across sessions e.g., [for authentication](https://playwright.dev/docs/cli#preserve-authenticated-state) |
| `--ignore-https-errors` | ignore HTTPS errors on network requests |
| `--proxy-server <proxy>` | specify URL of proxy server to use |
| `--wait-for-selector <selector>` | (**use with `screenshot` and `pdf`**) - wait for selector to take action |
| `--wait-for-timeout <timeout>` | (**use with `screenshot` and `pdf`**) - wait for timeout (in ms) to take action |
| `--full-page` | (**use with `screenshot`**) - capture full page (entire scrollable area) |
| `--output <filename>` | (**use with `codegen`**) save the generated script to this filename |
| `--debug` | (**use with `test`**) runs Playwright Test with Inspector for debugging |

The `playwright test` command has a much richer [set of options](https://playwright.dev/docs/test-cli#reference) that we will revisit in a separate post about Playwright Test runner.

## 8. Relevant Resources

Quick summary of what we covered today:

 * Use `npx playwright --help` to review CLI commands at any time.
 * Review the [Command line tools](https://playwright.dev/docs/cli) documentation.
 * Download the [visual cheatsheet](https://cloud-skills.dev/gallery/Playwright-01-CommandLine.png) for reviewing CLI at-a-glance.
 * Visit the [#playwright](https://dev.to/t/playwright) tag on dev.to - and share your learnings!
 * Browse the [device descriptors](https://github.com/microsoft/playwright/blob/16ae0af0fd2932efc9cc0e12e86a1b209be62da7/packages/playwright-core/src/server/deviceDescriptorsSource.json) for emulated devices supported.

## 8. Next Steps 

We explored [Command Line Tools](https://playwright.dev/docs/cli) at a high level, and tried out examples for a subset of CLI commands. But there are four commands that will get a lot more usage so we'll explore these in separate posts coming up next.

 * `show-trace` - explore [Trace Viewer](https://playwright.dev/docs/trace-viewer) | previously [done](https://nitya.github.io/learn-playwright/004-trace-viewer/) ✅
 * `show-report` - explore [Test Reporters](https://playwright.dev/docs/test-reporters) 
 * `codegen` - explore [Test Generator](https://playwright.dev/docs/codegen) 
 * `test` - explore [Playwright Test](https://playwright.dev/docs/test-cli) 
 * `test --debug` - explore [Playwright Inspector](https://playwright.dev/docs/inspector) + [Debugging Tools](https://playwright.dev/docs/debug)

Onwards to more Tool Talk!

