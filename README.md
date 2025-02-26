# STAMO: Structured Testing of Association with Mixed Outcomes 
[![License](https://img.shields.io/badge/license-LGPL--2.0-blue.svg)](https://www.gnu.org/licenses/old-licenses/lgpl-2.0.html)

## Overview

Genetic factors play a crucial role in disease development, but their low frequencies make it challenging to analyze individual variants. Additionally, clinical outcomes are complex, encompassing survival time and other binary or continuous outcomes such as recurrences and lymph node count, making it unclear how to effectively analyze their genetic associations.  To address these challenges, we propose Structured Testing of Association with Mixed Outcomes (**STAMO**), an approach that integrates known biological information of genetic variants while allowing for heterogeneous effects. This approach enables a more powerful and comprehensive analysis of genetic associations with mixed survival, binary, and continuous outcomes. Simulation studies demonstrate that our method maintains the correct type I error rate and is highly effective in identifying significant genetic variants ([https://onlinelibrary.wiley.com/doi/10.1002/gepi.22560](https://onlinelibrary.wiley.com/doi/10.1002/gepi.22560)). The approach was applied to a uterine corpus endometrial carcinoma study and successfully identified genetic pathways associated with clinical outcomes, highlighting the potential of the proposed method in uncovering disease-related genetic factors. 

The STAMO has been implemented into the **STAMO** package. This package provides a score-based test for evaluating associations between somatic mutations and multivariate clinical outcomes, including survival time and ancillary continuous or binary variables. This package is particularly valuable for researchers conducting survival analysis and genetic association studies, as it accounts for both fixed and random variant effects.

### Features:
- Score-based tests for multivariate outcomes, including survival time and ancillary variables.
- Handles both continuous and binary ancillary outcomes.
- Incorporates both fixed and random variant effects using genetic variant data.

## Installing the STAMO Package

You can install the **STAMO** package by downloading the `STAMO_0.1.0.tar.gz` file to your local system. Once downloaded, install it from your local file path using the following command in R:

```r
install.packages("path/to/STAMO_0.1.0.tar.gz", repos = NULL, type = "source")
```

## Usage 

### Example 1: Score Test for Association Between Somatic Mutations and Multivariate Outcomes

```{r}
library(STAMO)

# Load the example dataset
data("STAMO.data")

# visualize the datasets
str(dat1)
List of 6
 $ U    : num [1:500] 0.2718 0.053 0.0752 0.1733 0.9611 ...
 $ delta: num [1:500] 0 1 1 1 0 1 1 0 1 0 ...
 $ Y    : num [1:500] 1 0 0 0 0 1 0 1 0 1 ...
 $ X    : num [1:500, 1:2] 1 1 0 1 1 1 0 0 1 1 ...
 $ G    : int [1:500, 1:15] 0 0 0 0 0 0 0 1 0 0 ...
 $ W    : num [1:2, 1:15] 1 1 1 1 1 0 1 1 1 0 ...

str(dat2)
List of 6
  $ U    : num [1:500] 0.2713 0.0608 0.0752 0.3615 0.9611 ...
  $ delta: num [1:500] 1 1 1 1 0 1 1 0 1 0 ...
  $ Y    : num [1:500] 1.514 -0.491 -1.638 -1.066 -0.364 ...
  $ X    : num [1:500, 1:2] 1 1 0 1 1 1 0 0 1 1 ...
  $ G    : int [1:500, 1:15] 0 0 0 0 0 0 0 1 0 0 ...
  $ W    : num [1:2, 1:15] 1 1 1 1 1 0 1 1 1 0 ...
```
#### Structure of `dat1` and `dat2`

Both `dat1` and `dat2` are lists containing six components, each representing different aspects of the dataset:

- **`U`**: A numeric vector of censored failure times for each subject.
- **`delta`**: A numeric vector indicating censoring status (1 = event occurred, 0 = censored).
- **`Y`**: A numeric vector representing the ancillary variable. `Y` is **binary** in `dat1` and **continuous** in `dat2`. 
- **`X`**: A numeric matrix of covariates for each subject (excluding the intercept).
- **`G`**: A numeric matrix of genetic variants for each subject. Each row represents an individual, each column represents a genetic variant.
- **`W`**: A numeric matrix of variant annotations providing additional features related to the genetic variants. 

These datasets are designed for survival analysis and demonstrate different types of ancillary variables (`Y`), making them useful for evaluating methods that incorporate both binary and continuous variables in genetic association studies.

The STAMO test was performed on the first dataset (`dat1`), which includes survival outcomes and a binary ancillary variable.

```{r}
# Perform the STAMO test on the first dataset
out1 <- STAMO(dat1$U, dat1$delta, dat1$Y, dat1$X, dat1$G, dat1$W)

print(out1)
    pval.fix    pval.rand         pval
 1.915389e-05 6.712318e-04 2.464553e-07
```


The results from the STAMO test indicate a statistically significant association between the genetic variants and the multivariate clinical outcomes. The **pval.fix** value of **1.915389e-05** suggests that, under the assumption of a **fixed effect** (where the genetic variant has a consistent impact across all subjects), the association is highly significant. Similarly, the **pval.rand** value of **6.712318e-04** indicates that when allowing for **random effects** (where the variant effect varies across subjects), the association remains statistically meaningful. 

The overall **pval** of **2.464553e-07**, obtained by combining `pval.fix` and `pval.rand` using **Fisher’s procedure**, further strengthens the evidence of a robust association between somatic mutations and clinical outcomes. These results suggest that the genetic variants analyzed in this study may play a significant role in influencing survival and other related clinical factors, warranting further investigation into their biological relevance.

The STAMO test was performed on the first dataset (`dat1`), which includes survival outcomes and a continuous ancillary variable.

```{r}
# Perform the STAMO test on the second dataset
out2 <- STAMO(dat2$U, dat2$delta, dat2$Y, dat2$X, dat2$G, dat2$W)

print(out2)
 pval.fix  pval.rand       pval
0.02044900 0.43378911 0.05078407
```

The STAMO test results for the second dataset (`dat2`), which includes survival outcomes and a **continuous ancillary variable**, suggest a moderate association between genetic variants and clinical outcomes. The **pval.fix** value of **0.02044900** indicates a statistically significant association under the **fixed effect assumption**, suggesting that the genetic variant has a consistent impact across all subjects. However, the **pval.rand** value of **0.43378911** is not statistically significant, implying that when allowing for **random effects**, where the variant effect varies between subjects, the association is weak or absent. The overall **pval** of **0.05078407**, which combines `pval.fix` and `pval.rand` using **Fisher’s procedure**, is **borderline significant**, hovering around the conventional 0.05 threshold. This suggests that the genetic variant may have a weak but potentially meaningful association with the multivariate clinical outcomes. The discrepancy between `pval.fix` and `pval.rand` implies that the effect of the genetic variant is likely **more uniform across subjects** rather than highly variable. Given the borderline significance of the overall p-value, further validation through additional studies is needed to determine the biological relevance of this association.


## Citation

If you use the **STAMO** package in your research, please cite:

Liu, M., Su, Y.R., Liu, Y., Hsu, L. and He, Q., 2024. Structured testing of genetic association with mixed clinical outcomes. Genetic Epidemiology.


## License

This package is licensed under the LGPL-2.0 License. See the [LICENSE](LICENSE) file for details.

## Authors

- **Meiling Liu** - [mliu@fredhutch.org](mailto:mliu@fredhutch.org)
