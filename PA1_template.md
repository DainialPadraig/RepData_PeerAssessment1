# Reproducible Research: Peer Assessment 1

This report demonstrates some of the analysis of daily activities that is 
possible using personal activity monitoring devices (like a Fitbit, Nike 
Fuelband, or Jawbone Up) and a powerful data analysis language like R.
Specifically, this report will illustrate the R code necessary to input, 
process, visualize, and analyze the activity data collected by an anonymous
individual in the months of October and November 2012.

## Loading and preprocessing the data

The data for this analysis has been archived as a zip file, so the first step
will be to extract the files from the archive and save them in a data directory, 
but only if the files haven't been extracted previously. The following R code
will check to see if the data directory has already been created and will
unzip the archive if it hasn't.

```r
if(!file.exists("./data")) {    
    unzip("activity.zip", exdir = "./data", unzip = "internal")
}
```

Now that the data has been extracted from the archive, the contents of the data
directory can be listed.

```r
list.files("./data")
```

```
## [1] "activity.csv"
```

The data file contains a single comma-separated values (CSV) file, so the next
step is to read the file contents into an R object called activityData so it can
be analyzed.

```r
activityData <- read.csv("./data/activity.csv")
```

Viewing the first 15 rows of the R object and a summary will show what the
variable names are, what the data values look like, and what their ranges are.

```r
head(activityData, 15)
```

```
##    steps       date interval
## 1     NA 2012-10-01        0
## 2     NA 2012-10-01        5
## 3     NA 2012-10-01       10
## 4     NA 2012-10-01       15
## 5     NA 2012-10-01       20
## 6     NA 2012-10-01       25
## 7     NA 2012-10-01       30
## 8     NA 2012-10-01       35
## 9     NA 2012-10-01       40
## 10    NA 2012-10-01       45
## 11    NA 2012-10-01       50
## 12    NA 2012-10-01       55
## 13    NA 2012-10-01      100
## 14    NA 2012-10-01      105
## 15    NA 2012-10-01      110
```

```r
summary(activityData)
```

```
##      steps               date          interval   
##  Min.   :  0.0   2012-10-01:  288   Min.   :   0  
##  1st Qu.:  0.0   2012-10-02:  288   1st Qu.: 589  
##  Median :  0.0   2012-10-03:  288   Median :1178  
##  Mean   : 37.4   2012-10-04:  288   Mean   :1178  
##  3rd Qu.: 12.0   2012-10-05:  288   3rd Qu.:1766  
##  Max.   :806.0   2012-10-06:  288   Max.   :2355  
##  NA's   :2304    (Other)   :15840
```

It appears that the data file contains a *steps* variable, which may not be
available, the *date* of the observation in Year-Month-Day format, and an 
*interval* between observations of five minutes with the hundreds positions
indicating the hours (e.g., 105 indicates the interval at 1:05). The summary
also shows that there appear to be 288 observations per day and the maximum 
range of the *interval* is from 0 to 2355 (24 hours of observations).

It may be easier for later analysis to convert the *date* and *interval*
variables into a standard R date and time variable. The following R code uses
the **formatC** function to convert the varying length *interval* values to four
digits with leading zeros and the **strptime** function to convert the *date* 
and *interval* values into POSIX data-time format and adds them as a new column 
in *activityData*.

```r
activityData$datetime <- strptime(paste(activityData$date,
                                        formatC(activityData$interval,
                                                width = 4,
                                                format = "d",
                                                flag = "0")),
                                        format = "%Y-%m-%d %H%M")
head(activityData)
```

```
##   steps       date interval            datetime
## 1    NA 2012-10-01        0 2012-10-01 00:00:00
## 2    NA 2012-10-01        5 2012-10-01 00:05:00
## 3    NA 2012-10-01       10 2012-10-01 00:10:00
## 4    NA 2012-10-01       15 2012-10-01 00:15:00
## 5    NA 2012-10-01       20 2012-10-01 00:20:00
## 6    NA 2012-10-01       25 2012-10-01 00:25:00
```


## What is mean total number of steps taken per day?

The total number of steps per day can be visualized as a histogram using the
following code.

```r
stepsPerDay <- aggregate(activityData$steps ~ activityData$date, 
                         activityData, 
                         sum)
hist(stepsPerDay[ , 2], 
     main = "Total Steps Taken Per Day", 
     xlab = "Total Steps")
```

![plot of chunk unnamed-chunk-6](./PA1_template_files/figure-html/unnamed-chunk-6.png) 

The mean of the total number of steps per day can be calculated as follows.

```r
mean(stepsPerDay[ , 2])
```

```
## [1] 10766
```

Likewise, the median can also be determined.

```r
median(stepsPerDay[ , 2])
```

```
## [1] 10765
```

## What is the average daily activity pattern?

The following R code calculates and plots the average number of steps per time
interval.

```r
stepsPerInterval <- aggregate(activityData$steps ~ activityData$interval,
                              activityData, 
                              ave)
plot(stepsPerInterval[, 1], stepsPerInterval[ , 2][ , 1], type = "l", 
     main = "Average Steps Per Interval",
     xlab = "Interval", ylab = "Average Steps")
```

![plot of chunk unnamed-chunk-9](./PA1_template_files/figure-html/unnamed-chunk-9.png) 

The following code displays the interval for which the maximum average steps 
per interval was calculated.

```r
stepsPerInterval[max(stepsPerInterval[ , 2][ , 1]), 1]
```

```
## [1] 1705
```

## Imputing missing values

**TODO** *This part of the report needs to be completed.*

## Are there differences in activity patterns between weekdays and weekends?

**TODO** *This part of the report needs to be completed.*
