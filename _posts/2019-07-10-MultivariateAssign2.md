---
title: "Program: Multivariate Analysis 2"
date: 2019-07-10
tags: [DataScience,Multivariate Analysis,R,Machine Learning]
header:
  image: "/images/PCAutomation.jpg"
excerpt: "Multivariate Analysis,R,Machine Learning"
mathjax: "true"
---

Solution of the below questions is available here : [Code](https://github.com/MacPrash/Multivariate_Analysis_2/blob/master/_site/index.md)

*Question 1:*

Load the data set ‘airlinedata.csv’ from Blackboard/Brightspace into R. The data set details the airline
distances between 10 U.S. cities.

1 (a) Apply classical metric scaling to the airline distances data and choose a suitable number of dimensions
required to represent the resulting configuration. Explain your reasoning behind your choice of the required
number of dimensions. Plot the two dimensional configuration resulting from the application of classical
metric scaling to the airline data. Label each point in your plot using the name of the associated city. [5
marks]

1 (b) Apply Sammon’s metric least squares scaling and Kruskal’s non-metric scaling to the airline distances
data. Overlay the resulting two dimensional configurations on your plot of the classical scaling configuration.
Label each point using the name of the associated city. [3 marks]

1 (c) Use Procrustes analysis to match the three resulting configurations of the U.S. cities. Plot the Procrustes
residuals. Which configurations match best? Suggest a reason for your conclusion. [2 marks]

*Question 2:*

The 2012 Perceptions of Science and Technology Survey asked respondents to respond using a four point
scale (1 = “strongly disagree”, 2 = “disagree to some extent”, 3 = “agree to some extent”, 4 = “strongly
agree”) to the following statements to gauge attitudes towards science and technology:
1. Science and Technology are making our lives healthier, easier and more comfortable. [Comfort]
2. Scientific and technological research cannot play an important role in protecting the environment and
repairing it. [Environment]
3. The application of science and new technology will make work more interesting [Work].
4. Thanks to science and technology, there will be more opportunities for the future generations. [Future]
5. New technology does not depend on basic scientific research. [Technology]
6. Scientific and technological research do not play an important role in industrial development. [Industry]
7. The benefits of science are greater than any harmful effects it may have. [Benefit.]
Interest lies in clustering the respondents to uncover groups with similar attitudes to science and technology
and in exploring those attitudes.
Notably four statements are positively worded (Comfort, Work, Future, Benefit) with the remaining statements
negatively worded. The survey data have been recoded to be dichotomous: for the positively worded questions
“agree to some extent” and “strongly agree” have been coded as 1 and “strongly disgree” and “disagree to
some extent” coded as 0. The reverse coding has been used for the negatively worded statements.

2 (a) Download the data file ScienceTechSurvey.csv from Blackboard/Brightspace and load it into R. As
the data are now binary in nature, which of the clustering methods that we have seen to date could be used
to cluster the respondents? Apply your chosen clustering method to the survey data, detailing any decisions
you make in the process. How many clusters of respondents do you think are present? [5 marks]

2 (b) Latent class analysis (LCA) can be thought of as a model-based approach to clustering when the
recorded data are binary in nature. The 2011 Journal of Statistical Software paper poLCA: An R Package for
Polytomous Variable Latent Class Analysis by Linzer and Lewis is posted on Blackbord/Brightspace. (Note
that Linzer is famous as the ‘stats man who predicted Obama’s win’.) Read the Linzer and Lewis (2011)
paper, in particular the examples, to gain some background to and understanding of Latent Class Analysis.

You can ignore the latent class regression parts of the paper.) Familiarise yourself with the poLCA function
in R by reading its help file.

Use the poLCA function in R to cluster the survey respondents. Detail any decisions you make in the process.
How many clusters of respondents do you think are present now? On what basis did you make this decision?

Include any output or plots which you use to motivate your decision. [15 marks]

2 (c) Assume you’ve been requested by your employer to provide a synoptic report of your analysis of the
survey respondents. Write a report outlining your research and conclusions, based on your LCA analysis.
Your report should not be longer than 2 A4 pages, in 10 point size font, including plots and tables as you
deem necessary. Include commentary on the cluster sizes and cluster specific parameter interpretations. Also
include in your report an appropriate plot of the clustering uncertainties and associated commentary. [50
marks]

*Solution*

Solution is available is here [code](https://github.com/MacPrash/Multivariate_Analysis_2/blob/master/Study%20on%20LCA.pdf)

2 (d) Compare the resulting LCA clustering to the resulting clustering from part (a). Discuss your comparison.
[5 marks]
