# Case Study: Cyclist Bike-Share Company
 Google Data Analytics Capstone project


### About the company
In 2016, Cyclistic launched a successful bike-share offering. Since then, the program has grown to a fleet of 5,824 bicycles that
are geotracked and locked into a network of 692 stations across Chicago. The bikes can be unlocked from one station and
returned to any other station in the system anytime.

Until now, Cyclistic’s marketing strategy relied on building general awareness and appealing to broad consumer segments.
One approach that helped make these things possible was the flexibility of its pricing plans: single-ride passes, full-day passes,
and annual memberships. Customers who purchase single-ride or full-day passes are referred to as casual riders. Customers
who purchase annual memberships are Cyclistic members.

Cyclistic’s finance analysts have concluded that annual members are much more profitable than casual riders. Although the
pricing flexibility helps Cyclistic attract more customers, Moreno believes that maximizing the number of annual members will
be key to future growth. Rather than creating a marketing campaign that targets all-new customers, Moreno believes there is a
very good chance to convert casual riders into members. She notes that casual riders are already aware of the Cyclistic
program and have chosen Cyclistic for their mobility needs.

Moreno has set a clear goal: Design marketing strategies aimed at converting casual riders into annual members. In order to
do that, however, the marketing analyst team needs to better understand how annual members and casual riders differ, why
casual riders would buy a membership, and how digital media could affect their marketing tactics. Moreno and her team are
interested in analyzing the Cyclistic historical bike trip data to identify trends

Notes:
Casual rider = single-ride or full-day passes
Cyclistic member = annual memberships --> Moreno believes that maximizing this number will be key to future growth


## Ask

In this phase of the analysis process I articulate the business task by processing the available information and asking SMART questions if further clarification is needed.

The business task: extract insights from historical trip data that will guide the design of new marketing strategies aimed at maximizing the growth of the company.
The business task can be divided in three sections:
1) How do annual members and casual riders use Cyclistic bikes differently?
2) Why would casual riders buy Cyclistic annual memberships?
3) How can Cyclistic use digital media to influence casual riders to become members?

The key stakeholders are:
- Lily Moreno - Director of marketing
- Marketing analytics team

 ## Prepare
 
 Data source for historical trip data: https://divvy-tripdata.s3.amazonaws.com/index.html
 
 The data is located on the company's Amazon cloud server. It is safe to proceed to downloading the data as I have verified the domain (no phishing + SSL certified).
 In the provided location we have the following available data:
 * monthly historical trip data for period 01.2016 - 07.2022

Note: The reporting format has changed over the years. If longer-term analysis will be required I need to pay extra attention to the data formatting.

Downloaded the data and uploaded trip data to a secure cloud server to ensure data accessibility, security and ensure data will not get lost.

For each of the past 12 months we have a ".csv" file containing trip data. I have inspected the files and the **data formats:**

Internal - the data lives and is extracted from a company's own system
Structured - the data is organized in rows and columns (spreadsheet)
Wide - every ride_id has a single row with multiple columns to hold its attributes

**Data types and formats by variable:**
* ride_id - string; qualitative, nominal
* rideable_type - string; qualitative, nominal
* started_at - number; qualitative, ordinal
* ended_at - number; qualitative, ordinal
* start_station_name - string; qualitative, nominal
* start_station_id - string; qualitative, nominal
* end_station_name - string; qualitative, nominal
* end_station_id - string; qualitative, nominal
* start_lat - number; quantitative, continuous
* start_lng - number; quantitative, continuous
* end_lat - number; quantitative, continuous
* end_lng - number; quantitative, continuous
* member_casual - string; qualitative, ordinal

