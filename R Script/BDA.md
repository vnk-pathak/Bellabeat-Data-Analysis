Data Cleansing (Bellabeat Data Analysis)
================
Vinayak Kumar Pathak

## Setup

We’ll first install and load all the necessary packages.

``` r
install.packages("tidyverse", repos = "http://cran.us.r-project.org")
library("lubridate")
library("dplyr")
```

<hr>

## Import Data

We’ll then import raw data into data frames.

``` r
dailyActivity <- read.csv("F:\\Capstone_CS2\\Dataset\\Fitbase_Data_12416-12516\\dailyActivity_merged.csv")
head(dailyActivity)
```

    ##           Id ActivityDate TotalSteps TotalDistance TrackerDistance
    ## 1 1503960366    4/12/2016      13162          8.50            8.50
    ## 2 1503960366    4/13/2016      10735          6.97            6.97
    ## 3 1503960366    4/14/2016      10460          6.74            6.74
    ## 4 1503960366    4/15/2016       9762          6.28            6.28
    ## 5 1503960366    4/16/2016      12669          8.16            8.16
    ## 6 1503960366    4/17/2016       9705          6.48            6.48
    ##   LoggedActivitiesDistance VeryActiveDistance ModeratelyActiveDistance
    ## 1                        0               1.88                     0.55
    ## 2                        0               1.57                     0.69
    ## 3                        0               2.44                     0.40
    ## 4                        0               2.14                     1.26
    ## 5                        0               2.71                     0.41
    ## 6                        0               3.19                     0.78
    ##   LightActiveDistance SedentaryActiveDistance VeryActiveMinutes
    ## 1                6.06                       0                25
    ## 2                4.71                       0                21
    ## 3                3.91                       0                30
    ## 4                2.83                       0                29
    ## 5                5.04                       0                36
    ## 6                2.51                       0                38
    ##   FairlyActiveMinutes LightlyActiveMinutes SedentaryMinutes Calories
    ## 1                  13                  328              728     1985
    ## 2                  19                  217              776     1797
    ## 3                  11                  181             1218     1776
    ## 4                  34                  209              726     1745
    ## 5                  10                  221              773     1863
    ## 6                  20                  164              539     1728

``` r
dailyCalories <- read.csv("F:\\Capstone_CS2\\Dataset\\Fitbase_Data_12416-12516\\dailyCalories_merged.csv")
head(dailyCalories)
```

    ##           Id ActivityDay Calories
    ## 1 1503960366   4/12/2016     1985
    ## 2 1503960366   4/13/2016     1797
    ## 3 1503960366   4/14/2016     1776
    ## 4 1503960366   4/15/2016     1745
    ## 5 1503960366   4/16/2016     1863
    ## 6 1503960366   4/17/2016     1728

``` r
dailySteps <- read.csv("F:\\Capstone_CS2\\Dataset\\Fitbase_Data_12416-12516\\dailySteps_merged.csv")
head(dailySteps)
```

    ##           Id ActivityDay StepTotal
    ## 1 1503960366   4/12/2016     13162
    ## 2 1503960366   4/13/2016     10735
    ## 3 1503960366   4/14/2016     10460
    ## 4 1503960366   4/15/2016      9762
    ## 5 1503960366   4/16/2016     12669
    ## 6 1503960366   4/17/2016      9705

``` r
dailyIntensities <- read.csv("F:\\Capstone_CS2\\Dataset\\Fitbase_Data_12416-12516\\dailyIntensities_merged.csv")
head(dailyIntensities)
```

    ##           Id ActivityDay SedentaryMinutes LightlyActiveMinutes
    ## 1 1503960366   4/12/2016              728                  328
    ## 2 1503960366   4/13/2016              776                  217
    ## 3 1503960366   4/14/2016             1218                  181
    ## 4 1503960366   4/15/2016              726                  209
    ## 5 1503960366   4/16/2016              773                  221
    ## 6 1503960366   4/17/2016              539                  164
    ##   FairlyActiveMinutes VeryActiveMinutes SedentaryActiveDistance
    ## 1                  13                25                       0
    ## 2                  19                21                       0
    ## 3                  11                30                       0
    ## 4                  34                29                       0
    ## 5                  10                36                       0
    ## 6                  20                38                       0
    ##   LightActiveDistance ModeratelyActiveDistance VeryActiveDistance
    ## 1                6.06                     0.55               1.88
    ## 2                4.71                     0.69               1.57
    ## 3                3.91                     0.40               2.44
    ## 4                2.83                     1.26               2.14
    ## 5                5.04                     0.41               2.71
    ## 6                2.51                     0.78               3.19

