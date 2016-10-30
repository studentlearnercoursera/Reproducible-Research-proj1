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


## What is mean total number of steps taken per day?


```r
stepsum <- aggregate(activity$steps, by = list(date=activity$date), FUN=sum)
hist(stepsum$x, main = "Histogram of total number of steps per day",
      xlab = "Total number of steps per day")
```

![](PA1_template_files/figure-html/unnamed-chunk-2-1.png)<!-- -->

```r
stepsumsummary <- summary(stepsum)
stepsumsummary
```

```
##          date          x        
##  2012-10-01: 1   Min.   :   41  
##  2012-10-02: 1   1st Qu.: 8841  
##  2012-10-03: 1   Median :10765  
##  2012-10-04: 1   Mean   :10766  
##  2012-10-05: 1   3rd Qu.:13294  
##  2012-10-06: 1   Max.   :21194  
##  (Other)   :55   NA's   :8
```
Mean   :10766  
Median :10765  

## What is the average daily activity pattern?



```r
stepavbyint <- aggregate(activity$steps, by =list(interval=activity$interval), 
                         FUN = mean, na.rm = T)
plot(stepavbyint$interval,stepavbyint$x, type = 'l')
```

![](PA1_template_files/figure-html/unnamed-chunk-3-1.png)<!-- -->

```r
stepavbyintmax <- stepavbyint[stepavbyint$x == max(stepavbyint$x),]
stepavbyintmax
```

```
##     interval        x
## 104      835 206.1698
```
The interval 835 has max number of steps: 206.1698113

## Imputing missing values

```r
nas <- sum(!complete.cases(activity))
nas
```

```
## [1] 2304
```
There are 2304 missing values in the dataset.

We replace the missing values by the mean of steps for their interval.


```r
activity2 <- activity
stepmeanbyint <- aggregate(activity2$steps, by=list(interval=activity2$interval), FUN=mean, na.rm=T)
for(i in 1 : nrow(activity2)) {
  if(is.na(activity2$steps[i])) {
    activity2$steps[i] <- stepmeanbyint$x[stepmeanbyint$interval == activity2$interval[i]]
  }
}
sum(is.na(activity2))
```

```
## [1] 0
```

```r
stepsum2 <- aggregate(activity2$steps, by = list(date=activity2$date), FUN=sum)
hist(stepsum2$x, main = "Histogram of total number of steps per day",
      xlab = "Total number of steps per day")
```

![](PA1_template_files/figure-html/unnamed-chunk-5-1.png)<!-- -->

```r
stepsum2summary <- summary(stepsum2)
stepsum2summary
```

```
##          date          x        
##  2012-10-01: 1   Min.   :   41  
##  2012-10-02: 1   1st Qu.: 9819  
##  2012-10-03: 1   Median :10766  
##  2012-10-04: 1   Mean   :10766  
##  2012-10-05: 1   3rd Qu.:12811  
##  2012-10-06: 1   Max.   :21194  
##  (Other)   :55
```

With missing values imputed, mean and median are exactly the same. With missing values included, the median was 1 less than mean.

## Are there differences in activity patterns between weekdays and weekends?


```r
library(ggplot2)
activity2$day = as.factor(ifelse(weekdays(as.Date(activity2$date)) == "Sunday" | weekdays(as.Date(activity2$date)) == "Saturday", "weekend", "weekday"))
str(activity2)
```

```
## 'data.frame':	17568 obs. of  4 variables:
##  $ steps   : num  1.717 0.3396 0.1321 0.1509 0.0755 ...
##  $ date    : Factor w/ 61 levels "2012-10-01","2012-10-02",..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ interval: int  0 5 10 15 20 25 30 35 40 45 ...
##  $ day     : Factor w/ 2 levels "weekday","weekend": 1 1 1 1 1 1 1 1 1 1 ...
```

```r
avgstepsbyday <- aggregate(steps~interval+day,activity2,FUN = mean)
plot <- ggplot(avgstepsbyday,aes(interval,steps)) + geom_line() + facet_wrap(~day,ncol=1)
plot
```

![](PA1_template_files/figure-html/unnamed-chunk-6-1.png)<!-- -->

