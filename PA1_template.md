---
title: "Reproducible Research: Peer Assessment 1"
output: 
  html_document:
    keep_md: true
---


Assigment 1 
======================================================

##Loading and preprocessing the data

With those instruction load the data and convert the column of Dates from levels to Date objects.


```r
setwd("C:\\Users\\MR\\Documents\\Reproducible")
data<- read.csv("activity.csv")
```

##What is mean total number of steps taken per day?

1.Calculate the total number of steps taken per day.



```r
dataNoNA<-data[!is.na(data$steps),]
dataNoNA$date<-as.Date(dataNoNA$date,format="%Y-%m-%d")
datatapply<-tapply(dataNoNA$steps,dataNoNA$date,sum)
```

2.If you do not understand the difference between a histogram and a barplot, research the difference between them. Make a histogram of the total number of steps taken each day


```r
hist(datatapply,main="Number steps per day",xlab="Number Steps", col="yellow",breaks=20)
```

![plot of chunk histogram](figure/histogram-1.png) 

3.Calculate and report the mean and median of the total number of steps taken per day


```r
meanvalue <- mean(tapply(dataNoNA$steps,dataNoNA$date,sum))
medianvalue <- median(tapply(dataNoNA$steps,dataNoNA$date,sum))
```

The mean is 1.0766189 &times; 10<sup>4</sup> and the media is 10765.

##What is the average daily activity pattern?

1.Make a time series plot (i.e. type = "l") of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all days (y-axis)


```r
datatapply <- tapply(dataNoNA$steps, dataNoNA$interval, mean)
plot(names(datatapply),datatapply,type = "l",main = "Average number of steps by Interval", xlab="Time interval",ylab="Average of steps")
```

![plot of chunk meanbyInterval](figure/meanbyInterval-1.png) 

2.Which 5-minute interval, on average across all the days in the dataset, contains the maximum number of steps?


```r
posmax <- which.max(dataNoNA$steps)
maxvalue <- max(dataNoNA$steps)
```

The max number step in one interval is 806. The position is interval 14476.

##Imputing missing values

1.Calculate and report the total number of missing values in the dataset (i.e. the total number of rows with NAs)


```r
numberNa <-sum(is.na(data$steps))
```

There are 2304 rows with NA value.

2.Devise a strategy for filling in all of the missing values in the dataset. The strategy does not need to be sophisticated. For example, you could use the mean/median for that day, or the mean for that 5-minute interval, etc.

For this part I will use the average of the complete date to fill the NA data.


```r
datatapply <- tapply(dataNoNA$steps, dataNoNA$date, mean)
datatapply <- as.data.frame(as.table(datatapply))
names(datatapply) <- c("date", "avg_steps")
datatapply$avg_steps<-round(datatapply$avg_steps)
mergeddata <- merge(data, datatapply, by = "date")
```

3.Create a new dataset that is equal to the original dataset but with the missing data filled in.



4.Make a histogram of the total number of steps taken each day and Calculate and report the mean and median total number of steps taken per day. Do these values differ from the estimates from the first part of the assignment? What is the impact of imputing missing data on the estimates of the total daily number of steps?


##Are there differences in activity patterns between weekdays and weekends?

1.Create a new factor variable in the dataset with two levels-"weekday" and "weekend" indicating whether a given date is a weekday or weekend day.



2.Make a panel plot containing a time series plot (i.e. type = "l") of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all weekday days or weekend days (y-axis). See the README file in the GitHub repository to see an example of what this plot should look like using simulated data.



