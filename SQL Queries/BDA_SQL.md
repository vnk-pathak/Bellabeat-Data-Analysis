SQL Queries (Bellabeat Data Analysis)
================
Vinayak Kumar Pathak

<hr>

## General Queries

### (1) Query to find Number of Users and Records in each table

``` sql
SELECT
    'dailyActivity' "Table Name",
    COUNT(DISTINCT "Id") AS "Number of Users",
    COUNT(*) AS "Number of Records"
FROM
    public."dailyActivity"

UNION

SELECT
    'heartRate',
    COUNT(DISTINCT "Id"),
    COUNT(*)
FROM
    public."heartRate"

UNION

SELECT
    'hourActivity',
    COUNT(DISTINCT "Id"),
    COUNT(*)
FROM
    public."hourActivity"

UNION

SELECT
    'minuteActivity',
    COUNT(DISTINCT "Id"),
    COUNT(*)
FROM
    public."minuteActivity"

UNION

SELECT
    'minuteMET',
    COUNT(DISTINCT "Id"),
    COUNT(*)
FROM
    public."minuteMET"

UNION

SELECT
    'sleepInfo',
    COUNT(DISTINCT "Id"),
    COUNT(*)
FROM
    public."sleepInfo"

UNION

SELECT
    'weightInfo',
    COUNT(DISTINCT "Id"),
    COUNT(*)
FROM
    public."weightInfo"
```

<div class="knitsql-table">

| Table Name     | Number of Users | Number of Records |
|:---------------|----------------:|------------------:|
| dailyActivity  |              33 |               940 |
| weightInfo     |               8 |                67 |
| minuteMET      |              33 |           1325580 |
| sleepInfo      |              24 |               413 |
| minuteActivity |              33 |           1325580 |
| hourActivity   |              33 |             22099 |
| heartRate      |              14 |           2483658 |

7 records

</div>

### (2) Query to find Number of Users in each table with atleast 3 weeks of records

``` sql
SELECT
    'dailyActivity' "Table Name",
    COUNT(DISTINCT "Id") AS "Number Of Users"
FROM
    public."dailyActivity"
WHERE
    "Id" IN (SELECT "Id" FROM public."dailyActivity" GROUP BY "Id" HAVING COUNT(DISTINCT "ActivityDate") >= 21)

UNION

SELECT
    'heartRate',
    COUNT(DISTINCT "Id")
FROM
    public."heartRate"
WHERE
    "Id" IN (SELECT "Id" FROM public."heartRate" GROUP BY "Id" HAVING COUNT(DISTINCT "Date") >= 21)
    
UNION

SELECT
    'hourActivity',
    COUNT(DISTINCT "Id")
FROM
    public."hourActivity"
WHERE
    "Id" IN (SELECT "Id" FROM public."hourActivity" GROUP BY "Id" HAVING COUNT(DISTINCT "Date") >= 21)
    
UNION

SELECT
    'minuteActivity',
    COUNT(DISTINCT "Id")
FROM
    public."minuteActivity"
WHERE
    "Id" IN (SELECT "Id" FROM public."minuteActivity" GROUP BY "Id" HAVING COUNT(DISTINCT "Date") >= 21)
    
UNION

SELECT
    'minuteMET',
    COUNT(DISTINCT "Id")
FROM
    public."minuteMET"
WHERE
    "Id" IN (SELECT "Id" FROM public."minuteMET" GROUP BY "Id" HAVING COUNT(DISTINCT "Date") >= 21)
    
UNION

SELECT
    'sleepInfo',
    COUNT(DISTINCT "Id")
FROM
    public."sleepInfo"
WHERE
    "Id" IN (SELECT "Id" FROM public."sleepInfo" GROUP BY "Id" HAVING COUNT(DISTINCT "SleepDay") >= 21)
    
UNION

SELECT
    'weightInfo',
    COUNT(DISTINCT "Id")
FROM
    public."weightInfo"
WHERE
    "Id" IN (SELECT "Id" FROM public."weightInfo" GROUP BY "Id" HAVING COUNT(DISTINCT "Date") >= 21)
```

<div class="knitsql-table">

| Table Name     | Number Of Users |
|:---------------|----------------:|
| minuteMET      |              29 |
| minuteActivity |              29 |
| heartRate      |               9 |
| weightInfo     |               2 |
| hourActivity   |              29 |
| sleepInfo      |              12 |
| dailyActivity  |              29 |

7 records

</div>

### (3) Query to find Number of Users who are in other tables but not in “dailyActivity”

``` sql
SELECT
    'In HA, NotIn DA' "Condition",
    COUNT(DISTINCT "Id") AS "Number Of Users"
FROM
    public."hourActivity"
WHERE
    "Id" NOT IN (SELECT DISTINCT "Id" FROM public."dailyActivity")

UNION

SELECT
    'In MA, NotIn DA',
    COUNT(DISTINCT "Id")
FROM
    public."minuteActivity"
WHERE
    "Id" NOT IN (SELECT DISTINCT "Id" FROM public."dailyActivity")

UNION

SELECT
    'In MM, NotIn DA',
    COUNT(DISTINCT "Id")
FROM
    public."minuteMET"
WHERE
    "Id" NOT IN (SELECT DISTINCT "Id" FROM public."dailyActivity")

UNION

SELECT
    'In HR, NotIn DA',
    COUNT(DISTINCT "Id")
FROM
    public."heartRate"
WHERE
    "Id" NOT IN (SELECT DISTINCT "Id" FROM public."dailyActivity")

UNION

SELECT
    'In SI, NotIn DA',
    COUNT(DISTINCT "Id")
FROM
    public."sleepInfo"
WHERE
    "Id" NOT IN (SELECT DISTINCT "Id" FROM public."dailyActivity")

UNION

SELECT
    'In WI, NotIn DA',
    COUNT(DISTINCT "Id")
FROM
    public."weightInfo"
WHERE
    "Id" NOT IN (SELECT DISTINCT "Id" FROM public."dailyActivity")
```

<div class="knitsql-table">

| Condition       | Number Of Users |
|:----------------|----------------:|
| In HR, NotIn DA |               0 |
| In WI, NotIn DA |               0 |
| In SI, NotIn DA |               0 |
| In MM, NotIn DA |               0 |
| In MA, NotIn DA |               0 |
| In HA, NotIn DA |               0 |

6 records

</div>

### (4) Query to find Number of Users common among various tables

``` sql
SELECT
    'HR & SI' "Condition",
    COUNT(DISTINCT "Id") AS "Number of Users"
FROM
    public."heartRate"
WHERE
    "Id" IN (SELECT DISTINCT "Id" FROM public."sleepInfo")

UNION

SELECT
    'HR & WI',
    COUNT(DISTINCT "Id")
FROM
    public."heartRate"
WHERE
    "Id" IN (SELECT DISTINCT "Id" FROM public."weightInfo")

UNION

SELECT
    'SI & WI',
    COUNT(DISTINCT "Id")
FROM
    public."sleepInfo"
WHERE
    "Id" IN (SELECT DISTINCT "Id" FROM public."weightInfo")

UNION

SELECT
    'All Tables',
    COUNT(DISTINCT "Id")
FROM
    public."dailyActivity"
WHERE
    "Id" IN (SELECT DISTINCT "Id" FROM public."heartRate")
    AND
    "Id" IN (SELECT DISTINCT "Id" FROM public."sleepInfo")
    AND
    "Id" IN (SELECT DISTINCT "Id" FROM public."weightInfo")
```

<div class="knitsql-table">

| Condition  | Number of Users |
|:-----------|----------------:|
| All Tables |               3 |
| HR & WI    |               4 |
| SI & WI    |               6 |
| HR & SI    |              12 |

4 records

</div>

### (5) Query to find Number of Users common among various tables with atleast 3 weeks of records

``` sql
SELECT
    'HR & SI (3 Weeks)' "Condition",
    COUNT(DISTINCT "Id") AS "Number of Users"
FROM
    public."sleepInfo"
WHERE
    "Id" IN (SELECT DISTINCT "Id" FROM public."sleepInfo" GROUP BY "Id" HAVING COUNT(DISTINCT "SleepDay") >= 21)
    AND
    "Id" IN (SELECT DISTINCT "Id" FROM public."heartRate" GROUP BY "Id" HAVING COUNT(DISTINCT "Date") >= 21)

UNION

SELECT
    'HR & WI (3 Weeks)',
    COUNT(DISTINCT "Id")
FROM
    public."heartRate"
WHERE
    "Id" IN (SELECT DISTINCT "Id" FROM public."heartRate" GROUP BY "Id" HAVING COUNT(DISTINCT "Date") >= 21)
    AND
    "Id" IN (SELECT DISTINCT "Id" FROM public."weightInfo" GROUP BY "Id" HAVING COUNT(DISTINCT "Date") >= 21)

UNION

SELECT
    'SI & WI (3 Weeks)',
    COUNT(DISTINCT "Id")
FROM
    public."sleepInfo"
WHERE
    "Id" IN (SELECT DISTINCT "Id" FROM public."sleepInfo" GROUP BY "Id" HAVING COUNT(DISTINCT "SleepDay") >= 21)
    AND
    "Id" IN (SELECT DISTINCT "Id" FROM public."weightInfo" GROUP BY "Id" HAVING COUNT(DISTINCT "Date") >= 21)

UNION

SELECT
    'All Tables (3 Weeks)',
    COUNT(DISTINCT "Id")
FROM
    public."dailyActivity"
WHERE
    "Id" IN (SELECT DISTINCT "Id" FROM public."dailyActivity" GROUP BY "Id" HAVING COUNT(DISTINCT "ActivityDate") >= 21)
    AND
    "Id" IN (SELECT DISTINCT "Id" FROM public."heartRate" GROUP BY "Id" HAVING COUNT(DISTINCT "Date") >= 21)
    AND
    "Id" IN (SELECT DISTINCT "Id" FROM public."sleepInfo" GROUP BY "Id" HAVING COUNT(DISTINCT "SleepDay") >= 21)
    AND
    "Id" IN (SELECT DISTINCT "Id" FROM public."weightInfo" GROUP BY "Id" HAVING COUNT(DISTINCT "Date") >= 21)
```

<div class="knitsql-table">

| Condition            | Number of Users |
|:---------------------|----------------:|
| HR & WI (3 Weeks)    |               2 |
| SI & WI (3 Weeks)    |               1 |
| HR & SI (3 Weeks)    |               4 |
| All Tables (3 Weeks) |               1 |

4 records

</div>

<hr>

## Table Specific Queries : dailyActivity

### (6) Query to find Average Values of various metrics in “dailyActivity”

