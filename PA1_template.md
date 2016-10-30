# Reproducible Research: Peer Assessment 1


## Loading and preprocessing the data


```r
activity = read.csv("activity.csv")
str(activity)
```

```
## 'data.frame':	17568 obs. of  3 variables:
##  $ steps   : int  NA NA NA NA NA NA NA NA NA NA ...
##  $ date    : Factor w/ 61 levels "2012-10-01","2012-10-02",..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ interval: int  0 5 10 15 20 25 30 35 40 45 ...
```


## echo=TRUE

## raw = read.csv("activity.csv")
## str(raw)


# What is mean total number of steps taken per day? #
## totalsum <- aggregate(raw$steps, by = list(date=raw$date), FUN=sum)
## hist(totalsum$x, main = "Histogram of total number of steps per day",
##      xlab = "Total number of steps per day")
## Totalsummary<-summary(totalsum)
## Totalsummary
##          date          x        
##  2012-10-01: 1   Min.   :   41  
##  2012-10-02: 1   1st Qu.: 8841  
##  2012-10-03: 1   Median :10765  
##  2012-10-04: 1   Mean   :10766  
##  2012-10-05: 1   3rd Qu.:13294  
##  2012-10-06: 1   Max.   :21194  
##  (Other)   :55   NA's   :8
mean = 10766
median = 10765
