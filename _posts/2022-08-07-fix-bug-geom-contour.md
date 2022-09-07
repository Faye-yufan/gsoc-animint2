---
layout: post
title: Bugfix – geom-contour error
subtitle: replacing defaults() function
gh-repo: tdhock/animint2
gh-badge: [star, fork, follow]
tags: [pr, contribution, gsoc2022]
comments: true
---

### Motivation
An [issue](https://github.com/tdhock/animint2/issues/28) from 2019 has been there for a while, after I have done the major part of the `color_off` feature, I got some free time and decided to look into that.

### Development Log
This still credited to the debug tool. After loading namespace ggplot2, the Layer object calls this [plyr defaults()](https://github.com/tdhock/animint2/blob/0a2988ff02f73aab7ec6c3fb70d14439092bee27/R/layer.r#L176) function to compute aesthetics, which generate a list of quosure with `expr` and `env` causing the error. I think it is probably because the `[.quosures` or `c.quosures` S3 methods are overwritten by ggplot2 in this case.
```R
Browse[2]> defaults(layers[[1]]$mapping, plot$mapping)
$x
<quosure>
expr: ^waiting
env:  global

$y
<quosure>
expr: ^eruptions
env:  global

$z
<quosure>
expr: ^density
env:  global
```
And in the latest ggplot2, instead of importing from plyr, they copied some plyr functions [inside ggplot package](https://github.com/tidyverse/ggplot2/blob/5141ef58442114a40c733a0ab387710c8f9cc1ec/R/compat-plyr.R), including `defaults()`, so I did the same thing.

### Commits
[commits for pr #73](https://github.com/tdhock/animint2/pull/73/commits)

### Reference
- [quosures provides methods for \[and c()](https://rlang.r-lib.org/reference/new_quosures.html)