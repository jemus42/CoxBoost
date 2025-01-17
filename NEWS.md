# CoxBoost 1.5

- Initial CRAN submission.

## Verison 1.4

-   added a formula interface through 'iCoxBoost'
-   added generic function 'coef' for extracting estimated coefficients
-   added a plot routine that provides coefficient paths
-   added support for package 'parallel' (removing support for 'multicore' and older R versions)
-   convergence problems for unpenalized covariates now are catched


## Verison 1.3

-   added option 'criterion' to allow for selection according to unpenalized scores
-   added 'criterion="hpscore"' and 'criterion="hscore"' for heuristic evaluation
    of only a subset of covariates in each boosting step
-   Fixed a bug where results from "predict" without "newdata" and "linear.predictor"
    in CoxBoost objects would have the wrong order (introduced in 1.2-1)
-   added missing value check for covariate matrix
-   implemented observation weights


## Verison 1.2-2

-   fixed a bug in the predict function ocurred when all coefficients
    were equal to zero
-   fixed bug where 'estimPVal' would with only one boosting step
-   'estimPVal' now also works for zero boosting steps


## Verison 1.2-1

-   improved speed of the core selection routine
-   added faster code for the special case of binary covariate data
-   added an option for not returning the matrix with the score statistics
    for saving memory in applications with a huge number of covariates
-   optimized memory usage for a large number of covariates
-   covariates with standard deviation equal to zero now only are centered
-   a matrix of the employed penalties know is only stored if the penalties,
    changed. Otherwise the 'element' penalty is just a vector
-   added support for 'multicore' package for cross-validation and p-value
    estimation
-   added an option for fitting on subsets of observations
-   The coefficient matrix is now stored as a sparse matrix, employing
    package 'Matrix'
-   fixed the implementation of the p-value estimation


## Verison 1.2

-   added function 'estimPVal' for permutation-based p-value estimation
-   improved the speed of the penalty updating code in PathBoost


## Verison 1.1-1

-   fixed bug in print method (introduced in 1.0-1) where the number of
    non-zero coefficients would be taken from a wrong boosting step


## Verison 1.1

-   implemented penalty modification factors and penalty change distribution
    via a connection matrix
-   implemented estimation of models for competing risks

## Verison 1.0-1

-   implemented data adaptive rule for default penalty value
-   fixed bug where output of the selected covariate would print the
    wrong name in presence of unpenalized covariates
-   Boosting now starts a step 0, i.e., also the model before updating
    any of the coefficients of the penalized covariates is considered.
    However, the unpenalized covariates will already have non-zero
    values in boosting step 0.
    This change breaks code that relies on the size of elements
    "coefficients", "linear.predictors", or "Lambda" of CoxBoost objects
-   implements parallel evaluation of cross-validation folds,
    via package 'snowfall'
-   speed improvements by replacing 'apply' and 'rbind' , most noticeably 
    for a large number of observations with a small number of covariates


## Verison 1.0

* initial public release
