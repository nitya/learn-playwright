---
title: "#001 - Getting Started (Part 1)"
date: 2021-12-02T16:21:13-05:00
draft: false
toc: true
tags: [general, playwright, 30Days]
---

# Getting Started - Part 1

> Welcome to Day 1 of #30DaysOfPlaywright!! 

Today's goal is to make progress on the [Getting Started](https://playwright.dev/docs/intro) tutorial which should get us set up with the Playwright Test runner in our local development environment - and get us configured to do our first test run.

There's a lot covered in there, so I've decided to break this into multiple parts.

* `Part 1:` - Install the Playwright Test runner and validate setup by completing your first test run with the sample test script. This includes:

    * [Installing Playwright Test](https://playwright.dev/docs/intro#installation) - to use built-in test runner.
    * [Running your first test](https://playwright.dev/docs/intro#first-test) - to explore headless and headed modes
    * [Configuring test workflow](https://playwright.dev/docs/intro#configuration-file) - to support multi-browser testing
    * [Understanding CLI options](https://playwright.dev/docs/intro#command-line) - to customize test runner execution

<br/>

* `Part 2:` - Understand Playwright testing fundamentals and learn how to write more complex test scripts. This includes:
    * [Writing assertions](https://playwright.dev/docs/intro#writing-assertions) - to test if an _expect_-ed behavior holds true.
    * [Using test fixtures](https://playwright.dev/docs/intro#using-test-fixtures) - to set desired page/browser context for test run.
    * [Using test hooks](https://playwright.dev/docs/intro#using-test-hooks) - to setup and tear down resources between tests.

We'll cover Part 1 today and dive deeper into the rest in subsequent posts.

---


## 1. Install Playwright Test

Playwright Test is written in [Node](https://nodejs.org/en/) and installed using [NPM](https://www.npmjs.com/). Install the Playwright Test runner as follows:

```js 
$ npm i -D @playwright/test
```
The process is fairly straightforward once you have Node and NPM installed. (Hint: If you are installing Node for the first time, I recommend using the [Node Version Manager](https://github.com/nvm-sh/nvm#installing-and-updating) to install and use different Node.js versions easily).

---

## 2. Install Browsers

By default, Playwright Test runs in headless mode (here is no visible browser graphical user interface during testing). It can be configured to [run in headed mode](https://playwright.dev/docs/ci#running-headed) using a command-line flag but you'll need to ensure that required browser binaries exist in your local environment.

The command below installs _all_ supported browser binaries, which is useful for cross-browser testing later.

```js
$ npx playwright install
```

Each version of Playwright [needs specific versions of browser binaries to operate](https://playwright.dev/docs/browsers) - you can [manage the browser install](https://playwright.dev/docs/browsers#installing-browsers) for more granular control over the process. Browser binaries are installed by default in OS-specfic cache folders - it's useful to check those to see which binaries you currently have installed in your local device. For example, here's my macOS device listing:

```
$ du -hs ~/Library/Caches/ms-playwright/*
343M	chromium-930007
345M	chromium-939194
199M	firefox-1297
200M	firefox-1304
213M	webkit-1564
211M	webkit-1578
```

---

## 3. Write & Run Your First Test!

The tutorial provides a simple [First Test](https://playwright.dev/docs/intro#first-test) script that you can copy into a file. I chose to use the JavaScript version but you can find similar guidance for [other languages](https://playwright.dev/docs/languages) in the Playwright documentation.

```js
const { test, expect } = require('@playwright/test');

test('basic test', async ({ page }) => {
  await page.goto('https://playwright.dev/');
  const title = page.locator('.navbar__inner .navbar__title');
  await expect(title).toHaveText('Playwright');
});
```

The test script is readable for the most part. You can tell that this test involves visiting the specified page (Playwright website), selecting the element matching given classes, and asserting that it contains the expected text: `Playwright`. We'll unpack the syntax and semantics of this script in our next post - for now, let's validate the execution workflow.

To run the test, save the script and execute the command below. The output shows a successful run in a single [worker](https://playwright.dev/docs/api/class-worker) thread.

```js
$npx playwright test  

Running 1 test using 1 worker

  ✓  test.spec.js:3:1 › basic test (961ms)


  1 passed (1s)
```

> Want to see what happens if a test fails? 

Let's change the script to make it expect a different text string (e.g., "Play wright" instead of "Playwright"). Run the test again. Output now shows a meaningful error (expected X, received Y) along with a call log (truncated for clarity) to help you debug the cause.

```js
$ npx playwright test 

Running 1 test using 1 worker

  ✘  test.spec.js:3:1 › basic test (6s)


  1) test.spec.js:3:1 › basic test =================================================================

    Error: expect(received).toHaveText(expected)

    Expected string: "Play wright"
    Received string: "Playwright"

    Call log:
    ...
```

---

## 4. Run in Headed mode

The default Playwright test runner runs in headless mode - but what if you want to see the browser UI as the test runs? Just add a simple `--headed` flag:

```js
$npx playwright test --headed

Running 1 test using 1 worker

  ✓  test.spec.js:3:1 › basic test (2s)


  1 passed (3s)
```

The output is similar - but if you watch closely, you should see a browser window pop-up briefly, then close immediately on completion of the test run. 

---

## 5. Configure Test Runs


You might have noticed that headed mode uses Chromium by default. What if I want to run the test with a different browser (Firefox, WebKit)? Or, I want to do cross-browser testing using multiple browsers in the same run? This is where having [Configuration files](https://playwright.dev/docs/intro#configuration-file) can help.

Here is the default configuration file provide in the getting started tutorial. The main thing to notice is that the file has multiple named [projects](https://playwright.dev/docs/test-advanced#projects), each providing custom configuration parameters alongside the global parameters (e.g., `retries`) that precede them.

```js
// playwright.config.js
// @ts-check
const { devices } = require('@playwright/test');

/** @type {import('@playwright/test').PlaywrightTestConfig} */
const config = {
  forbidOnly: !!process.env.CI,
  retries: process.env.CI ? 2 : 0,
  use: {
    trace: 'on-first-retry',
  },
  projects: [
    {
      name: 'chromium',
      use: { ...devices['Desktop Chrome'] },
    },
    {
      name: 'firefox',
      use: { ...devices['Desktop Firefox'] },
    },
    {
      name: 'webkit',
      use: { ...devices['Desktop Safari'] },
    },
  ],
};

module.exports = config;
```

You can now run _all_ project configurations in the same test run (e.g., cross-browser testing) using:

```js
$ npx playwright test       
Using config at tests/playwright.config.js

Running 3 tests using 3 workers

  ✓  [chromium] › test.spec.js:3:1 › basic test (1s)
  ✓  [firefox] › test.spec.js:3:1 › basic test (2s)
  ✓  [webkit] › test.spec.js:3:1 › basic test (1s)

  3 passed (3s)

```

You can add multiple configurations for the same browser - for instance, I added the following as a fourth project to test against desktop and mobile targets. Re-running the test should give a successful outcome with 4 tests using 4 workers.

```js
    {
      name: 'Mobile Chrome',
      use: { ...devices['Pixel 5'] },
    },
```

You can also select a specific configuration (from project options) to test solo:

```js
$ npx playwright test --project="Mobile Chrome"
Using config at tests/playwright.config.js

Running 1 test using 1 worker

  ✓  [Mobile Chrome] › test.spec.js:3:1 › basic test (1s)


  1 passed (1s)
```

When testing multiple configurations in the same run, the default behavior allocates one worker thread to each project (so 4 tests for 4 workers). You can change that with a commandline flag, allowing you to control the degree of _parallelization_ of test execution.

```js
$ npx playwright test --workers=2
tests/playwright.config.js

Running 4 tests using 2 workers

  ✓  [chromium] › test.spec.js:3:1 › basic test (1s)
  ✓  [firefox] › test.spec.js:3:1 › basic test (2s)
  ✓  [webkit] › test.spec.js:3:1 › basic test (1s)
  ✓  [Mobile Chrome] › test.spec.js:3:1 › basic test (893ms)


  4 passed (4s)
```

We'll explore more [configuration options](https://playwright.dev/docs/test-configuration) in a future blog post. 

---

## 6. Explore Command Line Options

That covers the basics of creating and executing a Playwright test run with a single test script file. But the command line tool supports more versatile execution requirements as well. 

Use the following command to view and explore available options 

```js
# $ npx playwright test --help
Usage: npx playwright test [options] [test-filter...]
```
These include the ability to run tests that match specified regular expressions (at command line), limit the number of retries, control the degree of parallelization (with worker count) or choose the type of [reporter](https://playwright.dev/docs/api/class-reporter) to use for outcomes.

We'll explore [Command line examples](https://playwright.dev/docs/test-cli) in more depth in a future post.

---

## 7. Scaffold e2e tests for project

If you watched the [Introduction to Playwright Test runner](https://www.youtube.com/watch?v=JjhY2aFBTTk&t=1s) talk, you may recall seeing the `npm init playwright` command being used to scaffold out a complete end-to-end testing setup for a new project.

This uses the [`create-playwright`](https://github.com/microsoft/create-playwright) package to support quickstarts with a single command that installs Playwright (and dependencies) and sets up  basic testing and configuration files for end-to-end testing workflows. This is a good way to also initialize _existing_ projects to use Playwright for testing.

```js
$ npm init playwright demo  

Getting started with writing end-to-end tests with Playwright:
Initializing project in 'demo'
✔ Do you want to use TypeScript or JavaScript? · JavaScript
✔ Where to put your end-to-end tests? · e2e
✔ Add a GitHub Actions workflow? (Y/n) · true
Initializing NPM project (npm init -y)…

...

We suggest that you begin by typing:

  cd demo
  npm run test:e2e

And check out the following files:
  - ./demo/e2e/example.spec.js - Example end-to-end test
  - ./demo/playwright.config.js - Playwright Test configuration

Visit https://playwright.dev/docs/intro for more information. ✨

Happy hacking! 🎭
```

---

## Day 1: Review

If you made it this far, congratulations! Here's a checklist of tasks that you should have completed. 

* [X] Installed Playwright Test package 
* [X] Created simple test script.
* [X] Executed (headless) Playwright Test run - successfully.
* [X] Executed (headless) Playright Test run - with failure.
* [X] Created Configuration file (with mutliple project options)
* [X] Ran Playwright Test in headed mode.
* [X] Ran Playwright Test in headed mode - with multiple browsers.
* [X] Ran Playwright Test - with different worker counts.
* [X] Explore Playwright Test - commandline options
* [X] Scaffolded new project for Playright Test (end-to-end)

---

## Day 2: Up Next

Tomorrow we'll unpack part 2 of the tutorial - which includes:

* [Writing assertions](https://playwright.dev/docs/intro#writing-assertions) - to test if an _expect_-ed behavior holds true.
* [Using test fixtures](https://playwright.dev/docs/intro#using-test-fixtures) - to create the right page/browser context for a test run.
* [Using test hooks](https://playwright.dev/docs/intro#using-test-hooks) - to setup and tear down resources between tests.