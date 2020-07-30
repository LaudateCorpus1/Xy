[![Build Status](https://img.shields.io/travis/andrebleier/Xy/master.svg)](https://travis-ci.org/andrebleier/Xy)   [![codecov](https://codecov.io/github/andrebleier/Xy/branch/master/graphs/badge.svg)](https://codecov.io/github/andrebleier/Xy) 

Simulating Supervised Learning Data <img src="/img/Xy.png" alt="drawing" width="150px" align="right"/> 
===================================



With `Xy()` you can convienently simulate supervised learning data. The simulation can be
very specific, since there are many degrees of freedom for the user. For instance,
the functional shape of the nonlinearity is user-defined as well. Interactions can be formed and (co)variances altered.

There are numerous applications for the package, e.g.:

* Learning a new machine learning algorithm in a controlled environment
* Benchmarking feature selection algorithms in a controlled environment
* Create sample data to test productionized ML applications in a CI / CD pipeline.

## Usage

The usage is pretty straight forward. I strongly encourage you to read the help document to explore all functionalities.

### Install

Install the package with <code>remotes</code>:

```
# install.packages("remotes") 
# install from github
remotes::install_github("andrebleier/Xy")            
```

### Simulate data 

You can simulate regression and classification data with interactions and a user-specified non-linearity. The usage is in a tidy way. First you create a simulation recipe, which is a combination of the overall task invoked by `Xy()` . Afterwards effects can be added to the recipe with the `add_*` functions as can be seen in the example below. Finally the `simulate()` function cooks this recipe. 

```
# load the library
library(Xy)
# simulate regression data
task <- Xy(task = "regression")

# build the recipe
recipe <- task %>%
  # adding linear features
  add_linear(5) %>%
  # adding non-linear cubic features
  add_nonlinear(3, nlfun= function(x) x^3) %>%
  # add uninformative effects
  add_uninformative(p = 3, collinearity = TRUE, family = xy_normal()) %>%
  # add dummy variables
  add_discrete(p = 3, levels = 3) %>%
  # add normally distributed noise, which correlates with the features
  add_noise(collinearity = TRUE, family = xy_normal()) %>%
  # add an intercept
  add_intercept()
  
# cook the recipe
sim <- recipe %>%
  simulate(n = 100, r_squared = 0.8)

# fetch the data
sim %>% pull_xy()
```

### Feature Importance

You can extract a feature importance of your simulation. For instance, to benchmark feature selection algorithms.

```
# Feature Importance 
variable_importance <- sim %>% importance()
```
<img src="/img/imp.png" alt="drawing"/> 

Feel free to [contact](mailto:andre.bleier@statworx.com) me with input and ideas.

