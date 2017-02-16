# Homework2
---
title: "Stat 341 Homework 2"
author: "Brad McNeney"
date: '2017-02-03'
output: html_document
---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
```

In this homework we will write a function to read in 
and summarize data files in the format of 
the example file `hmp.txt`, which you can obtain from
canvas or GitHub. When string manipulation is
necessary, please use functions from `stringr`.
```{r}
library(stringr)
```


1. Determine the delimiter of the file `hmp.txt` and
use the appropriate `read.` function to read it into 
an R data frame named `hmp`.
**Do not** read the character strings in as factors.
Save the dimensions of `hmp` in a variable `hmp.dim`.

```{r}
setwd("~/Desktop/课业文档/Statistics/STAT 341/SFUStat341/Homeworks")
hmp<-read.delim("hmp.txt",header=TRUE,sep="\t",stringsAsFactors = FALSE)
str(hmp)
hmp.dim<-dim(hmp)
```

2. Extract the first 11 columns of `hmp`,
and save them as a data frame called `marker.info`.
In the ninth column of `marker.info` you will see
long character strings. Some of these contain
the substring `MITOCHONDRIA`. Create a logical
vector `mit` that is `TRUE` for strings
that contain `MITOCHONDRIA` and `FALSE` otherwise.
Add `mit` to the `marker.info` data frame.

```{r}
marker.info<-data.frame(hmp[,1:11])
str(marker.info)
marker.info$mit<-regexpr("MITOCHONDRIA",marker.info[,9])>0
marker.info
```

3. Progamatically extract the remaining columns 
(that is, write code that uses `hmp.dim`
to calculate the indices of the columns to extract),
_transpose_ this subset and save as a matrix `marker.matrix`.
(Notice that transposing has coerced `marker.matrix` to 
a matrix of characters and that the column names
from `hmp` have become the row names of `marker.matrix`.)
Set the column names of `marker.matrix` to be 
the character strings in the first column
of `marker.info` (strings that start with `rs`).
Print the first six rows of `marker.matrix`.



4. **Background:**
The `NN` entries in `marker.matrix` 
are missing data. Other than 
missing data, the possible letters in a column of 
`marker.matrix` are given in the `alleles`
column of the corresponding row of `marker.info`.
For example, the possible letters in
the first column of `marker.matrix` are `A` and `T`.
The "minor" allele is the letter that
is least common in the data, once
missing data are excluded. 
Hint: If `vv` is a column of `marker.matrix`, 
then `str_split_fixed(vv,pattern="",n=2)` would 
be useful (`str_split_fixed` is from `stringr`).
**Your task:** Write a function called 
`MAsummary()` that takes
a column of `marker.matrix` and the corresponding
`alleles` entry from `model.info` as input 
and returns a list with elements (i) `MA`, the minor
allele (the letter), (ii) `MAF`, the frequency of
the minor allele (proportion of letters
other than `N` that are the `MA`), 
and (iii) `MAcount`, a vector of minor allele
counts for each entry of the input vector. 
Test your function on the first column of 
`marker.matrix` with
`MAsummary(marker.matrix[,1],marker.info[1,"alleles"])`
Your function should find that
the `MA` is `A`, the `MAF` is 0.0227, and 
the first six elements of `MAcount` are
`c(NA,0,NA,NA,0,0)`.

5. Create vectors `MA` and `MAF` of 
length `nrow(marker.info)` and a matrix 
`MAcount` with dimension `dim(marker.matrix)`.
Use a for loop with index `i` to 
loop through the columns of `marker.matrix` 
and use the output of 
`MAsummary(marker.matrix[,i],marker.info[i,"alleles"])`
to fill in the `i`th elements of `MA` and `MAF`, and 
the `i`th column of `MAcount`. 
Add `MA` and `MAF` to the `marker.info` data frame.
Copy `MAcount` to `marker.matrix`; that is, overwrite
`marker.matrix` with `MAcount`.

6. Verify your code by (i) bundling 
`marker.info` and `MAcount` into a list with
`mylist <- list(marker.info=marker.info,MAcount=MAcount)`,
and (ii) using `identical()` to 
compare `mylist` to the output in the object
`testlist` that you can load from
[http://people.stat.sfu.ca/~mcneney/Teaching/Stat341/Test/testhw2.rda]


setwd("~/Desktop/课业文档/Statistics/STAT 341/SFUStat341/Homeworks")
hmp<-read.delim("hmp.txt",header=TRUE,sep="\t",stringsAsFactors = FALSE)
str(hmp)
hmp.dim<-dim(hmp)
