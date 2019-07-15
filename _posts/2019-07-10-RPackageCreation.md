---
title: "Program: How to create a R package"
date: 2019-07-10
tags: [R, Package, R Package, Package Creation, Programming]
header:
  image: "/images/PCAutomation.jpg"
excerpt: "R Package"
mathjax: "true"
---

Here we are going to Create a new R package which follows all the standards of CRAN.

The package will have three functions:
load_clim
fit
plot

The latter two will be S3 methods based on the output of load_clim and fit respectively

STEP 0: create a skeleton for the package

First steps are to go to RStudio and:

File > New Project > New package

Specify a suitable name and the working directory for the package

Then go to the console window and run:


R code block to load basic packages:

```r
install.packages(c("devtools", "roxygen2", "knitr"))
library(devtools)
library(roxygen2)
library(knitr)
```r

and any other packages you need

#STEP 1: R code

Go and fill in all the R code available in [R folder] https:// for what you want the package to do

As you are writing the package hit: Ctrl/Cmd + Shift + B to build and re-load
Make sure you’ve set up the setting correctly in project options otherwise it doesn’t seem to wipe the workspace

I usually put each function in a different file but some people do not recommend this

#STEP 2: DESCRIPTION file

Load up the DESCRIPTION file
- Fill in what the package does
- Think about version numbers (major, minor, patch)
- Use Authors@R to fill in the authors. Amazing list of roles: http://www.loc.gov/marc/relators/relaterm.html
e.g., person("Prashanth","Guntal", email = "prashanthguntal@gmail.com",
role =c("aut", "cre"))
- Delete the maintainer field
- Fill in description
- Add in imports. Packages your package NEEDS to run. Note they will not be explicitly loaded
- Add in suggests: if you have packages which might be nice for certain examples, etc. You need to embed these in if statements when you use them
- The way to do the above is to use e.g. devtools::use_package("dplyr")
- For my package I'm using readr, dplyr, magrittr, stats, ggplot2, tibble so I'm running the above for each of those.
- In theory you should now go back through your functions and add in the package names in the right places.
- Choose a license: see http://choosealicense.com/licenses/. I usually choose GPL>=2
- Note that if you're using things like magrittr the above will break your package until you've run the documentation steps below!


STEP 3: Documentation

- This is very important if you want people to use your package
- We'll use the roxygen2 package to add documentation directly into the function files. Common fields here include
@param which lists the different arguments of your function
@return which details what the function returns
@seealso to link to other pages with \code{\link{fun_name}}
@examples which shows the function in action
There is code completion here so type @ to see the other fields
- Call in library(roxygen2)
- Workflow is:
1) Go to each function in turn
2) Click Code > Insert roxygen template
3) Fill in all details
4) Repeat for each function
5) Type devtools::document()
6) Hit CTRL/CMD-Shift-B to build and reload function, or just type devtools::document()
7) Check your help files with ?fun_name
8) Repeat the above until you're happy



Finally, add in either:
#' @importFrom pkg_name "fun_name_1" "fun_name_2"
if you're importing specific functions from other packages, or
#' @import pkg_name
if you want to import everything from a package

If you want to have a look at the NAMESPACE file, now is the time, but don't edit it


STEP 4: Build + check + CRAN submission

Other useful things:
- The NAMESPACE file. Don't touch this! Let devtools do the work for you

Finally you want to check and release your package. To do this run devtools::check() or cmd/ctrl-shift-E. When you do this first of all you will often find lots of errors/warnings/notes. You need to eliminate ALL OF THEM. This will take many hours. There's so much here that could possibly go wrong you will likely need to plenty of reading and Googling to identify all the possible problems.


The current acceptable solution is to use: globalVariables so you put something like
if(getRversion() >= "2.15.1") utils::globalVariables(c('DJF','Dec','J-D','Jan','SON','Year','month','pred','quarter','temp','x','year'))
at the top of each appropriate function.


The perhaps easier alternative is that most of the tidyverse now has standard evaluation versions. These are the same as the usual function names (mutate, summarise, etc) but have an underscore on the end. You then put the argument to the function in quotes. What a pain! I haven't done this here for simplicity but if you were to submit this to cran you'd have to go back and do it

Finally, once you've got rid of all notes/warnings/errors build your package with build() and build_win() to make sure that it works on multiple platforms. Then submit your .tar.gz file to http://cran.r-project.org/submit.html and hope for the best!
