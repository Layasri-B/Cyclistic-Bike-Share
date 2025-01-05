Google Data Analytics Professional Capstone
Case Study 1: How Does A Bike-Share Navigate Speedy Success?


1. Introduction

Cyclistic is a bike-share program that offers inclusive options such as reclining bikes, hand tricycles, and cargo bikes, making it accessible to people with disabilities. While most riders choose traditional bikes, about 8% use the assistive options. The program is popular for leisure rides, but around 30% of users also rely on it for daily commuting. Cyclistic aims to shift its marketing focus towards converting casual riders into annual members as they have proven to be more profitable. By analyzing historical bike trip data, the marketing team aims to understand the differences between annual members and casual riders, determine why casual riders would consider a membership, and explore the impact of digital media on marketing strategies.

2. Scenario

As a junior data analyst on the marketing team at Cyclistic, a bike-share company in Chicago, the director of marketing believes that the company's success hinges on maximizing annual memberships. Your team's objective is to gain an understanding of how casual riders and annual members utilize Cyclistic bikes differently. With these insights, you will develop a new marketing strategy to convert casual riders into annual members. However, your recommendations must be supported by compelling data insights and professional data visualizations in order to obtain approval from Cyclistic executives.

3. Data Analysis Process

In this case study, the six steps of the data analysis process will be used in order to solve this problem. Those 6 steps include:
1. Ask
2. Prepare
3. Process
4. Analyze
5. Share
6. Act

1: Ask

So here are the analysis questions that will help in the analysis:

1. How do annual members and casual riders use Cyclistic bikes differently?
2. What are the causes that drive casual riders to buy Cyclistic annual memberships?
3. How can Cyclistic use digital media as a tool to convert casual riders to members?

For this case, Lily Moreno who is the director of marketing has assigned me as a junior data analyst to the first question: *How do annual members and casual riders use Cyclistic bikes differently?*

2: Prepare

This step will address the data source that will be used for the analysis and the organization of the data structure.

