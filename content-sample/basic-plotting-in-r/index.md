---
title: Basic Plotting in R
hidden: true
---
## [Return to R tutorials list](%base_url&/?r-language)

# Basic Plotting in R

First, download [Plant_height.csv](%base_url%/Plant_height.csv)

```{r,echo=F,fig.width=6, fig.height=2.5}
Plant_height = read.csv(file = "Plant_height.csv", header = TRUE)

par(mfrow=c(1,3)) 

hist(Plant_height$height,main=NULL,xlab="Plant heigth (cm)",col="red")

model = lm(loght ~ temp, data = Plant_height)
plot(loght ~ temp, data = Plant_height, xlab = "Temperature (C)", ylab = "log(Plant height)",pch=16,col="darkgreen")
abline(model, col = "red")

boxplot(loght~hemisphere, data = Plant_height, ylab = "log(Plant height)",xlab = "Hemisphere", names = c("South","North"),col="lightblue")
```

R has a very wide range of functions and packages for visualising data. Here is some help for some very simple plots using the base functions in R for data with:  

* [one continuous variable](%base_url%/?one-continuous-variable/) - histograms and box plots  
* [two continuous variables](%base_url/?two-continuous-variables/) - scatter plots  
* [one continuous vs categorical variables](%base_url%/?single-continuous-vs-categorical-variables/) - box plots and bar plots  

# [Go to next page - Single Continuous Variable](%base_url%/?one-continuous-variable)

`Last updated: May 15, 2020.`