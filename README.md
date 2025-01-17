
<!-- README.md is generated from README.Rmd. Please edit that file -->

# CoxBoost

<!-- badges: start -->

[![R-CMD-check](https://github.com/jemus42/CoxBoost/actions/workflows/R-CMD-check.yaml/badge.svg)](https://github.com/jemus42/CoxBoost/actions/workflows/R-CMD-check.yaml)
<!-- badges: end -->

CoxBoost fits Cox proportional hazards models by componentwise
likelihood based boosting. It is especially suited for models with a
large number of predictors and allows for mandatory covariates with
unpenalized parameter estimates.

## Installation

You can install the development version of CoxBoost from
[GitHub](https://github.com/) with:

``` r
# install.packages("pak")
pak::pak("binderh/CoxBoost")
```

## Example

A new feature that currently is only available in the version here, but
not on CRAN, is weighted stratified regression for dealing with known
subgroups. In particular, the new function `resample.CoxBoost` (and
associated plotting routines `stabtrajec` and `weightfreqmap`) allows to
explore different weightings of subgroups. The plotting routines help to
understand which covariates might be relevant just for one subgroup, or
relevant to some degree for more subgroups. The following is a usage
example, adapted from the package help page of `resample.CoxBoost`:

``` r
library(CoxBoost)
n <- 400
p <- 1000
set.seed(129)
group <- rbinom(n, 1, 0.5)
x <- matrix(rnorm(n * p, 0, 1), n, p)
beta.vec1 <- c(c(1, 1, 1, 1, 1), rep(0, p - 5))
beta.vec0 <- c(c(0, 0, 0, 0, 0), rep(0, p - 5))
linpred <- ifelse(group == 1, x %*% beta.vec1, x %*% beta.vec0)
set.seed(1234)
real.time <- (-(log(runif(n))) / (1 / 20 * exp(linpred)))
cens.time <- rexp(n, rate = 1 / 20)
obs.status <- ifelse(real.time <= cens.time, 1, 0)
obs.time <- ifelse(real.time <= cens.time, real.time, cens.time)

RIF <- resample.CoxBoost(
  time = obs.time, status = obs.status, x = x, rep = 100, maxstepno = 200, multicore = FALSE,
  mix.list = c(0.001, 0.01, 0.05, 0.1, 0.25, 0.35, 0.5, 0.7, 0.9, 0.99),
  stratum = group, stratnotinfocus = 0, penalty = sum(obs.status) * (1 / 0.02 - 1),
  criterion = "hscore", unpen.index = NULL
)

stabtrajec(RIF)
weightfreqmap(RIF)
```
