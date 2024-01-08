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

The datasets related to rides in Q1 of 2019 and Q1 of 2020 have been retrieved from [Q1_2019](https://docs.google.com/spreadsheets/d/1uCTsHlZLm4L7-ueaSLwDg0ut3BP_V4mKDo2IMpaXrk4/copy) & [Q1_2020](https://docs.google.com/spreadsheets/d/179QVLO_yu5BJEKFVZShsKag74ZaUYIF6FevLYzs3hRc/copy).

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
> df1 <- read_csv("Divvy_Trips_2019_Q1.csv")
Rows: 365069 Columns: 15                                                                                             
── Column specification ──────────────────────────────────────────────────────────
Delimiter: ","
chr (8): rideable_type, started_at, ended_at, start_station_name, end_station_...
dbl (6): ride_id, start_station_id, end_station_id, bikeid, birthyear, day_of_...
num (1): tripduration

ℹ Use `spec()` to retrieve the full column specification for this data.
ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
> df2 <- read_csv("Divvy_Trips_2020_Q1.csv")
Rows: 426887 Columns: 15                                                                                             
── Column specification ──────────────────────────────────────────────────────────
Delimiter: ","
chr  (7): ride_id, rideable_type, started_at, ended_at, start_station_name, en...
dbl  (7): start_station_id, end_station_id, start_lat, start_lng, end_lat, end...
time (1): ride_length

ℹ Use `spec()` to retrieve the full column specification for this data.
ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

Every dataset includes 15 variables including both qualitative and quantitative data with diverse data types. By utilizing the 'glimpse' function, we can observe the column names, data types of each column, and an overview of the initial rows within the dataset. The most recent dataset is as follows:

### Overview of the initial rows within the dataset
```{R Overview of the initial rows within the dataset}
> glimpse(df2)
Rows: 426,887
Columns: 15
$ ride_id            <chr> "EACB19130B0CDA4A", "8FED874C809DC021", "789F3C21E472…
$ rideable_type      <chr> "docked_bike", "docked_bike", "docked_bike", "docked_…
$ started_at         <chr> "1/21/2020 20:06", "1/30/2020 14:22", "1/9/2020 19:29…
$ ended_at           <chr> "1/21/2020 20:14", "1/30/2020 14:26", "1/9/2020 19:32…
$ start_station_name <chr> "Western Ave & Leland Ave", "Clark St & Montrose Ave"…
$ start_station_id   <dbl> 239, 234, 296, 51, 66, 212, 96, 96, 212, 38, 117, 181…
$ end_station_name   <chr> "Clark St & Leland Ave", "Southport Ave & Irving Park…
$ end_station_id     <dbl> 326, 318, 117, 24, 212, 96, 212, 212, 96, 100, 632, 9…
$ start_lat          <dbl> 41.9665, 41.9616, 41.9401, 41.8846, 41.8856, 41.8899,…
$ start_lng          <dbl> -87.6884, -87.6660, -87.6455, -87.6319, -87.6418, -87…
$ end_lat            <dbl> 41.9671, 41.9542, 41.9402, 41.8918, 41.8899, 41.8846,…
$ end_lng            <dbl> -87.6674, -87.6644, -87.6530, -87.6206, -87.6343, -87…
$ member_casual      <chr> "member", "member", "member", "member", "member", "me…
$ ride_length        <time> 00:07:31, 00:03:43, 00:02:51, 00:08:49, 00:05:32, 00…
$ day_of_week        <dbl> 3, 5, 5, 2, 5, 6, 6, 6, 6, 6, 3, 4, 4, 5, 3, 3, 3, 2,…
```

The dataset has important info for our question. We'll focus on the 'member_casual' column to compare user types. There are also dates and times for when each trip starts and ends. This helps us find trends in user ride times throughout the day, week, or year. The dataset also has location info, which helps us see any trends in where bikes are used.

### Deliverable:
A description of all data sources used: Prepare a well-organized, credible, and ethically handled dataset that can be effectively utilized to derive meaningful insights and address the marketing objectives outlined in the Cyclistic case study. The data utilized in this case study has been sourced from a publicly accessible repository of monthly datasets provided by Motivate International Inc. The datasets, available for download [here](https://divvy-tripdata.s3.amazonaws.com/index.html), are comprised of 'csv' files.


## Process
Key Objective: Make your data useful and easy to find by putting it together neatly, arranging it well, doing calculations, and spotting trends and connections.

First, we're going to aggregate both trips datasets together.

### Aggregate the datasets
```{R }
trips <- bind_rows(df1, df2)
```

