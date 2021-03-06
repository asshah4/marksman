
<!-- README.md is generated from README.Rmd. Please edit that file -->

# octomod

<!-- badges: start -->

[![Lifecycle:
experimental](https://img.shields.io/badge/lifecycle-experimental-orange.svg)](https://www.tidyverse.org/lifecycle/#experimental)
[![Github commit
frequency](https://img.shields.io/github/commit-activity/w/asshah4/octomod)](https://github.com/asshah4/octomod/graphs/commit-activity)
[![R-CMD-check](https://github.com/asshah4/octomod/workflows/R-CMD-check/badge.svg)](https://github.com/asshah4/octomod/actions)

<!-- badges: end -->

## Overview

The `octomod` package provides a simple and tidy way to organize the
many models built during a research project. The package consists of
individual hypothesis objects that are all linked together around a
single data set. In causality-based research, such as epidemiology, the
focus is on specific hypothesis that can be tested. However, the number
of models and tests can become unruly. This serves as a way to organize
the many.

## Story

The reason why this was developed, as listed above, was based on the
struggle of trying to hold many hypotheses together for a thesis
project. The multiple datasets, the multiple inferential and supervised
learning models, and the gaps in times before revisiting areas created
this sticky situation.

The *metaphor* that seemed to fit the best was that of the *octopus*, in
that it was a central breathing organism, with multiple arms. The
metaphor expanded that to modeling, in that each arm is a family of
hypotheses encircling a central data structure. The end of each arm
could be equipped with the test findings. If an arm needed to be removed
or revised, it should be easy to regenerate without effecting the hole.

Forgive the misdirected creativity, but as the metaphor seemed to hold
true, the name stuck. Thus, the `octomod` was made, armed and ready to
handle any number of research questions.

## Usage

The package is simple to use. First, lets load the basic packages. The
two hypotheses in this case are a linear model, and a paired t-test. The
`iris` dataset will serve as the example.

You can see that `parsnip` is used, as this allows **many** model
specifications to be called in a very similar manner, making modeling
much easier.

``` r
library(magrittr)
library(parsnip)
```

To use the `octomod`, simply initialize the structure and add data. The
steps are:

1.  Create the `octomod`
2.  Add the core or central data that the hypotheses revolve around
    using `core()`
3.  Add the specific hypothetical arms with `arm()`
4.  Equip or outfit each arm with the appropriate test results with
    `equip()`

``` r
library(octomod)
om <-
  octomod() %>%
  core(iris) %>%
  arm(
    title = "t_test",
    plan = Sepal.Length + Sepal.Width ~ Petal.Length,
    pattern = "direct",
    approach = "t.test",
    paired = TRUE
  ) %>%
  arm(
    title = "linear",
    plan = Petal.Width ~ Sepal.Length + Sepal.Width + Petal.Length,
    pattern = "sequential",
    approach = linear_reg() %>% set_engine("lm")
  ) %>%
    equip()

# Showcase the findings
om$equipment
#> $t_test
#> # A tibble: 2 x 6
#>   outcomes     test_num vars      formulas  fit     tidied              
#>   <chr>           <int> <list>    <list>    <list>  <list>              
#> 1 Sepal.Length        1 <chr [1]> <formula> <htest> <tibble[,8] [1 × 8]>
#> 2 Sepal.Width         2 <chr [1]> <formula> <htest> <tibble[,8] [1 × 8]>
#> 
#> $linear
#> # A tibble: 3 x 6
#>   outcomes    test_num vars      formulas  fit      tidied              
#>   <chr>          <int> <list>    <list>    <list>   <list>              
#> 1 Petal.Width        1 <chr [1]> <formula> <fit[+]> <tibble[,7] [2 × 7]>
#> 2 Petal.Width        2 <chr [2]> <formula> <fit[+]> <tibble[,7] [3 × 7]>
#> 3 Petal.Width        3 <chr [3]> <formula> <fit[+]> <tibble[,7] [4 × 7]>
```

The functions are relatively simple, but the ability to specify **how**
the relationship between outcomes and exposures should be built is quite
powerful. It allows a different combination of hypothesis-based formulas
to be built, and allows for multiple outcomes to specified (a feature
not yet built into other modeling packages). Please see the vignettes
for a more thorough breakdown!
