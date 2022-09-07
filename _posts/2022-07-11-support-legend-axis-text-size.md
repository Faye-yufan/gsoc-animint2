---
layout: post
title: New feature – legend/axis text size
subtitle: to be aligned with ggplot2 theme() syntax
gh-repo: tdhock/animint2
gh-badge: [star, fork, follow]
tags: [pr, contribution, gsoc2022]
comments: true
---

### Summary
Enable `size` options in `axis.text.x`, ` axis.text.y`, `legend.text` to be aligned with ggplot2 syntax. Try to enable `axis.text` and `text` as well, it may require more logic in compiler.

- [x] Add relative _'size'_ parameters into the compiler for extracting input parameters in theme().
- [x] Add d3 attributes in the renderer to display the text size.
- [x] Modify plot strip/axis calculation in the renderer, to make the plot displayed properly.

### Motivation
Currently Animint theme() does not support arguments like [`text`](https://ggplot2.tidyverse.org/articles/extending-ggplot2.html?q=theme%20text#global-elements), [`axis.text`](https://ggplot2.tidyverse.org/reference/element.html#ref-examples) nor [`legend.text`](https://ggplot2.tidyverse.org/articles/faq-customising.html#how-can-i-change-the-font-sizes-in-the-legend) to change axis/legend text size like below:
```
+ theme(...,
        axis.text = element_text(size=X),
        legend.text = element_text(size=X),
        ...)
```
I think this feature would require add new functions in [z_animint.R](https://github.com/tdhock/animint2/blob/4b1049bc1d8ee8dbd78b31f52a5863496fae5931/R/z_animint.R#L146-L149), [add_plot func](https://github.com/tdhock/animint2/blob/92e3bc5dd6937e61c2d7bb247c37448cc9b02138/inst/htmljs/animint.js#L309-L321) and [add_legend func](https://github.com/tdhock/animint2/blob/92e3bc5dd6937e61c2d7bb247c37448cc9b02138/inst/htmljs/animint.js#L2011-2128) in animint.js. And the axis and lengend font size maybe default to 11pts and 12pts seperately if user not defined.  

It may require add conditions to extract `axis.text` params or another variable name for assigning axis text size, maybe like `axis.font`. For legend text size, I think simply add `.style("font-size","XXpt")` would work.

### Development Log
#### 2022-06-28
```
rel() is used to specify sizes relative to the parent.
```
The default font size is 11 pts, see this [post](https://stackoverflow.com/questions/44663915/what-is-parent-element-of-ggplot2)

#### 2022-06-29
`xsize` and `ysize` have been successfully extracted, and can be shown in `plot.json`.
`axis.text.x` and `axis.text.y` has enabled. but no commit. "Add axis text attr to animint.js"

`animint.js` L544-572 is for defining top strip.

#### 2022-07-01
* What is `draw_strip` for?

* How is the axis location been calculated?
animint.js L509-L511
```
plotdim.xstart =  current_col * plotdim.margin.left +
        (current_col - 1) * plotdim.margin.right +
        graph_width_cum + n_yaxes * axispaddingy + ytitlepadding;
```
After modify the axispaddingy logic, it seems the plot size changes as well?
`plotdim.xend` in animint.js

* legend size
Currently, legend font size is not defined in the `<table>/<td>/<tr>`.
The main idea of how legends work:
1. In getLegend in animint.R(the func is specifically defined in animintHelper.R) export the legend entries as a  list of rows that can be used in a data() bind in D3.
2. In add_legend in JS code creates a <table> for every legend, and then bind the legend entries to <tr>, <td>, and <svg> elements.

animint.js L2129 add `.attr("font-size", p_info.legendTextSize)` 


#### 2022-07-04
* [`.pt`](https://ggplot2.tidyverse.org/articles/ggplot2-specs.html#font-size) in ggplot2 is different, it equals to 72.27 / 25.4
```
ggplot2::.pt
[1] 2.845276
```

* rel() takes the parent object size, e.g. `axis.text.x = element_size(size = rel(2))` it will look up to `axis.text` first, since `axis.text.x` inherits from `axis.text`. So if `axis.text` size is defined to be 11pt, `axis.text.x` size would be 11*2 = 22pt. (https://github.com/tidyverse/ggplot2/blob/e0d54f6d2be6452bbab61a5018abc231a3172ca6/R/theme-elements.r#L417)

#### 2022-07-06
Todo:
1. if legend text size is not defined
2. testthat unit and consider edge cases
3. axis text size function moves to animintHelper.R. Using vec or df?
Support rel() in axes text.

### Commits
- First PR on this feature, [commits for pr #66](https://github.com/tdhock/animint2/pull/66)
- Bugfix on this feature, [pr #69](https://github.com/tdhock/animint2/pull/69/commits)

### Reference
[D3 API Reference](https://devdocs.io/d3~5/)