# Cyclistic Data Analysis
## Google Career Certificate Capstone Project

### Project Overview
**Deliverables**
Report and Presentation
1. A clear statement of the business task
2. A description of all data sources used
3. Documentation of any cleaning or manipulation of data
4. A summary of your analysis
5. Supporting visualizations and key findings
6. Top three recommendations based on your analysis

### Ask
**Company Hypothesis**
The company believes that converting casual riders to annual members is the key to future growth and wants to examine how the two groups of users differ in their usage of Cyclic.

**Analysis Objective**
Identify if there are any differences in usage of Cyclistic bikes between Annual membership riders and casual riders (single-ride and full-day). Additionally, see if there are any causes, particularly with regard to digital media, that lead Cyclic users to purchase annual memberships over single use?

**Questions to Answer**
- Do casual riders and annual members differ in their usage of Cyclic bikes?
- If, yes, how do they differ?

**Secondary Questions**
- What were the factors that led some users to become annual members vs casual riders?
- Are there any issues that prevent or dissuade a potential user from becoming an annual member?
- Does digital media play a role in influencing the decision to become an annual member?

**Stakeholders**
- Cyclistic executive team: Has final approval on whether to implement recommendations for changing the marketing program
- Cyclistic marketing team: Has requested the analysis in order to determine whether their hypothesis on annual vs casual riders is correct
- Lily Moreno: The director of marketing and the person in charge of this analysis project

### Prepare
**Data Location**
Data is being saved locally on a hard drive (behind a password locked screen) as well as on a Cloud service as backup. 

**Data Storage**
Locally, data is being saved in a main folder that contains a series of subfolders to foster organization. Original data files are saved in subfolders based on the type of file (.zip, .csv, .xlsx). Reports, presentations, and documentation will be saved in a separate subfolder labeled 'deliverables'.

**Data Structure**
Data is broken down by month (beginning June 2021 and ending May 2022). Each month is saved in a different spreadsheet (CSV) file. 

Fields in each spreadsheet are as follows:
- *ride_id*: A unique identifier for that trip. Data type - string.
- *rideable_type*: Designates the type of bicycle used for that trip. Options include 'classic_bike', 'docked_bike', and 'electric_bike'. Data type - string.
- *started_at*: Records the date and time (24 hour format) that the ride was initiated. Data type - custom (m/d/yy h:mm).
- *ended_at*: Records the date and time (24 hour format) that the ride was ended. Data type - custom (m/d/yy h:mm).
- *start_station_name*: Identifies the name of the Cyclistic station where the bike ride was initiated. Stations are identified by the two street names where the station is located (IE Wabash Ave & Grand Ave). Data type - string
- *start_station_id*: Identifies the Cyclistic station where the bike ride was initiated according to its unique ID. Data type - some are string, some are number
- *end_station_name*: Identifies the name of the Cyclistic station where the bike ride was ended. Stations are identified by the two street names where the station is located (IE Wabash Ave & Grand Ave). Data type - string
- *end_station_id*: Identifies the Cyclistic station where the bike ride was ended according to its unique ID. Data type - some are string, some are number
- *start_lat*: Records the latitude position where the ride was initiated. Data type - float
- *start_lng*: Records the longitudal position where the ride was initiated. Data type - float
- *end_lat*: Records the latitude position where the ride was ended. Data type - float
- *end_lng*: Records the longitudal position where the ride was ended. Data type - float
- *member_casual*: Identifies if the ride user is an annual member (designated as 'member') or a casual rider (designed as 'casual').

**Data Security and Privacy**
This data was collected by Cyclistic, so the company has free usage of it. Access to the data is restricted to the marketing data analysis team. Data is being saved both locally and on a Cloud with both requiring a password to access. 

**Data Credibility and Integrity**
Given the data is a first-hand source its reliability is considered to be high. Data will also be reviewed by the analyst to confirm accuracy and integrity. Any issues will be corrected with approval from the project manager. 

### Process
Due to the size of the dataset, data processing will be done using the R programming language and R Studio. 

