---
layout: post
title: Create a testthat unit 
subtitle: Create a testthat unit for Newton's Method demo.
gh-repo: Faye-yufan/animint2_Test
gh-badge: [star, fork, follow]
tags: [test]
comments: true
---

This is a sample to show how to write a testthat unit test based on one of Animint visualizations.

{: .box-note}
**Prerequisite:** 
<br/>- Docker and Selenium Docker Image(selenium/standalone-firefox-debug:2.53.0)
<br/>- animint2 Package
<br/>- VNC viewer(If you are using MAC or Windows)
{: .box-note}

<br/>
## YOUR-TEST.R

Here's the code for Newton's Method:

```R
newton.method = function(
  FUN = function(x) x^2 - 4, init = 10, rg = c(-1, 10), tol = 0.001, nmax=50
) {
  i = 1
  nms = names(formals(FUN))
  grad = deriv(as.expression(body(FUN)), nms, function.arg = TRUE)
  x = c(init, init - FUN(init)/attr(grad(init), 'gradient'))
  gap = FUN(x[2])
  fx = FUN(x[1])
  
  while (abs(gap) > tol & i <= nmax & !is.na(x[i + 1])) {
    
    gap = FUN(x[i + 1])
    x = c(x, x[i + 1] - FUN(x[i + 1])/attr(grad(x[i + 1]), 'gradient'))
    fx <- c(fx, FUN(x[i + 1]))
    i = i + 1
    
    if (i > nmax) warning('Maximum number of iterations reached!')
  }
  
  x.temp = seq(rg[1], rg[2], length = abs(rg[2]-rg[1])/0.1)
  curve = data.frame(x=x.temp)
  curve$fx = as.numeric(lapply(curve$x, FUN))
  
  fx <- c(fx, FUN(x[i + 1]))
  objective <- data.frame(iteration = 1:(i+1), x = x, fx = fx)
  invisible(
    list(curve = curve, objective = objective)
  )
}
```

Sample renderer test code:

```R
# Test of needed data columns in TSV files
chunk1.tsv <- file.path("animint-htmltest", "geom1_hline_contour_chunk1.tsv")
chunk1 <- read.table(chunk1.tsv, sep="\t", header=TRUE,
                     comment.char="", quote="")
```

<br/>
## Starting selenium docker image

{: .box-note}
**Note:** I use native Screen Sharing app on Mac as VNC.

Pull the image:
```bash
docker pull selenium/standalone-firefox-debug:2.53.0
```

Starting selenium docker image:
```bash
docker run -d -p 4444:4444 -p 5900:5900 selenium/standalone-firefox-debug:2.53.0
```
where ```4444:4444``` is for Selenium, and ```5900:5900``` is for VNC server.

<br/>

## Load libraries
You need to change working directory to tests directory.
```bash
library("testthat")
library("animint2")
library("RSelenium");library("XML")
setwd("testthat")
source("helper-functions.R")
source("helper-HTML.R")
source("helper-plot-data.r")
```
<br/>

## Start a Firefox browser and run test
Start a firefox browser instance on the selenium server.
```bash
tests_init("firefox")
```

We use the function ```tests_run()``` here.
```bash
tests_run() 
```