``` sql
SELECT
    "Id",
    FLOOR(AVG("TotalSteps")) AS "Average Daily Steps",
    FLOOR(AVG("TotalDistance")) AS "Average Daily Distance",
    FLOOR(AVG("TrackerDistance")) AS "Average Daily Tracker Distance",
    AVG("LoggedActivitiesDistance") AS "Average Logged Activity Distance",
    AVG("VeryActiveDistance") AS "Average Very Active Distance",
    AVG("ModeratelyActiveDistance") AS "Average Moderately Active Distance",
    AVG("LightActiveDistance") AS "Average Light Active Distance",
    AVG("SedentaryActiveDistance") AS "Average Sedentary Active Distance",
    FLOOR(AVG("VeryActiveMinutes")) AS "Average Very Active Minutes",
    FLOOR(AVG("FairlyActiveMinutes")) AS "Average Fair Active Minutes",
    FLOOR(AVG("LightlyActiveMinutes")) AS "Average Lightly Active Minutes",
    FLOOR(AVG("SedentaryMinutes")) AS "Average Sedentary Minutes",
    FLOOR(AVG("Calories")) AS "Average Daily Calories",
    COUNT(DISTINCT "ActivityDate") AS "Number Of Days"
FROM
    public."dailyActivity"
GROUP BY
    "Id"
ORDER BY
    COUNT(DISTINCT "ActivityDate") DESC
```

<div class="knitsql-table">

|         Id | Average Daily Steps | Average Daily Distance | Average Daily Tracker Distance | Average Logged Activity Distance | Average Very Active Distance | Average Moderately Active Distance | Average Light Active Distance | Average Sedentary Active Distance | Average Very Active Minutes | Average Fair Active Minutes | Average Lightly Active Minutes | Average Sedentary Minutes | Average Daily Calories | Number Of Days |
|-----------:|--------------------:|-----------------------:|-------------------------------:|---------------------------------:|-----------------------------:|-----------------------------------:|------------------------------:|----------------------------------:|----------------------------:|----------------------------:|-------------------------------:|--------------------------:|-----------------------:|---------------:|
| 8877689391 |               16040 |                     13 |                             13 |                        0.0000000 |                    6.6374194 |                          0.3377419 |                      6.188710 |                         0.0051613 |                          66 |                           9 |                            234 |                      1112 |                   3420 |             31 |
| 1624580081 |                5743 |                      3 |                              3 |                        0.0000000 |                    0.9393548 |                          0.3606452 |                      2.606774 |                         0.0061290 |                           8 |                           5 |                            153 |                      1257 |                   1483 |             31 |
| 4319703577 |                7268 |                      4 |                              4 |                        0.0000000 |                    0.2780645 |                          0.5022581 |                      3.768710 |                         0.0000000 |                           3 |                          12 |                            228 |                       735 |                   2037 |             31 |
| 4388161847 |               10813 |                      8 |                              8 |                        0.0000000 |                    1.7193548 |                          0.9019355 |                      5.396129 |                         0.0000000 |                          23 |                          20 |                            229 |                       836 |                   3093 |             31 |
| 4445114986 |                4796 |                      3 |                              3 |                        0.0000000 |                    0.5232258 |                          0.0754839 |                      2.644839 |                         0.0000000 |                           6 |                           1 |                            209 |                       829 |                   2186 |             31 |
| 4558609924 |                7685 |                      5 |                              5 |                        0.0000000 |                    0.5493548 |                          0.6822581 |                      3.847742 |                         0.0000000 |                          10 |                          13 |                            284 |                      1093 |                   2033 |             31 |
| 4702921684 |                8572 |                      6 |                              6 |                        0.0000000 |                    0.4174194 |                          1.3048387 |                      5.225484 |                         0.0000000 |                           5 |                          26 |                            237 |                       766 |                   2965 |             31 |
| 5553957443 |                8612 |                      5 |                              5 |                        0.0000000 |                    1.4641935 |                          0.6690323 |                      3.504516 |                         0.0000000 |                          23 |                          13 |                            206 |                       668 |                   1875 |             31 |
| 6962181067 |                9794 |                      6 |                              6 |                        0.3236997 |                    1.6164516 |                          0.9600000 |                      4.001613 |                         0.0067742 |                          22 |                          18 |                            245 |                       662 |                   1982 |             31 |
| 7086361926 |                9371 |                      6 |                              6 |                        0.0000000 |                    2.7812903 |                          0.7732258 |                      2.818710 |                         0.0000000 |                          42 |                          25 |                            143 |                       850 |                   2566 |             31 |

Displaying records 1 - 10

</div>

### (7) Query to find errors and outliers for various metrics in “dailyActivity”

``` sql
SELECT
    'TotalDist < VAD+MAD+LAD+SAD' AS "Condition",
    COUNT(*) AS "Number Of Records",
    COUNT(DISTINCT "Id") AS "Number Of Users"
FROM
    public."dailyActivity"
WHERE
    "TotalDistance" < "VeryActiveDistance" + "ModeratelyActiveDistance" + "LightActiveDistance" + "SedentaryActiveDistance"

UNION

SELECT
    'TotalDist = VAD+MAD+LAD+SAD',
    COUNT(*),
    COUNT(DISTINCT "Id")
FROM
    public."dailyActivity"
WHERE
    "TotalDistance" = "VeryActiveDistance" + "ModeratelyActiveDistance" + "LightActiveDistance" + "SedentaryActiveDistance"

UNION

SELECT
    'TotalDist > VAD+MAD+LAD+SAD',
    COUNT(*),
    COUNT(DISTINCT "Id")
FROM
    public."dailyActivity"
WHERE
    "TotalDistance" > "VeryActiveDistance" + "ModeratelyActiveDistance" + "LightActiveDistance" + "SedentaryActiveDistance"

UNION

SELECT
    'TotalDist != TrackerDist',
    COUNT(*),
    COUNT(DISTINCT "Id")
FROM
    public."dailyActivity"
WHERE
    "TotalDistance" != "TrackerDistance"

UNION

SELECT
    'Sed. Minutes = 1440',
    COUNT(*),
    COUNT(DISTINCT "Id")
FROM
    public."dailyActivity"
WHERE
    "SedentaryMinutes" = 1440

UNION

SELECT
    'Sed. Minutes = 1440 & Steps > 1000',
    COUNT(*),
    COUNT(DISTINCT "Id")
FROM
    public."dailyActivity"
WHERE
    "SedentaryMinutes" = 1440
    AND
    "TotalSteps" > 1000

UNION

SELECT
    'Total Minutes < 1440',
    COUNT(*),
    COUNT(DISTINCT "Id")
FROM
    public."dailyActivity"
WHERE
    "VeryActiveMinutes" + "FairlyActiveMinutes" + "LightlyActiveMinutes" + "SedentaryMinutes" < 1440

UNION

SELECT
    'Total Minutes > 1440',
    COUNT(*),
    COUNT(DISTINCT "Id")
FROM
    public."dailyActivity"
WHERE
    "VeryActiveMinutes" + "FairlyActiveMinutes" + "LightlyActiveMinutes" + "SedentaryMinutes" > 1440

UNION

SELECT
    'Calories = 0',
    COUNT(*),
    COUNT(DISTINCT "Id")
FROM
    public."dailyActivity"
WHERE
    "Calories" = 0

UNION

SELECT
    'Calories = 0 & Dist. > 0',
    COUNT(*),
    COUNT(DISTINCT "Id")
FROM
    public."dailyActivity"
WHERE
    "Calories" = 0
    AND
    "VeryActiveDistance" + "ModeratelyActiveDistance" + "LightActiveDistance" + "SedentaryActiveDistance" > 0

UNION

SELECT
    'Calories = 0 & Steps > 0',
    COUNT(*),
    COUNT(DISTINCT "Id")
FROM
    public."dailyActivity"
WHERE
    "Calories" = 0
    AND
    "TotalSteps" > 0

UNION

SELECT
    'Total Minutes = 0',
    COUNT(*),
    COUNT(DISTINCT "Id")
FROM
    public."dailyActivity"
WHERE
    "VeryActiveMinutes" + "FairlyActiveMinutes" + "LightlyActiveMinutes" + "SedentaryMinutes" = 0
```

<div class="knitsql-table">

| Condition                           | Number Of Records | Number Of Users |
|:------------------------------------|------------------:|----------------:|
| Total Minutes \< 1440               |               462 |              33 |
| Calories = 0 & Dist. \> 0           |                 0 |               0 |
| Total Minutes = 0                   |                 0 |               0 |
| Total Minutes \> 1440               |                 0 |               0 |
| TotalDist != TrackerDist            |                15 |               2 |
| TotalDist \> VAD+MAD+LAD+SAD        |               414 |              33 |
| Calories = 0                        |                 4 |               4 |
| Sed. Minutes = 1440 & Steps \> 1000 |                 7 |               3 |
| Calories = 0 & Steps \> 0           |                 0 |               0 |
| TotalDist = VAD+MAD+LAD+SAD         |               304 |              32 |

Displaying records 1 - 10

</div>

### (8) Query to find Average Missing Minutes for each user

``` sql
SELECT
    "Id",
    FLOOR(AVG(1400 - (("VeryActiveMinutes" + "FairlyActiveMinutes" + "LightlyActiveMinutes" + "SedentaryMinutes")))) AS "Average Missing Minutes"
FROM
    public."dailyActivity"
WHERE
    ("VeryActiveMinutes" + "FairlyActiveMinutes" + "LightlyActiveMinutes" + "SedentaryMinutes") < 1440
GROUP BY
    "Id"
```

<div class="knitsql-table">

|         Id | Average Missing Minutes |
|-----------:|------------------------:|
| 4558609924 |                      88 |
| 2320127002 |                     235 |
| 1624580081 |                     403 |
| 4319703577 |                     451 |
| 5577150313 |                     427 |
| 4445114986 |                     394 |
| 2026352035 |                     453 |
| 1927972279 |                     277 |
| 1844505072 |                     562 |
| 5553957443 |                     489 |

Displaying records 1 - 10

</div>

### (9) Query to find Average Missing Distance for each user

``` sql
SELECT
    "Id",
    AVG(("VeryActiveDistance" + "ModeratelyActiveDistance" + "LightActiveDistance" + "SedentaryActiveDistance") - "TotalDistance") AS "Average Missing Distance"
FROM
    public."dailyActivity"
WHERE
    "TotalDistance" < ("VeryActiveDistance" + "ModeratelyActiveDistance" + "LightActiveDistance" + "SedentaryActiveDistance")
GROUP BY
    "Id"
```

<div class="knitsql-table">

|         Id | Average Missing Distance |
|-----------:|-------------------------:|
| 4558609924 |                0.0027274 |
| 2320127002 |                0.0000002 |
| 1624580081 |                0.0044444 |
| 4319703577 |                0.0033334 |
| 5577150313 |                0.0018183 |
| 4445114986 |                0.0000003 |
| 2026352035 |                0.0000000 |
| 1927972279 |                0.0000000 |
| 1844505072 |                0.0100000 |
| 5553957443 |                0.0050000 |

Displaying records 1 - 10

</div>

### (10) Query to find Average Excess Distance for each user

``` sql
SELECT
    "Id",
    AVG("TotalDistance" - ("VeryActiveDistance" + "ModeratelyActiveDistance" + "LightActiveDistance" + "SedentaryActiveDistance")) AS "Average Excess Distance"
FROM
    public."dailyActivity"
WHERE
    "TotalDistance" > ("VeryActiveDistance" + "ModeratelyActiveDistance" + "LightActiveDistance" + "SedentaryActiveDistance")
GROUP BY
    "Id"
```

