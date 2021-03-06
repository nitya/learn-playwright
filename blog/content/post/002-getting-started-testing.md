---
title: "#002 - Getting Started (Part 2)"
date: 2021-12-03T15:46:09-05:00
draft: false
toc: true
tags: [general, playwright, 30Days]
---

> Welcome to **Day 2** of #30DaysOfPlaywright!! 

![](https://nitya.github.io/learn-playwright/images/002-banner.png)

Our [`Day 1` exploration of Playwright](https://nitya.github.io/learn-playwright/001-getting-started-setup/) helped us setup and validate our local testing envrionment. Today we'll complete the tutorial by reviewing fundamental Playwright concepts namely:

* [Writing assertions](https://playwright.dev/docs/intro#writing-assertions) - to test if an _expect_-ed behavior holds true.
* [Using test fixtures](https://playwright.dev/docs/intro#using-test-fixtures) - to set desired page/browser context for test run.
* [Using test hooks](https://playwright.dev/docs/intro#using-test-hooks) - to setup and tear down resources between tests.

We'll quickly go over what each does and why we need them - and revisit each topic for a deeper dive in a future post. Let's get started!

---

## 1. Playwright Fundamentals

In my [kickoff post](https://nitya.github.io/learn-playwright/000-hello-30days/) I recommended the [Introduction to Playwright Test runner](https://youtu.be/JjhY2aFBTTk) talk as a good introduction to Playwright fundamentals. Today we'll reference it in the context of various sections of the Getting Started guide.

{{< youtube JjhY2aFBTTk >}}

Here's a handy list of segments with direct links to the relevant section of the talk, for convenience. I recommend you check out at least Ch 1-3 for today.

 * [What is Playwright Test?](https://youtu.be/JjhY2aFBTTk?t=121)
 * [Why create another Test runner?](https://youtu.be/JjhY2aFBTTk?t=149)
 * [Ch 1: Get Started - Demo](https://youtu.be/JjhY2aFBTTk?t=246) - Playwright Test demo
 * [Ch 1: Get Started - Script](https://youtu.be/JjhY2aFBTTk?t=560) - Unpack Test script
 * [Ch 2: Fundamentals](https://youtu.be/JjhY2aFBTTk?t=609) - Test Isolation, Auto Waiting, Web First Assertions
 * [Ch 3: Configuration](https://youtu.be/JjhY2aFBTTk?t=971) - Fixtures, Data-Driven Tests, Reporters, Devices
 * [Ch 4: Inspector and Codegen](https://youtu.be/JjhY2aFBTTk?t=1423) - Playwright authoring & debugging tools
 * [Ch 5: Playwright Tracing](https://youtu.be/JjhY2aFBTTk?t=1987) - Post-mortem Debugging (workflow & viewer)
 * [Recap](https://youtu.be/JjhY2aFBTTk?t=2548) - Start, configure, customize, debug, author & trace.
 * [Q&A](https://youtu.be/JjhY2aFBTTk?t=2671) - Languages support, comparison to other test frameworks.

---

## 2. Unpack the Test Script

Let's revisit the code fr the [first test](https://playwright.dev/docs/intro#first-test) we created and ran on Day 1.

```js
const { test, expect } = require('@playwright/test');

test('basic test', async ({ page }) => {
  await page.goto('https://playwright.dev/');
  const title = page.locator('.navbar__inner .navbar__title');
  await expect(title).toHaveText('Playwright');
});
```

This simple test script illustrates some fundamental Playwright concepts that enable more reliable end-to-end testing:

 * [Auto-waiting](https://playwright.dev/docs/actionability) - performs actionability checks on elements before making a requested action.
 * [Web-First Assertions](https://playwright.dev/docs/test-timeouts#expect-timeout) - retries checks until a necessary condition is met, before proceeding.
 * [Test Isolation with fixtures](https://playwright.dev/docs/test-fixtures#introduction-to-fixtures) - establish reliable environment for tests, enable parallelization.
 * [Test Hooks](https://playwright.dev/docs/intro#using-test-hooks) - for just-in-time setup and teardown of resources shared between tests.

Let's explore these briefly below.

---

## 3. Web Assertions.

Playwright [uses the expect library](https://jestjs.io/docs/expect) for test assertions. Developers can test if the current state of the application matches an expected result - with helpful _matchers_ to test various conditions for that state. For example:

```js
expect(value).toEqual(0);
```

Modern web applications are dynamic with application state in constant flux. Testing assertions can be challenging if the matcher condition is not stable. Playwright makes this more reliable by extending the expect library with [_convenience async matchers_](https://playwright.dev/docs/test-assertions) that will now **wait** till the expected condition is met (and timeout otherwise).

```js
await expect(page).toHaveTitle("Playwright");
```

Now let's try the [Writing Assertions](https://playwright.dev/docs/intro#writing-assertions) example (see below):

```js
// assert.spec.js
const { test, expect } = require('@playwright/test');

test('my test', async ({ page }) => {
  await page.goto('https://playwright.dev/');

  // Expect a title "to contain" a substring.
  await expect(page).toHaveTitle(/Playwright/);

  // Expect an attribute "to be strictly equal" to the value.
  await expect(page.locator('text=Get Started').first()).toHaveAttribute('href', '/docs/intro');

  // Expect an element "to be visible".
  await expect(page.locator('text=Learn more').first()).toBeVisible();

  await page.click('text=Get Started');
  // Expect some text to be visible on the page.
  await expect(page.locator('text=Introduction').first()).toBeVisible();
});
```
Save it to a new file (`assert.spec.js`) and run it as follows:

```
$ npx playwright test assert.spec.js
```

The `{ page }` argument provided to the test function is an example of a _test fixture_. We'll talk about what fixtures are in the next section. 

For now, we can look at the [page](https://playwright.dev/docs/api/class-page/) class documentation and see that it provides methods to interact with a single tab in a browser and handle various events emitted from it. 
 * The [page.locator()](https://playwright.dev/docs/api/class-page#page-locator) method resolves to a [view](https://playwright.dev/docs/api/class-locator) of elements in that page that match the associated selector. 
 * The [page.goto()](https://playwright.dev/docs/api/class-page#page-goto) method navigates the browser to the provided URL.
 * The [page.click()](https://playwright.dev/docs/api/class-page#page-click) methods 'clicks' the element matching that selector.

We'll explore [available assertions in Playwright](https://playwright.dev/docs/test-assertions/) in depth in a future post.

---

## 4. Test Fixtures.

The `{ page }` argument passed into the `test()` function above is an example of a [test fixture](https://playwright.dev/docs/intro/#using-test-fixtures). So, what is a fixture?

Inspired by [pytest](https://docs.pytest.org/en/latest/explanation/fixtures.html#about-fixtures), test fixtures are a tool for establishing a _reliable and consistent_ test environment that provides tests exactly what they need for the current run - and nothing more. Playwright Test analyzes all the fixtures a test needs, merges required values into a single object - and makes that available to the `test` as a first parameter. The fixture is torn down after the test completes.

Why fixtures? Because they create a consistent environment (test repeatability) and provide effective isolation for testing with the following benefits:
 * Efficiency - run multiple tests in parallel, faster, with less memory used.
 * Targeting - retry just failed tests without re-running the entire suite.
 * Grouping - group tests based on shared meaning, not just setup.

Playwright comes with a number of [built-in fixtures](https://playwright.dev/docs/api/class-fixtures/) that you can configure (see below). You can also add your own:
 * [fixtures.browser](https://playwright.dev/docs/api/class-fixtures#fixtures-browser) - shared browser instance (for worker)
 * [fixtures.browserName](https://playwright.dev/docs/api/class-fixtures#fixtures-browser-name) - currently running (chromium, firefox or webkit)
 * [fixtures.context](https://playwright.dev/docs/api/class-fixtures#fixtures-context) - isolated browser context (per test) 
 * [fixtures.page](https://playwright.dev/docs/api/class-fixtures#fixtures-page) - isolated page (per test)

You can use [Test Fixtures](https://playwright.dev/docs/test-fixtures/#test-fixtures) at the granularity of a test run, or use [Worker Fixtures](https://playwright.dev/docs/test-fixtures/#worker-fixtures) to apply them at the level of a worker process (across all tests it oversees).

The bottom line is fixtures enable consistent and isolated testing environments in Playwright Test in a flexible and customizable manner.

---

## 5. Test Hooks.

While fixtures allow you to setup the environment for a test run, [test hooks](https://playwright.dev/docs/intro/#using-test-hooks) provide placeholders for writing code needed to setup and teardown resources that may be shared across tests (in a group) or used on a per-test basis. Here's an example from the getting started guide:

```js
// example.spec.js
const { test, expect } = require('@playwright/test');

test.describe('feature foo', () => {
  test.beforeEach(async ({ page }) => {
    // Go to the starting url before each test.
    await page.goto('https://playwright.dev/');
  });

  test('my test', async ({ page }) => {
    // Assertions use the expect API.
    await expect(page).toHaveURL('https://playwright.dev/');
  });
});
```

Here, the [`test.describe`](https://playwright.dev/docs/api/class-test/#test-describe) method is used to declare a group of tests. The test hooks we need to familiarize ourselves with are:
 * [test.beforeEach](https://playwright.dev/docs/api/class-test/#test-before-each) - runs before starting each test in group.
 * [test.afterEach](https://playwright.dev/docs/api/class-test/#test-before-each) - runs after completing each test in group.
 * [test.beforeAll](https://playwright.dev/docs/api/class-test/#test-before-all) - runs _once_ per group, before any tests are started.
 * [test.afterAll](https://playwright.dev/docs/api/class-test/#test-after-all) - runs _once_ per group, after all tests complete.


---

## Day 2: Review & Resources

 * [X] Learned about the [expect](https://jestjs.io/docs/expect) library for default assertions.
 * [X] Learned about [web assertions](https://playwright.dev/docs/test-assertions/) available in Playwright.
 * [X] Learned [about fixtures](https://docs.pytest.org/en/latest/explanation/fixtures.html#about-fixtures) in pytest.
 * [X] Learned about [fixtures](https://aka.ms/playwright/fixtures) available in Playwright.
 * [X] Explored the [Playwright Test API](https://playwright.dev/docs/api/class-test/) support for test hooks.
 * [X] Completed the [Getting Started](https://playwright.dev/docs/intro) tutorial for Playwright.

 This has been a lot to absorb - but never fear. In the upcoming days, we'll dive into these topics and APIs in more detail with simple examples. For now, you have a working Playwright environment and a grasp of key terms and concept for end-to-end testing using this framework! Onwards!

---

## Day 3: Up Next

We now have a sense of how to create and run a simple test, and the core components that make up a test script. In the next few posts, let's explore some of the tools available for authoring and debugging Playwright test scripts - before we start diving into the Playwright API in more detail.

Here is a short list of tooling resources that may be worth scanning for a head start.

 * [Command line tools](https://playwright.dev/docs/cli) - open pages, generate code, screenshots, PDF etc.
 * [Playwright Inspector](https://playwright.dev/docs/inspector) - GUI-based tool to author and debug scripts.
 * [Trace Viewer](https://playwright.dev/docs/trace-viewer) - GUI-based tool to analyze Playwright Test run traces.
 * [Test Generator](https://playwright.dev/docs/codegen) - CLI tool to record page interactions as test scripts.

In addition, check out the  [Community Showcase](https://playwright.dev/docs/showcase#tools) for tools, frameworks and examples authored by the broader community.
