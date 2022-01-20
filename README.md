
<!-- README.md is generated from README.Rmd. Please edit that file -->

# CytOpT

<!-- badges: start -->

[![R-CMD-check](https://github.com/sistm/CytOpt-R/workflows/R-CMD-check/badge.svg)](https://github.com/sistm/CytOpt-R/actions)
<!-- badges: end -->

`CytOpT` uses regularized optimal transport to directly estimate the
different cell population proportions from a biological sample
characterized with flow cytometry measurements.

## Overview

`CytOpT` is an `R` package that provides a new algorithm relying
regularized optimal transport to directly estimate the different cell
population proportions from a biological sample characterized with flow
cytometry measurements. Algorithm is based on the regularized
Wasserstein metric to compare cytometry measurements from different
samples, thus accounting for possible mis-alignment of a given cell
population across sample (due to technical variability from the
technology of measurements).

The main function of the package is `CytOpT()`.

The methods implemented in this package are detailed in the following
article:

> Paul Freulon, Jérémie Bigot, Boris P. Hejblum. CytOpT: Optimal
> Transport with Domain Adaptation for Interpreting Flow Cytometry data
> <https://arxiv.org/abs/2006.09003>

## Installation

You can install the development version of CytOpT like so:

``` r
remotes::install_github("sistm/CytOpT-R")
library(CytOpT)
```

## Example

This is a basic example of `CytOpt` usage:

### Data import

``` r
library(CytOpT)
#> Warning: remplacement de l'importation précédente 'data.table::melt' par
#> 'reshape2::melt' lors du chargement de 'CytOpT'
# Load source Data
X_source <- read.csv('tests/ressources/W2_1_values.csv')
Lab_source <- read.csv('tests/ressources/W2_1_clust.csv')[, 2]

# Load target Data
X_target <- read.csv('tests/ressources/W2_7_values.csv')
Lab_target <- read.csv('tests/ressources/W2_7_clust.csv')[, 2]
```

``` r
# Define the true proportions in the target data set X_source
theta_true <- rep(0,10)
for (k in 1:10) theta_true[k] <- sum(Lab_target == k)/length(Lab_target)
```

### Proportion estimations using *optimal transport* and *minmax swapping* procedures

``` r
# Run CytOpt and compare two optimisation methods
res <- CytOpT(X_source, X_target, Lab_source, theta_true=theta_true, method='both')
```

``` r
res$h_hat
#> NULL
res$true_proportions
#> NULL
```

``` r
Bland_Atlman(proportions = res$proportions)
```

<img src="man/figures/README-example BA plot-1.png" width="100%" />
