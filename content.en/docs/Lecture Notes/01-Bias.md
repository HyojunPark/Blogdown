---
title: Survival Analysis - Bias
weight: 1
---

# Bias assessment




The maps demonstrate the geographic gradients of obesity rate in the state of Utah. The left panel is estimates at the census tract level, while the right panel is estimates at the county level.


\newpage
## Bias due to omitted confounders

`$$y_i = \beta_0 + \beta_1 x_i + \beta_2 x_2 + \dots + \epsilon_i; \;\; for \;\; i=1, \dots, n$$`


where the errors `\(\epsilon_i \sim N(0, \sigma^2)\)` with independent and identically distributed (*i.i.d.*)

Let's assume the following association is true (i.e., gold standard) without any selection bias, measurement bias, and other unmeasured confoundings.


```r
N <- 100000
C <- rnorm(N)
X <- .5 * C + rnorm(N)
Y <- .3 * C + .4 * X + rnorm(N)
```


### Gold standard

With the correct model specification (i.e., `\(C\)` as a confounder), we get an unbiased estimate of `\(X\)` on `\(Y\)`.



```r
# Gold standard
glm.unbiased <- glm(Y~X + C, family="gaussian")
summary(glm.unbiased)
```

```
## 
## Call:
## glm(formula = Y ~ X + C, family = "gaussian")
## 
## Coefficients:
##               Estimate Std. Error t value Pr(>|t|)    
## (Intercept) -0.0006523  0.0031495  -0.207    0.836    
## X            0.4003488  0.0031472 127.208   <2e-16 ***
## C            0.2967375  0.0035189  84.327   <2e-16 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 0.9918467)
## 
##     Null deviance: 139902  on 99999  degrees of freedom
## Residual deviance:  99182  on 99997  degrees of freedom
## AIC: 282974
## 
## Number of Fisher Scoring iterations: 2
```


### Misspecified model: a confounder, `\(C\)`, was omitted from the model

By omitting `\(C\)`, the estimate of `\(X\)` was biased either "away from" or "towards to" the null


```r
# C was omitted
glm.unbiased <- glm(Y~X, family="gaussian")
summary(glm.unbiased)
```

```
## 
## Call:
## glm(formula = Y ~ X, family = "gaussian")
## 
## Coefficients:
##               Estimate Std. Error t value Pr(>|t|)    
## (Intercept) -0.0004492  0.0032595  -0.138     0.89    
## X            0.5188395  0.0029145 178.020   <2e-16 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 1.062369)
## 
##     Null deviance: 139902  on 99999  degrees of freedom
## Residual deviance: 106235  on 99998  degrees of freedom
## AIC: 289842
## 
## Number of Fisher Scoring iterations: 2
```

### Bias "away from" or "towards to" the null?


```r
N <- 100000
C <- rnorm(N)
X <- -.5 * C + rnorm(N)
Y <- -.3 * C + .4 * X + rnorm(N)
```


```r
# C was omitted
glm.unbiased <- glm(Y~X + C, family="gaussian")
summary(glm.unbiased)
```

```
## 
## Call:
## glm(formula = Y ~ X + C, family = "gaussian")
## 
## Coefficients:
##              Estimate Std. Error t value Pr(>|t|)    
## (Intercept) -0.002241   0.003160  -0.709    0.478    
## X            0.400445   0.003134 127.755   <2e-16 ***
## C           -0.296551   0.003535 -83.884   <2e-16 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 0.9986744)
## 
##     Null deviance: 140617  on 99999  degrees of freedom
## Residual deviance:  99864  on 99997  degrees of freedom
## AIC: 283660
## 
## Number of Fisher Scoring iterations: 2
```



```r
glm.unbiased <- glm(Y~X, family="gaussian")
summary(glm.unbiased)
```

```
## 
## Call:
## glm(formula = Y ~ X, family = "gaussian")
## 
## Coefficients:
##              Estimate Std. Error t value Pr(>|t|)    
## (Intercept) -0.002889   0.003269  -0.884    0.377    
## X            0.516677   0.002909 177.625   <2e-16 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 1.068938)
## 
##     Null deviance: 140617  on 99999  degrees of freedom
## Residual deviance: 106892  on 99998  degrees of freedom
## AIC: 290458
## 
## Number of Fisher Scoring iterations: 2
```

### A `\(C\)` is not a confounder on `\(X\)` and `\(Y\)`


