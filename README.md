# STSOM: Structured Testing of Somatic Mutations in Association with Multiple Clinical Outcomes

[![License](https://img.shields.io/badge/license-LGPL--2.0-blue.svg)](https://www.gnu.org/licenses/old-licenses/lgpl-2.0.html)

## Overview

The **STSOM** package provides a score-based test for evaluating the association between somatic mutations and multivariate clinical outcomes, such as survival time and ancillary variables (continuous or binary). This package is especially useful for researchers conducting survival analysis and genetic association studies where both fixed and random variant effects are taken into account.

### Features:
- Score-based tests for multivariate outcomes, including survival time and ancillary variables.
- Handles both continuous and binary ancillary outcomes.
- Incorporates both fixed and random variant effects using genetic variant data.

## Installation

You can install the development version of **STSOM** from GitHub using the following commands:

```{r}
# Install devtools if necessary
install.packages("devtools")

# Install STSOM from GitHub
devtools::install_github("yourusername/STSOM")
```

## Usage 

### Example 1: Score Test for Association Between Somatic Mutations and Multivariate Outcomes

```{r}
# Load the example dataset
data("STSOM.data")

# Perform the STSOM test on the first dataset
out1 <- STSOM(dat1$U, dat1$delta, dat1$Y, dat1$X, dat1$G, dat1$W)
print(out1)

# Perform the STSOM test on the second dataset
out2 <- STSOM(dat2$U, dat2$delta, dat2$Y, dat2$X, dat2$G, dat2$W)
print(out2)
```


## Citation

If you use the **STSOM** package in your research, please cite:

Liu, M., Su, Y.R., Liu, Y., Hsu, L. and He, Q., 2024. Structured testing of genetic association with mixed clinical outcomes. Genetic Epidemiology.


## License

This package is licensed under the LGPL-2.0 License. See the [LICENSE](LICENSE) file for details.

## Authors

- **Meiling Liu** - [mliu@fredhutch.org](mailto:mliu@fredhutch.org)