``` r
heartRate <- read.csv("F:\\Capstone_CS2\\Dataset\\Fitbase_Data_12416-12516\\heartrate_seconds_merged.csv")
head(heartRate)
```

    ##           Id                 Time Value
    ## 1 2022484408 4/12/2016 7:21:00 AM    97
    ## 2 2022484408 4/12/2016 7:21:05 AM   102
    ## 3 2022484408 4/12/2016 7:21:10 AM   105
    ## 4 2022484408 4/12/2016 7:21:20 AM   103
    ## 5 2022484408 4/12/2016 7:21:25 AM   101
    ## 6 2022484408 4/12/2016 7:22:05 AM    95

``` r
hourCalories <- read.csv("F:\\Capstone_CS2\\Dataset\\Fitbase_Data_12416-12516\\hourlyCalories_merged.csv")
head(hourCalories)
```

    ##           Id          ActivityHour Calories
    ## 1 1503960366 4/12/2016 12:00:00 AM       81
    ## 2 1503960366  4/12/2016 1:00:00 AM       61
    ## 3 1503960366  4/12/2016 2:00:00 AM       59
    ## 4 1503960366  4/12/2016 3:00:00 AM       47
    ## 5 1503960366  4/12/2016 4:00:00 AM       48
    ## 6 1503960366  4/12/2016 5:00:00 AM       48

``` r
hourSteps <- read.csv("F:\\Capstone_CS2\\Dataset\\Fitbase_Data_12416-12516\\hourlySteps_merged.csv")
head(hourSteps)
```

    ##           Id          ActivityHour StepTotal
    ## 1 1503960366 4/12/2016 12:00:00 AM       373
    ## 2 1503960366  4/12/2016 1:00:00 AM       160
    ## 3 1503960366  4/12/2016 2:00:00 AM       151
    ## 4 1503960366  4/12/2016 3:00:00 AM         0
    ## 5 1503960366  4/12/2016 4:00:00 AM         0
    ## 6 1503960366  4/12/2016 5:00:00 AM         0

``` r
hourIntensities <- read.csv("F:\\Capstone_CS2\\Dataset\\Fitbase_Data_12416-12516\\hourlyIntensities_merged.csv")
head(hourIntensities)
```

    ##           Id          ActivityHour TotalIntensity AverageIntensity
    ## 1 1503960366 4/12/2016 12:00:00 AM             20         0.333333
    ## 2 1503960366  4/12/2016 1:00:00 AM              8         0.133333
    ## 3 1503960366  4/12/2016 2:00:00 AM              7         0.116667
    ## 4 1503960366  4/12/2016 3:00:00 AM              0         0.000000
    ## 5 1503960366  4/12/2016 4:00:00 AM              0         0.000000
    ## 6 1503960366  4/12/2016 5:00:00 AM              0         0.000000

``` r
minuteCalories <- read.csv("F:\\Capstone_CS2\\Dataset\\Fitbase_Data_12416-12516\\minuteCaloriesNarrow_merged.csv")
head(minuteCalories)
```

    ##           Id        ActivityMinute Calories
    ## 1 1503960366 4/12/2016 12:00:00 AM   0.7865
    ## 2 1503960366 4/12/2016 12:01:00 AM   0.7865
    ## 3 1503960366 4/12/2016 12:02:00 AM   0.7865
    ## 4 1503960366 4/12/2016 12:03:00 AM   0.7865
    ## 5 1503960366 4/12/2016 12:04:00 AM   0.7865
    ## 6 1503960366 4/12/2016 12:05:00 AM   0.9438

