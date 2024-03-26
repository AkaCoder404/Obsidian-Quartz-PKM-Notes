---
creation_date: 2023年11月05日
banner: "![[daily-note-banner.gif]]"
banner_icon: "🌞"
tags: "#笔记"
banner_y: 0.4705
---

# R Programming

Different from S?
Sweave

# 01 Background
**Resources**
- 📖 R for Data Science ⭐️⭐️⭐️⭐️⭐️
- 📖 The Book of R
- 📖 The Art of R Programming
- 📹 Data Science: Foundations using R Specialization by Coursera ⭐️⭐️⭐️
- 🌐 R for Beginners by CRAN 
- https://r-graph-gallery.com/

Limited in big data, too big to load in memory
# 02 Core Concepts
**Key Terms**
- Facets
- Geoms
- Asethetic
- Atomic: (1) character (2) number (real) (3) integer (4) complex (5) logical
Graphs tutorial
- Attributes: (1) names, dimnames (2) dimensions (matrices, arrays)
 (3) class (4) length (5) other user-defined attributes/metadata 
 - Coercion Implicit / Explicit
 - Matrices
 - Factors Ordered / Unordered
 - Missing Values NaN is Na, but Na is not NaN
 - Data Frames
 - Reading Data
	 - `read.table` and `read.csv` for tabular
	 - `source` for reading R code files
- Dput, Dget, Dumping
- Subsetting
- Partial Matching
 - https://www.storybench.org/getting-started-data-visualization-r-using-ggplot2/
 - `cacher` package
 - Plotting `base`, `lattice`, `ggplot2`, `tidyverse`


```R
dir
?args  # helper
args() # determine the arguments of a 

```

"`...`" argument used heavily in generic functions

Lexical Scoping

Complete Cases
Times and Dates
- `as.Date` for dates
- `as.POSIXct` and `as.POSIXlt` for times
- `strptime` can be used to coerce to the above


Loop Functions (to make looping more easy on the command line)
- `lapply` look over a list and evaluate a function on each element
- `sapply` same as lapply but try to simplify the result
- `apply` apply function over the margins of array
- `tapply` apply function over subsets of a vector
- `mapply` multivariate version of lappy

Factor Variables


Debugging Tools - Basic Tools
- `traceback`
- `debug`
- `browser`
- `trace`
- `recover`

`str` Function
- compactly display the internal structure of an R object
- an alternative to `summary`

Simulation
- `rnorm` generate random Normal variates with a given mean and standard deviation
- `dnorm` evaluate the Normal probability (with a given mean/SD) at a point (or vector of points)
- `pnorm` evaluate the cumulative distribution function for a Normal distribution
- `rpois` generate random Poisson variates with a given rate

R Profiler
- `system.time()`
- `Rprof`
- `summaryRprof()`


Good folder hiearchy
```
Project.Rproj
Data/
Scripts/
Output/
```


Knitr

Rpubs

R Markdown


Subsetting
```R
X[,1:5] # all rows for columns 1 to 5
X[1:5,] # rows 1 to 5 for all columns
X[(x$var1 <= 3), ] # All rows where value at column var1 is <= 3

# Deal with missing values
X[which(X$var2 > 8), ] # return indices are greater than 8, does not return Na's

# Sort
sort(X$var1)
X[order(X$var2, X$var3), ] # order by var2, then var3

# Ordering with plyr


# Adding rows and columns
X$var4 <- rnorm(5)
Y <- cbind(X, rnnom(5)) # add a column to the right hand side of X


# Check for missing values
sum(is.na(X$var1)) # number of missing values in column var1
any(is.na(X))
colSums(is.na(X)) # number of Na's per column
all(colSums(is.na(X))) # number of Na's in entire data set
```

Cross tabs - similar to x table in Excel
```R
> data("UCBAdmissions")
> DF <- as.data.frame(UCBAdmissions)
> summary(DF)
      Admit       Gender   Dept       Freq      
 Admitted:12   Male  :12   A:4   Min.   :  8.0  
 Rejected:12   Female:12   B:4   1st Qu.: 80.0  
                           C:4   Median :170.0  
                           D:4   Mean   :188.6  
                           E:4   3rd Qu.:302.5  
                           F:4   Max.   :512.0  
> xt <- xtabs(Freq ~ Gender + Admit, data = DF)
> xt
        Admit
Gender   Admitted Rejected
  Male       1198     1493
  Female      557     1278
```

Flat Tables