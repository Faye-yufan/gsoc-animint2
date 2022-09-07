---
layout: post
title: New feature – color-off
subtitle: user configurable selection style
gh-repo: tdhock/animint2
gh-badge: [star, fork, follow]
tags: [pr, contribution, gsoc2022]
comments: true
---

### Summary
The feature `colour_off`/ `color_off` enables user to specify the color/outline/stroke of the non-selected part of data. 

Here are some examples using colour_off, for [geom_line](http://bl.ocks.org/Faye-yufan/raw/e45611c3e2cd22a5be54c5e0011ee39a/) and [geom_point](http://bl.ocks.org/Faye-yufan/raw/90baf635a161b5ed84f13990d04e75c6/). For the second geom_point example, the R code would be something like this:
```R
geom_point(...
data = mtcars,
  aes(x=wt, y=mpg, fill = disp),
  clickSelects = "gear"),
  colour="red",  # selected
  colour_off="transparent",  # not selected
...)
```
where param `colour` specifies the color/outline/stroke of the selection and `colour_off` stands for the outline color of data that are not selected. 

In this PR, I also changed some functions to support using `colour_off` and `alpha_off` jointly, see the examples above,  which probably would be easier to implement further selection styles, such as `fill_off`.

### Motivation
```R
debug(getLegendList)
debug: plot <- plistextra$plot
Browse[2]> plot
Warning message:
In guide_train.colorbar(guide, scale) :
  colorbar guide needs colour or fill scales.
```

  When click the variables, in the JS side, it calls `update_selector()` --> `update_geom()` --> `draw_panels()` --> `draw_geom()`
  
  when call the `path/line` geom, the select variable panel would not work for the `color_off`. I think it probably because each time it was selected, it would call the `draw_geom`, and path geom is a special category of geom, so it would update the selection with the `colour` param.
  
  - How to define multiple selection style?
  	- use array or object
  - How to pass `color_off` as hex color code to json?
    - compute it in compiler, there is a function called `toRGB`.
  - `clickSelects` originally bind with legend opacity change. `update_legend_opacity` function should also be changed, add a new `update_legend_color`, or create a general function `update_legend_aes`?
  - Future work: There is no way to avoid this?
```
  if(g_info.select_style != "stroke"){
          e.style("stroke", get_colour);
        }
```
Since the `e.style()` cannot distinguish what is selected and what is not.
  
### Commits
- major part of the [commits in pr #72](https://github.com/tdhock/animint2/pull/72/commits)
- fix bugs in [pr #78](https://github.com/tdhock/animint2/pull/78/commits)