``` r
minuteSteps <- read.csv("F:\\Capstone_CS2\\Dataset\\Fitbase_Data_12416-12516\\minuteStepsNarrow_merged.csv")
head(minuteSteps)
```

    ##           Id        ActivityMinute Steps
    ## 1 1503960366 4/12/2016 12:00:00 AM     0
    ## 2 1503960366 4/12/2016 12:01:00 AM     0
    ## 3 1503960366 4/12/2016 12:02:00 AM     0
    ## 4 1503960366 4/12/2016 12:03:00 AM     0
    ## 5 1503960366 4/12/2016 12:04:00 AM     0
    ## 6 1503960366 4/12/2016 12:05:00 AM     0

``` r
minuteIntensities <- read.csv("F:\\Capstone_CS2\\Dataset\\Fitbase_Data_12416-12516\\minuteIntensitiesNarrow_merged.csv")
head(minuteIntensities)
```

    ##           Id        ActivityMinute Intensity
    ## 1 1503960366 4/12/2016 12:00:00 AM         0
    ## 2 1503960366 4/12/2016 12:01:00 AM         0
    ## 3 1503960366 4/12/2016 12:02:00 AM         0
    ## 4 1503960366 4/12/2016 12:03:00 AM         0
    ## 5 1503960366 4/12/2016 12:04:00 AM         0
    ## 6 1503960366 4/12/2016 12:05:00 AM         0

``` r
minuteMET <- read.csv("F:\\Capstone_CS2\\Dataset\\Fitbase_Data_12416-12516\\minuteMETsNarrow_merged.csv")
head(minuteMET)
```

    ##           Id        ActivityMinute METs
    ## 1 1503960366 4/12/2016 12:00:00 AM   10
    ## 2 1503960366 4/12/2016 12:01:00 AM   10
    ## 3 1503960366 4/12/2016 12:02:00 AM   10
    ## 4 1503960366 4/12/2016 12:03:00 AM   10
    ## 5 1503960366 4/12/2016 12:04:00 AM   10
    ## 6 1503960366 4/12/2016 12:05:00 AM   12

``` r
sleepInfo <- read.csv("F:\\Capstone_CS2\\Dataset\\Fitbase_Data_12416-12516\\sleepDay_merged.csv")
head(sleepInfo)
```

    ##           Id              SleepDay TotalSleepRecords TotalMinutesAsleep
    ## 1 1503960366 4/12/2016 12:00:00 AM                 1                327
    ## 2 1503960366 4/13/2016 12:00:00 AM                 2                384
    ## 3 1503960366 4/15/2016 12:00:00 AM                 1                412
    ## 4 1503960366 4/16/2016 12:00:00 AM                 2                340
    ## 5 1503960366 4/17/2016 12:00:00 AM                 1                700
    ## 6 1503960366 4/19/2016 12:00:00 AM                 1                304
    ##   TotalTimeInBed
    ## 1            346
    ## 2            407
    ## 3            442
    ## 4            367
    ## 5            712
    ## 6            320

``` r
weightInfo <- read.csv("F:\\Capstone_CS2\\Dataset\\Fitbase_Data_12416-12516\\weightLogInfo_merged.csv")
head(weightInfo)
```

    ##           Id                  Date WeightKg WeightPounds Fat   BMI
    ## 1 1503960366  5/2/2016 11:59:59 PM     52.6     115.9631  22 22.65
    ## 2 1503960366  5/3/2016 11:59:59 PM     52.6     115.9631  NA 22.65
    ## 3 1927972279  4/13/2016 1:08:52 AM    133.5     294.3171  NA 47.54
    ## 4 2873212765 4/21/2016 11:59:59 PM     56.7     125.0021  NA 21.45
    ## 5 2873212765 5/12/2016 11:59:59 PM     57.3     126.3249  NA 21.69
    ## 6 4319703577 4/17/2016 11:59:59 PM     72.4     159.6147  25 27.45
    ##   IsManualReport        LogId
    ## 1           True 1.462234e+12
    ## 2           True 1.462320e+12
    ## 3          False 1.460510e+12
    ## 4           True 1.461283e+12
    ## 5           True 1.463098e+12
    ## 6           True 1.460938e+12

<hr>

## Clean Data

### Remove redundant data

Some data frames seems to be redundant, so we’ll check for redundancy
and remove if they are so.