<div class="knitsql-table">

|         Id | Average Excess Distance |
|-----------:|------------------------:|
| 4558609924 |               0.0058334 |
| 2320127002 |               0.0090000 |
| 1624580081 |               0.0076923 |
| 4319703577 |               0.6270589 |
| 5577150313 |               0.0252942 |
| 4445114986 |               0.0087500 |
| 2026352035 |               0.0200000 |
| 1927972279 |               0.0033334 |
| 1844505072 |               0.0083334 |
| 5553957443 |               0.0090911 |

Displaying records 1 - 10

</div>

<hr>

## Table Specific Queries : hourActivity

### (11) Query to find Average Values of various metrics in “hourActivity”

``` sql
SELECT
    "Id",
    FLOOR(AVG("Calories")) AS "Average Hourly Calories",
    FLOOR(AVG("TotalIntensity")) AS "Average Hourly Intensity",
    FLOOR(AVG("StepTotal")) AS "Average Hourly Steps",
    COUNT(DISTINCT "Date") AS "Number Of Days",
    COUNT(*) AS "Number Of Records",
    FLOOR(COUNT(*)/COUNT(DISTINCT "Date")) AS "Average Records Per Day"
FROM
    public."hourActivity"
GROUP BY
    "Id"
ORDER BY
    COUNT(DISTINCT "Date") DESC
```

<div class="knitsql-table">

|         Id | Average Hourly Calories | Average Hourly Intensity | Average Hourly Steps | Number Of Days | Number Of Records | Average Records Per Day |
|-----------:|------------------------:|-------------------------:|---------------------:|---------------:|------------------:|------------------------:|
| 8877689391 |                     143 |                       19 |                  674 |             31 |               735 |                      23 |
| 1624580081 |                      62 |                        8 |                  241 |             31 |               736 |                      23 |
| 2026352035 |                      64 |                       10 |                  233 |             31 |               736 |                      23 |
| 2320127002 |                      72 |                        8 |                  198 |             31 |               735 |                      23 |
| 2873212765 |                      80 |                       15 |                  318 |             31 |               736 |                      23 |
| 4020332650 |                     101 |                        4 |                   93 |             31 |               732 |                      23 |
| 4319703577 |                      85 |                       11 |                  289 |             31 |               724 |                      23 |
| 4388161847 |                     128 |                       14 |                  435 |             31 |               735 |                      23 |
| 4445114986 |                      92 |                        9 |                  202 |             31 |               735 |                      23 |
| 4558609924 |                      85 |                       14 |                  323 |             31 |               736 |                      23 |

Displaying records 1 - 10

</div>

### (12) Query to find errors and outliers for various metrics in “hourActivity”

``` sql
SELECT
    'Calories = 0' "Condition",
    COUNT(*) AS "Number Of Records"
FROM
    public."hourActivity"
WHERE
    "Calories" = 0

UNION

SELECT
    'Calories = 0 & Intensity > 0',
    COUNT(*)
FROM
    public."hourActivity"
WHERE
    "Calories" = 0
    AND
    "TotalIntensity" > 0

UNION

SELECT
    'Calories = 0 & Steps > 0',
    COUNT(*)
FROM
    public."hourActivity"
WHERE
    "Calories" = 0
    AND
    "StepTotal" > 0

UNION

SELECT
    'Intensity = 0 & Steps > 0',
    COUNT(*)
FROM
    public."hourActivity"
WHERE
    "TotalIntensity" = 0
    AND
    "StepTotal" > 0

UNION

SELECT
    'Steps = 0 & Intensity > 0',
    COUNT(*)
FROM
    public."hourActivity"
WHERE
    "StepTotal" = 0
    AND
    "TotalIntensity" > 0
```

<div class="knitsql-table">

| Condition                     | Number Of Records |
|:------------------------------|------------------:|
| Calories = 0 & Steps \> 0     |                 0 |
| Intensity = 0 & Steps \> 0    |                 1 |
| Calories = 0                  |                 0 |
| Steps = 0 & Intensity \> 0    |               201 |
| Calories = 0 & Intensity \> 0 |                 0 |

5 records

</div>

<hr>

## Table Specific Queries : minuteActivity

### (13) Query to find Average Values of various metrics in “minuteActivity”

``` sql
SELECT
    "Id",
    AVG("Calories") AS "Average Minute Calories",
    AVG("Intensity") AS "Average Minute Intensity",
    FLOOR(AVG("Steps")) AS "Average Minute Steps",
    COUNT(DISTINCT "Date") AS "Number Of Days",
    COUNT(*) AS "Number Of Records",
    FLOOR(COUNT(*)/COUNT(DISTINCT "Date")) AS "Average Recordd Per Day"
FROM 
    public."minuteActivity"
GROUP BY
    "Id"
ORDER BY
    COUNT(DISTINCT "Date") DESC
```

<div class="knitsql-table">

|         Id | Average Minute Calories | Average Minute Intensity | Average Minute Steps | Number Of Days | Number Of Records | Average Recordd Per Day |
|-----------:|------------------------:|-------------------------:|---------------------:|---------------:|------------------:|------------------------:|
| 8877689391 |                2.399803 |                0.3182561 |                   11 |             31 |             44040 |                    1420 |
| 1624580081 |                1.040579 |                0.1339900 |                    4 |             31 |             44160 |                    1424 |
| 2026352035 |                1.080245 |                0.1802083 |                    3 |             31 |             44160 |                    1424 |
| 2320127002 |                1.210995 |                0.1457143 |                    3 |             31 |             44100 |                    1422 |
| 2873212765 |                1.338597 |                0.2516984 |                    5 |             31 |             44160 |                    1424 |
| 4020332650 |                1.677744 |                0.0726321 |                    1 |             31 |             43920 |                    1416 |
| 4319703577 |                1.423378 |                0.1885129 |                    4 |             31 |             43440 |                    1401 |
| 4388161847 |                2.135879 |                0.2385261 |                    7 |             31 |             44100 |                    1422 |
| 4445114986 |                1.535782 |                0.1633015 |                    3 |             31 |             43980 |                    1418 |
| 4558609924 |                1.426141 |                0.2408288 |                    5 |             31 |             44160 |                    1424 |

Displaying records 1 - 10

</div>

### (14) Query to find errors and outliers for various metrics in “minuteActivity”

``` sql
SELECT
    'Calories = 0' "Condition",
    COUNT(*) AS "Number Of Records"
FROM
    public."minuteActivity"
WHERE
    "Calories" = 0

UNION

SELECT
    'Calories = 0 & Intensity > 0',
    COUNT(*)
FROM
    public."minuteActivity"
WHERE
    "Calories" = 0
    AND
    "Intensity" > 0

UNION

SELECT
    'Calories = 0 & Steps > 0',
    COUNT(*)
FROM
    public."minuteActivity"
WHERE
    "Calories" = 0
    AND
    "Steps" > 0

UNION

SELECT
    'Intensity = 0 & Steps > 0',
    COUNT(*)
FROM
    public."minuteActivity"
WHERE
    "Intensity" = 0
    AND
    "Steps" > 0

UNION

SELECT
    'Steps = 0 & Intensity > 0',
    COUNT(*)
FROM
    public."minuteActivity"
WHERE
    "Steps" = 0
    AND
    "Intensity" > 0
```

<div class="knitsql-table">

| Condition                     | Number Of Records |
|:------------------------------|------------------:|
| Steps = 0 & Intensity \> 0    |             13854 |
| Calories = 0 & Intensity \> 0 |                 0 |
| Calories = 0                  |                 7 |
| Calories = 0 & Steps \> 0     |                 0 |
| Intensity = 0 & Steps \> 0    |               286 |

5 records

</div>

<hr>

## Table Specific Queries : minuteMET

### (15) Query to find Average Values of various metrics in “minuteMET”

``` sql
SELECT
    "Id",
    FLOOR(AVG("METs")) AS "Average MET",
    COUNT(DISTINCT "Date") AS "Number Of Days",
    COUNT(*) AS "Number Of Records",
    FLOOR(COUNT(*)/COUNT(DISTINCT "Date")) as "Average Records Per Day"
FROM
    public."minuteMET"
GROUP BY
    "Id"
ORDER BY
    COUNT(DISTINCT "Date") DESC
```

<div class="knitsql-table">

|         Id | Average MET | Number Of Days | Number Of Records | Average Records Per Day |
|-----------:|------------:|---------------:|------------------:|------------------------:|
| 8877689391 |          19 |             31 |             44040 |                    1420 |
| 1624580081 |          12 |             31 |             44160 |                    1424 |
| 2026352035 |          13 |             31 |             44160 |                    1424 |
| 2320127002 |          13 |             31 |             44100 |                    1422 |
| 2873212765 |          15 |             31 |             44160 |                    1424 |
| 4020332650 |          12 |             31 |             43920 |                    1416 |
| 4319703577 |          14 |             31 |             43440 |                    1401 |
| 4388161847 |          16 |             31 |             44100 |                    1422 |
| 4445114986 |          13 |             31 |             43980 |                    1418 |
| 4558609924 |          15 |             31 |             44160 |                    1424 |

Displaying records 1 - 10

</div>

<hr>

## Table Specific Queries : heartRate

### (16) Query to find Average, Maximum & Minimum Values of various metrics in “heartRate”

``` sql
SELECT
    "Id",
    FLOOR(AVG("Value")) AS "Average Heart Rate",
    MAX("Value") AS "Maximum Heart Rate",
    MIN("Value") AS "Minimum Heart Rate",
    COUNT(DISTINCT "Date") AS "Number Of Days",
    COUNT(*) AS "Number Of Records",
    FLOOR((COUNT(*)/COUNT(DISTINCT "Date"))) AS "Average Records Per Day"
FROM
    public."heartRate"
GROUP BY
    "Id"
ORDER BY
    COUNT(DISTINCT "Date") DESC
```

<div class="knitsql-table">

|         Id | Average Heart Rate | Maximum Heart Rate | Minimum Heart Rate | Number Of Days | Number Of Records | Average Records Per Day |
|-----------:|-------------------:|-------------------:|-------------------:|---------------:|------------------:|------------------------:|
| 2022484408 |                 80 |                203 |                 38 |             31 |            154104 |                    4971 |
| 4558609924 |                 81 |                199 |                 44 |             31 |            192168 |                    6198 |
| 5553957443 |                 68 |                165 |                 47 |             31 |            255174 |                    8231 |
| 6962181067 |                 77 |                184 |                 47 |             31 |            266326 |                    8591 |
| 8877689391 |                 83 |                180 |                 46 |             31 |            228841 |                    7381 |
| 4388161847 |                 66 |                180 |                 39 |             30 |            249748 |                    8324 |
| 5577150313 |                 69 |                174 |                 36 |             28 |            248560 |                    8877 |
| 7007744171 |                 91 |                166 |                 54 |             24 |            133592 |                    5566 |
| 6117666160 |                 83 |                189 |                 52 |             23 |            158899 |                    6908 |
| 6775888955 |                 92 |                177 |                 55 |             18 |             32771 |                    1820 |

Displaying records 1 - 10

</div>

### (17) Query to find errors and outliers for various metrics in “heartRate”

