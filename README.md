
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

Installing and loading all the libraries.
```{r}
#load libraries 
install.packages("dplyr")
library(dplyr)
install.packages("tidyverse")
library(tidyverse)
```

This part is to check whether the data is well-formatted and cleaned.

```{r}
# To be able to view the data
data <- read.csv("202201_tripdata.csv")

# Display the first 10 rows of the data frame to check the Dataframe
head(data, n=10)

# Filter the data to include only rows where the end stations and longitudes are 41.79 and -87.58. This was to see if there are any important empty cells
filtered_data <- data %>%
  filter(end_lat == 41.79000 & end_lng == -87.58)

head(filtered_data, n=10)

```

Then, I merged the dataset from Jan to April into a single dataframe for further analysis.
```{r}
# Merge all the data into a single Dataframe
data2 <- read.csv("202202_tripdata.csv")
data3 <- read.csv("202203_tripdata.csv")
data4 <- read.csv("202204_tripdata.csv")
merged_data <- rbind(data,data2,data3,data4)

```