``` r
dailyActivitytemp1 <- dailyActivity %>%
  select(Id, ActivityDate, Calories)
dailyActivitytemp1 <- setNames(dailyActivitytemp1, c("Id", "ActivityDay", "Calories"))
dailyActivitytemp2 <- dailyActivity %>%
  select(Id, ActivityDate, TotalSteps)
dailyActivitytemp2 <- setNames(dailyActivitytemp2, c("Id", "ActivityDay", "StepTotal"))
dailyActivitytemp3 <- dailyActivity %>%
  select(Id, ActivityDate, SedentaryMinutes, LightlyActiveMinutes, FairlyActiveMinutes, VeryActiveMinutes, SedentaryActiveDistance, LightActiveDistance, ModeratelyActiveDistance, VeryActiveDistance)
dailyActivitytemp3 <- setNames(dailyActivitytemp3, c("Id", "ActivityDay", "SedentaryMinutes", "LightlyActiveMinutes", "FairlyActiveMinutes", "VeryActiveMinutes", "SedentaryActiveDistance", "LightActiveDistance", "ModeratelyActiveDistance", "VeryActiveDistance"))

if (identical(dailyActivitytemp1, dailyCalories)) {
  rm(dailyCalories)
  rm(dailyActivitytemp1)
}

if (identical(dailyActivitytemp2, dailySteps)) {
  rm(dailySteps)
  rm(dailyActivitytemp2)
}

if (identical(dailyActivitytemp3, dailyIntensities)) {
  rm(dailyIntensities)
  rm(dailyActivitytemp3)
}
```

–

### Check Data

We’ll then check our remaining data for values and data types.

``` r
glimpse(dailyActivity)
```

    ## Rows: 940
    ## Columns: 15
    ## $ Id                       <dbl> 1503960366, 1503960366, 1503960366, 150396036…
    ## $ ActivityDate             <chr> "4/12/2016", "4/13/2016", "4/14/2016", "4/15/…
    ## $ TotalSteps               <int> 13162, 10735, 10460, 9762, 12669, 9705, 13019…
    ## $ TotalDistance            <dbl> 8.50, 6.97, 6.74, 6.28, 8.16, 6.48, 8.59, 9.8…
    ## $ TrackerDistance          <dbl> 8.50, 6.97, 6.74, 6.28, 8.16, 6.48, 8.59, 9.8…
    ## $ LoggedActivitiesDistance <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, …
    ## $ VeryActiveDistance       <dbl> 1.88, 1.57, 2.44, 2.14, 2.71, 3.19, 3.25, 3.5…
    ## $ ModeratelyActiveDistance <dbl> 0.55, 0.69, 0.40, 1.26, 0.41, 0.78, 0.64, 1.3…
    ## $ LightActiveDistance      <dbl> 6.06, 4.71, 3.91, 2.83, 5.04, 2.51, 4.71, 5.0…
    ## $ SedentaryActiveDistance  <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, …
    ## $ VeryActiveMinutes        <int> 25, 21, 30, 29, 36, 38, 42, 50, 28, 19, 66, 4…
    ## $ FairlyActiveMinutes      <int> 13, 19, 11, 34, 10, 20, 16, 31, 12, 8, 27, 21…
    ## $ LightlyActiveMinutes     <int> 328, 217, 181, 209, 221, 164, 233, 264, 205, …
    ## $ SedentaryMinutes         <int> 728, 776, 1218, 726, 773, 539, 1149, 775, 818…
    ## $ Calories                 <int> 1985, 1797, 1776, 1745, 1863, 1728, 1921, 203…

``` r
glimpse(heartRate)
```

    ## Rows: 2,483,658
    ## Columns: 3
    ## $ Id    <dbl> 2022484408, 2022484408, 2022484408, 2022484408, 2022484408, 2022…
    ## $ Time  <chr> "4/12/2016 7:21:00 AM", "4/12/2016 7:21:05 AM", "4/12/2016 7:21:…
    ## $ Value <int> 97, 102, 105, 103, 101, 95, 91, 93, 94, 93, 92, 89, 83, 61, 60, …

