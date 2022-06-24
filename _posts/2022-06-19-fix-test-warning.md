---
layout: post
title: My 1st PR: Fix test warnings
subtitle: Fix test warnings in animint2 Github CI.
gh-repo: Faye-yufan/animint2_Test
gh-badge: [star, fork, follow]
tags: [pr, contribution, gsoc2022]
comments: true
---

Github Continuous integration (CI) is a software practice that requires frequently committing code to a shared repository. The workflow in Animint repo has been used to debug when finding the source of an error. It makes sure that every developer in the team are running and testing the code on the same environment.

We found there are many warnings in animint2 GH CI which is quite annoying when we try to find logs that really matter.
<br/>
## Warnings
- [x] to ensure that smooth transitions are interpretable, aes(key) should be specifed for geoms with showSelected=year
- [x] ~~expect_less_than()~~ has been deprecated.
- [x] ~~expect_that(... , is_true())~~ has been deprecated.
- [x] ~~expect_more_than()~~ has been deprecated.
- [x] `geom_bar()` no longer has a `binwidth` parameter. Please use `geom_histogram()` instead.
- [ ] 'options(stringsAsFactors = TRUE)' is deprecated and will be disabled
- [ ] Using size for a discrete variable is not advised.

<br/>
## Solutions
### `aes(key) / smooth transitions` warnings
In most cases, this type of warning can be easily solved by choosing a variables as you selected, data points exist both before and after. For details, check [animint book](https://rcdata.nau.edu/genomic-ml/animint2-manual/Ch03-showSelected.html) about how to use aes(key) to make smooth transitions.

As for the warning in test-renderer3-lilac_chaser.R. I tried modifing the function by adding a new column `grp` to support smooth transitions and avoid the warning, it works as well.

### `expect_that()/expect_more_than()/expect_less_than()` has been deprecated.
Use `expect_true()`, `expect_lt()`, `expect_gt()` instead. More details can be found [here](https://www.rdocumentation.org/packages/testthat/versions/3.0.3/topics/expect_less_than).

### `geom_bar()` no longer has a `binwidth` parameter. Please use `geom_histogram()` instead.
This warning is not quite intuitive, but [ggplot bar charts examples](https://ggplot2.tidyverse.org/reference/geom_bar.html#ref-examples) shows that `binwidth` are now only exists in `geom_histogram()`.  
`binwidth` has determined how many bins in a histogram, since it is grouping data within binwidth. Whereas `width` in `geom_bar` would just change the width of each bin and no grouping, so there might be warning message appears for overlapping, like below:
```
Warning message:
position_stack requires non-overlapping x intervals 
```

## Commit Behavior
Great commit message shall be:
> * Separate subject from body with a blank line
> * Do not end the subject line with a period
> * Capitalize the subject line and each paragraph
> * Use the imperative mood in the subject line
> * Wrap lines at 72 characters
> * Use the body to explain what and why you have done something. In most cases, you can leave out details about how a change has been made.

More details can be found on:
* [Commit Message Guidelines](https://gist.github.com/robertpainsi/b632364184e70900af4ab688decf6f53#rules-for-a-great-git-commit-message-style)
* [Git Commit Good Practice](https://wiki.openstack.org/wiki/GitCommitMessages#Information_in_commit_messages)