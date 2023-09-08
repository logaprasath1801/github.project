# BACKGROUND #

## Cyclistic ##
A bike-share program that features more than 5,800 bicycles and 600 docking stations. Cyclistic sets itself apart by also offering reclining bikes, hand tricycles, and cargo bikes, making bike-share more inclusive to people with disabilities and riders who can’t use a standard two-wheeled bike. The majority of riders opt for traditional bikes; about 8% of riders use the assistive options. Cyclistic users are more likely to ride for leisure, but about 30% use them to commute to work each day.

Until now, Cyclistic’s marketing strategy relied on building general awareness and appealing to broad consumer segments. One approach that helped make these things possible was the flexibility of its pricing plans: single-ride passes, full-day passes, and annual memberships. Customers who purchase single-ride or full-day passes are referred to as casual riders. Customers who purchase annual memberships are Cyclistic members.

Cyclistic’s finance analysts have concluded that annual members are much more profitable than casual riders. Although the pricing flexibility helps Cyclistic attract more customers, Moreno (the director of marketing and my manager) believes that maximizing the number of annual members will be key to future growth. Rather than creating a marketing campaign that targets all-new customers, Moreno believes there is a very good chance to convert casual riders into members. She notes that casual riders are already aware of the Cyclistic program and have chosen Cyclistic for their mobility needs.

Moreno has set a clear goal: Design marketing strategies aimed at converting casual riders into annual members. In order to do that, however, the marketing analyst team needs to better understand how annual members and casual riders differ, why casual riders would buy a membership, and how digital media could affect their marketing tactics. Moreno and her team are interested in analyzing the Cyclistic historical bike trip data to identify trends.

## Scenario ##

I am assuming to be a junior data analyst working in the marketing analyst team at Cyclistic, a bike-share company in Chicago. The director of marketing believes the company’s future success depends on maximizing the number of annual memberships. Therefore, my team wants to understand how casual riders and annual members use Cyclistic bikes differently. From these insights, my team will design a new marketing strategy to convert casual riders into annual members. But first, Cyclistic executives must approve our recommendations, so they must be backed up with compelling data insights and professional data visualizations.


# ASK #

## BUSINESS TASK ##
Devise marketing strategies to convert casual riders to members.

Analysis Questions
Three questions will guide the future marketing program:

* How do annual members and casual riders use Cyclistic bikes differently?
* Why would casual riders buy Cyclistic annual memberships?
* How can Cyclistic use digital media to influence casual riders to become members?

Moreno has assigned me the first question to answer: How do annual members and casual riders use Cyclistic bikes differently?

# PREPARE #

## Data Source: ##
I will use Cyclistic’s historical trip data to analyze and identify trends from Jan 2022 to Dec 2022 which can be downloaded from divvy_tripdata. The data has been made available by Motivate International Inc.

This is public data that can be used to explore how different customer types are using Cyclistic bikes. But note that data-privacy issues prohibit from using riders’ personally identifiable information. This means that we won’t be able to connect pass purchases to credit card numbers to determine if casual riders live in the Cyclistic service area or if they have purchased multiple single passes.

## DATA ORGANIZATION ##

There are 12 files with naming convention of YYYYMM-divvy-tripdata and each file includes information for one month, such as the ride id, bike type, start time, end time, start station, end station, start location, end location, and whether the rider is a member or not. The corresponding column names are ride_id, rideable_type, started_at, ended_at, start_station_name, start_station_id, end_station_name, end_station_id, start_lat, start_lng, end_lat, end_lng and member_casual.

## PROCESS ##

BigQuery is used to combine the various datasets into one dataset and clean it.
Reason:
A worksheet cannot be able to manage large amounts of data. The Cyclistic dataset has more than 5.6 million rows, it is essential to use a platform like BigQuery that supports huge volumes of data.

