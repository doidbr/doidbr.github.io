---
title: Making Interactive Maps in R
hidden: true
---
## [Return to R tutorials](%base_url%/?r-language)

# Making Interactive Maps in R

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
```

# Making interactive maps in R with leaflet

Being able to produce interactive maps on the fly can greatly speed up exploratory analysis and is a useful tool for displaying data that would be less informative on a [static map](/making-simple-maps).

Leaflet is an open source JavaScript library that is used to create interactive maps on websites. In this post we will look at the the [leaflet R package](https://github.com/rstudio/leaflet) and create some cool interactive maps!

## Installation

The `leaflet` R package can be installed from CRAN by running: 

```{r, eval = F}
install.packages("leaflet")
```

## Basics

Creating a basic interactive map is simple! 

```{r basic, fig.width=8}
library(leaflet)
leaflet() %>% 
  addTiles()
```
![](%theme_url%/img/interactive.png)

The Leaflet R package has been designed to be used with pipes (`%>%`), which makes it easy to add layers and controls such as a scale bar and a mini map.

Most of the time however we will have an area or a study site that we are interested in that we want to view:

```{r b2,fig.width=8}
leaflet() %>% 
  addTiles() %>%
  addScaleBar() %>% 
  setView(lng = 151.2, lat = -33.86, zoom = 10) %>% 
  addMiniMap()
```
![](%theme_url%/img/interactive1.png)

## Markers

Lets plot some species occurence data from [GBIF](https://www.gbif.org/) using the [rgbif](https://github.com/ropensci/rgbif) package:
We will be displaying all eucalypt observations within the Macquarie Marshes region:

```{r b3,fig.width=8}
## Getting the data
# install.packages('gbif')
library(rgbif)
gbif_query <- occ_search(genusKey = 7493935,
                         geometry = rgbif::gbif_bbox2wkt(minx = 147.8,miny = -30.6,maxx = 147.4,maxy = -31))
euc <- gbif_query$data
euc$label <- paste(euc$name, '|', euc$vernacularName, '|', euc$year, '-', month.abb[euc$month])
## Creating a map
base <- leaflet() %>% 
  addTiles() %>%
  addScaleBar() %>% 
  setView(lat = mean(euc$decimalLatitude), lng = mean(euc$decimalLongitude), zoom = 10)

base %>% addMarkers(lng = euc$decimalLongitude, lat = euc$decimalLatitude,
                    label = euc$label)
```
![](%theme_url%/img/interactive2.png)

## Clustering markers

Nice! but it is a bit cluttered, we can add clustering by specifying the clusterOptions argument to try and solve this issue:

```{r b4,fig.width=8}
base %>% 
  addMarkers(lng = euc$decimalLongitude, lat = euc$decimalLatitude,
             clusterOptions = markerClusterOptions(),
             label = euc$label)
```
![](%theme_url%/img/interactive3.png)

## Colouring markers

You can also add circle markers which can have custom colours, and add a legend:

```{r b5,fig.width=8}
color_function <- colorFactor("RdYlBu", domain = NULL)

base %>% 
  addCircleMarkers(lng = euc$decimalLongitude, lat = euc$decimalLatitude,
                   color = color_function(euc$name),
                   label = euc$label) %>% 
  addLegend(pal = color_function, values = euc$name)
```
![](%theme_url%/img/interactive4.png)

## Other stuff to try out!

* `addMeasure()` adds a ruler and an area estimator control to the map
* `addProviderTiles()` Other tiles (base maps) can be added by using this function. Try out: `leaflet() %>% addProviderTiles(provider = providers$CartoDB)` !
* `addLayersControl()` adds a selector for choosing multiple layers if you have added them.
* `addRasterImage()` creates an image overlay from raster data!
* `addGeoJSON()` adds GeoJSON polygons to the interactive map!

## Resources: 

* [RStudios leaflet guide](https://rstudio.github.io/leaflet/)
* [rgbif](https://github.com/ropensci/rgbif)


# [Go to next page - Statistics -Categorical Data Analyses](%base_url%/?categorical-data-analyses)


`Last updated: May 15, 2020`