``` sql
SELECT
    'HR < 40' AS "Condition",
    COUNT(*) AS "Number Of Records"
FROM
    public."heartRate"
WHERE
    "Value" < 40

UNION

SELECT
    'HR > 200',
    COUNT(*)
FROM
    public."heartRate"
WHERE
    "Value" > 200
```

<div class="knitsql-table">

| Condition | Number Of Records |
|:----------|------------------:|
| HR \< 40  |                23 |
| HR \> 200 |                13 |

2 records

</div>

<hr>

## Table Specific Queries : sleepInfo

### (18) Query to find Average Values of various metrics in “sleepInfo”

``` sql
SELECT
    "Id",
    FLOOR(AVG("TotalMinutesAsleep")) AS "Average Sleep Minutes",
    FLOOR(AVG("TotalTimeInBed")) AS "Average InBed Minutes",
    FLOOR(AVG("TotalTimeInBed") - AVG("TotalMinutesAsleep")) AS "Average Woke InBed Minutes",
    COUNT(DISTINCT "SleepDay") AS "Number Of Days",
    SUM("TotalSleepRecords") AS "Total Sleep Records"
FROM
    public."sleepInfo"
GROUP BY
    "Id"
ORDER BY
    COUNT(DISTINCT "SleepDay") DESC
```

<div class="knitsql-table">

|         Id | Average Sleep Minutes | Average InBed Minutes | Average Woke InBed Minutes | Number Of Days | Total Sleep Records |
|-----------:|----------------------:|----------------------:|---------------------------:|---------------:|--------------------:|
| 5553957443 |                   463 |                   505 |                         42 |             31 |                  38 |
| 8378563200 |                   443 |                   483 |                         39 |             31 |                  36 |
| 6962181067 |                   448 |                   466 |                         18 |             31 |                  34 |
| 4445114986 |                   385 |                   416 |                         31 |             28 |                  39 |
| 3977333714 |                   293 |                   461 |                        167 |             28 |                  32 |
| 2026352035 |                   506 |                   537 |                         31 |             28 |                  28 |
| 4702921684 |                   421 |                   441 |                         20 |             27 |                  30 |
| 5577150313 |                   432 |                   460 |                         28 |             26 |                  27 |
| 4319703577 |                   476 |                   501 |                         25 |             26 |                  27 |
| 1503960366 |                   360 |                   383 |                         22 |             25 |                  27 |

Displaying records 1 - 10

</div>

### (19) Query to find errors and outliers for various metrics in “sleepInfo”

``` sql
SELECT
    'Sleep Minutes = 0 ' "Condition",
    COUNT(*) AS "Number Of Records"
FROM
    public."sleepInfo"
WHERE
  "TotalMinutesAsleep" = 0

UNION

SELECT
    'Sleep Minutes > 1000',
    COUNT(*)
FROM
    public."sleepInfo"
WHERE
  "TotalMinutesAsleep" > 1000

UNION

SELECT
    'Sleep Minutes = 1440',
    COUNT(*)
FROM
    public."sleepInfo"
WHERE
  "TotalMinutesAsleep" = 1440

UNION

SELECT
    'Sleep Minutes > Minutes in Bed',
    COUNT(*)
FROM
    public."sleepInfo"
WHERE
  "TotalMinutesAsleep" > "TotalTimeInBed"

UNION

SELECT
    'Minutes in Bed = 0',
    COUNT(*)
FROM
    public."sleepInfo"
WHERE
  "TotalTimeInBed" = 0

UNION

SELECT
    'Minutes in Bed > 1000',
    COUNT(*)
FROM
    public."sleepInfo"
WHERE
  "TotalTimeInBed" > 1000

UNION

SELECT
    'Minutes in Bed = 1440',
    COUNT(*)
FROM
    public."sleepInfo"
WHERE
  "TotalTimeInBed" = 1440

UNION

SELECT
    'Total Sleep Records = 0',
    COUNT(*)
FROM
    public."sleepInfo"
WHERE
  "TotalSleepRecords" = 0

UNION

SELECT
    'Total Sleep Records = 0 & Sleep Minutes > 0',
    COUNT(*)
FROM
    public."sleepInfo"
WHERE
  "TotalSleepRecords" = 0
  AND
  "TotalMinutesAsleep" > 0
```

<div class="knitsql-table">

| Condition                                    | Number Of Records |
|:---------------------------------------------|------------------:|
| Sleep Minutes \> Minutes in Bed              |                 0 |
| Minutes in Bed \> 1000                       |                 0 |
| Minutes in Bed = 0                           |                 0 |
| Total Sleep Records = 0                      |                 0 |
| Minutes in Bed = 1440                        |                 0 |
| Sleep Minutes = 1440                         |                 0 |
| Sleep Minutes \> 1000                        |                 0 |
| Sleep Minutes = 0                            |                 0 |
| Total Sleep Records = 0 & Sleep Minutes \> 0 |                 0 |

9 records

</div>

<hr>

## Table Specific Queries : weightInfo

### (20) Query to find Average Values of various metrics in “weightInfo”

``` sql
SELECT
    "Id",
    FLOOR(AVG("WeightKg")) AS "Average Weight (Kg)",
    FLOOR(AVG("BMI")) AS "Average BMI",
    COUNT(DISTINCT "Date") AS "Number Of Days"
FROM
    public."weightInfo"
GROUP BY
    "Id"
ORDER BY
    COUNT(DISTINCT "Date") DESC
```

<div class="knitsql-table">

|         Id | Average Weight (Kg) | Average BMI | Number Of Days |
|-----------:|--------------------:|------------:|---------------:|
| 6962181067 |                  61 |          24 |             30 |
| 8877689391 |                  85 |          25 |             24 |
| 4558609924 |                  69 |          27 |              5 |
| 4319703577 |                  72 |          27 |              2 |
| 1503960366 |                  52 |          22 |              2 |
| 2873212765 |                  57 |          21 |              2 |
| 5577150313 |                  90 |          28 |              1 |
| 1927972279 |                 133 |          47 |              1 |

8 records

</div>

### (21) Query to find Number of Records for each kind of report

``` sql
SELECT
    "IsManualReport" AS "Type of Manual Report",
    COUNT(*) AS "Number Of Reports"
FROM
    public."weightInfo"
GROUP BY
    "IsManualReport"
```

<div class="knitsql-table">

| Type of Manual Report | Number Of Reports |
|:----------------------|------------------:|
| FALSE                 |                26 |
| TRUE                  |                41 |

2 records

</div>

### (22) Query to find Users and their number of non-manual reports

``` sql
SELECT
    "Id",
    COUNT("IsManualReport") AS "Number Of True Reports"
FROM
    public."weightInfo"
WHERE
    NOT("IsManualReport")
GROUP BY
    "Id"
```

<div class="knitsql-table">

|         Id | Number Of True Reports |
|-----------:|-----------------------:|
| 8877689391 |                     24 |
| 5577150313 |                      1 |
| 1927972279 |                      1 |

3 records

</div>

<hr>

## Multi Table Queries

### (23) Query to find Users for whom Number of Days in “hourActivity” is not equal to Number of Days in “dailyActivity”

``` sql
SELECT
    DA."Id",
    DA."NumOfDays_DA" AS "Number Of Days (DA)",
    HA."NumOfDays_HA" AS "Number Of Days (HA)",
    (DA."NumOfDays_DA" - HA."NumOfDays_HA") AS "Difference"
FROM
    (SELECT "Id", COUNT(DISTINCT "ActivityDate") AS "NumOfDays_DA" FROM public."dailyActivity" GROUP BY "Id") AS DA,
    (SELECT "Id", COUNT(DISTINCT "Date") AS "NumOfDays_HA" FROM public."hourActivity" GROUP BY "Id") AS HA
WHERE
    DA."Id" = HA."Id"
    AND
    DA."NumOfDays_DA" != HA."NumOfDays_HA"
ORDER BY
    DA."Id"
```

<div class="knitsql-table">

|         Id | Number Of Days (DA) | Number Of Days (HA) | Difference |
|-----------:|--------------------:|--------------------:|-----------:|
| 1503960366 |                  31 |                  30 |          1 |
| 3977333714 |                  30 |                  29 |          1 |
| 6290855005 |                  29 |                  28 |          1 |
| 8253242879 |                  19 |                  18 |          1 |
| 8583815059 |                  31 |                  30 |          1 |
| 8792009665 |                  29 |                  28 |          1 |

6 records

</div>

### (24) Query to find Users for whom Number of Days in “hourActivity” is not equal to Number of Days in “minuteActivity”

``` sql
SELECT
    HA."Id",
    HA."NumOfDays_HA" AS "Number Of Days (HA)",
    MA."NumOfDays_MA" AS "Number Of Days (MA)",
    (HA."NumOfDays_HA" - MA."NumOfDays_MA") AS "Difference"
FROM
    (SELECT "Id", COUNT(DISTINCT "Date") AS "NumOfDays_HA" FROM public."hourActivity" GROUP BY "Id") AS HA,
    (SELECT "Id", COUNT(DISTINCT "Date") AS "NumOfDays_MA" FROM public."minuteActivity" GROUP BY "Id") AS MA
WHERE
    HA."Id" = MA."Id"
    AND
    HA."NumOfDays_HA" != MA."NumOfDays_MA"
ORDER BY
    HA."Id"
```

<div class="knitsql-table">

|  Id | Number Of Days (HA) | Number Of Days (MA) | Difference |
|----:|--------------------:|--------------------:|-----------:|

0 records

</div>

### (25) Query to find records common in tables “heartRate” and “minuteActivity” with corresponding metrics at minute level

``` sql
WITH HR AS (
    SELECT "Id", FLOOR(AVG("Value")) AS "HeartRate", "Date", "Hour", "Minute"
    FROM public."heartRate"
    GROUP BY "Id", "Date", "Hour", "Minute"
),
MA AS (
    SELECT "Id", "Calories", "Intensity", "Steps", "Date", "Hour", "Minute"
    FROM public."minuteActivity"
)

SELECT
    HR."Id",
    HR."HeartRate",
    MA."Calories",
    MA."Intensity",
    MA."Steps",
    HR."Date",
    HR."Hour",
    HR."Minute"
FROM
    HR, MA
WHERE
    (HR."Id" = MA."Id" AND HR."Date" = MA."Date" AND HR."Hour" = MA."Hour" AND HR."Minute" = MA."Minute")
ORDER BY
    HR."Id", HR."Date", HR."Hour", HR."Minute"
```

<div class="knitsql-table">

