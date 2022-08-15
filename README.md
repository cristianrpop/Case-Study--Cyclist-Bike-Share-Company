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
* Comprehensive - I have found historical trip data for a considerable length of time and the files contain all critical information needed to answer my questions and find a solution to my business task
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









 