#### Cleaning Data
For this analysis, the package 'Tidyverse' was the only one used in R studio. All data sets were imported into R Studio from their respective CSV files. The contents of the datasets were inspected by using the 'head', 'colnames', and 'str' functions. The fields listed above were confirmed. In order to avoid repetitive coding all 12 data sets were combined into one master data set titled 'full_year_cyclistic_dataset'. 

It was determined that for the purpose of this analysis the columns *ride_id*, *start_station_name*, *end_station_name*, *start_lat*, *start_lng*, *end_lat* and *end_lng* were not required. If necessary, each ride's start and end location can be identified by the unique record stored in the *start_station_id* and *end_station_id* and the *ride_id* does not share any information that is relevant to this analysis. This data frame was titled 'cyclistic_tripdata_2205_member'. The starting and ending latitudes and longitudes were also determined to be unnecessary for the purpose of this analysis. At this point, one additional column was added to the data frame titled *ride_time* using the mutate function. This column was calculated by using the *started_at* and *ended_at* time values. The *ride_time* is calculated in seconds. The day of the week that rides occurred on was also created in a new column using the 'weekdays' function.

It was also checked to see if there were any negative values in the *ride_time* column as that would indicate the ride ended before it began.  The order of the days of the week was not in a logical order, so it was re-ordered using the 'ordered' function with a setting from Sunday to Saturday.

### Descriptive Analysis
Basic statistics were run on the cleaned data set based on the membership type. The average/mean (mean_ride_time), median (median_ride_time), minimum (min_ride_time), and maximum (max_ride_time) ride times were calculated using the 'aggregate' function. The calculations were collated into one new data set titled 'calculated_ride_times'. The same four calculations were run again, but this time also considering the day of the week in order to determine if the day affected ridership. These calculations followed the same naming convention as the prior calculations, but 'ride' was changed to 'day' (IE 'mean_day_time'). These calculations were collated into a new data frame titled 'calculated_day_ride_times'. Finally, the number of total rides for each day of the week (based on membership level) was calculated. This was completed using the 'table' function on the *member_casual* and *weekend* (day of the week) columns. This data was saved into a data frame titled 'member_rides_by_day'.

These three new data frames ('calculated_ride_times', 'calculated_day_ride_times', and 'rides_by_day') were then exported as CSV files for visualization using Google Sheets.

In Google Sheets, the three CSV files were imported into one sheet for a final cleaning, analysis, and visualization. This file was titled 'cyclistic-cleaned-data'. For the 'calculated_ride_times' and 'calculated_day_ride_times' sheets, the columns were re-named as 'Member Type', 'Average Ride', 'Median Ride', Min Ride', and 'Max Ride' respectively. In the 'rides_by_day' sheet the columns were renamed 'Day of the Week', 'Member Type' and 'Number of Rides' respectively.

The ride times calculated in R Studio was done in seconds. So, in order to make the times more easily understandable, they were converted to minutes and seconds format in Google Sheets. However, the 'Max Ride' category was only converted in days. This is because there was data in the Casual member category that went beyond a month long. Converting this data into a months:hours:minutes:seconds type of formatting was not particularly useful and Google Sheets stuggled to accurately complete this data conversion. A final change in formatting was done in the 'calculated_ride_times' sheet as the original imported data was in a wide format. In order to create a visualization it was changed to a long format.

#### Calculated Ride Times Analysis
Analysis of the calculated ride times by membership status revealed a few interesting points.

* Casual membership riders on average used the bicycle for a longer time than annual membership riders. Casual riders averaged a ride of 30 minutes and 33 seconds while Member riders averaged a ride of 13 minutes and 3 seconds.
* A similar point was seen in the median ride length. Casual riders had a median ride length of 15 minutes and 16 seconds while Member riders had a median ride length of 9 minutes and 7 seconds. 
* The max ride time also showed a striking difference between the two. The maximum ride length for a Casual rider was 38.8 days long while the maximum ride length for a Member rider was 1.08 days long.

![Average and median ride length per membership type](average-median-ride-membership.png)

#### Calculated Day Ride Times Analysis
The trends seen in the 'Calculated Ride Times' analysis were also reflected in the 'Calculated Day Ride Times' analysis. Casual riders per day of the week, had an average ride length higher than Member riders. This was also true in the median ride length.

