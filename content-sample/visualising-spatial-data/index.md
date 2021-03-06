---
title: Visualising Spatial Data
hidden: true
---
## [Return to R tutorials list](%base_url%/?r-language)

# Visualising Spatial Data

Download csv files: [Herbivore_specialisation.csv](%base_url%/Herbivore_specialisation.csv), [Harbour_metals.csv](%base_url%/Harbour_metals.csv)

```{r,echo=F,fig.width=8, fig.height=4,warning=FALSE,message=FALSE}

Herbivores = read.csv(file = "Herbivore_specialisation.csv", header = TRUE)

par(mfrow=c(1,2)) 

# MDS ordination

Habitat <- Herbivores$Habitat
Herb_community <- Herbivores[,4:10]
library(vegan)
Herb_community.mds <- metaMDS(comm = Herb_community, distance = "bray", trace = FALSE)
plot(Herb_community.mds$points, col = Habitat, pch = 16,xlab="",ylab="",xaxt="n",yaxt="n") 

# Cluster diagram

library(dendextend)
Harbour_metals <- read.csv(file="Harbour_metals.csv", header=TRUE)
Harbour_metals2 <- Harbour_metals[,4:8]
rownames(Harbour_metals2) <- Harbour_metals$Rep
H_metals.sim <- dist(Harbour_metals2, method = "euclidean")
H_metals.cluster <- hclust(H_metals.sim, method = "single")
dend <- as.dendrogram(H_metals.cluster)
sample_colours <- as.numeric(Harbour_metals$Location)
sample_colours <- sample_colours[order.dendrogram(dend)]
labels_colors(dend) <- sample_colours
plot(dend,ylab = "")
```

R has a very wide range of functions and packages for visualising multivariate data. Here is some help for some of the more commonly used techniques:  

* [multidimensional scaling](%base_url%/?mds)
* [principal componenents analysis](%base_url%/?principal-components-analysis/)  
* [cluster analysis](%bse_url%/?cluster-analysis/)  

# [ Go to next page - Categorical Data Analysis](%base_url%/?categorical-data-analyses)


`Last updated: May 15, 2020` 