``` r
glimpse(hourCalories)
```

    ## Rows: 22,099
    ## Columns: 3
    ## $ Id           <dbl> 1503960366, 1503960366, 1503960366, 1503960366, 150396036…
    ## $ ActivityHour <chr> "4/12/2016 12:00:00 AM", "4/12/2016 1:00:00 AM", "4/12/20…
    ## $ Calories     <int> 81, 61, 59, 47, 48, 48, 48, 47, 68, 141, 99, 76, 73, 66, …

``` r
glimpse(hourIntensities)
```

    ## Rows: 22,099
    ## Columns: 4
    ## $ Id               <dbl> 1503960366, 1503960366, 1503960366, 1503960366, 15039…
    ## $ ActivityHour     <chr> "4/12/2016 12:00:00 AM", "4/12/2016 1:00:00 AM", "4/1…
    ## $ TotalIntensity   <int> 20, 8, 7, 0, 0, 0, 0, 0, 13, 30, 29, 12, 11, 6, 36, 5…
    ## $ AverageIntensity <dbl> 0.333333, 0.133333, 0.116667, 0.000000, 0.000000, 0.0…

``` r
glimpse(hourSteps)
```

    ## Rows: 22,099
    ## Columns: 3
    ## $ Id           <dbl> 1503960366, 1503960366, 1503960366, 1503960366, 150396036…
    ## $ ActivityHour <chr> "4/12/2016 12:00:00 AM", "4/12/2016 1:00:00 AM", "4/12/20…
    ## $ StepTotal    <int> 373, 160, 151, 0, 0, 0, 0, 0, 250, 1864, 676, 360, 253, 2…

``` r
glimpse(minuteCalories)
```

    ## Rows: 1,325,580
    ## Columns: 3
    ## $ Id             <dbl> 1503960366, 1503960366, 1503960366, 1503960366, 1503960…
    ## $ ActivityMinute <chr> "4/12/2016 12:00:00 AM", "4/12/2016 12:01:00 AM", "4/12…
    ## $ Calories       <dbl> 0.7865, 0.7865, 0.7865, 0.7865, 0.7865, 0.9438, 0.9438,…

``` r
glimpse(minuteSteps)
```

    ## Rows: 1,325,580
    ## Columns: 3
    ## $ Id             <dbl> 1503960366, 1503960366, 1503960366, 1503960366, 1503960…
    ## $ ActivityMinute <chr> "4/12/2016 12:00:00 AM", "4/12/2016 12:01:00 AM", "4/12…
    ## $ Steps          <int> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0…

``` r
glimpse(minuteIntensities)
```

    ## Rows: 1,325,580
    ## Columns: 3
    ## $ Id             <dbl> 1503960366, 1503960366, 1503960366, 1503960366, 1503960…
    ## $ ActivityMinute <chr> "4/12/2016 12:00:00 AM", "4/12/2016 12:01:00 AM", "4/12…
    ## $ Intensity      <int> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0…

``` r
glimpse(minuteMET)
```

    ## Rows: 1,325,580
    ## Columns: 3
    ## $ Id             <dbl> 1503960366, 1503960366, 1503960366, 1503960366, 1503960…
    ## $ ActivityMinute <chr> "4/12/2016 12:00:00 AM", "4/12/2016 12:01:00 AM", "4/12…
    ## $ METs           <int> 10, 10, 10, 10, 10, 12, 12, 12, 12, 12, 12, 12, 10, 10,…

``` r
glimpse(sleepInfo)
```

    ## Rows: 413
    ## Columns: 5
    ## $ Id                 <dbl> 1503960366, 1503960366, 1503960366, 1503960366, 150…
    ## $ SleepDay           <chr> "4/12/2016 12:00:00 AM", "4/13/2016 12:00:00 AM", "…
    ## $ TotalSleepRecords  <int> 1, 2, 1, 2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, …
    ## $ TotalMinutesAsleep <int> 327, 384, 412, 340, 700, 304, 360, 325, 361, 430, 2…
    ## $ TotalTimeInBed     <int> 346, 407, 442, 367, 712, 320, 377, 364, 384, 449, 3…

