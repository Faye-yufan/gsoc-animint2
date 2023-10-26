---
layout: post
title: animint2pages()
subtitle: New feature
gh-repo: animint/animint2
gh-badge: [star, fork, follow]
tags: [pr, contribution, gsoc2023]
comments: true
---

## Motivation
Since `animint2gist` does not work as expected because of the archive of bl.ocks, when we use animint2gist() function, it is not showing the viz as expected. 
And we do want a straight-forward way to share our viz. So I would like to utilize the functionality of Github Pages, which is free, and mostly used for hosting
static files, like blogs.

## Setting Up GitHub Authentication
Initially, the function didn’t work as expected. It required GitHub authentication. One way to authenticate is to set up a GitHub token, 
which can be done from your GitHub account settings. I used `gitcreds::gitcreds_get()` to get the credential in the environment.
After some analysis, see [here](https://github.com/animint/animint2/pull/101#discussion_r1328033433), I decided to use `gitcreds_get()` still.
