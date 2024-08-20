---
layout: post
title: GSoC 23' Summary
subtitle: Thoughts and experience
gh-repo: animint/animint2
gh-badge: [star, fork, follow]
tags: [pr, contribution, gsoc2023]
comments: true
---
## Basic Info
- **Name**: Yufan Fei
- **Email**: yufanfei8@gmail.com
- **Github Username**: Faye-yufan
- **Mentors**: Toby Dylan Hocking, Faizan Uddin Fahad Khan
- **Project Title**: Enhance Selection, Viz & Automation
- **Project Link**: [animint2](https://github.com/animint/animint2)

## Work Summary

| Pull Requests   |      Issues      |
| :------ |:--- |
| [#104 Support for feature fill_off](https://github.com/animint/animint2/pull/104) | [#65 User configurable colour and fill for selected and deselected elements](https://github.com/animint/animint2/issues/65)  |
| [#82 Replace phantomjs with chrome as headless browser](https://github.com/animint/animint2/pull/82) | N/A      |
| [#92 Fix scrolling issue in Firefox](https://github.com/animint/animint2/pull/92) | [#90 Observations on a Scrolling Bug](https://github.com/animint/animint2/issues/90) |
| [#101 created animint2pages()](https://github.com/animint/animint2/pull/101) | [#84 move gallery from blocks to gh pages](https://github.com/animint/animint2/issues/84) |
| [#107 Refactor draw_geom() and turn into a Geom class for scalability](https://github.com/animint/animint2/pull/107) | [#48 Move scale range calculation from compiler to the renderer.](https://github.com/animint/animint2/issues/48) |

## **Overview**

This blog shares my work and experience during Google Summer of Code 2023.

Being part of Google Summer of Code again this year has been a fantastic trip. A big thank you goes to my mentor for giving me the chance to be on this project again and for trusting me with it. I have noticed a tangible improvement in my comfort level and confidence when it comes to coding, which I credit largely to the experiences and learnings of the past year.

Balancing a full-time job during the daylight hours while dedicating my evenings and weekends to developing for animint has indeed posed its challenges. The increased responsibilities and the reduced time for development have made this year's journey a bit more tough compared to last year. Nevertheless, the support and understanding from my mentor have been important in navigating these challenges. His willingness to provide a flexible coding schedule has helped me a lot in managing my commitments effectively.

This GSoC project has really opened my eyes to so much more than just R programming. I got to learn about SSH tokens, which help keep online connections safe and private. I also dived into object-oriented programming, which is a smart way to organize code.

I even got to have some research and learned headless testing, which is a great way to check that software works right without having to click around manually, and learning the latest testing suite development is really fun.

Overall, this year’s Google Summer of Code has been a lot more than just writing code. It's been about learning new things, figuring out how to adapt, and always chasing after more knowledge. I'm really excited to use what I've learned to make the project better and to help out more in the open source community.

## **Replace phantomjs with chrome as headless browser**

We decided to make change in how we test our software. We used to use a tool called `PhantomJS` to test our web pages without showing them on a screen, but we ran into problems because it is no longer supported and cannot capture the dynamic JS code change. So, we tried to switch to using Chrome.

When we started using Chrome, most of our problems with the tests got fixed. But not all of them – some tests still didn't work because Chrome shows some web page effects differently than Firefox. Also, I had some issues when our tests tried to work with certain kinds of data.

To make sure our testing works well on any computer, whether it's a Mac, Windows, or Linux, I tried to get the current version of Chrome on the computer. 

We talked about the problem of keeping Chrome, the browser, and ChromeDriver, the tool we use for testing, updated together. If they're not the same version, our tests might not work. We discussed whether we should update Chrome ourselves or wait for new updates of ChromeDriver to fix our problem. Chrome and ChromeDriver are now being updated at the same time, which might help us avoid these version issues. But our tools for getting the right version of ChromeDriver might not be up to date with these changes. I think the best way to fix it should be wait for the web driver manager library `wdman` to update to get the latest Chrome driver. I saw the PR has passed the test, once they updated the version, I would like to see if it works on our test suite.

I looked at how other people are dealing with similar issues and found out that many are starting to use a tool called `Chromote`. This might be something we could do too. However, since we also use Docker for test which uses `Firefox` , I am afraid if we switch the whole test set into using Chrome, we might have more issues with the GH actions and Docker test.

I'm working to make our tests run smoothly, and while we've made progress, there's still more to do to make sure our software works right on all computers.

## **Scrolling issue on Firefox**

In the beginning of GSoC, thanks to [@ampurr](https://github.com/ampurr), found the scrolling issue on Firefox, it is a little bit tricky problem, where users were experiencing some annoying scrollbar jumping when they interacted with animint viz. I identified that it was linked to how we were setting the height of a table element, which didn't play well with the way Firefox handles rendering.

To tackle this, I made a change where we delay setting the height of this **`selector_table`** element. By wrapping the height adjustment in a **`setTimeout`** with zero delay, we can let all the other page rendering and tasks finish up before we adjust the height. This simple yet effective fix stopped the scrollbar from moving around unexpectedly.

However, there are still some little bugs on this issue, see [here](https://github.com/animint/animint2/issues/90#issuecomment-1593541482).

Then, to ensure we really squashed this bug, I wrote a test case. I initially rolled back the fix to confirm the test would fail without the correction, as it should. But here's where it got complicated. We generally use Selenium for testing, but it doesn’t have the capability to monitor the scrollbar's position directly. So, I had to use a bit of JavaScript to check where the scrollbar was.

The first version of the test didn't capture the bug, so I needed to think about another approach to reproduce the issue correctly. While I was figuring this out, I also checked our tests against a Docker image of Firefox used by Selenium. Interestingly, the bug didn’t show up there in version 2.53.0, which probably explains why the test didn’t catch it initially – the environment where the test was running didn’t have the bug to begin with!

## Create **`animint2pages()`**

I tackled the task of creating the **`animint2pages()`** function, which is intended to ease the deployment of animint visualizations to GitHub Pages. And also help moving animint gallery onto GitHub Pages. This addition aims to streamline the process by handling the commits and push operations within R, leveraging the **`git2r`** and **`gh`** packages.

During the development process, I encountered a few problems, particularly with the **`git2r`** package. One issue was that creating a new repository through GitHub's API does not automatically create a 'main' branch, unlike when a repo is manually made via the GitHub interface. Additionally, the **`git2r::commit()`** function presented a challenge since it requires an S3 class **`git_commit`**, which isn't created when initializing a new repository.

To overcome these, I made several commits to refine the path setting, handle nested file structures, and ensure proper authentication via **`git2r`**. 

Confronted by an authentication issue with the **`git2r::push()`** function in my local environment, I found that the OpenSSH-formatted keys were not functioning as expected. After consulting with my mentor @tdhock, I learned the importance of securing SSH keys with passphrases to avoid impersonation risks and started exploring alternative methods to interact with GitHub.

One potential solution was the **`use_github_pages()`** function from the **`usethis`** package, which seemed promising. I also experimented with switching from SSH to HTTPS protocol by employing the **`gert`** package, which appeared to solve some issues. I've successfully utilized this approach to upload an animint visualization onto a private GitHub page, maintaining the original directory structure.

Initially, my intention was to utilize local Git user credentials to create repositories specifically for local login accounts, as detailed in the pull request [here](https://github.com/animint/animint2/pull/101/files#diff-6c77230e5f2bdcb3a3c21e98224ada8f337bb04f677ede3b90406a3933087509R73-R81). However, I hadn't tested this with the organization account and was curious about the possibility of accessing and creating new repos for the organization using local Git. Meanwhile, I considered adding an error message for clarity.

As for testing, I'm considering following the precedent set by **`animint2gist()`** which checks object classes and expected error messages. 

To keep the code clean, I've also removed unnecessary spaces and made sure the 'Collate' field in the DESCRIPTION file was correctly ordered to facilitate remote installation.

Besides, I resolved the redundancy of using both **`gert`** and **`git2r`** packages, which overlapped in functionality as bindings to the **`libgit2`** library. With thoughtful consideration, I shifted our approach to exclusively use **`gert`**, replacing the **`git2r::commits()`** with **`gert::git_log()`**, thereby streamlining our toolset.

After some adjustments, I successfully replaced the function from **`git2r`** with one from **`gert`**, aligning with our goal to simplify our tools. Alongside this, I also took the opportunity to perform some code cleaning, like deleting **`animint2gist`** and running **`roxygen2::roxygenise()`** to update documentation.

My mentor @tdhock provided feedback, he suggested including a second line under the "New animint visualization" section that directs users to the visualizations, like the one they created at **`https://tdhock.github.io/tdhock-figure-nnet-regression-degrees/`**. I resonated with this idea and implemented it.

As I continue refining the code, my focus is on resolving any remaining issues, including the GH Action test failures and repository creation under organizations, to finalize the branch for merging.

## Selection feature `fill_off`

This feature allows users to define how selected/non-selected parts of data should be filled(tieh what color), particularly when certain data points are selected using interactive features like **`clickSelects`**.

The **`fill_off`** property complements existing properties like **`color_off`** and **`alpha_off`**, which define the color and transparency of non-selected data, respectively. This suite of features provides users with the capability to enhance the visual distinction between selected and non-selected data, contributing to a more interactive and visually intuitive data exploration experience in Animint visualizations.

Key Components of the PR:

- Implementation of **`fill_off`** functionality in JavaScript and R (**`geom-.r`**) in a way that's analogous to existing **`color_off`** and **`alpha_off`** features.
- Refactoring of the **`select_styles`** logic to incorporate **`fill_off`**, which includes the use of a helper function for property checks to streamline the process.
- Addition of unit tests specifically designed to ensure that the **`fill_off`** feature behaves as expected across different geoms.

I've been diving deep into the details of the recently merged PR that introduced the **`fill_off`** feature into our Animint library. This extension builds on our existing interactive features, allowing for an improved visual distinction in our visualizations. We now have **`fill_off`** joining the ranks of **`color_off`** and **`alpha_off`**, creating a rich set of interactive capabilities for our users.

During this process, I've focused on seamlessly integrating the new feature, making sure it works harmoniously with the existing ones. The efforts included not just the core implementation, but also the crucial step of refining the logic through a new helper function, ensuring that our styling checks are efficient and scalable.

I’ve also taken a critical eye to our unit tests, make sure to cover a variety of use cases, particularly for different geoms, to guarantee that our new feature works for all expected behaviors. The feedback from our reviews was invaluable, pushing me to improve naming conventions for clarity and to consider additional scenarios, like those involving **`geom_rect`**.

The review from my mentor @tdhock led to an insightful discussion about potential refactoring for the animint2 codebase. We're exploring the idea of a class-based approach to define our geoms, aiming to encapsulate specific styles and behaviors better. This conversation is just the beginning of what seems to be an exciting path towards enhancing our code's structure.

## Refactoring

I have been working on refactoring the **`draw_geom`** function by encapsulating existing procedural code into class methods within [this PR](https://github.com/animint/animint2/pull/107). This enhances scalability and sets the stage for future feature additions linked to [issue #48](https://github.com/animint/animint2/issues/48).

Changes

- Restructured the **`Geom`** class for better OOP adherence.
- Introduced methods to simplify selection and styling.
- Optimized data preparation logic.

To-Do List

- Develop virtual classes for group/ungroup geom mechanisms.
- Create specific geom class inherits from the virtual classes.
- Optimize **`selected_funs`**.
- Replace old code after new code testing.
- Document the function logic within the code.

In the beginning, I tried to address the **`Geom`** class refactoring, data preparation for various geometries, implementing special D3 data binding, updating graphical elements, adding **`linkActions`**, and removing the old **`draw_geom`** function.

In recent updates, I've made 11 commits, creating virtual classes for group/ungroup geometries, optimizing code, and developing classes for specific geometries like points and lines, streamlining their selection and data binding processes.

### Discussions and Clarifications

After discussing with my menter @tdhock, I gained clarity on **`key_fun`** and **`id_fun`** functions used for D3 data-binding and HTML ID assignment, respectively. This informed my initiative to propose virtual classes **`GroupedGeom`** and **`UngroupedGeom`** for better logical organization.

### Documentation and Progress

I plan to document the refactored logic within the code comments and eventually create a wiki page on 'JS internals'.
