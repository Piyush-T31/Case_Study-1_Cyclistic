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
5. [Share]
6. [Act]

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
