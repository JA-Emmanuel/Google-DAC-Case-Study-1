# Google Data Analysis Capstone - Case Study 1: Cyclistic Bike Share Analysis


## Introduction 

This is my version of the Google Data Analytics Capstone - Case Study 1. The full document to the case study can be found in the Google Data Analytics Capstone: [Complete a Case Study]{https://www.coursera.org/learn/google-data-analytics-capstone/supplement/7PGIT/case-study-1-how-does-a-bike-share-navigate-speedy-success}.
The ask, prepare, process, analyse, share, act steps would be used to reach my goal of completing this case study.

I work as a junior data analyst in the marketing analysis team at Cyclistic, a bike share company in Chicago. In 2016, Cyclistic launched a successful bike-share offering. Since then, the program has grown to a fleet of 5,824 bicycles that are geotracked and locked into a network of 692 stations across Chicago. The bikes can be unlocked from one station and returned to any other station in the system anytime.
  Until now, Cyclistic’s marketing strategy relied on building general awareness and appealing to broad consumer segments. One approach that helped make these things possible was the flexibility of its pricing plans: single-ride passes, full-day passes,and annual memberships. Customers who purchase single-ride or full-day passes are referred to as casual riders. Customers who purchase annual memberships are Cyclistic members.
  Cyclistic’s finance analysts have concluded that annual members are much more profitable than casual riders. Although the pricing flexibility helps Cyclistic attract more customers, Lily Moreno,the director of marketing and my manager believes that maximizing the number of annual members will be key to future growth. Rather than creating a marketing campaign that targets all-new customers, She believes there is a very good chance to convert casual riders into members. She notes that casual riders are already aware of the Cyclistic program and have chosen Cyclistic for their mobility needs.

 
## Ask Phase
  
 Moreno has set a clear goal: Design marketing strategies aimed at converting casual riders into annual members. In order to do that, I and the rest of the marketing analyst team needs to better understand how annual members and casual riders differ, why casual riders would buy a membership, and how digital media could affect their marketing tactics. We are all interested in analyzing the Cyclistic historical bike trip data to identify trend. From these insights, the team will design a new marketing strategy to convert casual riders into annual members. But first, Cyclistic executives must approve my recommendations, so they must be backed up with compelling data insights and professional data visualizations.
 
## Data Preperation 
  Cyclistic's historical data to analyse and identify trends is downloaded and stored appropriately in a folder.The data source here being the Motivate International Inc. The recommended cyclistic trip data to be downloaded is that of the previous 12 months which spans from April 2021 to March 2022. The data for each month stored in the folder are then named according to the necessary naming conventions with the usage of underscores, capital letters and numbers. An example being the data for the month of April 2021 stored as "April_2021_tripdata". There are no issues with the data's credibility and the data is good data and that is because it is reliable: comes from a good data source and under a valid license, the data is original: validated with the company's original source, the data is comprehensive: contains all critical information we need, the data is current: downloaded the previous 12 months and so it is very useful at this particular time, the data is cited: from a source and a reliable one at that. 
  
## Data Processing 
  Clean data is the launchpad for any strong analysis. The integrity of data is very important and so this is always in a direct relationship with clean data, one of the famous rules of a data's integrity is it's ability to not be compromised. Thus getting rid of duplicates, blanks, irrelevant data that do not align with objectives, inconsistent data, outdated data, inaccurate/incorrect data helps us ensure data integrity- assurance of, data accuracy and consistency over its entire life-cycle. This data must be unique , values must match prescribed patterns, the data must be consistent, the data must be relevant to determine our course of action to achieve our intended objective. This phase ensures that data meets all these requirements.
  
The preferred tool for data processing for this analysis is R and RStudio. This is often considered the best place for this task and that is because it handles large complex data which excel and SQL would not be able to handle, it's convenient library of packages which offer a helpful combination of code and reusable R function among others. The processing of data using RStudio in R begins by installing the necessary packages. These packages include tidyverse-a collection of packages in R with a common design philosophy for data manipulation and exploration and visaulizations, lubridate-an R package that makes it easier to work with dates and times, dplyr- an R package that contains a set of functions (or “verbs”) that perform common data manipulation operations such as filtering for rows, selecting specific columns etc,ggplot2- an R package used for visualizations and readr- an R package for data importing. It's important to know that all these pacakges are core packages under the tidyverse

```{r data processing_installing_packages}
install.packages("tidyverse")
install.packages("lubridate")
install.packages("dplyr")
install.packages("ggplot2")
install.packages("readr")
install.packages("hydroTSM")
```
The next step of the data processing phase is to load the various packages using the library function

```{r data processing_load_packages}
library(tidyverse)
library(lubridate)
library(dplyr)
library(ggplot2)
library(readr)
library(hydroTSM)
```
I then imported the data into RStduio using the read_csv function contained in the readr package. This function would be used for each month's trip csv data 

```{r data processing_Import_csv}
April_2021_tripdata <- read_csv("202104-divvy-tripdata.csv")
May_2021_tripdata <- read_csv("202105-divvy-tripdata.csv")
JUn_2021_tripdata <- read_csv("202106-divvy-tripdata.csv")
July_2021_tripdata <- read_csv("202107-divvy-tripdata.csv")
Aug_2021_tripdata <- read_csv("202108-divvy-tripdata.csv")
Sep_2021_tripdata <- read_csv("202109-divvy-tripdata.csv")
Oct_2021_tripdata <- read_csv("202110-divvy-tripdata.csv")
Nov_2021_tripdata <- read_csv("202111-divvy-tripdata.csv")
Dec_2021_tripdata <- read_csv("202112-divvy-tripdata.csv")
Jan_2022_tripdata <- read_csv("202201-divvy-tripdata.csv")
Feb_2022_tripdata <- read_csv("202202-divvy-tripdata.csv")
Mar_2022_tripdata <- read_csv("202203-divvy-tripdata.csv")
```

Proceeded to merge every month's data into a larger data set using the rbind function. merging data makes analysis easier 

```{r data procesing_data_merging}
ride_tripdata <- rbind(April_2021_tripdata,May_2021_tripdata,JUn_2021_tripdata,July_2021_tripdata,Aug_2021_tripdata,Sep_2021_tripdata,Oct_2021_tripdata,Nov_2021_tripdata,Dec_2021_tripdata,Jan_2022_tripdata,Feb_2022_tripdata,Mar_2022_tripdata)
```

I then used the glimpse function to get an overview of the merged data set, ride_tripdata. This function reveals data type, the column names, the number of columns and also shows the rows in the merged data set.

```{r data processing_data_overview}
glimpse(ride_tripdata)
```
 I proceeded by extracting the day and month from the started_at column which is in the POSIXct format. This helps in deriving more insights from this data set
 
```{r data processing_extract_month_column_from_new_date_column}
ride_tripdata$date <- as.Date(ride_tripdata$started_at) #add a date column
ride_tripdata$month <- format(as.Date(ride_tripdata$started_at), "%B_%y")   #add a month column formatted as month and a 2-digit year (e.g. April_2021)
```

```{r data processing_extract_day_year_and_weekday_columns}
ride_tripdata$day <- format(as.Date(ride_tripdata$date), "%d")  #add a two-digit day column
ride_tripdata$year <- format(as.Date(ride_tripdata$date), "%Y")  #add a four-digit year column
ride_tripdata$weekday <- format(as.Date(ride_tripdata$date), "%A")  #add a weekday column
```
The ride length between the ended_time and started_time is calculated. Getting the ride length is important because it gives s fresh insights for our analysis 

```{r data processing_ride_length}
ride_tripdata$ride_length <- (as.double(difftime(ride_tripdata$ended_at,ride_tripdata$started_at)))/60 #calculate ride_length in minutes
```

```{r data processing_time_seasons}
ride_tripdata$year_season <- time2season(ride_tripdata$started_at,out.fmt = "seasons")
```

I now have a large data set with the necessary columns for analysis but first the data has to be cleaned; get rid of null values, get rid of duplicates, get rid of unnecessary information/columns.

```{r data_processing_data_cleaning}
ride_tripdata <- distinct(ride_tripdata) # get ride of duplicates
ride_tripdata <- ride_tripdata %>% drop_na() # get rid of null values
```

Remove ride_lengths of values less than a minute and more than an 1440 minutes/ an hour

```{r data processing_get_rid_of_unneccesary_ride_length}
ride_tripdata <- ride_tripdata %>% filter(ride_length > 1) %>% filter(ride_length < 1440) # get rid of ride_lengths of more than a day (1440 minutes)
```
Select columns that would be useful for this analysis

```{r data processing_select_columns}
ride_tripdatanew <- ride_tripdata%>%
  select(ride_id, rideable_type, started_at, ended_at, start_station_name,end_station_name, member_casual, month, year, weekday, ride_length,year_season)
```

```{r data processing_random_checks}
unique(ride_tripdatanew$rideable_type) # Check rideable types
unique(ride_tripdatanew$member_casual) # check member_casual 
```
## Analyse Phase

The next phase after the process phase(cleaning phase) is the analysis phase. Here calculations would take place which we would then use for data insights.

Explored the number of rides by weekdays

```{r data analyze_number_of_rides_by_weekday}
ride_tripdatanew %>% group_by(weekday) %>% 
  summarise(count=n()) %>% arrange(desc(count)) %>% View()
```

Calculated the mean, maximum and minimum ride length 

```{r data analyze_mean_max_min}
ride_tripdatanew %>%  # this was used to get the mean, max and min
  summarise(mean=mean(ride_length),max=max(ride_length), min=min(ride_length))
```

Calculated the mean ride_length for members and casual 

```{r data analyze_mean_ride_length_for_member_and_casual}
aggregate(ride_tripdatanew$ride_length ~ ride_tripdatanew$member_casual, FUN = mean)
```

I calculated the minimum and maximum ride lengths for the casual and member 

```{r data analyze_min_and_max_for_casual_and_member}
aggregate(ride_tripdatanew$ride_length ~ ride_tripdatanew$member_casual, FUN = max)
aggregate(ride_tripdatanew$ride_length ~ ride_tripdatanew$member_casual, FUN = min)
```

I calculated the mean ride length for users by day of the week 
```{r data analyze_mean_ride_length_weekdays}
aggregate(ride_tripdatanew$ride_length ~ ride_tripdatanew$weekday, FUN = mean)
```
I calculated the count of rides per season of the year

```{r data analyze_number_of_rides_by_season}
ride_tripdatanew %>% group_by(year_season) %>% 
  summarise(count=n()) %>% arrange(desc(count)) %>% View()
```
I then exported the data set to my PC for visualizations in power BI

```{r export}
write.csv(ride_tripdatanew,"C:\\Users\\Public\\Documents\\Cyclistic.csv", row.names=FALSE)
```

## Data Visualization
Below is a dashboard that perfectly visualizes the data for which we can draw insights  

![0002](https://user-images.githubusercontent.com/105708682/173146170-136eda49-cf3a-405a-849a-8937e86b5326.jpg)




## Data Insights 

Below are the data insights retrieved from the visualization of the Cyclistic data 

From the number of rides per season chart it is observed that members take more rides than casual over every season except during the summer period and that's because for casual riders the appeal of the summer is forever enticing for a stroll on the bike as compared to every other season. The members however dominate other seasons because it's very clear that the rides are their source of transportation to and from work. This is the same reason why in the number of rides per weekday line chart, we see the  members dominate over every weekday and casuals over the weekend.

As earlier seen, although the casual dominates number of rides only over the weekends the last line chart shows a total domination of average ride lengths for casual over members across every weekday. This means the casual members have a greater ride length with a fewer rides than the members who have more rides with lesser ride length. This proves that for members, their rides are a routine one comprising of similar ride lengths but for casuals we have rides of different increased durations.

### Recommendations 

A digital ad named "Passion Into Members" should be launched where we convince the casual riders to switch to members. How do we do this? For the members, its just a routine but for casaul riders it's something they are passionate about and hence we put out an ad that speaks to their passion, something they can relate to.  

We know of the decreasing number of rides during the winter, spring and autumn therefore we should provide for them a discount for their membership conversion, this shows how much we care and how much we know of the obsatcles they face in the form of weather.

Still on luring them, we can organise bicycle ride competitions for them every summer period. This lures other passionate bicylcle riders to enlist our services and thus be members. 