Next, we'll inspect the data source to check for bias and credibility issues.
As instructed by Google, the datasets have been made available by Motivate International Inc. under this [license](https://www.divvybikes.com/data-license-agreement)
I am using the "ROCCC" framework
* Reliable - taking into account the data has been provided by Google for this case study I'll assume that the data is reliable. Also the data looks to come from a real bike-share operator. (Cyclist is a fictional company so data has been sourced from 'divvy' operator)
* Original - first-party data, no issues
* Comprehensive - I have found historical trip data for a considerable length of time and the files contain all critical information needed to answer my questions and find a solution to my business task. However it would be useful to also have the prices of each ride for more complex analysis.
* Current - I have access to the last 12 months of trip data
* Cited - n/a

**Data integrity** considerations:

* Accuracy - we can't really check for data accuracy as Cyclist is a fictional company
* Completeness - we have inspected the trip data files from the last 12 months and they all contain detailed information about each trip and for each type of member
* Consistency - data is stored on a cloud server location so it can be accessed by anyone who has access to the link
* Trustworthiness throughout the data life-cycle - n/a - I do not have information regarding the data life-cycle of trip data

We can analyze the bike utilization behaviour differences between the two kind of members by looking at:
* the number of rides
* the duration of rides
* the length(in km) of rides (if we could find a way to translate geo locations to a length measurement)
* the type of bike used

From the above analysis points we can then derive further insights on why would casual riders buy Cyclistic annual memberships.

## Process

### Tools selection:

- **SQL Server** - exploring, cleaning and transforming data (the csv files are quite large and time-consuming to work with in a spreadsheet)
- **Excel** - will be used mostly in the analysis phase for ad-hoc calculations on smaller datasets exported from database queries

### 1) Imported the .csv files into a SQL database using SQL Server (SSMS).
### 2) Merged all monthly tables into one using UNION ALL SQL function.

### 3) Data cleaning procedures
- [x] Completeness check (import errors) - verify number of rows and columns with original .csv file
```
SELECT COUNT(*) FROM [google_capstone].[dbo].[trip_data];

exec sp_columns [trip_data];
```
- [x] Check for null values in ride_id and member_casual
```
SELECT
	ride_id,
	member_casual,
	started_at
FROM [google_capstone].[dbo].[trip_data]
WHERE member_casual IS NULL OR ride_id IS NULL;
```
- [x] Check for duplicate values in ride_id (each ride/trip should have a unique observation)
```
SELECT
	ride_id,
	COUNT(*) AS occurences
FROM [google_capstone].[dbo].[trip_data]
GROUP BY ride_id
HAVING COUNT(*) > 1
```

Errors
- There are observations with null values in the start_station_name, start_station_id, end_station_name and end_station_id columns

### 4) Data transformation
* Created two computed columns
  - ride_length - showing the length in minutes for each ride ```DATEDIFF(minute, started_at, ended_at)```
  - day_of_week - extracted the week day in number format (Sunday = 1, Saturday = 7) ```DATEPART(weekday, started_at)```

I have ensured that my data is clean and ready for analysis by performing the cleaning procedures mentioned in the previous section.
- My data has no duplicate ride_id trips
- Each row has a *ride_id* and a *member_casual* attribute
- Column names are descriptive

## Analyze

I will derive insights about each type of member behaviour. In this regard I have decided to perform the following procedures.
It is always good to keep the big picture in mind: our goal is to find behavioural differences between casual riders and Cyclist members.

1) I'll start with something simple. Let's look at the **total number of trips taken by each user category**:
```
SELECT 
	member_casual,
	COUNT(*) AS num_trips
FROM [trip_data]
GROUP BY member_casual
```
![image](https://user-images.githubusercontent.com/24313609/184867828-f65f54fd-b28c-4e1f-aedc-c100d6b8a104.png)

I have exported the results into an excel file for some rapid math and the conclusion is that members have taken 795,309 more trips than casual users (26.83% more). However we do not have any data regarding the number of users so we can't form a complete picture.

2) Tried to see if there are any patterns in the preference for certain bike types between the two categories.

SQL query:
```
SELECT 
	member_casual,
	rideable_type,
	COUNT(*) AS num_trips
FROM [trip_data]
GROUP BY member_casual, rideable_type
ORDER BY member_casual ASC, num_trips DESC
```

![image](https://user-images.githubusercontent.com/24313609/184937553-dfabda82-ba0a-447d-aa21-7f09ceee6cb1.png)
![image](https://user-images.githubusercontent.com/24313609/184937562-e59184b0-6d12-466f-b022-917482d68b5a.png)

***Finding: It looks like members have a slight preference for classic bikes over electric_bikes over the last 12 months (58% vs 42%). Assuming that a good part of members are commuting to work, they might not be aware of the benefits of using an electric bike (which are generally faster and less tiresome for the cyclist). However we do not know the number of electric and classic bikes in the network. Also it might be the case that electric bikes are more expensive to rent. In this case I would recommend emphasizing the benefits of using electric bikes against the cost in a marketing campaign. Nevertheless this is not in the scope of our project, but could be an opportunity for furture improvement and investigation.***
I'm curious about the electric bike utilization monthly trend. Let's check it:

```
SELECT 
	member_casual,
	rideable_type,
	DATEPART(mm,started_at) AS ride_month,
	COUNT(*) AS num_trips
FROM [trip_data]
GROUP BY member_casual, rideable_type, DATEPART(mm,started_at)
ORDER BY member_casual ASC, ride_month ASC, num_trips DESC
```
![image](https://user-images.githubusercontent.com/24313609/184950050-cb252da4-2816-40d7-bfb1-67bdb08ecca2.png)

- Clear seasonality -> bikes utilization shows a rapid utilization starting in the spring season and peaking in July. Both classic and electric bike utilization start to drop significantly in August and really bottoms out during the winter season. 
- It is no surprise that people opt out for more comfortable transport means (bus, personal car, ...) when the weather is cold
- ***Finding: Converting casual users to members should result in more income for the firm while the wear of the bikes stays relatively the same (no significant difference in seasonality identified between the two categories). This is in line with the financial department analysis. To reach a clear conclusion in this regard I require data that shows the number of users and members (I cannot infer if people who purchased an annual membership use the service more often)***
- Data shows that 'docked bikes' are only used by casual users. However, I am not sure what that means - do they represent classic or electric bikes? In a real world situation I would check that with my colleagues and/or my manager.










 