# COMBINING DATA #
[SQL Query: Data Combining](https://github.com/logaprasath1801/github.project/blob/main/Data%20Combination.Sql)
12 csv files are uploaded as tables in the dataset '2022_tripdata'. Another table named "combined_data" is created, containing 5,667,717 rows of data for the entire year.

# DATA EXPLORATION #
[SQL Query: Data Exploration](https://github.com/logaprasath1801/github.project/blob/main/Data%20Exploration.Sql)
Before cleaning the data, I am familiarizing myself with the data to find the inconsistencies
 * Made the Ride_id as a primary key.
 * Found the null values in each column.
 * As ride_id has no null values, let's use it to check for duplicates.
 * All ride_id values have length of 16 so no need to clean it.
 * The started_at and ended_at shows start and end time of the trip in YYYY-MM-DD hh:mm:ss UTC format. New column ride_length can be created to find the total trip duration. There are 5360 trips which 
   has duration longer than a day and 122283 trips having less than a minute duration or having end time earlier than start time so need to remove them. Other columns day_of_week and month can also be 
   helpful in analysis of trips at different times in a year.
 * Total of 833064 rows have both start_station_name and start_station_id missing which needs to be removed.
 * Total of 892742 rows have both end_station_name and end_station_id missing which needs to be removed.
 * Total of 5858 rows have both end_lat and end_lng missing which needs to be removed.
 * member_casual column has 2 uniqued values as member or casual rider.
 * Columns that need to be removed are start_station_id and end_station_id as they do not add value to analysis of our current problem. Longitude and latitude location columns may not be used in 
   analysis but can be used to visualise a map.


# DATA CLEANING #

[SQL Query: Data Cleaning](https://github.com/logaprasath1801/github.project/blob/main/Data%20Cleaning.Sql)

* All the rows having missing values are deleted.
* 3 more columns ride_length for duration of the trip, day_of_week and month are added.
* Trips with duration less than a minute and longer than a day are excluded.
* Total 1,375,912 rows are removed in this step.


# ANALYZE AND SHARE #
[SQL Query: Data Analysis](https://github.com/logaprasath1801/github.project/blob/main/Data%20Analysis)

The data is stored appropriately and is now prepared for analysis. I queried multiple relevant tables for the analysis and visualized them in Tableau.
The analysis question is: How do annual members and casual riders use Cyclistic bikes differently?

First of all, member and casual riders are compared by the type of bikes they are using.

![Screenshot 2023-09-08 090632](https://github.com/logaprasath1801/github.project/assets/142376223/bd61c988-df8c-4df6-a5c6-242d0186700a)

The members make 59.7% of the total while remaining 40.3% constitutes casual riders. Each bike type chart shows percentage from the total. Most used bike is classic bike followed by the electric bike. Docked bikes are used the least by only casual riders.

Next the number of trips distributed by the months, days of the week and hours of the day are examined.

![Screenshot 2023-09-08 091253](https://github.com/logaprasath1801/github.project/assets/142376223/ae27fe5e-0f21-4e9c-a589-d3fe079619dc)

Months: When it comes to monthly trips, both casual and members exhibit comparable behavior, with more trips in the spring and summer and fewer in the winter. The gap between casuals and members is closest in the month of july in summmer.
Days of Week: When the days of the week are compared, it is discovered that casual riders make more journeys on the weekends while members show a decline over the weekend in contrast to the other days of the week.
Hours of the Day: The members shows 2 peaks throughout the day in terms of number of trips. One is early in the morning at around 6 am to 8 am and other is in the evening at around 4 pm to 8 pm while number of trips for casual riders increase consistently over the day till evening and then decrease afterwards.

We can infer from the previous observations that member may be using bikes for commuting to and from the work in the week days while casual riders are using bikes throughout the day, more frequently over the weekends for leisure purposes. Both are most active in summer and spring.

Ride duration of the trips are compared to find the differences in the behavior of casual and member riders.


![Screenshot 2023-09-08 091358](https://github.com/logaprasath1801/github.project/assets/142376223/45c09044-0dc3-4095-a4c2-eb010cd9ead9)

Take note that casual riders tend to cycle longer than members do on average. The length of the average journey for members doesn't change throughout the year, week, or day. However, there are variations in how long casual riders cycle. In the spring and summer, on weekends, and from 10 am to 2 pm during the day, they travel greater distances. Between five and eight in the morning, they have brief trips.

These findings lead to the conclusion that casual commuters travel longer (approximately 2x more) but less frequently than members. They make longer journeys on weekends and during the day outside of commuting hours and in spring and summer season, so they might be doing so for recreation purposes.

SUMMARY:                                                                                                                                                                                                           
 |                  CASULAL                        |                                                                                   MEMBER                                                                  |
 |:------------------------------------------------------------------------------------------------------------| :---------------------------------------------------------------------------------------------|
 |  Prefer using bikes throughout the day, more frequently  over the summer and spring for leisure activities. |Prefer riding bikes on week days during commute hours (8 am / 5pm) in summer and spring.       |                                                          
 | Travel 2 times longer but less frequently than members                                                      | 	Travel more frequently but shorter rides (approximately half of casual riders' trip duration)| 

 # ACT #
 After identifying the differences between casual and member riders, marketing strategies to target casual riders can be developed to persuade them to become members.
 ## RECOMMENDATIONS: ##
 * Marketing campaigns might be conducted in spring and summer at tourist/recreational locations popular among casual riders.
 * Casual riders are most active on weekends and during the summer and spring, thus they may be offered seasonal or weekend-only memberships.
 * Casual riders use their bikes for longer durations than members. Offering discounts for longer rides may incentivize casual riders and entice members to ride for longer periods of time.