```r
N <- 100000
C <- rnorm(N)
X <- rnorm(N)
Y <- .4 * X + rnorm(N)
```


### Correct model specification: Without `\(C\)`


```r
glm.unbiased <- glm(Y~X, family="gaussian")
summary(glm.unbiased)
```

```
## 
## Call:
## glm(formula = Y ~ X, family = "gaussian")
## 
## Coefficients:
##              Estimate Std. Error t value Pr(>|t|)    
## (Intercept) -0.001127   0.003163  -0.356    0.722    
## X            0.401433   0.003162 126.950   <2e-16 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 1.00076)
## 
##     Null deviance: 116202  on 99999  degrees of freedom
## Residual deviance: 100074  on 99998  degrees of freedom
## AIC: 283868
## 
## Number of Fisher Scoring iterations: 2
```


### Misspecified model with `\(C\)`


```r
glm.unbiased <- glm(Y~X + C, family="gaussian")
summary(glm.unbiased)
```

```
## 
## Call:
## glm(formula = Y ~ X + C, family = "gaussian")
## 
## Coefficients:
##              Estimate Std. Error t value Pr(>|t|)    
## (Intercept) -0.001124   0.003164  -0.355    0.722    
## X            0.401434   0.003162 126.949   <2e-16 ***
## C           -0.001240   0.003157  -0.393    0.694    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 1.000768)
## 
##     Null deviance: 116202  on 99999  degrees of freedom
## Residual deviance: 100074  on 99997  degrees of freedom
## AIC: 283870
## 
## Number of Fisher Scoring iterations: 2
```


### A `\(C\)` is a colloder on `\(X\)` and `\(Y\)`


```r
N <- 100000
X <- rnorm(N)
Y <- .7 * X + rnorm(N)
C <- 1.2 * X + .6 * Y + rnorm(N)
```



### Correct model specification: Without `\(C\)`


```r
glm.unbiased <- glm(Y~X, family="gaussian")
summary(glm.unbiased)
```

```
## 
## Call:
## glm(formula = Y ~ X, family = "gaussian")
## 
## Coefficients:
##               Estimate Std. Error t value Pr(>|t|)    
## (Intercept) -0.0007421  0.0031643  -0.235    0.815    
## X            0.6972677  0.0031531 221.134   <2e-16 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 1.001257)
## 
##     Null deviance: 149085  on 99999  degrees of freedom
## Residual deviance: 100124  on 99998  degrees of freedom
## AIC: 283917
## 
## Number of Fisher Scoring iterations: 2
```


### Misspecified model with `\(C\)`

This is one of examples of selection bias. For example, let's say, `\(X\)` is Education, `\(Y\)` is income, and `\(C\)` is social welfare program. People at lower education (i.e., high risk group in terms of exposure) and lower income (i.e., higher risk group in terms of outcome) are more likely to register social welfare program. If survey was conducted based on the registered social welfare program, the "estimated" association from this "disproportionally selected" respondents are likely biased.



```r
glm.unbiased <- glm(Y~X + C, family="gaussian")
summary(glm.unbiased)
```

```
## 
## Call:
## glm(formula = Y ~ X + C, family = "gaussian")
## 
## Coefficients:
##               Estimate Std. Error t value Pr(>|t|)    
## (Intercept) -0.0008762  0.0027144  -0.323 0.746832    
## X           -0.0167518  0.0046387  -3.611 0.000305 ***
## C            0.4416978  0.0023312 189.471  < 2e-16 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 0.7367652)
## 
##     Null deviance: 149085  on 99999  degrees of freedom
## Residual deviance:  73674  on 99997  degrees of freedom
## AIC: 253244
## 
## Number of Fisher Scoring iterations: 2
```




## Overadjustment bias

Please note that this is not a comprehensive example; only reflect one aspect of potential overadjustement bias.

Let's assume a model with `\(M\)` as a mediator.


```r
N <- 100000
X <- rnorm(N)
M <- .5 * X + rnorm(N)
Y <- .3 * X + .4 * M + rnorm(N)
```


## Total effect


```r
glm.unbiased <- glm(Y~X, family="gaussian")
summary(glm.unbiased)
```