|         Id | HeartRate | Calories | Intensity | Steps | Date       | Hour | Minute |
|-----------:|----------:|---------:|----------:|------:|:-----------|-----:|-------:|
| 2022484408 |       101 |  3.32064 |         1 |    17 | 2016-04-12 |    7 |     21 |
| 2022484408 |        87 |  3.94326 |         1 |     9 | 2016-04-12 |    7 |     22 |
| 2022484408 |        58 |  1.34901 |         0 |     0 | 2016-04-12 |    7 |     23 |
| 2022484408 |        58 |  1.03770 |         0 |     0 | 2016-04-12 |    7 |     24 |
| 2022484408 |        56 |  1.03770 |         0 |     0 | 2016-04-12 |    7 |     25 |
| 2022484408 |        53 |  2.49048 |         1 |     7 | 2016-04-12 |    7 |     26 |
| 2022484408 |        55 |  1.03770 |         0 |     0 | 2016-04-12 |    7 |     27 |
| 2022484408 |        65 |  2.69802 |         1 |    13 | 2016-04-12 |    7 |     28 |
| 2022484408 |        73 |  2.49048 |         1 |     9 | 2016-04-12 |    7 |     29 |
| 2022484408 |        78 |  2.69802 |         1 |    15 | 2016-04-12 |    7 |     30 |

Displaying records 1 - 10

</div>

### (26) Query to find errors and outliers for various metrics in the common table of “heartRate” and “minuteActivity”

``` sql
WITH HR AS (
    SELECT "Id", FLOOR(AVG("Value")) AS "HeartRate", "Date", "Hour", "Minute"
    FROM public."heartRate"
    GROUP BY "Id", "Date", "Hour", "Minute"
),
MA AS (
    SELECT "Id", "Calories", "Intensity", "Steps", "Date", "Hour", "Minute"
    FROM public."minuteActivity"
),
HRMA_MINUTE_COMMON AS (
    SELECT
        HR."Id" AS "Id",
        HR."HeartRate" AS "HeartRate",
        MA."Calories" AS "Calories",
        MA."Intensity" AS "Intensity",
        MA."Steps" AS "Steps",
        HR."Date" AS "Date",
        HR."Hour" AS "Hour",
        HR."Minute" AS "Minute"
    FROM
        HR, MA
    WHERE
        (HR."Id" = MA."Id" AND HR."Date" = MA."Date" AND HR."Hour" = MA."Hour" AND HR."Minute" = MA."Minute")
)

SELECT
    'HR<60 & C>3' "Condition",
    COUNT("HeartRate") AS "Number Of Records"
FROM
    HRMA_MINUTE_COMMON
WHERE
    "HeartRate" < 60 AND "Calories" > 3

UNION

SELECT
    'HR>100 & C<1',
    COUNT("HeartRate")
FROM
    HRMA_MINUTE_COMMON
WHERE
    "HeartRate" > 100 AND "Calories" < 1

UNION

SELECT
    'HR>150 & C<5' "Condition",
    COUNT("HeartRate")
FROM
    HRMA_MINUTE_COMMON
WHERE
    "HeartRate" > 150 AND "Calories" < 5
```

<div class="knitsql-table">

| Condition      | Number Of Records |
|:---------------|------------------:|
| HR\<60 & C\>3  |              1142 |
| HR\>150 & C\<5 |                16 |
| HR\>100 & C\<1 |                24 |

3 records

</div>

### (27) Query to find records common in tables “heartRate” and “hourActivity” with corresponding metrics at hour level

``` sql
WITH HR AS (
    SELECT "Id", FLOOR(AVG("Value")) AS "HeartRate", "Date", "Hour"
    FROM public."heartRate"
    GROUP BY "Id", "Date", "Hour"
),
HA AS (
    SELECT "Id", "Calories", "TotalIntensity", "StepTotal", "Date", "Hour"
    FROM public."hourActivity"
)

SELECT
    HR."Id",
    HR."HeartRate",
    HA."Calories",
    HA."TotalIntensity",
    HA."StepTotal",
    HR."Date",
    HR."Hour"
FROM
    HR, HA
WHERE
    (HR."Id" = HA."Id" AND HR."Date" = HA."Date" AND HR."Hour" = HA."Hour")
ORDER BY
    HR."Id", HR."Date", HR."Hour"
```

<div class="knitsql-table">

|         Id | HeartRate | Calories | TotalIntensity | StepTotal | Date       | Hour |
|-----------:|----------:|---------:|---------------:|----------:|:-----------|-----:|
| 2022484408 |        83 |      136 |             28 |       847 | 2016-04-12 |    7 |
| 2022484408 |        68 |       99 |             13 |       334 | 2016-04-12 |    8 |
| 2022484408 |        66 |       90 |             13 |       243 | 2016-04-12 |    9 |
| 2022484408 |       106 |      369 |            143 |      5243 | 2016-04-12 |   10 |
| 2022484408 |        67 |       99 |             20 |       323 | 2016-04-12 |   11 |
| 2022484408 |        66 |       86 |             11 |       184 | 2016-04-12 |   12 |
| 2022484408 |        83 |      124 |             19 |       658 | 2016-04-12 |   13 |
| 2022484408 |        80 |      197 |             54 |      2168 | 2016-04-12 |   14 |
| 2022484408 |        68 |       91 |              9 |       327 | 2016-04-12 |   15 |
| 2022484408 |        71 |      112 |             19 |       539 | 2016-04-12 |   16 |

Displaying records 1 - 10

</div>

### (28) Query to find errors and outliers for various metrics in the common table of “heartRate” and “hourActivity”

``` sql
WITH HR AS (
    SELECT "Id", FLOOR(AVG("Value")) AS "HeartRate", "Date", "Hour"
    FROM public."heartRate"
    GROUP BY "Id", "Date", "Hour"
),
HA AS (
    SELECT "Id", "Calories", "TotalIntensity", "StepTotal", "Date", "Hour"
    FROM public."hourActivity"
),
HRHA_HOUR_COMMON AS (
    SELECT
        HR."Id",
        HR."HeartRate",
        HA."Calories",
        HA."TotalIntensity",
        HA."StepTotal",
        HR."Date",
        HR."Hour"
    FROM
        HR, HA
    WHERE
        (HR."Id" = HA."Id" AND HR."Date" = HA."Date" AND HR."Hour" = HA."Hour")
)

SELECT
    'HR<60 & C>180' "Condition",
    COUNT("HeartRate") AS "Number Of Records"
FROM
    HRHA_HOUR_COMMON
WHERE
    "HeartRate" < 60 AND "Calories" > 180

UNION

SELECT
    'HR>100 & C<60',
    COUNT("HeartRate")
FROM
    HRHA_HOUR_COMMON
WHERE
    "HeartRate" > 100 AND "Calories" < 60

UNION

SELECT
    'HR>150 & C<300',
    COUNT("HeartRate")
FROM
    HRHA_HOUR_COMMON
WHERE
    "HeartRate" > 150 AND "Calories" < 300
```

<div class="knitsql-table">

| Condition        | Number Of Records |
|:-----------------|------------------:|
| HR\>150 & C\<300 |                 1 |
| HR\>100 & C\<60  |                 2 |
| HR\<60 & C\>180  |                 1 |

3 records

</div>

### (29) Query to find records common in tables “sleepInfo” and “dailyActivity”

``` sql
WITH DA AS (
    SELECT *
    FROM public."dailyActivity"
),
SI_AGG AS (
    SELECT "Id", "SleepDay", SUM("TotalMinutesAsleep") AS "TotalMinutesAsleep", SUM("TotalTimeInBed") AS "TotalTimeInBed"
    FROM public."sleepInfo"
    GROUP BY "Id", "SleepDay"
)

SELECT
    DA."Id",
    DA."ActivityDate",
    DA."TotalSteps",
    DA."TotalDistance",
    DA."VeryActiveDistance",
    DA."ModeratelyActiveDistance",
    DA."LightActiveDistance",
    DA."SedentaryActiveDistance",
    DA."VeryActiveMinutes",
    DA."FairlyActiveMinutes",
    DA."LightlyActiveMinutes",
    DA."SedentaryMinutes",
    DA."Calories",
    SI_AGG."TotalMinutesAsleep",
    SI_AGG."TotalTimeInBed"
FROM
    DA, SI_AGG
WHERE
    DA."Id" = SI_AGG."Id"
    AND
    DA."ActivityDate" = SI_AGG."SleepDay"
```

<div class="knitsql-table">

|         Id | ActivityDate | TotalSteps | TotalDistance | VeryActiveDistance | ModeratelyActiveDistance | LightActiveDistance | SedentaryActiveDistance | VeryActiveMinutes | FairlyActiveMinutes | LightlyActiveMinutes | SedentaryMinutes | Calories | TotalMinutesAsleep | TotalTimeInBed |
|-----------:|:-------------|-----------:|--------------:|-------------------:|-------------------------:|--------------------:|------------------------:|------------------:|--------------------:|---------------------:|-----------------:|---------:|-------------------:|---------------:|
| 1503960366 | 2016-04-12   |      13162 |          8.50 |               1.88 |                     0.55 |                6.06 |                       0 |                25 |                  13 |                  328 |              728 |     1985 |                327 |            346 |
| 1503960366 | 2016-04-13   |      10735 |          6.97 |               1.57 |                     0.69 |                4.71 |                       0 |                21 |                  19 |                  217 |              776 |     1797 |                384 |            407 |
| 1503960366 | 2016-04-15   |       9762 |          6.28 |               2.14 |                     1.26 |                2.83 |                       0 |                29 |                  34 |                  209 |              726 |     1745 |                412 |            442 |
| 1503960366 | 2016-04-16   |      12669 |          8.16 |               2.71 |                     0.41 |                5.04 |                       0 |                36 |                  10 |                  221 |              773 |     1863 |                340 |            367 |
| 1503960366 | 2016-04-17   |       9705 |          6.48 |               3.19 |                     0.78 |                2.51 |                       0 |                38 |                  20 |                  164 |              539 |     1728 |                700 |            712 |
| 1503960366 | 2016-04-19   |      15506 |          9.88 |               3.53 |                     1.32 |                5.03 |                       0 |                50 |                  31 |                  264 |              775 |     2035 |                304 |            320 |
| 1503960366 | 2016-04-20   |      10544 |          6.68 |               1.96 |                     0.48 |                4.24 |                       0 |                28 |                  12 |                  205 |              818 |     1786 |                360 |            377 |
| 1503960366 | 2016-04-21   |       9819 |          6.34 |               1.34 |                     0.35 |                4.65 |                       0 |                19 |                   8 |                  211 |              838 |     1775 |                325 |            364 |
| 1503960366 | 2016-04-23   |      14371 |          9.04 |               2.81 |                     0.87 |                5.36 |                       0 |                41 |                  21 |                  262 |              732 |     1949 |                361 |            384 |
| 1503960366 | 2016-04-24   |      10039 |          6.41 |               2.92 |                     0.21 |                3.28 |                       0 |                39 |                   5 |                  238 |              709 |     1788 |                430 |            449 |

Displaying records 1 - 10

</div>

### (30) Query to find errors and outliers for various metrics in the common table of “sleepInfo” and “dailyActivity”

