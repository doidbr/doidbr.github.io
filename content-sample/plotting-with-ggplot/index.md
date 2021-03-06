---
title: Introduction to ggplot
hidden: true
---
## [Return to R tutorials list](%base_url%/?r-language)

# Introduction to ggplot

```{r, echo=FALSE, warning=F}
library(ggplot2)
g1 <- ggplot(iris, aes(Sepal.Length, Petal.Length, colour=Species)) + geom_point() +theme_classic()
print(g1 + labs(y="Petal length (cm)", x = "Sepal length (cm)"))
```
<br>

[ggplot2](http://ggplot2.org/) is a powerful graphing package in R that can be used to create professional looking plot for reports, essays or papers. It can create a variety of plots including boxplots, scatterpots and histograms and they can be highly customised to suit your data. 

The package is named after a book called [The Grammar of Graphics](https://books.google.com.au/books/about/The_Grammar_of_Graphics.html?id=ZiwLCAAAQBAJ&source=kp_cover&hl=en) about how to effectively communicate data graphically. 
<br><br>


Start with the [basics](%base_url%/?plotting-with-ggplot-the-basics/) to learn the basic syntax of making a graph

Then, visit our other pages to further customise the aesthetics of the graph, including colour and formatting:  
* [altering the overall appearance](%base_url%/?plotting-with-ggplot-altering-the-overall-appearance/)  
* [adding titles and axis names](%base_url%/?plotting-with-ggplot-adding-titles-and-axis-names/)  
* [colours and symbols](%base_url%/?plotting-with-ggplot-colours-and-symbols/).   

Help on all the ggplot functions can be found at the [The master ggplot help site](http://docs.ggplot2.org/current/).

A useful cheat sheet on commonly used functions can be downloaded [here](https://www.rstudio.com/wp-content/uploads/2015/03/ggplot2-cheatsheet.pdf).

<p style="margin-left: .5in; text-indent: -.5in;">Chang, W (2012) *R Graphics cookbook.* O'Reilly Media. - a guide to ggplot with quite a bit of help online [here](http://www.cookbook-r.com/Graphs/)</p>
<br><br>

# [Go to next page - Plotting With ggplot The Basics](%base_url%/?plotting-with-ggplot-the-basics)

`Last updated: May 15, 2020`
