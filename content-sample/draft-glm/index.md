---
title: Draft GLM
author: "Alison Silva"
date: "Thursday, August 06, 2015"
hidden: true
---
## [Return to R tutorials list](%base_url%/?r-language)

# Draft GLM

# Usage

Linear models (linear regression) is used to model the relationship between a **continuous** response variable ![](https://latex.codecogs.com/svg.latex?y) and one or more explanatory variables ![](https://latex.codecogs.com/svg.latex?x_1,x_2,\cdots). When we have a **discrete** response we use generalised linear models (GLM's).



![](%theme_url%/img/Crab.jpg)



## Properties of GLM's

Discrete response data, like counts and presence/absence data, generally exhibit a mean-variance relationship. For example; for counts that are on average 5, we would expect most samples to be between about 1 and 9, but for counts that are on average 500, most of the observations will tend to be between 450 and 550, giving us a much larger variance when the mean is large. 


Linear models assume constant variance. You might have learned to transform count data and then fit a linear model. This can reduce the mean variance relationship, but it won't get rid of it completely, especially if you have a lot of zeros in your data. To analyse discrete data accurately we need to use GLM's.

A GLM makes some important assumptions (we'll check these later for our examples)

1. The observed ![](https://latex.codecogs.com/svg.latex?y) are independent, conditional on some predictors ![](https://latex.codecogs.com/svg.latex?x)

2. The response ![](https://latex.codecogs.com/svg.latex?y) come from a known distribution with a known mean-variance relationship

3. There is a straight line relationship between a known function ![](https://latex.codecogs.com/svg.latex?g) of the mean of ![](https://latex.codecogs.com/svg.latex?y) and the predictors ![](https://latex.codecogs.com/svg.latex?x)

![](https://latex.codecogs.com/svg.latex?g(\mu_y)=\alpha+\beta_1x_1+\beta_2x_2+\cdots)

Note: link functions `g()` are an important part of fitting GLM's, but beyond the scope of this tutorial. All you need to know is that the default link for binomial data is the `logit()` and for count data it's `log()`. For more information see `?family`.

# Fitting models

Fitting GLM's uses very similar syntax to fitting linear models. We use the `glm()` function instead of `lm()`. We also need to add a `family` argument, this depends on whether your data is counts, binomial etc.

## Binomial data (presence/absence, 0/1)
We will analyse a dataset of crab presence/absence. We are interested whether the probability of crab presence changes with time and distance. For binomial data we use `family=binomial`. 
Download the [Crabs.csv](%base_url%/Crabs.csv)

```{r}
CrabDat = read.csv("Crabs.csv", header = T)
head(CrabDat)
ft.crab = glm(CrabPres~Time*Dist,family=binomial,data=CrabDat)
```


### Checking assumptions

Before we look at the results of our analysis it's important to check that our data met the assumptions of the model we used. Let's look at all the assumptions in order.

#### Assumption 1 : The observed ![](https://latex.codecogs.com/svg.latex?y) are independent, conditional on some predictors ![](https://latex.codecogs.com/svg.latex?x)

We can't check this assumption, but you can ensure it's true by taking a random sample for your experimental design. If your experimental design involves any pseudo-replication, this assumption will be violated. For certain types of pseudo-replication you can use mixed models instead.

#### Assumption 2 : The response ![](https://latex.codecogs.com/svg.latex?y) come from a known distribution with a known mean-variance relationship

The mean variance relationship is the main reason we use GLM's instead of linear models. We need to check the distribution models the mean-variance relationship of our data well. For binomial data this is not a big concern, later on when we analyse count data it'll be very important. To check this assumption we look at a plot of residuals, and try to see if there is a fan shape. Unfortunately the `glm()` plot function gives us a very odd looking plot due to the discreteness of the data. 
    
```{r,out.width = '500px'}
plot(ft.crab,which=1)
```

For a more useful plot we can instead fit the model using the `manyglm()` function in the `mvabund` package. We need a slight change to the family argument, for `manyglm()` we write `family = "binomial"`

```{r, echo = FALSE}
set.seed(1)
```

```{r,out.width = '500px'}
library(mvabund)
ft.crab.many = manyglm(CrabPres~Time*Dist,family="binomial",data=CrabDat)
plot(ft.crab.many)

```

Now we can look for a fan shape. For these data there doesn't seem to be a fan shape, so we can conclude the mean-variance assumption the model made was reasonable for our data. The residuals in this plot have a random component. If you see a pattern it's best to repeat the plot a few times to see if the pattern is real.


#### Assumption 3 : There is a straight line relationship between a known function ![](https://latex.codecogs.com/svg.latex?g) of the mean of ![](https://latex.codecogs.com/svg.latex?y) and the predictors ![](https://latex.codecogs.com/svg.latex?x)

To check this assumption, we check the residual plot above for non-linearity, or a U-shape.  In our case there is no evidence of non-linearity. If the residuals seem to go down then up, or up then down, we may need to add a polynomial function of the predictors using the `poly` function.

### Understanding results
If all the assumption checks are okay, we can have a look at the results the model gave us. The two main functions for inference are the same as for linear models: `summary()` and `anova()`. The p-values these give you if you use `glm()` for fitting the model work well in large samples, although they are still approximate. For binomial models in particular the p-values from the `summary()` function can be funny, and we prefer to use the `anova()` function to see if predictors are significant. The `summary()` function is still useful to look at the model equation.

```{r}
ft.crab = glm(CrabPres~Time*Dist,family=binomial,data=CrabDat)
anova(ft.crab,test="Chisq")
```

The p-value for Time is small (P<0.01), so we conclude there is an effect Time on the presence of Crabs, but no effect of Dist or an interaction between Time and Dist.  This sample is reasonably large, so these p-values should be a good approximation. For a small sample it is often better to use resampling to calculate p-values. When you use `manyglm()` the `summary()` and `anova()` functions use resampling by default.

```{r}
ft.crab.many = manyglm(CrabPres~Time*Dist,family="binomial",data=CrabDat)
anova(ft.crab.many)
```

In this case the results are quite similar, but in small samples it can often make a big difference. You can also use `summary()` with either the `glm()` or `manyglm()` function. This is interpreted in a similar manner as for linear regression, but we need to include the link function `g`.


```{r}
summary(ft.crab)
```
If ![](https://latex.codecogs.com/svg.latex?p) is the probability of Crab presence, this output tells us

![](https://latex.codecogs.com/svg.latex?logit(p)=-3.01+0.26\times\text{Time}-0.03\times\text{Dist}+0.01\times\text{Time}\times\text{Dist})


### Count dtata

In this example we have counts of different species at control sites and sites where bush regeneration has been carried out (treatment). We want to know if the count of `Soleolifera` is affected by the treatment. 

The default distribution for count data is the Poisson. The Poisson distribution assumes the variance equals the mean. This is quite a restrictive assumption which ecological count data often violates. We may need to use the more flexible negative-binomial distribution instead. 

#### Checking assumptions

We will use the `manyglm()` function instead of `glm()` so we have access to more useful residual plots. Firstly we fit a Poisson GLM and look at the residual plot to check the assumptions.
Download the [reveg.csv](%base_url%/reveg.csv)

```{r,out.width = '500px'}
reveg = read.csv("reveg.csv", header = T)
ft.sol.pois = manyglm(Soleolifera~Treatment,family="poisson",data=reveg)
plot(ft.sol.pois)
```

It's hard to say whether there is any non-linearity in this plot, this is because the predictor is binary (treatment vs control). Looking at the variance assumption, it does appear as though there is a fan shape. The residuals are more spread out on the right than the left (his pattern is consistent if we do the plot again). This tells us the variance assumption of the Poisson may be too restrictive. We can instead fit a negative-binomial distribution in `manyglm` by changing the family argument to `family="negative binomial"`.

```{r,out.width = '500px'}
ft.sol.nb = manyglm(Soleolifera~Treatment,family="negative binomial",data=reveg)
plot(ft.sol.nb)
```

This seems to have improved the residual plot. There is no longer a strong fan shape, so we can go ahead and look at the results.

```{r}
anova(ft.sol.nb)
summary(ft.sol.nb)
```

Both tests indicate strong evidence of a treatment effect (p<0.01). To extract the model equation we can look at the coefficients from the fit.

```{r}
ft.sol.nb$coefficients
```

The default link function for Poisson and negative binomial models is ![](https://latex.codecogs.com/svg.latex?\log). If we write the mean count as ![](https://latex.codecogs.com/svg.latex?\lambda)  

![](https://latex.codecogs.com/svg.latex?\log(\lambda)=-0.92+2.12\times\text{Treatment})



### Communicating results

Results of GLM's are communicated in the same way as results for linear models. For one predictor it suffices to write one line, e.g. "There is strong evidence (p<0.001) of positive effect of bush regeneration on the abundance of Soleolifera". For multiple predictors it's best to display the results in a table. You should also indicate which distribution was used (e.g. negative-binomial) and if resampling was used for inference. 

### Further help

You can type `?glm` and `?manyglm` into R for help with these functions

# [Go to next page - GLM Binary](%base_url%/?glm-binary)

`Last updated: May 17, 2020`
