## CASE STUDY: Bellabeat Fitness Data Analysis
**Author: Kevon Nalls**

**Date: November 14, 2024**
#
_The case study follows the six step data analysis process:_


###  [Ask](#step-1-ask)
###  [Prepare](#step-2-prepare)
###  [Process](#step-3-process)
###  [Analyze](#step-4-analyze)
###  [Share](#step-5-share)
###  [Act](#step-6-act)

#
![BellabeatLogo](https://user-images.githubusercontent.com/77591203/196562658-bfe5df3b-4e68-4c4e-97b8-d9c057d28dec.jpg)

## Introduction 

Bellabeat is a high-tech manufacturer of health-focused products for women. Collecting data on activity, sleep, stress, and reproductive health has allowed Bellabeat to empower women with knowledge about their own health and habits.
Although Bellabeat is a successful small company, they have the potential to become a larger player in the global smart device market. Urška Sršen, cofounder and Chief Creative Officer of Bellabeat, believes that analyzing smart device fitness data could help unlock new growth opportunities for the company.

## Step 1: Ask 

## Business Task 
To uncover opportunities for growth and offer recommendations to enhance Bellabeat's marketing strategy by analyzing trends in smart device usage.

**Key Stakeholders:** 

* Urška Sršen: Bellabeat's cofounder and Chief Creative Officer
* Sando Mur: Mathematician and Bellabeat’s co-founder

**Questions to explore for the analysis:**

1. What are some trends in smart device usage?
2. How could these trends apply to Bellabeat customers?
3. How could these trends help influence Bellabeat marketing strategy?

## Step 2: Prepare

The data being used in this case study can be found here: [FitBit Fitness Tracker Data](https://www.kaggle.com/datasets/arashnic/fitbit) CC0: Public Domain, dataset made available through [Mobius](https://www.kaggle.com/arashnic)


The dataset, hosted on Kaggle and analyzed in R Studio, features personal fitness tracker data from 30 Fitbit users who consented to share minute-level insights into their physical activity, heart rate, and sleep patterns. With detailed metrics on daily activity, step counts, and heart rate, the data provides a comprehensive look into users' lifestyles and habits.

The data set contains 11 CSV files organized in long format. Below is a breakdown of the data using the ROCCC approach:

* Reliability - LOW: The dataset is based on contributions from 30 Fitbit users who voluntarily shared minute-level personal tracker data, including details on physical activity, heart rate, and sleep monitoring.
* Original - LOW: The data was collected by a third party using Amazon Mechanical Turk, which raises questions about its authenticity.
* Comprehensive - MED: The dataset provides multiple fields, including daily activity intensity, calories burned, steps taken, sleep duration, and weight records, offering moderate insight into user habits.
* Current - LOW: The data covers the period from March 2016 to May 2016, making it outdated and possibly unreflective of current trends and behaviors.
* Cited - LOW: As the data was collected from a third party, the original source remains unknown and uncited.

  ### Loading packages

```
library(tidyverse)
library(lubridate) 
library(dplyr)
library(ggplot2)
library(tidyr)
library(janitor)
```

## Step 3: Process 
[Back to top](#introduction)
### Importing the datasets
# Set the file path
file_path <- "C:/Users/Kevon/Downloads/mturkfitbit_export_3.12.16-4.11.16/Fitabase Data 3.12.16-4.11.16/dailyActivity_merged.csv"
file_path <- ("C:\\Users\\Kevon\\Downloads\\mturkfitbit_export_3.12.16-4.11.16\\Fitabase Data 3.12.16-4.11.16\\weightLogInfo_merged.csv") 

# Read the CSV file
dailyActivity_data <-  read.csv(dailyActivity_merged.csv)
weightLogInfo_merged <- read_csv("C:\\Users\\Kevon\\Downloads\\mturkfitbit_export_3.12.16-4.11.16\\Fitabase Data 3.12.16-4.11.16\\weightLogInfo_merged.csv")

# Preview files

![HeadWeightLoginInfo](https://github.com/Nallkevo/Case-study-Fitabase/blob/main/Screenshot%202024-11-18%20003202.png)

![HeadActivity]("C:\Users\Kevon\OneDrive\Pictures\Screenshots\Screenshot 2024-11-18 005524.png")


 








