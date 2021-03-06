---
title       : MOOCS
subtitle    : How do courses break down among universities on Coursera?
author      : Layale Matta
job         : February 21, 2015
logo        : coursera_logo.png
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight     # {highlight.js, prettify, highlight}
hitheme     : default       # zenburn, tomorrow  
widgets     : []            # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides
---

## Taking the Pulse of MOOCs

* Coursera is a technology platform that kickstarted the current MOOCs boom.
* And it still remains one of the leading companies between MOOCs players now. 
* Like many websites these days, Coursera offers its data through REST APIs.
* Coursera offers a number of its Catalog APIs, available without OAuth authentication.
* We can find out the details of courses offered by Coursera with these APIs.
  * _Courses:_ https://api.coursera.org/api/catalog.v1/courses
  * _Categories:_ https://api.coursera.org/api/catalog.v1/categories
  * _Universities:_ https://api.coursera.org/api/catalog.v1/universities  
<br><br>
* We can try to answer questions like:
  * How do courses break down among universities on Coursera?

---  

## JSON Parsing in R

* JSON is a very common data format for REST APIs.
* And Coursera's APIs also returns results in JSON format.
* I have cleaned the data and saved it in the file "courses.json".
* Let's try parsing Coursera APIs:


```r
library(jsonlite)
courses = fromJSON(paste("[",paste(readLines("courses.json"),collapse=","),"]"))
courses <- courses[order(courses$id),]
rownames(courses) <- NULL
```
* This library "jsonlite" returns the JSON response as a list of data frames.
* We need to plot the ratio of courses per category in order to see the relative representation of academic disciplines on Coursera. A course can belong to multiple categories, and in such cases a count is split equally across the included categories. 

--- 

## Creating the datasets

* The steps I made to create my datasets:
  * get the count of categories by university
  * segment the universities by number of courses
  * get the ranking of categories by number of courses
  * get the ratio of courses by category
  

```r
library(pander)
univ <- read.csv("coursera-univ.csv")
pander(head(univ), style = 'rmarkdown', split.cells = Inf)
```



|  id  |  shortName  |  stem  |  notstem  |  courses  |  ratio  |  year  |
|:----:|:-----------:|:------:|:---------:|:---------:|:-------:|:------:|
|  1   | 'stanford'  |   25   |     6     |    31     | 0.8065  |  2012  |
|  3   |   'umich'   |   8    |     7     |    15     | 0.5333  |  2012  |
|  4   | 'princeton' |   9    |     5     |    14     | 0.6429  |  2012  |
|  6   |   'penn'    |   16   |    18     |    34     | 0.4706  |  2012  |
|  7   |   'duke'    |   14   |    12     |    26     | 0.5385  |  2012  |
|  8   |    'jhu'    |   16   |    19     |    35     | 0.4571  |  2012  |

---

## Summary

* It looks like there was more STEM bias among the early adopters (universities with a lot of courses) but new entrants (universities with fewer courses) tend to have more non-STEM courses. Categories like Social Sciences, Humanities, Business and Management, Education, Teacher Professioal Development, Music, Film and Audio are on the rise.
* Why do we see this non-STEM shift? There are a number of possible explanations: 
* In the beginning, Coursera courses relied on autograders. They were well suited for quantitative STEM subjects, but not for non-STEM subjects. 
* Later, Coursera introduced a crowd sourced essay grading system that can be used across multiple courses. This led to rapid expansion of course offerings and made non-STEM subjects viable. 
* There are no strict prerequisites for Coursera courses, but the bar is still high for STEM courses. Therefore it is quite possible that the potential market size is larger for non-STEM subjects.
* You can check this plot implemented at http://yalisme.shinyapps.io/MoocShiny/

