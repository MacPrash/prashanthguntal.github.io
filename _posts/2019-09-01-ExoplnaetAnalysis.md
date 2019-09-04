---
title: "DataScience: Data Wrangling and Visualisation on Exoplanet Data"
date: 2019-09-01
tags: [DataScience, Machine Learning, Shiny, Animated Visualisation]
header:
  image: "/images/PCAutomation.jpg"
excerpt: "Shiny, DataScience"
mathjax: "true"
---


Here we are going to analyse and visualise the exoplanet data. Below are the set of questions for which i'm providing solutions. Shiny is also used.

Complete commented code and output is available here : [code](https://github.com/MacPrash/Exoplanet_Analysis_R_Project/blob/master/_site/index.md)

Shiny App output is available here : [Shiny](https://macprash.shinyapps.io/shinycode/)

*Note : Question 4 plot just displays the plot but on hover doesn't work on github, please do copy paste and run the code it work perfectly. *

**QUESTIONS:**

1) Import the dataset exo_data.csv as a tibble. Columns 1, 16, 17, 18, 25 should be characters. Columns 2, 14 should be factors. Column 15 should be integers. The remaining columns should be doubles.

*Note: the file metadata.txt contains useful information about this dataset. Also, you may consult https://en.wikipedia.org/wiki/Exoplanet*

2) Exclude the exoplanets with an unknown method of discovery.

3) Create a histogram for the log-distances from the Sun, highlighting the methods of discovery.

4) Create scatterplots of the log-mass versus log-distances, separating by methods of discovery. Hovering with the cursor highlights the point and displays its name, and, if you click, the exoplanet's page on the Open Exoplanet Catalogue will be opened. (paste the id after http://www.openexoplanetcatalogue.com/planet/ ).

5) Rename the radius into jupiter_radius, and create a new column called earth_radius which is 11.2 times the Jupiter radius.

6) Focus only on the rows where log-radius and log-period have no missing values, and perform kmeans with four clusters on these two columns.

7*) Add the clustering labels to the dataset through a new factor column called 'type', with levels 'rocky', 'hot_jupiters', 'cold_gas_giants', 'others'; similarly to https://en.wikipedia.org/wiki/Exoplanet#/media/File:ExoplanetPopulations-20170616.png

8) Use a histogram and a violin plot to illustrate how these clusters relate to the log-mass of the exoplanet.

9*) transform r_asc and decl into the equivalent values in seconds and use these as coordinates to represent a celestial map for the exoplanets.

10) create an animated time series where multiple lines illustrate the evolution over time of the total number of exoplanets discovered for each method up to that year.

11*) create an interactive plot with Shiny where you can select the year (slider widget, with values >= 2009) and exoplanet type. Exoplanets appear as points on a scatterplot (log-mass vs log-distance coloured by method) only if they have already been discovered. If type is equal to "all" all types are plotted together.

12) Use STAN to perform likelihood maximisation on a regression model where log-period is the response variable and the logs of host_mass, host_temp and axis are the covariates (exclude rows that contain at least one missing value). Include an intercept term in the regression model.

13) Extend the model in (12) by specifying standard Gaussian priors for the intercept and slope terms, and a Gamma(1,1) prior for the standard deviation of errors. Obtain approximate samples from the posterior distribution of the model.

14) Include in your RMarkdown document a few posterior summaries plots (e.g. estimated posterior densities) from (13) for the parameters of interest.

15) Embed the Shiny app from (11) in your RMarkdown document.