Average ride length by day of the week (Casual - Member):

* Sunday: 35:20 - 14:47
* Monday: 30:32 - 12:39
* Tuesday: 26:15 - 12:17
* Wednesday: 26:39 - 12:19
* Thursday: 27:42 - 12:27
* Friday: 28:47 - 12:47
* Saturday: 33:30 - 14:37

![Average ride length by day of the week per membership type](average-time-day-membership.png)

Median ride length by day of the week (Casual - Member):

* Sunday: 17:40 - 10:08
* Monday: 15:13 - 8:44
* Tuesday: 13:30 - 8:40
* Wednesday: 13:29 - 8:48
* Thursday: 13:32 - 8:48
* Friday: 14:26 - 8:57
* Saturday: 17:09 - 10:15

![Median ride length by day of the week per membership](median-time-day-membership.png)

#### Rides By Day Analysis
Looking at the 'Rides By Day' analysis offered another important piece of information. While the other two data sets showed that Casual riders use the Cyclistic bicycle longer than their Member rider counterparts, the 'Rides By Day' data set revealed that Member riders take more rides than Casual members do except for weekends.

Number of rides by day of the week (Casual - Member):

* Sunday: 470,098 - 394,650
* Monday: 302,042 - 466,086
* Tuesday: 286,986 - 524,769
* Wednesday: 285,732 - 512,565
* Thursday: 308,584 - 501,778
* Friday: 359,969 - 459,766
* Saturday: 546,090 - 441,015

![Number of rides by day of the week per membership type](total-rides-day-membership.png)

#### Overall Analysis
In total, Casual riders took 2,559,501 rides on Cyclistic bicycles while Member riders took 3,300,629 rides. Conversely, Casual riders used Cyclistic bicycles for a total of 54,297 hours while Member riders used the bicycles for a total of 29,897 hours. 

Another way to view those numbers is that in terms of the number of times a Cyclistic bicycle was used 56.32% of those were by Member riders while 43.68% were by Casual riders. 

![Percent of total Cyclistic bicycle rides per membership type](percent-total-rides-rider.png)

However, of total hours that Cyclistic bicycles were in use, 64.49% were being used by Casual riders and only 35.51% were by Member riders.

![Percent of total ride hours per membership type](percent-total-hours-used-rider.png)

The data from the past 12 months indicates that Casual riders don't use Cyclistic bicycles as often as Member riders, but when they do use Cyclistic they tend to make longer rides and keep the bicycles for longer periods of time. Member riders on the other hand make shorter, but more frequent trips on Cyclistic bicycles.

### Conclusions, Recommendations, and Next Steps
As mentioned in the 'Overall Analysis' section, the data from the past 12 months indicates there are significant differences in how Casual and Member riders differ in their usage of Cyclistic bicycles. 

* Casual riders use Cyclistic bicycles less often, but use them for longer periods of time per ride. Casual riders tend to use Cyclistic bicycles significantly more on weekends than on weekdays. 
* Member riders conversely use Cyclistic bicycles more frequently, but for a shorter period of time per ride. Member riders use Cyclistic bicycles more on weekdays than on weekends.

Recommendations:

1. Target current Casual riders who regularly use Cyclistic bicycles during the week to convert to annual members.
2. Create marketing material showing the cost benefits of annual membership over casual riding for individuals who regularly use Cyclistic bicycles (cost per hour of use and/or cost per trip).
3. Identify how Casual members who use Cyclistic bicycles for long individual rides could be converted to annual members.

Next Steps:

1. Data regarding how often Casual members use Cyclistic bicycles per week could be useful in determining how much of a market there is to easily convert to annual memberships.
2. How riders were exposed to Cyclistic and how they learn about Cyclistic programs could help determine the most effective way to reach riders.
3. Qualitative research into what prompted current Member riders to sign up from Casual rider status could help determine the nature of the messaging for future marketing campaigns.
4. Data regarding how many Member riders immediately signed up for annual membership vs how many were converted after trying Cyclistic as Casual members.