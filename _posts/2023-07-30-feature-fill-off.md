---
layout: post
title: Enhancing Data Visualization with the `fill_off` Feature
subtitle: New feature
gh-repo: animint/animint2
gh-badge: [star, fork, follow]
tags: [pr, contribution, gsoc2023]
comments: true
---

## Motivation
After `colour_off` feature implemented, here is the introduction of the `fill_off` feature.

## The Genesis: `colour_off/color_off`

Before diving into the `fill_off` feature, it's essential to understand its predecessor: the `colour_off/color_off` feature. This functionality enables users to specify the color, outline, or stroke of the non-selected part of the data. It's a subtle yet powerful way to highlight specific data points while dimming out the rest, ensuring that the viewer's attention is drawn to the right places.

For instance, consider a visualization using `geom_point`:

```R
geom_point(
  data = mtcars,
  aes(x=wt, y=mpg, fill = disp),
  clickSelects = "gear"),
  colour="red",  # selected
  colour_off="transparent",  # not selected
)
```
Here, the `colour` parameter specifies the color of the selected data, while `colour_off` determines the color of the data that are not selected.

## Introducing `fill_off`
Building on the success and utility of `colour_off`, the `fill_off` feature was introduced. 
This new feature allows users to specify the fill of the non-selected parts of the data. 
It's especially useful in visualizations where the fill color plays a significant role in conveying information.

Using the `fill_off` feature, a visualization might look something like this:
```R
geom_point(
  data = mtcars,
  aes(x=wt, y=mpg, fill = disp),
  clickSelects = "gear"),
  fill="blue",  # selected
  fill_off="transparent",  # not selected
)
```

## Combined Power: Using `colour_off`, `alpha_off`, and `fill_off` Together
One of the most exciting developments is the ability to use `colour_off`, `alpha_off`, and `fill_off` jointly. 
This combination offers a richer set of visual cues for data selection, allowing for more nuanced visualizations.
