# Mustafa-s_portfolio
Data Analytics Portfolio

# How Does a Bike-Share Navigate Speedy Success?

## Introduction

This case study is part of the [Google Data Analytics Professional Certificate](https://www.coursera.org/professional-certificates/google-data-analytics?). The certificate program covers the data analysis process, as defined by Google:

- Ask
- Prepare
- Process
- Analyze
- Share
- Act

### Scenario

The Cyclistic bike-share program seeks to increase annual memberships. Lily Moreno, the Director of Marketing, aims to convert casual riders into members. The analytics team, analyzes historical data to understand usage patterns. The goal is to design a targeted marketing strategy for growth.

### Characters and teams

- Cyclistic.
- Lily Moreno.
- Cyclistic marketing analytics team.
- Cyclistic executive team.



## Ask
### Key Objective:
Identify the specific business question aligned with the stakeholders needs.

Cyclistic, launched in 2016, operates a successful bike-share program in Chicago over 5,800 bikes across 692 stations. The marketing strategy, based on general awareness and flexible pricing plans, distinguishes between casual riders (single or full-day passes) and Cyclistic members (annual memberships). Financially, annual members are more profitable. Lily Moreno, the Director of Marketing, aims to boost annual memberships by converting casual riders. The team plans to analyze historical bike trip data to uncover category-specific trends in pricing plans (single-ride, full-day, annual memberships) and rider types (casual riders vs. annual members) for targeted marketing strategies.


Three questions will guide the future marketing program:

1. How do annual members and casual riders use Cyclistic bikes differently?
2. Why would casual riders buy Cyclistic annual memberships?
3. How can Cyclistic use digital media to influence casual riders to become members?

### A clear statement of the business task
The goal is to convert casual riders to be members by identifying the key difference between annual members and casual riders using Cyclistic bikes.



## Prepare
### Key Objective:
Understand where the data is located and how it is organized within the datasets, and checking for biases and ensuring it adheres to the ROCCC principles.

The data utilized in this case study has been sourced from a publicly accessible repository of monthly datasets provided by Motivate International Inc. The datasets, available for download [here](https://divvy-tripdata.s3.amazonaws.com/index.html), are comprised of 'csv' files with [license](https://divvybikes.com/data-license-agreement).

The datasets related to rides in Q4 of 2023 have been retrieved from [Q4_2023](https://divvy-tripdata.s3.amazonaws.com/index.html). Choosing more data might lead to errors because the data exceeds the
memory available in the free plan.

### install packages
``` {R install packages}
> install.packages("tidyverse")
Installing package into ‘/cloud/lib/x86_64-pc-linux-gnu-library/4.3’
(as ‘lib’ is unspecified)
trying URL 'http://rspm/default/__linux__/focal/latest/src/contrib/tidyverse_2.0.0.tar.gz'
Content type 'application/x-gzip' length 425237 bytes (415 KB)
==================================================
downloaded 415 KB

* installing *binary* package ‘tidyverse’ ...
* DONE (tidyverse)

The downloaded source packages are in
	‘/tmp/RtmpcwTFsj/downloaded_packages’

> install.packages("geosphere")
Installing package into ‘/cloud/lib/x86_64-pc-linux-gnu-library/4.3’
(as ‘lib’ is unspecified)
trying URL 'http://rspm/default/__linux__/focal/latest/src/contrib/geosphere_1.5-18.tar.gz'
Content type 'application/x-gzip' length 1024851 bytes (1000 KB)
==================================================
downloaded 1000 KB

* installing *binary* package ‘geosphere’ ...
* DONE (geosphere)

The downloaded source packages are in
	‘/tmp/RtmpcwTFsj/downloaded_packages’
```
### Load Packages
```{R Load Packages}
> library(tidyverse)
> library(geosphere)

── Attaching packages ─────────────────────────────────────── tidyverse 1.3.1 ──

✔ ggplot2 3.3.5     ✔ purrr   0.3.4
✔ tibble  3.1.6     ✔ dplyr   1.0.8
✔ tidyr   1.2.0     ✔ stringr 1.4.0
✔ readr   2.1.2     ✔ forcats 0.5.1

── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
✖ dplyr::filter() masks stats::filter()
✖ dplyr::lag()    masks stats::lag()
```

### Importing datasets
```{R Importing datasets}
> df1 <- read_csv("202310-divvy-tripdata.csv")
Rows: 537113 Columns: 15                                                                                             
── Column specification ──────────────────────────────────────────────────────────
Delimiter: ","
chr  (9): ride_id, rideable_type, started_at, ended_at, start_station_name, st...
dbl  (5): start_lat, start_lng, end_lat, end_lng, day_of_week
time (1): ride_length

ℹ Use `spec()` to retrieve the full column specification for this data.
ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
Warning message:
One or more parsing issues, call `problems()` on your data frame for details,
e.g.:
  dat <- vroom(...)
  problems(dat) 
> df2 <- read_csv("202311-divvy-tripdata.csv")
Rows: 362518 Columns: 15                                                                                             
── Column specification ──────────────────────────────────────────────────────────
Delimiter: ","
chr  (9): ride_id, rideable_type, started_at, ended_at, start_station_name, st...
dbl  (5): start_lat, start_lng, end_lat, end_lng, day_of_week
time (1): ride_length

ℹ Use `spec()` to retrieve the full column specification for this data.
ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
Warning message:
One or more parsing issues, call `problems()` on your data frame for details,
e.g.:
  dat <- vroom(...)
  problems(dat) 
> df3 <- read_csv("202312-divvy-tripdata.csv")
Rows: 224073 Columns: 15                                                                                             
── Column specification ──────────────────────────────────────────────────────────
Delimiter: ","
chr  (9): ride_id, rideable_type, started_at, ended_at, start_station_name, st...
dbl  (5): start_lat, start_lng, end_lat, end_lng, day_of_week
time (1): ride_length

ℹ Use `spec()` to retrieve the full column specification for this data.
ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

Every dataset includes 15 variables including both qualitative and quantitative data with diverse data types. By utilizing the 'glimpse' function, we can observe the column names, data types of each column, and an overview of the initial rows within the dataset. The most recent dataset is as follows:

### Overview of the initial rows within the dataset
```{R Overview of the initial rows within the dataset}
> glimpse(df3)
Rows: 224,073
Columns: 15
$ ride_id            <chr> "C9BD54F578F57246", "CDBD92F067FA620E", "ABC0858E52CB…
$ rideable_type      <chr> "electric_bike", "electric_bike", "electric_bike", "e…
$ started_at         <chr> "12/2/2023 18:44", "12/2/2023 18:48", "12/24/2023 1:5…
$ ended_at           <chr> "12/2/2023 18:47", "12/2/2023 18:54", "12/24/2023 2:0…
$ start_station_name <chr> "", "", "", "", "", "", "", "", "", "", "", "", "", "…
$ start_station_id   <chr> "", "", "", "", "", "", "", "", "", "", "", "", "", "…
$ end_station_name   <chr> "", "", "", "", "", "", "", "", "", "", "", "", "", "…
$ end_station_id     <chr> "", "", "", "", "", "", "", "", "", "", "", "", "", "…
$ start_lat          <dbl> 41.92, 41.92, 41.89, 41.95, 41.92, 41.91, 41.99, 42.0…
$ start_lng          <dbl> -87.66, -87.66, -87.62, -87.65, -87.64, -87.63, -87.6…
$ end_lat            <dbl> 41.92, 41.89, 41.90, 41.94, 41.93, 41.88, 42.00, 41.9…
$ end_lng            <dbl> -87.66, -87.64, -87.64, -87.65, -87.64, -87.65, -87.6…
$ member_casual      <chr> "member", "member", "member", "member", "member", "me…
$ ride_length        <chr> "0:03:50", "0:06:29", "0:07:37", "0:04:52", "0:01:41"…
$ day_of_week        <int> 7, 7, 1, 1, 1, 1, 1, 1, 2, 1, 7, 7, 7, 1, 7, 1, 7, 3,…
```

The dataset has important info for our question. We'll focus on the 'member_casual' column to compare user types. There are also dates and times for when each trip starts and ends. This helps us find trends in user ride times throughout the day, week, or year. The dataset also has location info, which helps us see any trends in where bikes are used.

### Deliverable:
A description of all data sources used: Prepare a well-organized, credible, and ethically handled dataset that can be effectively utilized to derive meaningful insights and address the marketing objectives outlined in the Cyclistic case study. The data utilized in this case study has been sourced from a publicly accessible repository of monthly datasets provided by Motivate International Inc. The datasets, available for download [here](https://divvy-tripdata.s3.amazonaws.com/index.html), are comprised of 'csv' files.


## Process
Key Objective: Make your data useful and easy to find by putting it together neatly, arranging it well, doing calculations, and spotting trends and connections.

First, we're going to aggregate both trips datasets together.

### Merge the datasets
```{R }
trips <- bind_rows(df1,df2,df3)
```

Now that we've consolidated all datasets into a single dataframe, we proceed with the following actions to clean, aggregate, and transform the data for analysis.