**Data Source**:  Cyclistic’s historical trip data from Jan 2022 to Dec 2022 which is a public dataset published by Motivate International Inc. will be used to analyze and identify trends. [Click Here](https://divvy-tripdata.s3.amazonaws.com/index.html) for the dataset.

**Data Information**: In the data source, there are 12 files in total following the naming convention of *"YYYYMM-divvy-tripdata"*. Each file contains data for a specific month, including other details such as ride ID, bike type, start time, end time, start station, end station, start location, end location, and member status. The corresponding column names are:
- ride_id
- rideable_type
- started_at
- ended_at
- start_station_name
- start_station_id
- end_station_name
- end_station_id
- start_lat
- start_lng
- end_lat
- end_lng
- member_casual

3: Process

Data Combination

To help with data combination, the following SQL query is implemented in order to combine all 12 files into a single dataset. A new table named ***"all_tables"*** has been generated using the following code.:

```
  CREATE TABLE all_tables AS 
(
  SELECT * FROM Dec
  UNION ALL
  SELECT * FROM jan
  UNION ALL
  SELECT * FROM feb
  UNION ALL
  SELECT * FROM mar
  UNION ALL
  SELECT * FROM apr
  UNION ALL
  SELECT * FROM may
  UNION ALL
  SELECT * FROM june
  UNION ALL
  SELECT * FROM july
  UNION ALL
  SELECT * FROM aug
  UNION ALL
  SELECT * FROM sep
  UNION ALL
  SELECT * FROM oct
  UNION ALL
  SELECT * FROM nov
);
```

Then, to check the total row numbers, we perform this SQL query. The new dataset ***"all_tables"*** holds a total of 5,667,717 data rows encompassing the entire year:

```
SELECT COUNT(*) AS total_records FROM all_tables;
```
We perform the following code to show the complete dataset

```
select * from all_tables;
```

We perform the following code to show the first 100 rows of the dataset in order to understand the dataset better

```
select * from all_tables limit 100;
```

Data Exploration && Data Cleaning

To help ensure data cleanness, we have to check if the dataset has any null values in any column. However, it appears that there are no ***null*** values in the dataset:

```
select 
ride_id,
rideable_type,
started_at,
ended_at,
start_station_name,
end_station_name,
member_casual
from all_tables where
ride_id IS NULL
or rideable_type Is NULL
or started_at IS NULL
or ended_At IS NULL
or start_station_name IS NULL
or end_station_name IS NULL
or member_casual IS NULL;

```
If there are any null values in the dataset need to delete them from the dataset to ensure that the data is clean.

To delete the null values we can use the following code

```
delete from all_tables where start_station_name is NULL;
delete from all_tables where end_station_name is NULL;

```
After deleting check the dataset 
```
select * from all_tables;
```

After checking the null values, we also need to check if the dataset has any duplicate values. By performing this following code:

```
select 
ride_id,
rideable_type,
started_at,
ended_at,
start_station_name,
end_station_name,
member_casual ,
count(*) as duplicatecount from all_tables
group by 
ride_id,
rideable_type,
started_at,
ended_at,
start_station_name,
end_station_name,
member_casual 
having count(*)>1;

4 & 5: Analyze and Share

Data Analysis 
we have to analyse the Riders by using the member and casual users using the following code:

```
select count(ride_id) as no_of_riders, 
member_casual 
from all_tables 
group by member_casual;
```

We have to analyse the Rideable_type by using the member and casual users  using the following code:

```
select count(ride_id) as no_of_riders, 
rideable_type, member_casual 
from all_tables 
group by rideable_type, member_casual
order by count(ride_id) DESC;
```
The mostly used start stations by member and casual users 

```
select 
count(ride_id) as no_of_riders, 
start_station_name,
member_casual 
from all_tables 
group by start_station_name, member_casual
order by count(ride_id) desc limit 10;
```
The mostly used end stations by member and casual users 

```
select 
count(ride_id) as no_of_riders, 
end_station_name,
member_casual 
from all_tables 
group by end_station_name, member_casual
order by count(ride_id) desc;
```

start time analysis by using member and casual users through the following code:

The monthly analysis of the start time:

```
select 
count(ride_id) as no_of_users,
DATE_PART('MONTH', started_at) as started_month,
--datepart(weekday, started_at) as started_day,
--datepart(HOUR, started_at) as started_hour,
member_casual from all_tables
group by 
member_casual,
DATE_PART('MONTH', started_at)
order by count(ride_id) desc;
```

The weekly analysis of the start time:

```
select 
count(ride_id) as no_of_users,
--DATE_PART('MONTH', started_at) as started_month,
DATE_PART('day', started_at) as started_day,
--datepart('HOUR', started_at) as started_hour,
member_casual from all_tables
group by 
member_casual,
DATE_PART('day', started_at)
order by count(ride_id) desc;
```

The hour analysis of the start time:

```
select 
count(ride_id) as no_of_users,
--DATE_PART('month', started_at) as started_month,
--DATE_PART('day', started_at) as started_day,
DATE_PART('hour', started_at) as started_hour,
member_casual 
from all_tables
group by 
member_casual,
DATE_PART('hour', started_at)
order by count(ride_id) desc;
```

The End time analysis by using member and casual users through the following code:

The monthly analysis of the end time:

```
select 
count(ride_id) as no_of_users,
DATE_PART('month', ended_at) as ended_month,
--DATE_PART('day', ended_at) as ended_day,
--DATE_PART('hour', ended_at) as ended_hour,
member_casual 
from all_tables
group by 
member_casual,
DATE_PART('month', ended_at)
order by count(ride_id) desc;
```

The weekly analysis of the end time:

```
select 
count(ride_id) as no_of_users,
--DATE_PART('month', ended_at) as ended_month,
DATE_PART('day', ended_at) as ended_day,
--DATE_PART('hour', ended_at) as ended_hour,
member_casual 
from all_tables
group by 
member_casual,
DATE_PART('day', ended_at)
order by count(ride_id) desc;
```

The hour analysis of the end time:

```
select 
count(ride_id) as no_of_users,
--DATE_PART('month', ended_at) as ended_month,
--DATE_PART('day', ended_at) as ended_day,
DATE_PART('hour', ended_at) as ended_hour,
member_casual 
from all_tables
group by 
member_casual,
DATE_PART('hour', ended_at)
order by count(ride_id) desc;
```

Data Visualization

The data has been meticulously stored and prepared for thorough analysis. Relevant information was extracted from multiple tables and then effectively visualized using **Power Bi**. The primary objective of this analysis is *to understand the distinct usage patterns of Cyclistic bikes between annual members and casual riders.*

To initiate this comparison, we carefully examine and compare the specific types of bikes that each group, comprising annual members and casual riders, frequently utilizes.
![Uploading Total Bike Types.PNG…]()


The composition of Cyclistic users can be divided into two major categories: members, accounting for approximately 59.7% of the total, and casual riders, comprising the remaining 40.3%. The percentage distribution for each bike type reveals that the classic bike is the most popular among both member and casual rider groups, with the electric bike ranking second in popularity. Interestingly, docked bikes are predominantly used by casual riders and represent the least utilized bike type within the Cyclistic service.

Moving on, we delve into the analysis of trip distributions based on different time parameters. Specifically, we explore the number of trips distributed across months, days of the week, and hours of the day.
![Uploading Total Trips in 2024.PNG…]()


In terms of monthly trips, both casual riders and members exhibit similar patterns, with a higher number of trips occurring during the spring and summer months, and a decline during the winter season. Interestingly, the gap between casual riders and members is narrowest in the month of July during the summer.

When comparing trips based on the days of the week, it is evident that casual riders tend to take more journeys over the weekends, while members show a decline in trip frequency during the weekends compared to the rest of the weekdays.
![Uploading Total trips at start location.PNG…]()


Analyzing trips based on the hours of the day reveals distinct patterns for members and casual riders. Members display two peak periods for trips, occurring in the early morning from around 6 am to 8 am, and in the evening from around 4 pm to 8 pm. In contrast, the number of trips for casual riders steadily increases throughout the day, peaking in the evening and then gradually decreasing.

From these observations, we can infer that members may predominantly use bikes for commuting to and from work on weekdays, while casual riders utilize bikes more frequently throughout the day, particularly on weekends for leisure purposes. Both groups exhibit higher activity levels during the summer and spring months.

To understand the differences in behavior between casual and member riders, the ride duration of trips is compared in the subsequent analysis.

It is important to note that casual riders tend to have longer cycling trips compared to members on average. The average journey length for members remains consistent throughout the year, week, and day. However, there are notable variations in the ride durations of casual riders. Specifically, during the spring and summer seasons, on weekends, and between 10 am and 2 pm during the day, they cover greater distances. Conversely, they have shorter trips between five and eight in the morning.

Based on these findings, it can be inferred that casual riders travel longer distances, approximately twice as much as members, but less frequently. Their longer journeys are often observed on weekends and during the day outside of typical commuting hours, particularly in the spring and summer months, suggesting that they might be cycling for recreational purposes.

To gain further insights into the differences between casual and member riders, an analysis of the starting and ending stations' locations can be conducted. By filtering and examining stations with the most trips, additional conclusions can be drawn.
![Uploading Total trips at end location.PNG…]()


Casual riders often initiate their trips from stations located near museums, parks, beaches, harbors, and aquariums, whereas members tend to start their journeys from stations in proximity to universities, residential areas, restaurants, hospitals, grocery stores, theaters, schools, banks, factories, train stations, parks, and plazas.

A comparable pattern is evident in the ending station locations. Casual riders tend to conclude their journeys near parks, museums, and other recreational sites, while members end their trips in proximity to universities, residential, and commercial areas. This observation further supports the notion that casual riders utilize bikes primarily for leisure activities, whereas members heavily rely on them for their daily commutes.

***Here are the summary:***

*Casual Rider*

Casual riders exhibit a preference for using bikes throughout the day, with increased usage over the weekends, especially during the summer and spring seasons, for leisure activities. They tend to travel approximately twice the distance of members but take fewer trips overall.

Furthermore, casual riders commonly commence and conclude their journeys near various recreational sites, including parks, museums, coastal areas, and other leisure destinations.

*Member Rider*

Member riders prefer using bikes primarily for commuting purposes during weekdays, particularly in the summer and spring seasons, with higher usage during typical commute hours (8 am and 5 pm). They take more frequent but shorter rides compared to casual riders, with trip durations approximately half as long.

Moreover, member riders tend to start and end their bike journeys near locations such as universities, residential areas, and commercial districts, indicating their reliance on bikes for daily commuting needs.

6: Act

Following the analysis of disparities between casual and member riders, there is an opportunity to devise marketing strategies to encourage casual riders to convert into members. The following recommendations are suggested:

1. Targeted marketing campaigns could be launched during the spring and summer seasons at tourist and recreational hotspots popular among casual riders.
2. Considering that casual riders are more active on weekends and during the summer and spring, offering seasonal or weekend-only memberships might be an effective approach.
3. Since casual riders tend to have longer bike rides than members, providing discounts for extended ride durations could serve as an incentive for casual riders and possibly encourage members to also ride for longer periods.

