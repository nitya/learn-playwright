---
title: "#003 - Playwright ♥️ Azure Static Web Apps"
date: 2021-12-07T14:39:33-05:00
draft: false
toc: true
tags: [30Days, staticwebapps, github, playwright ]
---


> Welcome to **Day 3** of [#30DaysOfPlaywright](https://dev.to/nitya/series/15755)!! 

Alright - so I got through the [Getting Started]() tutorial and have the [Playwright Test] runner setup on my local environment. Where to next?

---

## About Azure Static Web Apps

My original goal was to [explore Playwright Test tools](https://nitya.github.io/learn-playwright/002-getting-started-testing/#day-3-up-next) in Playwright, and dive deeper into command-line, authoring and debugging support in more detail. But it occured to me that doing this with a somewhat realistic app as an experiment sandbox, might make more sense.

Enter [Azure Static Web Apps](https://docs.microsoft.com/en-us/azure/static-web-apps/) - an Azure service that helps you build and deploy a full stack web app to Azure from a code repository. Code changes trigger build/deploy actions, making the developer experience painless. Plus, there's a free tier for personal and hobby projects that is ideal for projects like this. 

The key benefit to static web apps is their ability to separate static assets (HTML/CSS/JS files, images etc.) from the web server - making it easier to distribute and host static assets globally for better performance. Web server interactions can be abstracted into API endpoints hosted using a [serverless architecture](https://docs.microsoft.com/en-us/azure/azure-functions/functions-overview) - leading to more efficiencies in performance and costs.

Azure Static Web Apps has tutorials for many popular [static site generators](https://docs.microsoft.com/en-us/azure/static-web-apps/publish-gatsby) and [front-end frameworks](https://docs.microsoft.com/en-us/azure/static-web-apps/getting-started?tabs=react) to get you up-and-running fairly quickly. And don't forget to check [community posts](https://dev.to/t/azurestaticwebapps) with additional tips and tricks.


## End-to-End Testing with Playwright

The next question is - can Playwright be used with Azure Static Web Apps, and how difficult is it to do so? Turns out, there was an amazing 20-minute talk on just this subject at the June 2021 [Static Web Apps: Code To Scale](https://docs.microsoft.com/en-us/events/learn-tv-static-web-apps-code-to-scale/) virtual event. It gives you a quick intro to Playwright and shows an interactive demo highlighting Playwright usage with an Azure Static Web App built with React.

{{< youtube VMl8aV-ddMA >}}

Alright then - it's settled. I want to build a fun real-world app using Azure Static Web Apps with a supported static site generator or a front-end framework. And I can then use it to explore the Playwright tools and API!

So what's a fun use case you ask?

---

## Recipes4AJ: Yes, I Cooked Up An App!

This was really the fun part! Like millions of people around the world, I'd been cooking up a storm over the past couple of years (thanks pandemic!). And I'd posted quite a few tweet-sized recipes under [#RecipesForAJ](https://twitter.com/hashtag/Recipes4AJ?src=hashtag_click&f=live) or [#Recipes4AJ](https://twitter.com/search?q=%23RecipesForAJ&src=typed_query&f=live)). 

What if I build a static web site that featured these recipes - and had simple tags and search functionality to help folks browse or discover recipe cards easily? Would that work? And how long would it take me to build it?

Two things sealed the deal:

 * I'm a huge fan of [Hugo](https://gohugo.io) - it's fast and flexible, with a rich set of themes for a rapid start. And voila- there was a [Cookbook Theme](https://github.com/deranjer/hugo-cookbook) that met my needs!
 * Azure Static Web Apps has a [quickstart](https://docs.microsoft.com/en-us/azure/static-web-apps/publish-hugo) for using Hugo. And yes, it took next to no time to complete the tutorial and have my basic (empty) blog scaffolded and deployed - do go [check out the site](https://bit.ly/recipes-for-aj)!

Here's my celebratory post that morning!

{{< twitter 1467903204218355712 >}}

---

## Tutorial: Azure Static Web Apps + Hugo

This post is meant to focus on getting to the point where we have Playwright setup for end-to-end testing so I want to speed up to that point. If you want to try this out, follow the steps in the [Azure Static Web Apps Hugo Quickstart](https://docs.microsoft.com/en-us/azure/static-web-apps/publish-hugo) - you can also watch an older [video from my colleague](https://www.youtube.com/watch?v=Ik9AfYKn-p0) that walks through the basic steps interactively!

For convenience, I documented my experience in the project [README](https://github.com/nitya/aswa-hugo-recipes4aj#readme) - it might be useful if you want to learn how to use this specific theme, or check out issues encountered and how to troubleshoot them. The key steps are:
 * [Install Hugo](https://github.com/nitya/aswa-hugo-recipes4aj/blob/main/README.md#21-install-hugo)
 * [Scaffold your blog](https://github.com/nitya/aswa-hugo-recipes4aj/blob/main/README.md#22-create-new-blog-site)
 * [Initialize Git](https://github.com/nitya/aswa-hugo-recipes4aj/blob/main/README.md#23-initialize-git-repo)
 * [Add a Hugo Theme](https://github.com/nitya/aswa-hugo-recipes4aj/blob/main/README.md#24-add-hugo-theme)
 * [Customize and Configure Theme](https://github.com/nitya/aswa-hugo-recipes4aj/blob/main/README.md#25-customize-and-configure-theme)
 * [Create the first post](https://github.com/nitya/aswa-hugo-recipes4aj/blob/main/README.md#26-create-your-first-content-item)
 * [Push Commits to GitHub](https://github.com/nitya/aswa-hugo-recipes4aj/blob/main/README.md#28-push-to-github)
 * [Deploy to Azure Static Web Apps](https://github.com/nitya/aswa-hugo-recipes4aj/blob/main/README.md#29-deploy-your-web-app)

Note that all but the last step are related to the use of the Hugo static site generator. The actual Azure Static Web Apps deployment happens in the final step - you should see something like this when done.

{{<figure src="https://github.com/nitya/aswa-hugo-recipes4aj/raw/main/img/aswa-building.png" title="Screenshot of Azure Static Web Apps Deploy in Progress" >}}

---

## Automation with GitHub Actions!

A key benefit of using Azure Static Web Apps is that it sets up GitHub Actions to automate the build-deploy steps for you, on every commit. 

Here is what the default YAML configuration file looked like for my project. You can see it is set up for two main trigger events -- _push_ events (commits) and _pull requests_. And has two associated jobs (_build and deploy_ and _close pull request_) that will be run based on a matching condition for these events.

```yaml
name: Azure Static Web Apps CI/CD

on:
  push:
    branches:
      - main
  pull_request:
    types: [opened, synchronize, reopened, closed]
    branches:
      - main

jobs:
  build_and_deploy_job:
    if: github.event_name == 'push' || (github.event_name == 'pull_request' && github.event.action != 'closed')
    runs-on: ubuntu-latest
    name: Build and Deploy Job
    steps:
      - uses: actions/checkout@v2
        with:
          submodules: true
      - name: Build And Deploy
        id: builddeploy
        uses: Azure/static-web-apps-deploy@v1
        with:
          azure_static_web_apps_api_token: ${{ secrets.AZURE_STATIC_WEB_APPS_API_TOKEN_GREEN_STONE_0EF96EF10 }}
          repo_token: ${{ secrets.GITHUB_TOKEN }} # Used for Github integrations (i.e. PR comments)
          action: "upload"
          ###### Repository/Build Configurations - These values can be configured to match your app requirements. ######
          # For more information regarding Static Web App workflow configurations, please visit: https://aka.ms/swaworkflowconfig
          app_location: "/" # App source code path
          api_location: "" # Api source code path - optional
          output_location: "public" # Built app content directory - optional
          ###### End of Repository/Build Configurations ######

  close_pull_request_job:
    if: github.event_name == 'pull_request' && github.event.action == 'closed'
    runs-on: ubuntu-latest
    name: Close Pull Request Job
    steps:
      - name: Close Pull Request
        id: closepullrequest
        uses: Azure/static-web-apps-deploy@v1
        with:
          azure_static_web_apps_api_token: ${{ secrets.AZURE_STATIC_WEB_APPS_API_TOKEN_GREEN_STONE_0EF96EF10 }}
          action: "close"
```

Now, any changes you make - e.g., adding a new post, or changing the content in an existing post - automatically triggers a rebuild and deploy of the blog. You can track status of past and ongoing runs in the [Actions tab](https://github.com/nitya/aswa-hugo-recipes4aj/runs/4433120328?check_suite_focus=true) of your GitHub project. Check out the [Test GitHub Actions](https://github.com/nitya/aswa-hugo-recipes4aj#210-test-the-github-actions-workflow) step of my README to see how you can see this in action by adding a new blog post.

{{<figure src="https://github.com/nitya/aswa-hugo-recipes4aj/raw/main/img/aswa-updated.png" title="Screenshot of Azure Static Web Apps Deploy in Progress" >}}

At the end of this step, my blog was deployed and the first post was visible as shown above. Now, it was time to think about integrating Playwright!!

---

## Setup Playright Tests

My initial goal is simply to use this blog as an experimental sandbox to explore Playwright tools with a real-world app deployment. But what kinds of tests might we write as we explore the Playwright API further? 

Some initial thoughts include:

 * Cross-Browser - verify it works on Chromium, WebKit and Firefox
 * Mobile - use emulation to check responsiveness of design
 * Content - make sure all posts contain specific section or tag
 * Navigation - check that all links in post are valid
 * Accessibility - check if site meets web accessibility guidelines
 * Performance - trace views to detect and fix inefficiencies 

But let's start by first making sure we have Playwright setup for the project.


### 1. Continuous Integration Options

The Playwright documentation has a section on [Continuous Integration](https://playwright.dev/docs/ci/) with advice for configuring various systems including [Azure Pipelines](https://playwright.dev/docs/ci/#azure-pipelines) and [GitHub Actions](https://playwright.dev/docs/ci/#github-actions).

Also check out this excellent [Demo.Playwright](https://github.com/MarcusFelling/Demo.Playwright/blob/main/.github/workflows/pipeline.yml) example using the playwright container environment and running richer actions (e.g., upload and publish reports) as part of the pipeline.

### 2. My Playwright Actions Setup 

I started with something simple and used the [Scaffold e2e tests for project](https://nitya.github.io/learn-playwright/001-getting-started-setup/#7-scaffold-e2e-tests-for-project) approach to create and initialize a `testing` folder for Playwright-driven testing. Here is what that looked like (output truncated for clarity):

```
$ npm init playwright testing

Getting started with writing end-to-end tests with Playwright:
Initializing project in 'testing'
✔ Do you want to use TypeScript or JavaScript? · JavaScript
✔ Where to put your end-to-end tests? · e2e
✔ Add a GitHub Actions workflow? (Y/n) · false
Initializing NPM project (npm init -y)…
Wrote to <..>/testing/package.json:

{
  "name": "testing",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC"
}

Installing Playwright Test (npm install --save-dev @playwright/test)…
...
...
...

We suggest that you begin by typing:

  cd testing
  npx playwright test

And check out the following files:
  - ./testing/e2e/example.spec.js - Example end-to-end test
  - ./testing/playwright.config.js - Playwright Test configuration

Visit https://playwright.dev/docs/intro for more information. ✨

Happy hacking! 🎭
```

### 3. Troubleshooting

1. _Setup Playwright in its own subfolder (here `testing/`) for simplicity._ Otherwise the presence of Node.js config file (like `package.json`) causes an unexpected conflict with the Azure Static Web Apps workflow, causing that Action to fail.

2. _Playwright setup gives you the option to have a GitHub Actions file created_. I declined it so I could (manually) update the one I already had for this project. If you choose to do otherwise, make sure you add a dependency on that build/deploy job so this runs only after that succeeds.

### 4. Customizing Tests

The default playwright configuration is in `testing/playwright.config.js` - make changes here if you want to configure projects for cross-browser testing (e.g., add mobile targets) or configure [other test options](https://playwright.dev/docs/api/class-testoptions).

I elected to make only one change - turning [trace recording](https://playwright.dev/docs/api/class-testoptions/#test-options-trace) on by default, for every run (rather than only on first retry). This ensures that a `trace.zip` file is created on each run for analysis.

```js
    /* Collect trace when retrying the failed test. See https://playwright.dev/docs/trace-viewer */
    trace: 'on' // 'on-first-retry',
```


The default end-to-end tests are in the `testing/e2e/example.spec.js` file - and do a basic test that navigates to the Playwright site, checksif the title matches expectations, and clicks an element (matching a text selector).

This is a good time to just modify these to match your own deployed Azure Static Web Apps site. Here's what mine looks like:

```js
// @ts-check
const { test, expect } = require('@playwright/test');

test('basic test', async ({ page }) => {
  await page.goto('https://bit.ly/recipes-for-aj');
  await expect(page).toHaveTitle("Recipes 4 AJ");

  await page.locator('text=Tags').click();
});
```

It's a trivial change, but it starts the process of connecting the test suites to the current project.

### 5. Running Tests (Locally)

With this setup, you should be able to run the end-to-end tests in your local environment immediately. Try this out:

```
$npx playwright test
Using config at <..>/aswa-hugo-recipes4aj/testing/playwright.config.js

Running 3 tests using 3 workers

  3 passed (4s)

To open last HTML report run:

  npx playwright show-report
```

### 6. Show Report (Locally)

The default configuration is setup to create an HTML report of the test run - and since we turned on traces, it should also have a link to the trace report that we can analyze in the [TraceViewer](https://playwright.dev/docs/trace-viewer).

For now - let's just run that last command and see what happens:

```
$ npx playwright show-report

  Serving HTML report at http://127.0.0.1:9323. Press Ctrl+C to quit.
```

In the next blog post, we'll dive into more details on what this report shows - and explore other local testing tools in more detail.

---

## Automate Testing with GitHub Actions

For now, let's close by looking at how we can automate running these tests with every commit. We already have a GitHub Actions file with a `build and deploy` job - we just need to add a `test` job that will run once the build/deploy completes.

This is what my Playwright job section configuration looks like (sections truncated for clarity) - the current project file can be found [here](https://github.com/nitya/aswa-hugo-recipes4aj/blob/main/.github/workflows/azure-static-web-apps-green-stone-0ef96ef10.yml)

```js
name: Azure Static Web Apps CI/CD

on:
  push:
    branches: 
      - main
  pull_request:
    types: [opened, synchronize, reopened, closed]
    branches:
      - main

jobs:

  build_and_deploy_job:
  // TRUNCATED FOR CLARITY
  
  test:
    name: "Test site using Playwright"
    timeout-minutes: 60
    needs: build_and_deploy_job
    runs-on: ubuntu-20.04  
    steps:
      - uses: actions/checkout@master
      - uses: actions/setup-node@v2
        with:
          node-version: '14.x'
    
      - name: Install dependencies
        run: | 
          cd testing
          npm ci
          npx playwright install-deps
          npx playwright install
      - name: Run Playwright Tests        
        continue-on-error: false
        working-directory: testing       
        run: |   
          npx playwright test e2e --reporter=html --config playwright.config.js

    
  close_pull_request_job:
  // TRUNCATED FOR CLARITY

```

I'll highlight just three things:

 * The [timeout-minutes](https://docs.github.com/en/actions/learn-github-actions/workflow-syntax-for-github-actions#jobsjob_idtimeout-minutes) defines the longest a job will run before GitHub cancels it. The [Playwright docs](https://playwright.dev/docs/ci/#github-actions-on-deployment) example used 60 minutes.
 * The [needs](https://docs.github.com/en/actions/learn-github-actions/workflow-syntax-for-github-actions#jobsjob_idneeds) parameter identifies jobs that must complete before this can run. Here, our dependency is on the `build_and_deploy_job`.
 * The [working-directory](https://docs.github.com/en/actions/learn-github-actions/workflow-syntax-for-github-actions#defaultsrun) parameter should be set to `testing` for the test job - to ensure subsequent commands run in this directory (and not default root)

Save this updated workflow file. You should immediately see the [Actions run](https://github.com/nitya/aswa-hugo-recipes4aj/actions/runs/1547787844) -- note how the visual for the status shows the dependency between the _Build and Deploy Job_ and _Test site using Playwright_ jobs, indicating that the first one completes before the second one starts.

You can now [click on the job run](https://github.com/nitya/aswa-hugo-recipes4aj/runs/4439390564?check_suite_focus=true) to see a break down of the steps - each showing the actions taken, and the time taken to complete it.

🔥 Congratulations! We now have a Hugo-themed blog hosted on Azure Static Web Apps, with Playwright End-to-End testing workflow setup. Plus, build-deploy-test GitHub Actions workflows to streamline our development!


---

## Day 3: Review

Wow - that covered a lot today! Let's Recap!

* [X] Learned about [Azure Static Web Apps (ASWA)](https://docs.microsoft.com/en-us/azure/static-web-apps/)
* [X] Watched [E2E Testing With Playwright and ASWA](https://youtu.be/VMl8aV-ddMA)
* [X] Built & Deployed [a Hugo-themed Static Web Site](https://docs.microsoft.com/en-us/azure/static-web-apps/publish-hugo)
* [X] Explored [Continuous Integration](https://playwright.dev/docs/ci/) options for Playwright
* [X] Setup [Playwright Tests & Actions](https://github.com/nitya/aswa-hugo-recipes4aj) to automate build-deploy-test.
* [X] Maybe even [learned a new recipe!](https://bit.ly/recipes-for-aj)

That was a lot to cover - and to be honest, I got this done in a day but it was a looong day! Still, this is a great test bed to work with. Onwards!

---

## Day 4: Up Next

Let's revisit our previous objectives to [explore the tools available](https://nitya.github.io/learn-playwright/002-getting-started-testing/#day-3-up-next) for authoring, debugging and analyzing Playwright Test runs. In the upcoming days, we will explore the following:

 * [Trace Viewer](https://playwright.dev/docs/trace-viewer/) - dive into the trace report generated
 * [Reporters](https://playwright.dev/docs/test-reporters) - understand what `show-report` does and how we can customize it.
 * [Test Generator](https://playwright.dev/docs/codegen) - how can we generate tests out of the box?

---