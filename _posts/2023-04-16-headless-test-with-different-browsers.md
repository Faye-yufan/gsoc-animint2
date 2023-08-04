---
layout: post
title: Solving Headless Browser Challenges in an R Test Suite
subtitle: Update PhantomJS to Chrome
gh-repo: animint/animint2
gh-badge: [star, fork, follow]
tags: [pr, contribution, gsoc2023]
comments: true
---

This blog aims to explain the problem I encountered and the solution I found for the headless browser test suite for R package.
## Motivation
The original function `tests_init()` was primarily based on the PhantomJS browser. But we found it necessary to change to the Chrome browser for the test suite. PhantomJS has been deprecated and hasn't been developed since 2018, so it lacks the new web capabilities. And most important, the headless test suite do not work on the local machine at all.

the remote driver `getPageSource()` function returned the original skeletal HTML when attempting to retrieve XML.  
Here is an example of the output returned by the PhantomJS remote driver when requesting a page:
```
> remDr$getPageSource()
[[1]]
[1] "<!DOCTYPE html><html><head>\n\n    <meta http-equiv=\"content-type\" content=\"text/html;charset=utf-8\">\n    <title>Interactive animation</title>\n    <script type=\"text/javascript\" src=\"vendor/d3.v3.js\"></script>\n    <script type=\"text/javascript\" src=\"animint.js\"></script>\n    <link rel=\"stylesheet\" type=\"text/css\" href=\"styles.css\">\n    <!-- Selectize -->\n    <script type=\"text/javascript\" src=\"vendor/jquery-1.11.3.min.js\"></script>\n    <script type=\"text/javascript\" src=\"vendor/selectize.min.js\"></script>\n    <link rel=\"stylesheet\" type=\"text/css\" href=\"vendor/selectize.css\">\n  </head>\n\n  <body>\n\n    <div id=\"plot\"> </div>\n    <script>\n      var plot = new animint(\"#plot\",\"plot.json\");\n    </script>\n\n  \n\n\n</body></html>"
```
I'm not sure if it was caused by the PhantomJS's capabilities, but later on when I tested the same with Chrome, most of the unit tests successfully completed.
One of the challenges I faced was making the function compatible with the Chrome browser. The function needed to find the right Chrome driver, which corresponds to the installed Chrome version on the system, be it Windows, Linux, or MacOS. Furthermore, I aimed to run the Chrome browser in headless mode, which is a way to run the browser from a command-line interface without a visible UI.

## Challenges
- ### The incompatibility between the Chrome version and the Chrome Driver version on local machine
The Chrome version installed on the local machine was _115.0.5790.102_. However, the available ChromeDriver versions were 112.0.5615.28, ... 105.0.5195.19. As a result, the test suite failed to initialize.
```
[1] "115.0.5790.102" # the Chrome version on my local machine
[1] "112.0.5615.28" "111.0.5563.64" "111.0.5563.41" "111.0.5563.19" # the available Chrome Driver versions
[5] "106.0.5249.21" "105.0.5195.52" "105.0.5195.19"
Error in tests_init("chrome") : Cannot find compatible chrome driver.
```
**-- Possible solutions**:
1.  Manually Downgrade/Upgrade Chrome:
This is probably the most straightforward approach, but it's not the most convenient and feasible one because it requires more effort to do so.
2. Automatic Driver Management:
Utilize WebDriver managers like `wdman` that can automatically manage the driver versions according to your installed browser version.
4. Request/Wait for ChromeDriver Update:
Check the ChromeDriver website regularly to see if an update that supports local Chrome version has been released. This is a passive approach.

- ### `wdman` or `Chromote`?
Many functions in our existing test setup did not work well with `Chromote`. They are built based on `wdman` package. These functions were important and changing them would be a big task. So, I switched gears and chose to use the Chromedriver available in the wdman package. This worked better with our current testing tools, and although it didn't totally solve the version issue, it was a more practical choice.

- ### Running on Windows/Mac/Linux environment
For now, I haven't tried on Windows/Linux environment, will update this section as soon as I tested.

## Notes
Something I learned about `Selenium`, `wdman` and `Chromote`.
- `wdman` is to manage the downloading/running of third-party WebDriver binaries, like ChromeDriver, PhantomJS. But PhantomJS implements WebDriver protocal via GhostDriver.
- What is 

## Reference
[ChromeDriver Releases](https://chromedriver.chromium.org/downloads)
