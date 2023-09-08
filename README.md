# Case study 1 Google Analytics 
This capstone project is for the Google Analytics Certificate where I study a particular dataset and present my findings. 

The case study 1 is a fictitious scenario based on a bike-share company in Chicago.   


## Documentation

### Scenario 

I am junior data analyst working for the marketing team at Cyclistic, the company. I need to get some insights on how casual and annual riders use Cyclistic differently so as to increase the number of annual memberships. I need to analyze and present my findings so that the company can make some data-driven decisions.

### About the company

It was launched in 2016. The prgram features 5800 bicycles and 600 docking stations and even includes  reclining bikes, hand tricycles, and cargo bikes, making it more inclusive. The company believes that converting casual members to annual members will help to grow more.

I will follow the Case Study roadmap given by the Google Analytics course in order to answer the key business questions.




## Case Study Roadmap

The Roadmap has 6 steps: 

1. [Ask](#Ask)
2. [Prepare](#Prepare)
3. [Process](#Process)
4. [Analyze](#Analyze)
5. [Share](#Share)
6. [Act](#Act)

## Ask

**The main question is how do annual and casual members use Cyclistic differently.**

### Guiding Table - Ask

| Guiding Questions    | Answers             | 
| ---------------------| ----------------------| 
| What is the problem you are trying to solve?| The task given is to identify the differences between casual and annual riders and gain insights on how to make more casuals switch to annual. | 
| How can your insights drive business decisions?| By analyzing the data and and understanding the trends and patterns, I will be able to make evidence-based decisions.| 

## Prepare
To be able to analyze the dataset, I need to prepare it by checking if the data is well-formatted and the source is reliable. Below, I compiled all the concerned questions and answers to check the eligibilty of the dataset.


### Guiding Table - Prepare
| Guiding Questions        | Answers         |
| -----------------        | -----------------|
| Where is your data located?     | The data is located [here](https://divvy-tripdata.s3.amazonaws.com/index.html) and is stored locally as well for analysis. |
|How is the data organized?   | I will be using the data for the year 2022 and it is organized monthly in a spreadsheet.|
| Are there issues with bias or credibility in this data?            | The data comes from the original source so is reliable. It is collected from a random population, therefore there are no issues regarding bias or credibility.|
| How are you addressing licensing, privacy, security, and accessibility?      | The data has been made accessible under the Motivate International Inc. ([license](https://ride.divvybikes.com/data-license-agreement)) and the riders' private information are blocked as well.|
| How did you verify the data’s integrity? | The data comes from a reliable source and it is comprehensive, current and cited. |
| How does it help you answer your question? | The data contains valuable information about the casual and annual members that will allow me to analyze the problem and answer the question properly. |
| Are there any problems with the data? | The dataset is appropriate but there is a few null values in the station names columns. |

## Process

### Guiding Table - Process

| Guiding Questions                                                | Answers                                                                                                                                                                   |
|------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| What tools are you choosing and why?                             | To be able to clean and filter large dataset, it is better to use BigQuery and SQL.                                                                                       |
| What steps have you taken to ensure that your data is clean?     | I have checked that the data was well-formatted and the datatypes of each columns are appropriate.                                                                        |
| How can you verify that your data is clean and ready to analyze? | I have checked that the data is recognizable and usable in my database. I have checked if the relevant data had no errors and transformed the data for further analyzing. |

I have added a ride_length column to be able to know each riders' usage duration. I have also added a week_of_day column to be able to analyze the dataset for each day of the week. Both were done in Google Sheets. The processed data is saved in the Processed data subfolder.

## Analyze

To analyze the dataset, I will be using R programming language as it can deal with large dataset and it is very user-friendly for data visualization.

## Guiding Table - Analyze

| Guiding Questions                                            | Answers                                                                                                                            |
|--------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------|
| How should you organize your data to perform analysis on it? | I have imported my 2022 dataset onto R and I merged the January to April data into a single dataframe for a quarter year analysis. |
| Has your data been properly formatted?                       | I have check each dataset using colnames for consistency before merging them into one dataframe.                                   |

### Cyclistic_Exercise_Full_Year_Analysis ###

 This analysis is for case study 1 from the Google Data Analytics Certificate (Cyclistic).  It’s originally based on the case study "'Sophisticated, Clear, and Polished’: Divvy and Data Visualization" written by Kevin Hartman (found here: https://artscience.blog/home/divvy-dataviz-case-study). We will be using the Divvy dataset for the case study. The purpose of this script is to consolidate downloaded Divvy data into a single dataframe and then conduct simple analysis to help answer the key question: “In what ways do members and casual riders use Divvy bikes differently?”

* Install required packages
* tidyverse for data import and wrangling
* libridate for date functions
* ggplot for visualization

```{r}
library(tidyverse)  #helps wrangle data
library(lubridate)  #helps wrangle date attributes
library(ggplot2)  #helps visualize data

```
```{r}
#=====================
# STEP 1: COLLECT DATA. EACH DATA REPRESENTS A QUARTER OF THE YEAR
#=====================
# Upload Divvy datasets (csv files) here
q2_2019 <- read_csv("Divvy_Trips_2019_Q2.csv")
q3_2019 <- read_csv("Divvy_Trips_2019_Q3.csv")
q4_2019 <- read_csv("Divvy_Trips_2019_Q4.csv")
q1_2020 <- read_csv("Divvy_Trips_2020_Q1.csv")

```

```{r}
#====================================================
# STEP 2: WRANGLE DATA AND COMBINE INTO A SINGLE FILE
#====================================================
# Compare column names each of the files
# While the names don't have to be in the same order, they DO need to match perfectly before we can use a command to join them into one file
colnames(q3_2019)
colnames(q4_2019)
colnames(q2_2019)
colnames(q1_2020)

# Rename columns  to make them consisent with q1_2020 (as this will be the supposed going-forward table design for Divvy)

(q4_2019 <- rename(q4_2019
                   ,ride_id = trip_id
                   ,rideable_type = bikeid 
                   ,started_at = start_time  
                   ,ended_at = end_time  
                   ,start_station_name = from_station_name 
                   ,start_station_id = from_station_id 
                   ,end_station_name = to_station_name 
                   ,end_station_id = to_station_id 
                   ,member_casual = usertype))

(q3_2019 <- rename(q3_2019
                   ,ride_id = trip_id
                   ,rideable_type = bikeid 
                   ,started_at = start_time  
                   ,ended_at = end_time  
                   ,start_station_name = from_station_name 
                   ,start_station_id = from_station_id 
                   ,end_station_name = to_station_name 
                   ,end_station_id = to_station_id 
                   ,member_casual = usertype))

(q2_2019 <- rename(q2_2019
                   ,ride_id = "01 - Rental Details Rental ID"
                   ,rideable_type = "01 - Rental Details Bike ID" 
                   ,started_at = "01 - Rental Details Local Start Time"  
                   ,ended_at = "01 - Rental Details Local End Time"  
                   ,start_station_name = "03 - Rental Start Station Name" 
                   ,start_station_id = "03 - Rental Start Station ID"
                   ,end_station_name = "02 - Rental End Station Name" 
                   ,end_station_id = "02 - Rental End Station ID"
                   ,member_casual = "User Type"))

# Inspect the dataframes and look for inconguencies
str(q1_2020)
str(q4_2019)
str(q3_2019)
str(q2_2019)

# Convert ride_id and rideable_type to character so that they can stack correctly
q4_2019 <-  mutate(q4_2019, ride_id = as.character(ride_id)
                   ,rideable_type = as.character(rideable_type)) 
q3_2019 <-  mutate(q3_2019, ride_id = as.character(ride_id)
                   ,rideable_type = as.character(rideable_type)) 
q2_2019 <-  mutate(q2_2019, ride_id = as.character(ride_id)
                   ,rideable_type = as.character(rideable_type)) 

# Stack individual quarter's data frames into one big data frame
all_trips <- bind_rows(q2_2019, q3_2019, q4_2019, q1_2020)

# Remove lat, long, birthyear, and gender fields as this data was dropped beginning in 2020
all_trips <- all_trips %>%  
  select(-c(start_lat, start_lng, end_lat, end_lng, birthyear, gender, "01 - Rental Details Duration In Seconds Uncapped", "05 - Member Details Member Birthday Year", "Member Gender", "tripduration"))

```
```{r}
#======================================================
# STEP 3: CLEAN UP AND ADD DATA TO PREPARE FOR ANALYSIS
#======================================================
# Inspect the new table that has been created
colnames(all_trips)  #List of column names
nrow(all_trips)  #How many rows are in data frame?
dim(all_trips)  #Dimensions of the data frame?
head(all_trips)  #See the first 6 rows of data frame.  Also tail(qs_raw)
str(all_trips)  #See list of columns and data types (numeric, character, etc)
summary(all_trips)  #Statistical summary of data. Mainly for numerics

# There are a few problems we will need to fix:
# (1) In the "member_casual" column, there are two names for members ("member" and "Subscriber") and two names for casual riders ("Customer" and "casual"). We will need to consolidate that from four to two labels.
# (2) The data can only be aggregated at the ride-level, which is too granular. We will want to add some additional columns of data -- such as day, month, year -- that provide additional opportunities to aggregate the data.
# (3) We will want to add a calculated field for length of ride since the 2020Q1 data did not have the "tripduration" column. We will add "ride_length" to the entire dataframe for consistency.
# (4) There are some rides where tripduration shows up as negative, including several hundred rides where Divvy took bikes out of circulation for Quality Control reasons. We will want to delete these rides.

# In the "member_casual" column, replace "Subscriber" with "member" and "Customer" with "casual"
# Before 2020, Divvy used different labels for these two types of riders ... we will want to make our dataframe consistent with their current nomenclature
# N.B.: "Level" is a special property of a column that is retained even if a subset does not contain any values from a specific level
# Begin by seeing how many observations fall under each usertype
table(all_trips$member_casual)

# Reassign to the desired values (we will go with the current 2020 labels)
all_trips <-  all_trips %>% 
  mutate(member_casual = recode(member_casual
                                ,"Subscriber" = "member"
                                ,"Customer" = "casual"))

# Check to make sure the proper number of observations were reassigned
table(all_trips$member_casual)

# Add columns that list the date, month, day, and year of each ride
# This will allow us to aggregate ride data for each month, day, or year ... before completing these operations we could only aggregate at the ride level
# https://www.statmethods.net/input/dates.html more on date formats in R found at that link
all_trips$date <- as.Date(all_trips$started_at) #The default format is yyyy-mm-dd
all_trips$month <- format(as.Date(all_trips$date), "%m")
all_trips$day <- format(as.Date(all_trips$date), "%d")
all_trips$year <- format(as.Date(all_trips$date), "%Y")
all_trips$day_of_week <- format(as.Date(all_trips$date), "%A")

# Add a "ride_length" calculation to all_trips (in seconds)

all_trips$ride_length <- difftime(all_trips$ended_at,all_trips$started_at)

# Inspect the structure of the columns
str(all_trips)

# Convert "ride_length" from Factor to numeric so we can run calculations on the data
is.factor(all_trips$ride_length)
all_trips$ride_length <- as.numeric(as.character(all_trips$ride_length))
is.numeric(all_trips$ride_length)

# Remove "bad" data
# The dataframe includes a few hundred entries when bikes were taken out of docks and checked for quality by Divvy or ride_length was negative
# We will create a new version of the dataframe (v2) since data is being removed

all_trips_v2 <- all_trips[!(all_trips$start_station_name == "HQ QR" | all_trips$ride_length<0),]

```
```{r}
#=====================================
# STEP 4: CONDUCT DESCRIPTIVE ANALYSIS
#=====================================
# Descriptive analysis on ride_length (all figures in seconds)
mean(all_trips_v2$ride_length) #straight average (total ride length / rides)
median(all_trips_v2$ride_length) #midpoint number in the ascending array of ride lengths
max(all_trips_v2$ride_length) #longest ride
min(all_trips_v2$ride_length) #shortest ride

# You can condense the four lines above to one line using summary() on the specific attribute
summary(all_trips_v2$ride_length)

# Compare members and casual users
aggregate(all_trips_v2$ride_length ~ all_trips_v2$member_casual, FUN = mean)

| casual_members | average length of the ride  |
|----------------|-----------------------------|
| casual         | 3552.7502                   |
| member         | 850.0662                    |

aggregate(all_trips_v2$ride_length ~ all_trips_v2$member_casual, FUN = median)

| casual_members | median length of the ride  |
|----------------|----------------------------|
| casual         | 1546                       |
| member         | 589                        |

aggregate(all_trips_v2$ride_length ~ all_trips_v2$member_casual, FUN = max)

| casual_members | max length of the ride  |
|----------------|-------------------------|
| casual         | 9387024                 |
| member         | 9056634                 |

aggregate(all_trips_v2$ride_length ~ all_trips_v2$member_casual, FUN = min)

| casual_members | min length of the ride  |
|----------------|-------------------------|
| casual         | 2                       |
| member         | 1                       |

# See the average ride time by each day for members vs casual users
aggregate(all_trips_v2$ride_length ~ all_trips_v2$member_casual + all_trips_v2$day_of_week, FUN = mean)

# Notice that the days of the week are out of order. Let's fix that.
all_trips_v2$day_of_week <- ordered(all_trips_v2$day_of_week, levels=c("Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"))

# Now, let's run the average ride time by each day for members vs casual users
aggregate(all_trips_v2$ride_length ~ all_trips_v2$member_casual + all_trips_v2$day_of_week, FUN = mean)

# analyze ridership data by type and weekday
all_trips_v2 %>% 
  mutate(weekday = wday(started_at, label = TRUE)) %>%  #creates weekday field using wday()
  group_by(member_casual, weekday) %>%  #groups by usertype and weekday
  summarise(number_of_rides = n()							#calculates the number of rides and average duration 
            ,average_duration = mean(ride_length)) %>% 		# calculates the average duration
  arrange(member_casual, weekday)								# sorts
```
The table below was generated to find the average time casual and member riders take when they use Cyclistic for each day of the week:

| casual_member | day of the week | average ride length |
|---------------|-----------------|---------------------|
| casual        | Sunday          | 3581.4054           |
| member        | Sunday          | 919.9746            |
| casual        | Monday          | 3372.2869           |
| member        | Monday          | 842.5726            |
| casual        | Tuesday         | 3596.3599           |
| member        | Tuesday         | 826.1427            |
| casual        | Wednesday       | 3718.6619           |
| member        | Wednesday       | 823.9996            |
| casual        | Thursday        | 3682.9847           |
| member        | Thursday        | 823.9278            |
| casual        | Friday          | 3773.8351           |
| member        | Friday          | 824.5305            |
| casual        | Saturday        | 3331.9138           |
| member        | Saturday        | 968.9337            |

The table generated below has classified the number of rides and the average duration of the rides for casuals and members for each day of the week:

| member_casual  | weekday | number_of_rides | average_duration |
|----------------|---------|-----------------|------------------|
| casual         | Sun     | 181293          | 3581             |
| casual         | Mon     | 103296          | 3372             |
| casual         | Tue     | 90510           | 3596             |
| casual         | Wed     | 92457           | 3719             |
| casual         | Thu     | 102679          | 3683             |
| casual         | Fri     | 122404          | 3774             |
| casual         | Sat     | 209543          | 3332             |
| member         | Sun     | 267965          | 920              |
| member         | Mon     | 472196          | 843              |
| member         | Tue     | 508445          | 826              |
| member         | Wed     | 500329          | 824              |
| member         | Thu     | 484177          | 824              |
| member         | Fri     | 452790          | 825              |
| member         | Sat     | 287958          | 969              |

## Share

First visualization is a bar chart for both casual and members. It is the number of rides for each day of the week. 

```{r}
# Let's visualize the number of rides by rider type
all_trips_v2 %>% 
  mutate(weekday = wday(started_at, label = TRUE)) %>% 
  group_by(member_casual, weekday) %>% 
  summarise(number_of_rides = n()
            ,average_duration = mean(ride_length)) %>% 
  arrange(member_casual, weekday)  %>% 
  ggplot(aes(x = weekday, y = number_of_rides, fill = member_casual)) +
  geom_col(position = "dodge")
```

![](https://github.com/Piyush-T31/Case_Study-1_Cyclistic/blob/e67ebe3ff1662c61782ed4b994ef7d648461e05d/num_rides.png)

Second visualization is a bar chart for both casual and member riders. This one depicts the average duration of the rides for both riders for each day of the week.


```{r}

# Let's create a visualization for average duration
all_trips_v2 %>% 
  mutate(weekday = wday(started_at, label = TRUE)) %>% 
  group_by(member_casual, weekday) %>% 
  summarise(number_of_rides = n()
            ,average_duration = mean(ride_length)) %>% 
  arrange(member_casual, weekday)  %>% 
  ggplot(aes(x = weekday, y = average_duration, fill = member_casual)) +
  geom_col(position = "dodge")
```

![](https://github.com/Piyush-T31/Case_Study-1_Cyclistic/blob/e67ebe3ff1662c61782ed4b994ef7d648461e05d/duration.png)


## Act

From the first bar chart, for the year 2019, we can see that for every days of the week, annual members have a higher number of rides than casuals. The annual member are more active during the week days while the casual members are more active during the weekends.

From the second bar chart, however, we observe that casual members use the Cyclistic rides for much longer distance than annual members. 

- One step to convert casual members to annual could be using promotions or discounts for riders that travel much further.
- We can increase the number of bikes during the week days as they have a higher volume of riders than the weekend.