``` sql
WITH SI AS (
    SELECT "Id", "SleepDay", "TotalMinutesAsleep", "TotalTimeInBed"
    FROM public."sleepInfo"
),
DA AS (
    SELECT "Id", "ActivityDate","VeryActiveMinutes", "FairlyActiveMinutes", "LightlyActiveMinutes", "SedentaryMinutes"
    FROM public."dailyActivity"
)

SELECT
    'BedTime > SedentaryMinutes' "Condition",
    COUNT(*) AS "Number Of Records"
FROM
    SI, DA
WHERE
    (SI."Id" = DA."Id" AND SI."SleepDay" = DA."ActivityDate")
    AND
    (SI."TotalTimeInBed" > DA."SedentaryMinutes")

UNION

SELECT
    'SleepTime > SedentaryMinutes',
    COUNT(*)
FROM
    SI, DA
WHERE
    (SI."Id" = DA."Id" AND SI."SleepDay" = DA."ActivityDate")
    AND
    (SI."TotalMinutesAsleep" > DA."SedentaryMinutes")

UNION

SELECT
    'BedTime > AvailableMinutes',
    COUNT(*)
FROM
    SI, DA
WHERE
    (SI."Id" = DA."Id" AND SI."SleepDay" = DA."ActivityDate")
    AND
    (SI."TotalTimeInBed" > (DA."VeryActiveMinutes" + DA."FairlyActiveMinutes" + DA."LightlyActiveMinutes" + DA."SedentaryMinutes"))

UNION

SELECT
    'SleepTime > AvailableMinutes',
    COUNT(*)
FROM
    SI, DA
WHERE
    (SI."Id" = DA."Id" AND SI."SleepDay" = DA."ActivityDate")
    AND
    (SI."TotalMinutesAsleep" > (DA."VeryActiveMinutes" + DA."FairlyActiveMinutes" + DA."LightlyActiveMinutes" + DA."SedentaryMinutes"))
```

<div class="knitsql-table">

| Condition                     | Number Of Records |
|:------------------------------|------------------:|
| BedTime \> AvailableMinutes   |                17 |
| SleepTime \> AvailableMinutes |                11 |
| BedTime \> SedentaryMinutes   |                57 |
| SleepTime \> SedentaryMinutes |                44 |

4 records

</div>

### (31) Query to find errors and outliers for various metrics in the tables “dailyActivity”, “hourActivity” and “minuteActivity”

``` sql
WITH DA AS (
    SELECT *
    FROM public."dailyActivity"
),
HA_AGG AS (
    SELECT "Id", "Date", SUM("Calories") AS "TotalCalories", SUM("StepTotal") AS "TotalSteps", SUM("TotalIntensity") AS "TotalIntensity"
    FROM public."hourActivity"
    GROUP BY "Id", "Date"
),
MA_AGG AS (
    SELECT "Id", "Date", SUM("Calories") AS "TotalCalories", SUM("Steps") AS "TotalSteps", SUM("Intensity") AS "TotalIntensity", COUNT("Minute") AS "TotalMinutes"
    FROM public."minuteActivity"
    GROUP BY "Id", "Date"
)

SELECT
    'DA_Calories > HA_Calories' "Condition",
    COUNT(*) AS "Number Of Records",
    COUNT(DISTINCT DA."Id") AS "Number Of Users"
FROM
    DA, HA_AGG
WHERE
    (DA."Id" = HA_AGG."Id" AND DA."ActivityDate" = HA_AGG."Date")
    AND
    DA."Calories" > HA_AGG."TotalCalories"

UNION

SELECT
    'DA_Calories < HA_Calories',
    COUNT(*),
    COUNT(DISTINCT DA."Id")
FROM
    DA, HA_AGG
WHERE
    (DA."Id" = HA_AGG."Id" AND DA."ActivityDate" = HA_AGG."Date")
    AND
    DA."Calories" < HA_AGG."TotalCalories"

UNION

SELECT
    'DA_Calories = HA_Calories',
    COUNT(*),
    COUNT(DISTINCT DA."Id")
FROM
    DA, HA_AGG
WHERE
    (DA."Id" = HA_AGG."Id" AND DA."ActivityDate" = HA_AGG."Date")
    AND
    DA."Calories" = HA_AGG."TotalCalories"

UNION

SELECT
    'DA_Steps > HA_Steps',
    COUNT(*),
    COUNT(DISTINCT DA."Id")
FROM
    DA, HA_AGG
WHERE
    (DA."Id" = HA_AGG."Id" AND DA."ActivityDate" = HA_AGG."Date")
    AND
    DA."TotalSteps" > HA_AGG."TotalSteps"

UNION

SELECT
    'DA_Steps < HA_Steps',
    COUNT(*),
    COUNT(DISTINCT DA."Id")
FROM
    DA, HA_AGG
WHERE
    (DA."Id" = HA_AGG."Id" AND DA."ActivityDate" = HA_AGG."Date")
    AND
    DA."TotalSteps" < HA_AGG."TotalSteps"

UNION

SELECT
    'DA_Steps = HA_Steps',
    COUNT(*),
    COUNT(DISTINCT DA."Id")
FROM
    DA, HA_AGG
WHERE
    (DA."Id" = HA_AGG."Id" AND DA."ActivityDate" = HA_AGG."Date")
    AND
    DA."TotalSteps" = HA_AGG."TotalSteps"

UNION

SELECT
    'DA_Calories > MA_Calories',
    COUNT(*),
    COUNT(DISTINCT DA."Id")
FROM
    DA, MA_AGG
WHERE
    (DA."Id" = MA_AGG."Id" AND DA."ActivityDate" = MA_AGG."Date")
    AND
    DA."Calories" > MA_AGG."TotalCalories"

UNION

SELECT
    'DA_Calories < MA_Calories',
    COUNT(*),
    COUNT(DISTINCT DA."Id")
FROM
    DA, MA_AGG
WHERE
    (DA."Id" = MA_AGG."Id" AND DA."ActivityDate" = MA_AGG."Date")
    AND
    DA."Calories" < MA_AGG."TotalCalories"

UNION

SELECT
    'DA_Calories = MA_Calories',
    COUNT(*),
    COUNT(DISTINCT DA."Id")
FROM
    DA, MA_AGG
WHERE
    (DA."Id" = MA_AGG."Id" AND DA."ActivityDate" = MA_AGG."Date")
    AND
    DA."Calories" = MA_AGG."TotalCalories"

UNION

SELECT
    'DA_Steps > MA_Steps',
    COUNT(*),
    COUNT(DISTINCT DA."Id")
FROM
    DA, MA_AGG
WHERE
    (DA."Id" = MA_AGG."Id" AND DA."ActivityDate" = MA_AGG."Date")
    AND
    DA."TotalSteps" > MA_AGG."TotalSteps"

UNION

SELECT
    'DA_Steps < MA_Steps',
    COUNT(*),
    COUNT(DISTINCT DA."Id")
FROM
    DA, MA_AGG
WHERE
    (DA."Id" = MA_AGG."Id" AND DA."ActivityDate" = MA_AGG."Date")
    AND
    DA."TotalSteps" < MA_AGG."TotalSteps"

UNION

SELECT
    'DA_Steps = MA_Steps',
    COUNT(*),
    COUNT(DISTINCT DA."Id")
FROM
    DA, MA_AGG
WHERE
    (DA."Id" = MA_AGG."Id" AND DA."ActivityDate" = MA_AGG."Date")
    AND
    DA."TotalSteps" = MA_AGG."TotalSteps"

UNION

SELECT
    'HA_Calories > MA_Calories',
    COUNT(*),
    COUNT(DISTINCT HA_AGG."Id")
FROM
    HA_AGG, MA_AGG
WHERE
    (HA_AGG."Id" = MA_AGG."Id" AND HA_AGG."Date" = MA_AGG."Date")
    AND
    HA_AGG."TotalCalories" > MA_AGG."TotalCalories"

UNION

SELECT
    'HA_Calories < MA_Calories',
    COUNT(*),
    COUNT(DISTINCT HA_AGG."Id")
FROM
    HA_AGG, MA_AGG
WHERE
    (HA_AGG."Id" = MA_AGG."Id" AND HA_AGG."Date" = MA_AGG."Date")
    AND
    HA_AGG."TotalCalories" < MA_AGG."TotalCalories"

UNION

SELECT
    'HA_Calories = MA_Calories',
    COUNT(*),
    COUNT(DISTINCT HA_AGG."Id")
FROM
    HA_AGG, MA_AGG
WHERE
    (HA_AGG."Id" = MA_AGG."Id" AND HA_AGG."Date" = MA_AGG."Date")
    AND
    HA_AGG."TotalCalories" = MA_AGG."TotalCalories"

UNION

SELECT
    'HA_Steps > MA_Steps',
    COUNT(*),
    COUNT(DISTINCT HA_AGG."Id")
FROM
    HA_AGG, MA_AGG
WHERE
    (HA_AGG."Id" = MA_AGG."Id" AND HA_AGG."Date" = MA_AGG."Date")
    AND
    HA_AGG."TotalSteps" > MA_AGG."TotalSteps"

UNION

SELECT
    'HA_Steps < MA_Steps',
    COUNT(*),
    COUNT(DISTINCT HA_AGG."Id")
FROM
    HA_AGG, MA_AGG
WHERE
    (HA_AGG."Id" = MA_AGG."Id" AND HA_AGG."Date" = MA_AGG."Date")
    AND
    HA_AGG."TotalSteps" < MA_AGG."TotalSteps"

UNION

SELECT
    'HA_Steps = MA_Steps',
    COUNT(*),
    COUNT(DISTINCT HA_AGG."Id")
FROM
    HA_AGG, MA_AGG
WHERE
    (HA_AGG."Id" = MA_AGG."Id" AND HA_AGG."Date" = MA_AGG."Date")
    AND
    HA_AGG."TotalSteps" = MA_AGG."TotalSteps"

UNION

SELECT
    'HA_Intensity > MA_Intensity',
    COUNT(*),
    COUNT(DISTINCT HA_AGG."Id")
FROM
    HA_AGG, MA_AGG
WHERE
    (HA_AGG."Id" = MA_AGG."Id" AND HA_AGG."Date" = MA_AGG."Date")
    AND
    HA_AGG."TotalIntensity" > MA_AGG."TotalIntensity"

UNION

SELECT
    'HA_Intensity < MA_Intensity',
    COUNT(*),
    COUNT(DISTINCT HA_AGG."Id")
FROM
    HA_AGG, MA_AGG
WHERE
    (HA_AGG."Id" = MA_AGG."Id" AND HA_AGG."Date" = MA_AGG."Date")
    AND
    HA_AGG."TotalIntensity" < MA_AGG."TotalIntensity"

UNION

SELECT
    'HA_Intensity = MA_Intensity',
    COUNT(*),
    COUNT(DISTINCT HA_AGG."Id")
FROM
    HA_AGG, MA_AGG
WHERE
    (HA_AGG."Id" = MA_AGG."Id" AND HA_AGG."Date" = MA_AGG."Date")
    AND
    HA_AGG."TotalIntensity" = MA_AGG."TotalIntensity"

UNION

SELECT
    'DA_TotalMinutes > MA_TotalMinutes',
    COUNT(*),
    COUNT(DISTINCT DA."Id")
FROM
    DA, MA_AGG
WHERE
    (DA."Id" = MA_AGG."Id" AND DA."ActivityDate" = MA_AGG."Date")
    AND
    (DA."VeryActiveMinutes" + DA."FairlyActiveMinutes" + DA."LightlyActiveMinutes" + DA."SedentaryMinutes") > MA_AGG."TotalMinutes"

UNION

SELECT
    'DA_TotalMinutes < MA_TotalMinutes',
    COUNT(*),
    COUNT(DISTINCT DA."Id")
FROM
    DA, MA_AGG
WHERE
    (DA."Id" = MA_AGG."Id" AND DA."ActivityDate" = MA_AGG."Date")
    AND
    (DA."VeryActiveMinutes" + DA."FairlyActiveMinutes" + DA."LightlyActiveMinutes" + DA."SedentaryMinutes") < MA_AGG."TotalMinutes"

UNION

SELECT
    'DA_TotalMinutes = MA_TotalMinutes',
    COUNT(*),
    COUNT(DISTINCT DA."Id")
FROM
    DA, MA_AGG
WHERE
    (DA."Id" = MA_AGG."Id" AND DA."ActivityDate" = MA_AGG."Date")
    AND
    (DA."VeryActiveMinutes" + DA."FairlyActiveMinutes" + DA."LightlyActiveMinutes" + DA."SedentaryMinutes") = MA_AGG."TotalMinutes"

UNION

SELECT
    'DA_Calories = 0 BUT HA_Calories > 0',
    COUNT(*),
    COUNT(DISTINCT DA."Id")
FROM
    DA, HA_AGG
WHERE
    (DA."Id" = HA_AGG."Id" AND DA."ActivityDate" = HA_AGG."Date")
    AND
    DA."Calories" = 0 AND HA_AGG."TotalCalories" > 0

UNION

SELECT
    'DA_Calories = 0 BUT MA_Calories > 0',
    COUNT(*),
    COUNT(DISTINCT DA."Id")
FROM
    DA, MA_AGG
WHERE
    (DA."Id" = MA_AGG."Id" AND DA."ActivityDate" = MA_AGG."Date")
    AND
    DA."Calories" = 0 AND MA_AGG."TotalCalories" > 0
```