``` r
glimpse(weightInfo)
```

    ## Rows: 67
    ## Columns: 8
    ## $ Id             <dbl> 1503960366, 1503960366, 1927972279, 2873212765, 2873212…
    ## $ Date           <chr> "5/2/2016 11:59:59 PM", "5/3/2016 11:59:59 PM", "4/13/2…
    ## $ WeightKg       <dbl> 52.6, 52.6, 133.5, 56.7, 57.3, 72.4, 72.3, 69.7, 70.3, …
    ## $ WeightPounds   <dbl> 115.9631, 115.9631, 294.3171, 125.0021, 126.3249, 159.6…
    ## $ Fat            <int> 22, NA, NA, NA, NA, 25, NA, NA, NA, NA, NA, NA, NA, NA,…
    ## $ BMI            <dbl> 22.65, 22.65, 47.54, 21.45, 21.69, 27.45, 27.38, 27.25,…
    ## $ IsManualReport <chr> "True", "True", "False", "True", "True", "True", "True"…
    ## $ LogId          <dbl> 1.462234e+12, 1.462320e+12, 1.460510e+12, 1.461283e+12,…

–

### Correct Data Type

We’ll have to correct the data type of certain columns in order to
obtain correct results.

``` r
dailyActivity <- mutate(dailyActivity, ActivityDate = mdy(ActivityDate))
heartRate <- mutate(heartRate, Time = mdy_hms(Time))
hourCalories <- mutate(hourCalories, ActivityHour = mdy_hms(ActivityHour))
hourIntensities <- mutate(hourIntensities, ActivityHour = mdy_hms(ActivityHour))
hourSteps <- mutate(hourSteps, ActivityHour = mdy_hms(ActivityHour))
minuteCalories <- mutate(minuteCalories, ActivityMinute = mdy_hms(ActivityMinute))
minuteIntensities <- mutate(minuteIntensities, ActivityMinute = mdy_hms(ActivityMinute))
minuteSteps <- mutate(minuteSteps, ActivityMinute = mdy_hms(ActivityMinute))
minuteMET <- mutate(minuteMET, ActivityMinute = mdy_hms(ActivityMinute))
sleepInfo <- mutate(sleepInfo, SleepDay  = mdy_hms(SleepDay))
weightInfo <- mutate(weightInfo, Date = mdy_hms(Date))
```

–

### Merge data frames

We’ll now merge certain data frames into one to simplify the process of
our analysis.

``` r
hourActivity <- hourCalories %>%
  inner_join(hourIntensities, by = c("Id" = "Id", "ActivityHour" = "ActivityHour")) %>%
  inner_join(hourSteps, by = c("Id" = "Id", "ActivityHour" = "ActivityHour"))
glimpse(hourActivity)
```

    ## Rows: 22,099
    ## Columns: 6
    ## $ Id               <dbl> 1503960366, 1503960366, 1503960366, 1503960366, 15039…
    ## $ ActivityHour     <dttm> 2016-04-12 00:00:00, 2016-04-12 01:00:00, 2016-04-12…
    ## $ Calories         <int> 81, 61, 59, 47, 48, 48, 48, 47, 68, 141, 99, 76, 73, …
    ## $ TotalIntensity   <int> 20, 8, 7, 0, 0, 0, 0, 0, 13, 30, 29, 12, 11, 6, 36, 5…
    ## $ AverageIntensity <dbl> 0.333333, 0.133333, 0.116667, 0.000000, 0.000000, 0.0…
    ## $ StepTotal        <int> 373, 160, 151, 0, 0, 0, 0, 0, 250, 1864, 676, 360, 25…

``` r
minuteActivity <- minuteCalories %>%
  inner_join(minuteIntensities, by = c("Id" = "Id", "ActivityMinute" = "ActivityMinute")) %>%
  inner_join(minuteSteps, by = c("Id" = "Id", "ActivityMinute" = "ActivityMinute"))
glimpse(minuteActivity)
```

    ## Rows: 1,325,580
    ## Columns: 5
    ## $ Id             <dbl> 1503960366, 1503960366, 1503960366, 1503960366, 1503960…
    ## $ ActivityMinute <dttm> 2016-04-12 00:00:00, 2016-04-12 00:01:00, 2016-04-12 0…
    ## $ Calories       <dbl> 0.7865, 0.7865, 0.7865, 0.7865, 0.7865, 0.9438, 0.9438,…
    ## $ Intensity      <int> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0…
    ## $ Steps          <int> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0…

