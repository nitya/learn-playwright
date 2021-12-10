---
title: "#004 - Tool Talk: Hello, Trace Viewer"
date: 2021-12-09T14:09:45-05:00
draft: false
toc: true
tags: [30Days, tooling, traceviewer, playwright ]
---

> Welcome to **Day 4** of [#30DaysOfPlaywright](https://dev.to/nitya/series/15755)!! 

![](https://nitya.github.io/learn-playwright/images/004-banner.png)

In our [last post](https://nitya.github.io/learn-playwright/003-aswa-demo-app/) we looked at how to setup a demo app as a real-world testbed for Playwright. See the [source code](https://github.com/nitya/aswa-hugo-recipes4aj) and visit the [running site](https://bit.ly/recipes-for-aj) for reference.

Today, we'll kick off a series of _"Tool Talk"_ blog posts that use this app as a sandbox to learn about the authoring, debugging, profiling, and command-line, tools provided by the Playwright framework. Our tool focus today:  [Trace Viewer](https://playwright.dev/docs/trace-viewer)!

Here's a handy visual guide to the Trace Viewer docs. You can download the hi-res image [here](https://nitya.github.io/learn-playwright/004-trace-viewer/) or see a time-lapse of how this was created [here](https://twitter.com/nitya/status/1469384910142164994)

![Visual Summary of the TraceViewer Documentation](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/zt5g56kea37unl7mt5vz.png)


---

## Recap | The Recipes4AJ App Sandbox

In the [last post](https://nitya.github.io/learn-playwright/003-aswa-demo-app/) we built and deployed a recipe blog app, and configured GitHub actions to automate the build-deploy-test workflow. As part of that process, we did a couple of things that relate to this post:

 * [X] We [customized](https://nitya.github.io/learn-playwright/003-aswa-demo-app/#4-customizing-tests) the Playright configuration and test script files.
 * [X] We [ran tests locally](https://nitya.github.io/learn-playwright/003-aswa-demo-app/#5-running-tests-locally) to validate Playwright setup.
 * [X] We saw a [show report](https://nitya.github.io/learn-playwright/003-aswa-demo-app/#6-show-report-locally) link provided when the test run completed.

Today, we want to dive into that last part and understand what _"show report"_ does, and dive into details on the [Trace Viewer](https://playwright.dev/docs/trace-viewer) tool that plays a key role.

---

## Review | Test Configuration

First, let's take a quick look at the project's [playwright.config.js](https://github.com/nitya/aswa-hugo-recipes4aj/blob/main/testing/playwright.config.js) file to understand the default configuration settings. I've deliberately omitted a lot of the details from that file and highlighted only the options relevant to today's post:

```js
const config = {
  testDir: './e2e',

  // See: https://playwright.dev/docs/test-reporters/
  reporter: 'html',

  // See: https://playwright.dev/docs/trace-viewer
  use: {
    actionTimeout: 0,
    trace: 'on',
  },

  // See: https://playwright.dev/docs/api/class-testconfig
  outputDir: 'test-results/',
};
module.exports = config;
```

Let's look at the these options in more detail:

1. [reporter](https://playwright.dev/docs/test-reporters/) is set to `html`. <br/> 
Reporters enable Playwright to create custom test reports from a test run. The [HTML reporter](https://playwright.dev/docs/test-reporters#html-reporter) creates a folder (`playwright-report`) with report data and code that renders the data in a web page. Playwright offers other [built-in reporter](https://playwright.dev/docs/test-reporters/#built-in-reporters) options that we'll explore in the next _Tool Talk_ post.

2. [outputDir](https://playwright.dev/docs/api/class-testconfig#test-config-output-dir) is set to `test-results.`<br/> This test configuration parameter defines the location for files created during test execution. The folder is cleaned, and unique subfolders created for each (isolated) test project within the current test run.

3. [trace](https://playwright.dev/docs/trace-viewer#recording-a-trace) is set to `on`. <br/> This records a trace for every test (success or failure). Other options are `off` (don't record), `on-first-retry` (record only for first retried test) and `retain-on-failure` (record all, then remove traces for successful runs).

Now let's look at the test script.

---

## Run | Test Script (Locally)

Our [customized test script](https://github.com/nitya/aswa-hugo-recipes4aj/blob/main/testing/e2e/example.spec.js) is contained in a spec file (`example.spec.js`) and defines a single test (`basic test`) executed for three projects (targeting `chromium`, `firefox` and `webkit`) - as configured in [playwright.config.js](https://github.com/nitya/aswa-hugo-recipes4aj/blob/main/testing/playwright.config.js).

```js
// @ts-check
const { test, expect } = require('@playwright/test');

test('basic test', async ({ page }) => {
  await page.goto('https://bit.ly/recipes-for-aj');
  await expect(page).toHaveTitle("Recipes 4 AJ");

  await page.locator('text=Tags').click();
});
```

The test script defines three actions:
 * navigate to the [demo app](https://green-stone-0ef96ef10.azurestaticapps.net/) page
 * check if that page has the desired title (`Recipes 4 AJ`)
 * locate the page element matching the selector (`text=Tags`) - and click it

Let's run the test script:

```js
$ cd testing
$ npx playwright test
Using config at <..>/testing/playwright.config.js
Running 3 tests using 3 workers
  3 passed (5s)

To open last HTML report run:
  npx playwright show-report
```

If you list the local directory, you'll now find two new folders created: `test-results` and `playwright-report`. Let's take a look at what these contain:

```
$ ls test-results/*
test-results/example-basic-test-chromium:
trace.zip

test-results/example-basic-test-firefox:
trace.zip

test-results/example-basic-test-webkit:
trace.zip
```

The `test-results` directory (above) contains one subfolder for each project run - with the naming convention (e.g., spec name, test name, project name) reducing conflicts for parallel test runs.


```
$ ls playwright-report/*
playwright-report/index.html

playwright-report/data:
2cbc37ec2b02c3c5606cb8b007dfdc961a7b4a9f.zip
77e362c78816daec1c99e9c8459f6c9ed8e1fa9f.zip
f27c66e32b2332aa356b0e4f175b8a3321986a3e.zip

playwright-report/trace:
40e1017745522c215602.ttf	icon-384x384.png
app.bundle.js			icon-512x512.png
icon-16x16.png			index.html
icon-192x192.png		sw.bundle.js
icon-256x256.png		zip.min.js
icon-32x32.png
```

The `playwright-report` directory (above) contains folders with the data per project run (zipfiles), a trace viewer app, and a landing page (`index.html`) that can be served locally for viewing reports (`show-report`).

---

## Report | Open the HTML Report (locally)

Let's view the generated report using the [`show-report` command](https://playwright.dev/docs/test-reporters#html-reporter).

```
$ npx playwright show-report

Serving HTML report at http://127.0.0.1:9323. Press Ctrl+C to quit.
```

Check the tweet for a video walkthrough of the report (or grab the file [here](https://nitya.github.io/learn-playwright/videos/004-Show-Report.mp4)!) <br/>
{{< twitter 1469042532143321089 >}}

The HTML Report landing page (shown below) has one (collapsible) section per project test run. It comes with a built-in search bar (for easy filtering and discovery) and has color-coded tags for each browser target (click on tag to scope list down to only tests run on _that_ target type).

{{<figure src="https://nitya.github.io/learn-playwright/images/003-report-index.png" title="Screenshot of Landing Page for HTML Report" >}}

Clicking on a particular test (e.g., the `chromium` record) takes you to the detailed report for that run (shown below) - listing the actions run and time taken for each, along with a link to its _trace.zip_ file (since we configured Playwright to record all test runs).

{{<figure src="https://nitya.github.io/learn-playwright/images/003-report-chromium.png" title="Screenshot of chromium project `basic test` run report page" >}}

Click on the trace file - and we get to our intended destination for this post! I've linked an annotated screenshot of that page below, so we can dive a bit into what this does in the next section.

{{<figure src="https://nitya.github.io/learn-playwright/images/003-report-trace.png" title="Screenshot of chromium project `basic test` trace view" >}}

🚨 | _I've saved the trace file [here](https://nitya.github.io/learn-playwright/files/003-chromium-trace.zip) so we can explore this further next!_

---

## Trace Viewer | Launch options

[Trace Viewer](https://playwright.dev/docs/trace-viewer) is the built-in GUI-based tool in Playwright that lets you view and analyze the recorded traces from a test run. We already talked about [recording options](https://playwright.dev/docs/trace-viewer/#recording-a-trace) in the configuration file - and know that the trace will be stored in a _trace.zip_ file under the relevant project run subfolder.

Now let's talk about [Viewing options](https://playwright.dev/docs/trace-viewer/#viewing-the-trace) - there are three ways to get to the Trace Viewer page to explore the trace file from a test run.

 * `$npx playwright show-report` <br/> run full report, then navigate to Trace view.
 * `$npx playwright show-trace trace.zip` <br/> launch Trace viewer directly, with local file.
 * `$npx playwright show-trace <remote-trace-file>` <br/> launch Trace viewer directly, with remote file.

In the last case, try using [the previously-stored trace file](https://nitya.github.io/learn-playwright/files/003-chromium-trace.zip) and see what happens:

```
$ npx playwright show-trace https://nitya.github.io/learn-playwright/files/003-chromium-trace.zip
```

You should see a Trace Viewer window popup similar to the screenshot above - allowing you to view and interact with the trace data. This is a great way to analyze an archived trace, debug it collaboratively in a group project.

---

## Trace Viewer (PWA) | `trace.playwright.dev` 
But wait - there's now a fourth option: <br/> Visit [`trace.playwright.dev`](https://trace.playwright.dev/) in your browser and drop the trace file to activate the Trace Viewer. This is a great option if you are on a device that does not have Playwright installed, and you have a trace file to analyze.

_Want to try it out?_ <br/> Download the [saved trace file](https://nitya.github.io/learn-playwright/files/003-chromium-trace.zip) from before, then open [`trace.playwright.dev`](https://trace.playwright.dev/) and drag or upload this zipfile into the specified location on page.
 
This feature was announced in recently (skip to the [11:44 mark](https://www.youtube.com/watch?v=7iyIdeoAP04&t=704s) in the video below). Most importantly, it's a [Progressive Web App](https://docs.microsoft.com/en-us/microsoft-edge/progressive-web-apps-chromium/) which opens the trace file locally (no sending trace data to servers) to preserve data privacy.

{{< youtube 7iyIdeoAP04 >}}

---

## Trace Viewer | Layout & Components

Refer to the [screenshot above](https://nitya.github.io/learn-playwright/images/003-report-trace.png) that shows the Trace Viewer page launched from the HTML report - or watch the [screencast](https://nitya.github.io/learn-playwright/videos/004-Show-Report.mp4) for an interactive walkthrough.

The viewer layout has 5 main areas:
 1. Title Bar - which shows the test spec file and test name for context
 2. Film Strip - a timeline showing [screenshots](https://playwright.dev/docs/trace-viewer/#screenshots) of page state as actions run
 3. Script Actions - panel listing testing actions in current test script
 4. Action Snapshots - captured [DOM snapshots](https://playwright.dev/docs/trace-viewer/#snapshots) for each action
 5. Action Details - source location, execution + network logs for each action

> Hover over the film strip to get a magnified look at the screenshot 

Slide over the strip to see when different elements get rendered, to enable reliable testing. _For instance, this trace showed me that font-awesome icons get rendered a fraction of time later than the associated labels_.


> Selecting screenshots automatically sets context for Actions Panels below

 * Script Actions panel = shows the relevant actions at time of screenshot
 * Action Details panel = shows call logs and links to source (for test action).
 * Snapshots panel = shows DOM state _before_ & _after_ action (for debug).

And that's it! Visit the [Trace Viewer Documentation](https://playwright.dev/docs/trace-viewer/#recording-a-trace) for more updates.

---

## What's Next? |Tool Talk with Reporters!

We covered the Trace Viewer tool in today's _Tool Talk_ post. Tomorrow, we'll check out Playwright's support for [Test Reporters](https://playwright.dev/docs/test-reporters/) - from learning available options, to using them effectively (including in automated workflows) - and even building your own [custom reporter](https://playwright.dev/docs/test-reporters/#custom-reporters) using the [Reporter API](https://playwright.dev/docs/api/class-reporter).