# Reproducible Research: Peer Assessment 1


```r
> raw = read.csv("activity.csv")
> str(raw)
```

## What is mean total number of steps taken per day? #

```r
> totalsum <- aggregate(raw$steps, by = list(date=raw$date), FUN=sum)
> hist(totalsum$x, main = "Histogram of total number of steps per day",
>      xlab = "Total number of steps per day")
> Totalsummary<-summary(totalsum)
> Totalsummary
```

![alt tag](https://github.com/studentlearnercoursera/Reproducible-Research-proj1/blob/master/plots/plot1.png)

```r
>          date          x        
>  2012-10-01: 1   Min.   :   41  
>  2012-10-02: 1   1st Qu.: 8841  
>  2012-10-03: 1   Median :10765  
>  2012-10-04: 1   Mean   :10766  
>  2012-10-05: 1   3rd Qu.:13294  
>  2012-10-06: 1   Max.   :21194  
>  (Other)   :55   NA's   :8
```      
mean = 10766
median = 10765

## What is the average daily activity pattern?

```r
>avgptrn <- aggregate(raw$steps, by =list(interval=raw$interval), FUN = mean, na.rm = T)
>stepavbyint <- aggregate(activity$steps, by =list(interval=activity$interval), 
                         FUN = mean, na.rm = T)
>plot(stepavbyint$interval,stepavbyint$x, type = 'l')
>stepavbyintmax <- stepavbyint[stepavbyint$x == max(stepavbyint$x),]
>stepavbyintmax
```

![alt tag](https://github.com/studentlearnercoursera/Reproducible-Research-proj1/blob/master/plots/plot2.png)

```r
>stepavbyintmax <- stepavbyint[stepavbyint$x == max(stepavbyint$x),]
>stepavbyintmax
```

```
>     interval        x
> 104      835 206.1698
```
The interval 835 with max number of steps: 206.1698113

# Imputing missing values #
```r
> Missing <- sum(!complete.cases(activity))
> Missing
```
There are 2304 missing values

```
> raw2 <- raw
> meandata <- aggregate(raw2$steps, by=list(interval=raw2$interval), FUN=mean, na.rm=T)
> for(i in 1 : nrow(raw2)) {
>   if(is.na(raw2$steps[i])) {
>     raw2$steps[i] <- meandata$x[meandata$interval == raw2$interval[i]]
>   }
> }
> sum(is.na(raw2))
> totalsum2 <- aggregate(raw2$steps, by = list(date=raw2$date), FUN=sum)
> hist(totalsum2$x, main = "Histogram of total number of steps per day",
>       xlab = "Total number of steps per day")
> totalsummary2 <- summary(totalsum2)
> totalsummary2
```
![alt tag](https://github.com/studentlearnercoursera/Reproducible-Research-proj1/blob/master/plots/plot3.png)

```
>          date          x        
>  2012-10-01: 1   Min.   :   41  
>  2012-10-02: 1   1st Qu.: 9819  
>  2012-10-03: 1   Median :10766  
>  2012-10-04: 1   Mean   :10766  
>  2012-10-05: 1   3rd Qu.:12811  
>  2012-10-06: 1   Max.   :21194  
>  (Other)   :55

```
Do these values differ from the estimates from the first part of the assignment? NO
What is the impact of imputing missing data on the estimates of the total daily number of steps? No impact

# Are there differences in activity patterns between weekdays and weekends? #
```r
> library(ggplot2)
> raw2$day = as.factor(ifelse(weekdays(as.Date(raw2$date)) == "Sunday" | weekdays(as.Date(raw2$date)) == "Saturday",
"weekend", "weekday"))
> str(raw2)
> avgdaystep <- aggregate(steps~interval+day,raw2,FUN = mean)
> graph <- ggplot(avgdaystep,aes(interval,steps)) + geom_line() + facet_wrap(~day,ncol=1)
> graph
```

![alt tag](https://github.com/studentlearnercoursera/Reproducible-Research-proj1/blob/master/plots/plot4.png)


The only difference is that during weekdays, there are more steps taken in the morning
