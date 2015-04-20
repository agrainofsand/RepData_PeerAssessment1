---
title: "PA1_template"
author: "agrainofsand"
output: html_document
---

## Loading and preprocessing the data

```r
activity <- read.csv("/Users/agrainofsand/Documents/coursera_datascience/5 Reproducible Research/activity.csv",
                     stringsAsFactors=F)
```


## What is mean total number of steps taken per day?

```r
group_by_date <- aggregate(steps ~ date, activity, sum)
png("Histogram_Number_of_Steps_per_Day.png", width=480, height=480)
hist(group_by_date$steps, main = "Histogram of Total Number of Steps per Day", xlab="Steps")
dev.off()
```

![plot of chunk unnamed-chunk-2](figure/unnamed-chunk-2-1.png) 

```
## RStudioGD 
##         2
```

```r
mean_steps <- mean(group_by_date$steps)
mean_steps
```

```
## [1] 10766.19
```

```r
median_steps <- median(group_by_date$steps)
median_steps
```

```
## [1] 10765
```


## What is the average daily activity pattern?
1. Average daily pattern

```r
avg_daily_pattern <- aggregate(activity$steps, by = list(activity$interval), FUN=mean, na.rm = T)
colnames(avg_daily_pattern) <- c("intervals", "mean")

png("Avg_Daily_Pattern.png", width=480, height=480)
plot(avg_daily_pattern$intervals, avg_daily_pattern$mean, xlab = "Time interval of 5 min", ylab = "Avg number of steps 5 min interval", type = "l", main = "Avg daily activity pattern")
dev.off()
```

![plot of chunk unnamed-chunk-3](figure/unnamed-chunk-3-1.png) 

```
## RStudioGD 
##         2
```

2. Find interval with max steps

```r
max_steps <- max(avg_daily_pattern$mean)
interval_with_max_steps <- avg_daily_pattern[avg_daily_pattern$mean==max(max_steps),1]
interval_with_max_steps
```

```
## [1] 835
```


## Imputing missing values
1. Calculating and reporting total number of missing values

```r
number_of_rows_with_missing_values <- nrow(activity[is.na(activity$steps)==TRUE,])
number_of_rows_with_missing_values
```

```
## [1] 2304
```

2. Create a new data set, filling in missing values

```r
activity_new <- activity
row.names(avg_daily_pattern) <- avg_daily_pattern$intervals
i <- which(is.na(activity_new$steps))
activity_new[i, 1] <- activity_new[as.factor(activity_new[i,3]),2]
```

3. Histogram of total number of steps taken each day

```r
activity_new_by_date <- aggregate(steps ~ date, activity, sum)
png("Histogram_Number_of_Steps_per_Day_New.png", width=480, height=480)
hist(activity_new_by_date$steps, xlab ="Steps", ylab = "Frequency", main="Histogram for Number of steps taken per day")
dev.off()
```

![plot of chunk unnamed-chunk-7](figure/unnamed-chunk-7-1.png) 

```
## RStudioGD 
##         2
```

4. Mean and median of total number of steps now

```r
mean_new <- mean(activity_new$steps)
```

```
## Warning in mean.default(activity_new$steps): argument is not numeric or
## logical: returning NA
```

```r
mean_new
```

```
## [1] NA
```

```r
median_new <- median(activity_new$steps)
```

```
## Warning in mean.default(sort(x, partial = half + 0L:1L)[half + 0L:1L]):
## argument is not numeric or logical: returning NA
```

```r
median_new
```

```
## [1] NA
```

## Are there differences in activity patterns between weekdays and weekends?