```
## 
## Call:
## glm(formula = Y ~ X, family = "gaussian")
## 
## Coefficients:
##              Estimate Std. Error t value Pr(>|t|)    
## (Intercept) -0.000780   0.003396   -0.23    0.818    
## X            0.497084   0.003391  146.59   <2e-16 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 1.152897)
## 
##     Null deviance: 140061  on 99999  degrees of freedom
## Residual deviance: 115287  on 99998  degrees of freedom
## AIC: 298019
## 
## Number of Fisher Scoring iterations: 2
```

## Overadjustment


```r
glm.unbiased <- glm(Y~X + M, family="gaussian")
summary(glm.unbiased)
```

```
## 
## Call:
## glm(formula = Y ~ X + M, family = "gaussian")
## 
## Coefficients:
##              Estimate Std. Error t value Pr(>|t|)    
## (Intercept) 0.0007318  0.0031483   0.232    0.816    
## X           0.2984062  0.0035077  85.071   <2e-16 ***
## M           0.4010253  0.0031391 127.752   <2e-16 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for gaussian family taken to be 0.9911427)
## 
##     Null deviance: 140061  on 99999  degrees of freedom
## Residual deviance:  99111  on 99997  degrees of freedom
## AIC: 282903
## 
## Number of Fisher Scoring iterations: 2
```


## Logistic models

### Sex as a Confounder, `\(C\)`


```r
MYY <- data.frame(Sex = "Male",
                  Smoking = "Yes",
                  Cancer = 1,
                  freq = 5
                  )

MYN <- data.frame(Sex = "Male",
                  Smoking = "Yes",
                  Cancer = 0,
                  freq = 8
                  )

MNY <- data.frame(Sex = "Male",
                  Smoking = "No",
                  Cancer = 1,
                  freq = 45
                  )

MNN <- data.frame(Sex = "Male",
                  Smoking = "No",
                  Cancer = 0,
                  freq = 72
                  )


FYY <- data.frame(Sex = "Female",
                  Smoking = "Yes",
                  Cancer = 1,
                  freq = 25
                  )

FYN <- data.frame(Sex = "Female",
                  Smoking = "Yes",
                  Cancer = 0,
                  freq = 10
                  )

FNY <- data.frame(Sex = "Female",
                  Smoking = "No",
                  Cancer = 1,
                  freq = 25
                  )

FNN <- data.frame(Sex = "Female",
                  Smoking = "No",
                  Cancer = 0,
                  freq = 10
                  )

Ex_confounder <- rbind(MYY, MYN, MNY, MNN, FYY, FYN, FNY, FNN)
```


Convert Freq table to raw data


```r
library(tidyr)
raw_confounder <- Ex_confounder %>% 
  uncount(freq)
```




```r
glm.unbiased <- glm(Cancer ~ Smoking , family=binomial(link = "logit"), data=raw_confounder)
summary(glm.unbiased)
```

```
## 
## Call:
## glm(formula = Cancer ~ Smoking, family = binomial(link = "logit"), 
##     data = raw_confounder)
## 
## Coefficients:
##             Estimate Std. Error z value Pr(>|z|)  
## (Intercept)  -0.1582     0.1627  -0.972   0.3309  
## SmokingYes    0.6690     0.3397   1.970   0.0489 *
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for binomial family taken to be 1)
## 
##     Null deviance: 277.26  on 199  degrees of freedom
## Residual deviance: 273.28  on 198  degrees of freedom
## AIC: 277.28
## 
## Number of Fisher Scoring iterations: 4
```


- Full model:


```r
glm_logit <- glm(Cancer ~ Smoking + Sex , family=binomial(link = "logit"), data=raw_confounder)
glm_logit
```

```
## 
## Call:  glm(formula = Cancer ~ Smoking + Sex, family = binomial(link = "logit"), 
##     data = raw_confounder)
## 
## Coefficients:
## (Intercept)   SmokingYes      SexMale  
##   9.163e-01    4.266e-15   -1.386e+00  
## 
## Degrees of Freedom: 199 Total (i.e. Null);  197 Residual
## Null Deviance:	    277.3 
## Residual Deviance: 257 	AIC: 263
```


- Stratified models


```r
## For males
raw_confounder_M <- raw_confounder[ which(raw_confounder$Sex=='Male'), ]
glm_logit_m <- glm(Cancer ~ Smoking , family=binomial(link = "logit"), data=raw_confounder_M)
glm_logit_m
```