<div class="knitsql-table">

| Condition                            | Number Of Records | Number Of Users |
|:-------------------------------------|------------------:|----------------:|
| DA_Steps \> MA_Steps                 |               159 |              31 |
| DA_Calories = 0 BUT HA_Calories \> 0 |                 0 |               0 |
| DA_Steps = HA_Steps                  |               775 |              33 |
| HA_Intensity = MA_Intensity          |               930 |              33 |
| DA_TotalMinutes \> MA_TotalMinutes   |                19 |              19 |
| DA_Steps \< HA_Steps                 |                 0 |               0 |
| HA_Intensity \< MA_Intensity         |                 0 |               0 |
| HA_Calories \> MA_Calories           |               447 |              32 |
| HA_Calories = MA_Calories            |                 0 |               0 |
| DA_Calories \> MA_Calories           |               191 |              33 |

Displaying records 1 - 10

</div>

<hr>

## Queries to save data for Visualization

The output of the following queries was saved to be used in data
visualization.

### (32) Query to filter data of users having records of atleast 3 Weeks in the table “dailyActivity”

``` sql
SELECT
    *
FROM
    public."dailyActivity"
WHERE
    "Id" IN (SELECT "Id" FROM public."dailyActivity" GROUP BY "Id" HAVING COUNT(DISTINCT "ActivityDate") >= 21)
```

<div class="knitsql-table">

|         Id | ActivityDate | TotalSteps | TotalDistance | TrackerDistance | LoggedActivitiesDistance | VeryActiveDistance | ModeratelyActiveDistance | LightActiveDistance | SedentaryActiveDistance | VeryActiveMinutes | FairlyActiveMinutes | LightlyActiveMinutes | SedentaryMinutes | Calories | DayOfWeek |
|-----------:|:-------------|-----------:|--------------:|----------------:|-------------------------:|-------------------:|-------------------------:|--------------------:|------------------------:|------------------:|--------------------:|---------------------:|-----------------:|---------:|:----------|
| 1503960366 | 2016-04-12   |      13162 |          8.50 |            8.50 |                        0 |               1.88 |                     0.55 |                6.06 |                       0 |                25 |                  13 |                  328 |              728 |     1985 | Tuesday   |
| 1503960366 | 2016-04-13   |      10735 |          6.97 |            6.97 |                        0 |               1.57 |                     0.69 |                4.71 |                       0 |                21 |                  19 |                  217 |              776 |     1797 | Wednesday |
| 1503960366 | 2016-04-14   |      10460 |          6.74 |            6.74 |                        0 |               2.44 |                     0.40 |                3.91 |                       0 |                30 |                  11 |                  181 |             1218 |     1776 | Thursday  |
| 1503960366 | 2016-04-15   |       9762 |          6.28 |            6.28 |                        0 |               2.14 |                     1.26 |                2.83 |                       0 |                29 |                  34 |                  209 |              726 |     1745 | Friday    |
| 1503960366 | 2016-04-16   |      12669 |          8.16 |            8.16 |                        0 |               2.71 |                     0.41 |                5.04 |                       0 |                36 |                  10 |                  221 |              773 |     1863 | Saturday  |
| 1503960366 | 2016-04-17   |       9705 |          6.48 |            6.48 |                        0 |               3.19 |                     0.78 |                2.51 |                       0 |                38 |                  20 |                  164 |              539 |     1728 | Sunday    |
| 1503960366 | 2016-04-18   |      13019 |          8.59 |            8.59 |                        0 |               3.25 |                     0.64 |                4.71 |                       0 |                42 |                  16 |                  233 |             1149 |     1921 | Monday    |
| 1503960366 | 2016-04-19   |      15506 |          9.88 |            9.88 |                        0 |               3.53 |                     1.32 |                5.03 |                       0 |                50 |                  31 |                  264 |              775 |     2035 | Tuesday   |
| 1503960366 | 2016-04-20   |      10544 |          6.68 |            6.68 |                        0 |               1.96 |                     0.48 |                4.24 |                       0 |                28 |                  12 |                  205 |              818 |     1786 | Wednesday |
| 1503960366 | 2016-04-21   |       9819 |          6.34 |            6.34 |                        0 |               1.34 |                     0.35 |                4.65 |                       0 |                19 |                   8 |                  211 |              838 |     1775 | Thursday  |

Displaying records 1 - 10

</div>

### (33) Query to filter data of users having records of atleast 3 Weeks in the table “hourActivity”

``` sql
SELECT
    *
FROM
    public."hourActivity"
WHERE
    "Id" IN (SELECT "Id" FROM public."hourActivity" GROUP BY "Id" HAVING COUNT(DISTINCT "Date") >= 21)
```

<div class="knitsql-table">

|         Id | ActivityHour        | Calories | TotalIntensity | AverageIntensity | StepTotal | Hour | Date       |
|-----------:|:--------------------|---------:|---------------:|-----------------:|----------:|-----:|:-----------|
| 1503960366 | 2016-04-12 00:00:00 |       81 |             20 |         0.333333 |       373 |    0 | 2016-04-12 |
| 1503960366 | 2016-04-12 01:00:00 |       61 |              8 |         0.133333 |       160 |    1 | 2016-04-12 |
| 1503960366 | 2016-04-12 02:00:00 |       59 |              7 |         0.116667 |       151 |    2 | 2016-04-12 |
| 1503960366 | 2016-04-12 03:00:00 |       47 |              0 |         0.000000 |         0 |    3 | 2016-04-12 |
| 1503960366 | 2016-04-12 04:00:00 |       48 |              0 |         0.000000 |         0 |    4 | 2016-04-12 |
| 1503960366 | 2016-04-12 05:00:00 |       48 |              0 |         0.000000 |         0 |    5 | 2016-04-12 |
| 1503960366 | 2016-04-12 06:00:00 |       48 |              0 |         0.000000 |         0 |    6 | 2016-04-12 |
| 1503960366 | 2016-04-12 07:00:00 |       47 |              0 |         0.000000 |         0 |    7 | 2016-04-12 |
| 1503960366 | 2016-04-12 08:00:00 |       68 |             13 |         0.216667 |       250 |    8 | 2016-04-12 |
| 1503960366 | 2016-04-12 09:00:00 |      141 |             30 |         0.500000 |      1864 |    9 | 2016-04-12 |

Displaying records 1 - 10

</div>

### (34) Query to filter data of users having records of atleast 3 Weeks in the table “heartRate”

``` sql
SELECT
    *
FROM
    public."heartRate"
WHERE
    "Id" IN (SELECT "Id" FROM public."heartRate" GROUP BY "Id" HAVING COUNT(DISTINCT "Date") >= 21)
```

<div class="knitsql-table">

|         Id | Time                | Value | DayOfWeek | Date       | Hour | Minute |
|-----------:|:--------------------|------:|:----------|:-----------|-----:|-------:|
| 2022484408 | 2016-04-12 07:21:00 |    97 | Tuesday   | 2016-04-12 |    7 |     21 |
| 2022484408 | 2016-04-12 07:21:05 |   102 | Tuesday   | 2016-04-12 |    7 |     21 |
| 2022484408 | 2016-04-12 07:21:10 |   105 | Tuesday   | 2016-04-12 |    7 |     21 |
| 2022484408 | 2016-04-12 07:21:20 |   103 | Tuesday   | 2016-04-12 |    7 |     21 |
| 2022484408 | 2016-04-12 07:21:25 |   101 | Tuesday   | 2016-04-12 |    7 |     21 |
| 2022484408 | 2016-04-12 07:22:05 |    95 | Tuesday   | 2016-04-12 |    7 |     22 |
| 2022484408 | 2016-04-12 07:22:10 |    91 | Tuesday   | 2016-04-12 |    7 |     22 |
| 2022484408 | 2016-04-12 07:22:15 |    93 | Tuesday   | 2016-04-12 |    7 |     22 |
| 2022484408 | 2016-04-12 07:22:20 |    94 | Tuesday   | 2016-04-12 |    7 |     22 |
| 2022484408 | 2016-04-12 07:22:25 |    93 | Tuesday   | 2016-04-12 |    7 |     22 |

Displaying records 1 - 10

</div>

### (35) Query to filter data of users having records of atleast 3 Weeks in the table “sleepInfo”

``` sql
SELECT
    *
FROM
    public."sleepInfo"
WHERE
    "Id" IN (SELECT "Id" FROM public."sleepInfo" GROUP BY "Id" HAVING COUNT(DISTINCT "SleepDay") >= 21)
```

<div class="knitsql-table">

|         Id | SleepDay   | TotalSleepRecords | TotalMinutesAsleep | TotalTimeInBed | DayOfWeek |
|-----------:|:-----------|------------------:|-------------------:|---------------:|:----------|
| 1503960366 | 2016-04-12 |                 1 |                327 |            346 | Tuesday   |
| 1503960366 | 2016-04-13 |                 2 |                384 |            407 | Wednesday |
| 1503960366 | 2016-04-15 |                 1 |                412 |            442 | Friday    |
| 1503960366 | 2016-04-16 |                 2 |                340 |            367 | Saturday  |
| 1503960366 | 2016-04-17 |                 1 |                700 |            712 | Sunday    |
| 1503960366 | 2016-04-19 |                 1 |                304 |            320 | Tuesday   |
| 1503960366 | 2016-04-20 |                 1 |                360 |            377 | Wednesday |
| 1503960366 | 2016-04-21 |                 1 |                325 |            364 | Thursday  |
| 1503960366 | 2016-04-23 |                 1 |                361 |            384 | Saturday  |
| 1503960366 | 2016-04-24 |                 1 |                430 |            449 | Sunday    |

