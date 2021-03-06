# Into_to_Git
Intro to Git Course



```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
library(tidyverse)
library(skimr)
library(dlookr)
```


```{r Model Dataset - Schools}
##Create model dataset with 6 students, student numbers, class names and date of births.
student<-c("a", "b", "c", "d", "e", "f")
number<-c(1, 2, NA, 4, NA, 6)
class<-c("robins", "wrens" , "chaffinches", NA, "sparrows", "kingfishers")
dob<-c("1998-04-06", "1997-11-09", "1998-01-01", "1992-10-12", "1999-06-06", "1998-05-06")
df<-data.frame(student, number, class, dob)
```

# Skimr  

This package can help you profile your data at a high level very quickly. Documentation for the package can be found at this link [here](https://docs.ropensci.org/skimr/). A tutorial for using this package can be found at this link [here](https://www.youtube.com/watch?app=desktop&v=x23Lrn8smHk).

```{r - Skim data}
## Use the Skim package to give you a diagnostic report
skim(df)
```
<br>
<br>

### Determining the DAMA Dimensions from the above output

__Completeness__: n_missing and completeness in your report will tell you how many data points are missing in the dataset you have. You may want to compare your dataset with one of known completeness to ensure there are no missing records.  

__Uniqueness__: n_unique will show you how many unique values there are in your data. 

__Validity__: the report will give you a summary of the number of data types and the format they are in, as well as min and max values per variable. You should check that they conform to the standards you expect and need.  

__Timeliness__: you may have a date inputted variable which tells you when this data point was added; min and max will be able to tell you the range and latest date of this (N.B. make sure this is formatted as a date). You will then combine this with information you have about the analysis you want to do, to decide whether it has been updated within the time you need to get the output you want.  If the data points are not linked to a date, you will hopefully have metadata which contains the last date updated of the data. If you have not had this information provided to you here, you will need to speak to the source of the data to find out when it was last updated, and whether they have any policies about when this should be.  

__Accuracy__: for numeric values, the report will give you the sd (standard deviation) and a small histogram of the values to give you a small snapshot of variance within your dataset. You may need to do more in depth analysis to determine accuracy.  

__Consistency__: R will default to the character format if there is confusion in the format of the data. More variables in the character section of your report than you expected may highlight inconsistencies in the recording of that variable and you may need to do a deeper dive to identify the issues. To compare consistency with other datasets you need to use, you can use the joining functions in tidyverse (explained here) and re-run the skim command.






# dlookr
This package can provide more detailed quality information than skimr. The overview(df) report will provide you with a basic overview of data which will tell you some information about completeness and uniqueness. There are many more functions which will help you to diagnose, explore and transform the quality of your data within this package across the dimensions. The documentation for this can be found at [this link here](https://choonghyunryu.github.io/dlookr/articles/introduce.html#preface). A tutorial for using dlookr can be found at [this link here](https://www.youtube.com/watch?v=M7eNYbd4n1Y). 

```{r overview report and diagnose}
overview(df)
diagnose(df)
```

__Completeness__

```{r Completeness}
diagnose(df)
plot_na_pareto(df)
plot_na_hclust(df)
plot_na_intersect(df)	
```

__Uniqueness__

```{r Uniqueness}
diagnose(df)
```


__Validity__
```{r Validity}
diagnose(df)
```

This will tell you the types of the data you have in your dataset. We can see here that date of birth (dob) is listed as a character, not a date. We want to correct this.


```{r Correct dob and diagnose}
df$dob<-as.Date(df$dob)
diagnose(df)
```


Now the dob is in the correct format.

```{r valid ranges}
diagnose_category(df)
diagnose_numeric(df)
diagnose_outlier(df)
```