```
## 
## Call:  glm(formula = Cancer ~ Smoking, family = binomial(link = "logit"), 
##     data = raw_confounder_M)
## 
## Coefficients:
## (Intercept)   SmokingYes  
##  -4.700e-01    6.672e-16  
## 
## Degrees of Freedom: 129 Total (i.e. Null);  128 Residual
## Null Deviance:	    173.2 
## Residual Deviance: 173.2 	AIC: 177.2
```

```r
# For females
raw_confounder_F <- raw_confounder[ which(raw_confounder$Sex=='Female'), ]
glm_logit_f <- glm(Cancer ~ Smoking , family=binomial(link = "logit"), data=raw_confounder_F)
glm_logit_f
```

```
## 
## Call:  glm(formula = Cancer ~ Smoking, family = binomial(link = "logit"), 
##     data = raw_confounder_F)
## 
## Coefficients:
## (Intercept)   SmokingYes  
##   9.163e-01    9.400e-16  
## 
## Degrees of Freedom: 69 Total (i.e. Null);  68 Residual
## Null Deviance:	    83.76 
## Residual Deviance: 83.76 	AIC: 87.76
```


### Sex as a Moderator, `\(M\)`


```r
MYY <- data.frame(Sex = "Male",
                  Smoking = "Yes",
                  Cancer = 1,
                  freq = 5
                  )

MYN <- data.frame(Sex = "Male",
                  Smoking = "Yes",
                  Cancer = 0,
                  freq = 4
                  )

MNY <- data.frame(Sex = "Male",
                  Smoking = "No",
                  Cancer = 1,
                  freq = 45
                  )

MNN <- data.frame(Sex = "Male",
                  Smoking = "No",
                  Cancer = 0,
                  freq = 68
                  )


FYY <- data.frame(Sex = "Female",
                  Smoking = "Yes",
                  Cancer = 1,
                  freq = 25
                  )

FYN <- data.frame(Sex = "Female",
                  Smoking = "Yes",
                  Cancer = 0,
                  freq = 14
                  )

FNY <- data.frame(Sex = "Female",
                  Smoking = "No",
                  Cancer = 1,
                  freq = 25
                  )

FNN <- data.frame(Sex = "Female",
                  Smoking = "No",
                  Cancer = 0,
                  freq = 14
                  )

Ex_moderator <- rbind(MYY, MYN, MNY, MNN, FYY, FYN, FNY, FNN)
```


Convert Freq table to raw data


```r
library(tidyr)
raw_moderator <- Ex_moderator %>% 
  uncount(freq)
```


- Full model:


```r
glm_logit <- glm(Cancer ~ Smoking , family=binomial(link = "logit"), data=raw_moderator)
glm_logit
```

```
## 
## Call:  glm(formula = Cancer ~ Smoking, family = binomial(link = "logit"), 
##     data = raw_moderator)
## 
## Coefficients:
## (Intercept)   SmokingYes  
##     -0.1582       0.6690  
## 
## Degrees of Freedom: 199 Total (i.e. Null);  198 Residual
## Null Deviance:	    277.3 
## Residual Deviance: 273.3 	AIC: 277.3
```

- Stratified models


```r
## For males
raw_moderator_M <- raw_moderator[ which(raw_moderator$Sex=='Male'), ]
glm_logit_m <- glm(Cancer ~ Smoking , family=binomial(link = "logit"), data=raw_moderator_M)
glm_logit_m
```

```
## 
## Call:  glm(formula = Cancer ~ Smoking, family = binomial(link = "logit"), 
##     data = raw_moderator_M)
## 
## Coefficients:
## (Intercept)   SmokingYes  
##     -0.4128       0.6360  
## 
## Degrees of Freedom: 121 Total (i.e. Null);  120 Residual
## Null Deviance:	    165.1 
## Residual Deviance: 164.3 	AIC: 168.3
```

```r
# For females
raw_moderator_F <- raw_moderator[ which(raw_moderator$Sex=='Female'), ]
glm_logit_f <- glm(Cancer ~ Smoking , family=binomial(link = "logit"), data=raw_moderator_F)
glm_logit_f
```

```
## 
## Call:  glm(formula = Cancer ~ Smoking, family = binomial(link = "logit"), 
##     data = raw_moderator_F)
## 
## Coefficients:
## (Intercept)   SmokingYes  
##   5.798e-01   -2.621e-16  
## 
## Degrees of Freedom: 77 Total (i.e. Null);  76 Residual
## Null Deviance:	    101.8 
## Residual Deviance: 101.8 	AIC: 105.8
```