–

### Remove redundant data

Some data frames are again seemingly redundant, so we’ll check for
redundancy and remove if they are so.

``` r
hourCaloriestemp <- hourActivity %>%
  select(Id, ActivityHour, Calories)
hourIntensitiestemp <- hourActivity %>%
  select(Id, ActivityHour, TotalIntensity, AverageIntensity)
hourStepstemp <- hourActivity %>%
  select(Id, ActivityHour, StepTotal)

minuteCaloriestemp <- minuteActivity %>%
  select(Id, ActivityMinute, Calories)
minuteIntensitiestemp <- minuteActivity %>%
  select(Id, ActivityMinute, Intensity)
minuteStepstemp <- minuteActivity %>%
  select(Id, ActivityMinute, Steps)

if (identical(hourCaloriestemp, hourCalories)) {
  rm(hourCalories)
  rm(hourCaloriestemp)
}
if (identical(hourIntensitiestemp, hourIntensities)) {
  rm(hourIntensities)
  rm(hourIntensitiestemp)
}
if (identical(hourStepstemp, hourSteps)) {
  rm(hourSteps)
  rm(hourStepstemp)
}

if (identical(minuteCaloriestemp, minuteCalories)) {
  rm(minuteCalories)
  rm(minuteCaloriestemp)
}
if (identical(minuteIntensitiestemp, minuteIntensities)) {
  rm(minuteIntensities)
  rm(minuteIntensitiestemp)
}
if (identical(minuteStepstemp, minuteSteps)) {
  rm(minuteSteps)
  rm(minuteStepstemp)
}
```

–

### Add required Columns

We’ll then add some new columns which will be required for this
analysis.

``` r
dailyActivity$DayOfWeek <- as.character(wday(dailyActivity$ActivityDate, label = TRUE, abbr = FALSE))

heartRate$DayOfWeek <- as.character(wday(heartRate$Time, label = TRUE, abbr = FALSE))
heartRate$Date <- as.Date(ymd_hms(heartRate$Time))
heartRate$Hour <- as.integer(hour(heartRate$Time))
heartRate$Minute <- as.integer(minute(heartRate$Time))

hourActivity$Hour <- as.integer(hour(hourActivity$ActivityHour))
hourActivity$Date <- as.Date(ymd_hms(hourActivity$ActivityHour))

minuteActivity$Minute <- as.integer(minute(minuteActivity$ActivityMinute))
minuteActivity$Hour <- as.integer(hour(minuteActivity$ActivityMinute))
minuteActivity$Date <- as.Date(ymd_hms(minuteActivity$ActivityMinute))

minuteMET$Minute <- as.integer(minute(minuteMET$ActivityMinute))
minuteMET$Date <- as.Date(ymd_hms(minuteMET$ActivityMinute))

sleepInfo$DayOfWeek <- as.character(wday(sleepInfo$SleepDay, label = TRUE, abbr = FALSE))
```

<hr>

## Save Data

We’ll now save the final data frames as CSV files.

``` r
write.csv(dailyActivity, "F:\\Capstone_CS2\\Dataset\\Clean_Data\\dailyActivity.csv", row.names = FALSE)
write.csv(heartRate, "F:\\Capstone_CS2\\Dataset\\Clean_Data\\heartRate.csv", row.names = FALSE)
write.csv(hourActivity, "F:\\Capstone_CS2\\Dataset\\Clean_Data\\hourActivity.csv", row.names = FALSE)
write.csv(minuteActivity, "F:\\Capstone_CS2\\Dataset\\Clean_Data\\minuteActivity.csv", row.names = FALSE)
write.csv(minuteMET, "F:\\Capstone_CS2\\Dataset\\Clean_Data\\minuteMET.csv", row.names = FALSE)
write.csv(sleepInfo, "F:\\Capstone_CS2\\Dataset\\Clean_Data\\sleepInfo.csv", row.names = FALSE)
write.csv(weightInfo, "F:\\Capstone_CS2\\Dataset\\Clean_Data\\weightInfo.csv", row.names = FALSE)
```

\*\* Thank You \*\*
