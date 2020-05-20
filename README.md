## phyloglmstep: R package for stepwise selection of phyloglm models

The package provides a single method, `phyloglmstep`, which provides AIC-based
selection of phylogenetic generalized linear models. As such, it is a simple
extension on the [phylolm](https://github.com/lamho86/phylolm) package and 
provides behaviour analogous to the `phylolm::phylostep` function.

<!--

### Installation

Install `phyloglmstep` from `github`:
```{r}
devtools::install_github("naturalis/phyloglmstep")
```

-->

### Usage

To run a model selection, the workflow is as follows:

```{r}
library(phylolm)

# This is when used locally. One of the tasks is now to be able to do
# library(phyloglmstep) instead.
source('R/phyloglmstep.R')

# Likewise, this should be changed so that these data sets can be loaded from
# the package as data() calls
load(file='data/woodyTree.rda')
load(file='data/woodyData.rda')

# The full model
woody_formula_24a <- woody ~ Aspect + BulkDensity + ClayPercentage + monthCountByTemp10 + PETDriestQuarter + PETWettestQuarter + OrganicCarbon + PhCaCL + Slope + bio2 + bio5 + bio14 + bio15 + bio18 + bio19

# And now do the call:
phyloglmstep(woody_formula_24a, starting.formula = NULL, data=GIS_woody, phy=tree_woody, 
                         method="logistic_MPLE",
                         direction = "forward", trace = 2,
                         btol = 100, log.alpha.bound = 8, start.beta=NULL, 
                         start.alpha=NULL, boot = 0, full.matrix = TRUE,
                         k=2)

```

Once the call is made, a number of steps are taken, to finally result in:

```
Call:
phyloglm(formula = create.formula(plm), data = data, phy = phy, 
    method = method, btol = btol, log.alpha.bound = log.alpha.bound, 
    start.beta = start.beta, start.alpha = start.alpha, boot = boot, 
    full.matrix = full.matrix)
       AIC     logLik Pen.logLik 
     734.0     -358.0     -343.9 

Parameter estimate(s) from MPLE:
alpha: 5.292188 

Coefficients:
       (Intercept) monthCountByTemp10  PETWettestQuarter      OrganicCarbon             PhCaCL              Slope               bio5              bio14 
         1.8505486         -2.1249685          0.2921476          1.4043225         -1.1672455         -1.0020464          1.3392674         -0.2315700 
```