Displaying records 1 - 10

</div>

### (36) Query to filter data of users having records of atleast 3 Weeks in the common table of “dailyActivity” & “sleepInfo”

``` sql
WITH DA AS (
    SELECT *
    FROM public."dailyActivity"
    WHERE "Id" IN (SELECT "Id" FROM public."dailyActivity" GROUP BY "Id" HAVING COUNT(DISTINCT "ActivityDate") >= 21)
),
SI_AGG AS (
    SELECT "Id", "SleepDay", SUM("TotalMinutesAsleep") AS "TotalMinutesAsleep", SUM("TotalTimeInBed") AS "TotalTimeInBed"
    FROM public."sleepInfo"
    WHERE "Id" IN (SELECT "Id" FROM public."sleepInfo" GROUP BY "Id" HAVING COUNT(DISTINCT "SleepDay") >= 21)
    GROUP BY "Id", "SleepDay"
)

SELECT
    DA."Id",
    DA."ActivityDate",
    DA."TotalSteps",
    DA."TotalDistance",
    DA."VeryActiveDistance",
    DA."ModeratelyActiveDistance",
    DA."LightActiveDistance",
    DA."SedentaryActiveDistance",
    DA."VeryActiveMinutes",
    DA."FairlyActiveMinutes",
    DA."LightlyActiveMinutes",
    DA."SedentaryMinutes",
    DA."Calories",
    SI_AGG."TotalMinutesAsleep",
    SI_AGG."TotalTimeInBed"
FROM
    DA, SI_AGG
WHERE
    DA."Id" = SI_AGG."Id"
    AND
    DA."ActivityDate" = SI_AGG."SleepDay"
```

<div class="knitsql-table">

|         Id | ActivityDate | TotalSteps | TotalDistance | VeryActiveDistance | ModeratelyActiveDistance | LightActiveDistance | SedentaryActiveDistance | VeryActiveMinutes | FairlyActiveMinutes | LightlyActiveMinutes | SedentaryMinutes | Calories | TotalMinutesAsleep | TotalTimeInBed |
|-----------:|:-------------|-----------:|--------------:|-------------------:|-------------------------:|--------------------:|------------------------:|------------------:|--------------------:|---------------------:|-----------------:|---------:|-------------------:|---------------:|
| 1503960366 | 2016-04-12   |      13162 |          8.50 |               1.88 |                     0.55 |                6.06 |                       0 |                25 |                  13 |                  328 |              728 |     1985 |                327 |            346 |
| 1503960366 | 2016-04-13   |      10735 |          6.97 |               1.57 |                     0.69 |                4.71 |                       0 |                21 |                  19 |                  217 |              776 |     1797 |                384 |            407 |
| 1503960366 | 2016-04-15   |       9762 |          6.28 |               2.14 |                     1.26 |                2.83 |                       0 |                29 |                  34 |                  209 |              726 |     1745 |                412 |            442 |
| 1503960366 | 2016-04-16   |      12669 |          8.16 |               2.71 |                     0.41 |                5.04 |                       0 |                36 |                  10 |                  221 |              773 |     1863 |                340 |            367 |
| 1503960366 | 2016-04-17   |       9705 |          6.48 |               3.19 |                     0.78 |                2.51 |                       0 |                38 |                  20 |                  164 |              539 |     1728 |                700 |            712 |
| 1503960366 | 2016-04-19   |      15506 |          9.88 |               3.53 |                     1.32 |                5.03 |                       0 |                50 |                  31 |                  264 |              775 |     2035 |                304 |            320 |
| 1503960366 | 2016-04-20   |      10544 |          6.68 |               1.96 |                     0.48 |                4.24 |                       0 |                28 |                  12 |                  205 |              818 |     1786 |                360 |            377 |
| 1503960366 | 2016-04-21   |       9819 |          6.34 |               1.34 |                     0.35 |                4.65 |                       0 |                19 |                   8 |                  211 |              838 |     1775 |                325 |            364 |
| 1503960366 | 2016-04-23   |      14371 |          9.04 |               2.81 |                     0.87 |                5.36 |                       0 |                41 |                  21 |                  262 |              732 |     1949 |                361 |            384 |
| 1503960366 | 2016-04-24   |      10039 |          6.41 |               2.92 |                     0.21 |                3.28 |                       0 |                39 |                   5 |                  238 |              709 |     1788 |                430 |            449 |

Displaying records 1 - 10

</div>

### (37) Query to filter data of users having records of atleast 3 Weeks in the common table of “dailyActivity” & “heartRate”

``` sql
WITH DA AS (
    SELECT *
    FROM public."dailyActivity"
    WHERE "Id" IN (SELECT "Id" FROM public."dailyActivity" GROUP BY "Id" HAVING COUNT(DISTINCT "ActivityDate") >= 21)
),
HR_AGG AS (
    SELECT "Id", "Date", FLOOR(AVG("Value")) AS "AvgDailyHeartRate"
    FROM public."heartRate"
    WHERE "Id" IN (SELECT "Id" FROM public."heartRate" GROUP BY "Id" HAVING COUNT(DISTINCT "Date") >= 21)
    GROUP BY "Id", "Date"
)

SELECT
    DA."Id",
    DA."ActivityDate",
    DA."TotalSteps",
    DA."TotalDistance",
    DA."VeryActiveDistance",
    DA."ModeratelyActiveDistance",
    DA."LightActiveDistance",
    DA."SedentaryActiveDistance",
    DA."VeryActiveMinutes",
    DA."FairlyActiveMinutes",
    DA."LightlyActiveMinutes",
    DA."SedentaryMinutes",
    DA."Calories",
    HR_AGG."AvgDailyHeartRate"
FROM
    DA, HR_AGG
WHERE
    DA."Id" = HR_AGG."Id"
    AND
    DA."ActivityDate" = HR_AGG."Date"
```

<div class="knitsql-table">

|         Id | ActivityDate | TotalSteps | TotalDistance | VeryActiveDistance | ModeratelyActiveDistance | LightActiveDistance | SedentaryActiveDistance | VeryActiveMinutes | FairlyActiveMinutes | LightlyActiveMinutes | SedentaryMinutes | Calories | AvgDailyHeartRate |
|-----------:|:-------------|-----------:|--------------:|-------------------:|-------------------------:|--------------------:|------------------------:|------------------:|--------------------:|---------------------:|-----------------:|---------:|------------------:|
| 2022484408 | 2016-04-12   |      11875 |          8.34 |               3.31 |                     0.77 |                4.26 |                       0 |                42 |                  14 |                  227 |             1157 |     2390 |                75 |
| 2022484408 | 2016-04-13   |      12024 |          8.50 |               2.99 |                     0.10 |                5.41 |                       0 |                43 |                   5 |                  292 |             1100 |     2601 |                80 |
| 2022484408 | 2016-04-14   |      10690 |          7.50 |               2.48 |                     0.21 |                4.82 |                       0 |                32 |                   3 |                  257 |             1148 |     2312 |                72 |
| 2022484408 | 2016-04-15   |      11034 |          8.03 |               1.94 |                     0.31 |                5.78 |                       0 |                27 |                   9 |                  282 |             1122 |     2525 |                80 |
| 2022484408 | 2016-04-16   |      10100 |          7.09 |               3.15 |                     0.55 |                3.39 |                       0 |                41 |                  11 |                  151 |             1237 |     2177 |                75 |
| 2022484408 | 2016-04-17   |      15112 |         11.40 |               3.87 |                     0.66 |                6.88 |                       0 |                28 |                  29 |                  331 |             1052 |     2782 |                83 |
| 2022484408 | 2016-04-18   |      14131 |         10.07 |               3.64 |                     0.12 |                6.30 |                       0 |                48 |                   3 |                  311 |             1078 |     2770 |                82 |
| 2022484408 | 2016-04-19   |      11548 |          8.53 |               3.29 |                     0.24 |                5.00 |                       0 |                31 |                   7 |                  250 |             1152 |     2489 |                81 |
| 2022484408 | 2016-04-20   |      15112 |         10.67 |               3.34 |                     1.93 |                5.40 |                       0 |                48 |                  63 |                  276 |             1053 |     2897 |                83 |
| 2022484408 | 2016-04-21   |      12453 |          8.74 |               3.33 |                     1.11 |                4.31 |                       0 |               104 |                  53 |                  255 |             1028 |     3158 |                86 |

Displaying records 1 - 10

</div>

### (38) Query to filter data of users having records of atleast 3 Weeks in the common table of “hourActivity” & “heartRate”

``` sql
WITH HR AS (
    SELECT "Id", FLOOR(AVG("Value")) AS "HeartRate", "Date", "Hour"
    FROM public."heartRate"
    WHERE "Id" IN (SELECT "Id" FROM public."heartRate" GROUP BY "Id" HAVING COUNT(DISTINCT "Date") >= 21)
    GROUP BY "Id", "Date", "Hour"
),
HA AS (
    SELECT "Id", "Calories", "TotalIntensity", "StepTotal", "Date", "Hour"
    FROM public."hourActivity"
    WHERE "Id" IN (SELECT "Id" FROM public."hourActivity" GROUP BY "Id" HAVING COUNT(DISTINCT "Date") >= 21)
)

SELECT
    HR."Id",
    HR."HeartRate",
    HA."Calories",
    HA."TotalIntensity",
    HA."StepTotal",
    HR."Date",
    HR."Hour"
FROM
    HR, HA
WHERE
    (HR."Id" = HA."Id" AND HR."Date" = HA."Date" AND HR."Hour" = HA."Hour")
ORDER BY
    HR."Id", HR."Date", HR."Hour"
```

<div class="knitsql-table">

|         Id | HeartRate | Calories | TotalIntensity | StepTotal | Date       | Hour |
|-----------:|----------:|---------:|---------------:|----------:|:-----------|-----:|
| 2022484408 |        83 |      136 |             28 |       847 | 2016-04-12 |    7 |
| 2022484408 |        68 |       99 |             13 |       334 | 2016-04-12 |    8 |
| 2022484408 |        66 |       90 |             13 |       243 | 2016-04-12 |    9 |
| 2022484408 |       106 |      369 |            143 |      5243 | 2016-04-12 |   10 |
| 2022484408 |        67 |       99 |             20 |       323 | 2016-04-12 |   11 |
| 2022484408 |        66 |       86 |             11 |       184 | 2016-04-12 |   12 |
| 2022484408 |        83 |      124 |             19 |       658 | 2016-04-12 |   13 |
| 2022484408 |        80 |      197 |             54 |      2168 | 2016-04-12 |   14 |
| 2022484408 |        68 |       91 |              9 |       327 | 2016-04-12 |   15 |
| 2022484408 |        71 |      112 |             19 |       539 | 2016-04-12 |   16 |

Displaying records 1 - 10

</div>

<hr>

\*\* Thank You \*\*
