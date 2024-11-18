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
library("here")
library("skimr")
```

## Step 3: Process 
[Back to top](#introduction)
### Importing the datasets
# Set the file path
```
file_path <- "C:/Users/Kevon/Downloads/mturkfitbit_export_3.12.16-4.11.16/Fitabase Data 3.12.16-4.11.16/dailyActivity_merged.csv"
file_path <- ("C:\\Users\\Kevon\\Downloads\\mturkfitbit_export_3.12.16-4.11.16\\Fitabase Data 3.12.16-4.11.16\\weightLogInfo_merged.csv") 
```
# Read the CSV file
```
dailyActivity_data <-  read.csv(dailyActivity_merged.csv)
weightLogInfo_merged <- read_csv("C:\\Users\\Kevon\\Downloads\\mturkfitbit_export_3.12.16-4.11.16\\Fitabase Data 3.12.16-4.11.16\\weightLogInfo_merged.csv")
```

# Preview files
```
 head(daily_activity_data)
 head(weightLogInfo_merged)
 ```
![HeadActivity](https://github.com/Nallkevo/Case-study-Fitabase/blob/main/Screenshot%202024-11-18%20003202.png)

![HeadWeightLoginInfo]((https://github.com/Nallkevo/Case-study-Fitabase/blob/main/Screenshot%202024-11-18%20005524.png))
```
  colnames(daily_activity_data)
  colnames(weightLogInfo_merged)
```
![Dailyactivitydata](https://github.com/Nallkevo/Case-study-Fitabase/blob/main/Screenshot%202024-11-18%20071253.png)
![colnamesweight](https://github.com/Nallkevo/Case-study-Fitabase/blob/main/Screenshot%202024-11-18%20072710.png)
```
skim_without_charts(daily_activity_data)
skim_without_charts(weightLoginInfo_merged)
```
# Clean names 
```
clean_names(daily_activity_data)
Clean_names(weightLoginInfo_merged)
```
# Get summary statistics
```
summary(daily_activity_data)
summary(weightLoginInfo_merged)
```
# Check data structure (e.g., types of variables)
```
str(daily_activity_data)
str(weightLoginInfo_merged)
```
# Remove rows with NA values
```
data_clean<- na.omit(daily_activity_data)
data_clean <-na.omit(weightLoginInfo_merged)

```
# Replace NA with mean for numeric columns and "Unknown" for categorical columns
```
data_clean <- daily_activity_data %>%
  mutate(across(
    where(is.numeric), 
    ~ifelse(is.na(.), mean(., na.rm = TRUE), .)
  )) %>%
  mutate(across(
    where(is.character), 
    ~ifelse(is.na(.), "Unknown", .)))
data_clean1 <- weightLoginInfo%>%
  mutate(across(
    where(is.numeric), 
    ~ifelse(is.na(.), mean(., na.rm = TRUE), .)
  )) %>%
  mutate(across(
    where(is.character), 
    ~ifelse(is.na(.), "Unknown", .)))
```


# Remove rows with any remaining missing values
```
data_clean <- data_clean %>%
  drop()
data_clean1<-data_clean1 %>%
   drop()
```

# Show cleaned data
```
head(data_clean)
data_clean <- daily_activity_data %>%
  mutate(Id= ifelse(is.na(Id), median(Id, na.rm = TRUE), Id))
head(data_clean)
data_clean <- weightLoginInfo %>%
  mutate(Id= ifelse(is.na(Id), median(Id, na.rm = TRUE), Id))

```
# Remove duplicate rows
```
data_clean <- data_clean %>%
  distinct()
data_clean<- data_clean %>%
   distinct()
```
# Merge data
```
merged_data <- inner_join(data_clean1, data_clean, by = "Id", relationship = "many-to-many")
```

# Check for duplicates
```
nrow(merged_data)
```
# Compare with original dataset to confirm duplicates were removed
# Convert "ActivityDate" column to Date type if needed
```
merged_data<- merged_data %>%
  mutate(ActivityDate = as.Date(ActivityDate, format = "%m/%d/%Y"))
  ```

# Check if the data types are correct
```
str(merged_data)
```
# Remove outliers for a specific column, e.g., TotalSteps
```
merged_data <- merged_data %>%
  filter(TotalSteps <= quantile(TotalSteps, 0.95))
  
merged_data<-merged_data %>%
  filter(Calories <= quantile(Calories, 0.95))
  ```
# Check the filtered dataset
```
summary(merged_data)
```
# Final check: View summary and structure
```
summary(merged_data)
str(merged_data)
```

# Preview cleaned data
```
head(merged_data)
```
# Save the cleaned data as a CSV file
```
write.csv(merged_data, "cleaned_merged_data.csv", row.names = FALSE)
getwd()
merged_data <- read.csv("cleaned_merged_data.csv")
```
# Analyze data look for trends, and calculate summary statistics
```
data_clean %>%
  summarise(across(where(is.numeric), list(mean = mean, sd = sd), na.rm = TRUE))
```
# Scatter plot with a trend line
```
ggplot(data = merged_data, aes(x = TotalSteps, y = Calories)) +
  geom_point(color = "blue") + # Scatter plot
  geom_smooth(method = "lm", color = "red", se = TRUE) + 
  ```
![Scatterplot](https://github.com/Nallkevo/Case-study-Fitabase/blob/main/Screenshot%202024-11-18%20103928.png)
# This Shows a positive trendline between Calories and TotalSteps seems like the more steps you add on the daily bases helps your calorie rate.


  # Trend line with confidence interval
```
  labs(
    title = "Total Steps vs. Calories",
    x = "Total Steps",
    y = "Calories Burned"
  ) +
  theme_minimal()
  ggplot(time_trends, aes(x = ActivityDate, y = Calories)) +
      geom_col(fill = "blue") +
      theme_minimal() +
      labs(title = "Calories Over Time by Activity Date", x = "Activity Date", y = "Calories")
      ```
```
![bargraph](https://github.com/Nallkevo/Case-study-Fitabase/blob/main/Screenshot%202024-11-18%20112535.png)
  # Correlate
      
      ```
    cor(merged_data$VeryActiveMinutes,merged_data$Calories)
    ```
  # 0.6930595 this shows that its positive trend
  ```
    cor(merged_data$SedentaryActiveDistance,merged_data$Calories)
  ```
  # -0.001368087 this shows that it is a negative trend between these colnames
  ```
 cor_matrix = cor(merged_data %>% select(where(is.numeric)), use = "complete.obs")
    print(cor_matrix)
    ```
 # More visuals
      ```
    ggplot(merged_data, aes(x = TotalSteps, y = BMI)) +
      geom_point() +
      theme_minimal() + geom_smooth()
     ```
```
![visuals](https://github.com/Nallkevo/Case-study-Fitabase/blob/main/Screenshot%202024-11-18%20110816.png)
 # Aggregate data by date
    ```
    time_trends <- merged_data %>%
      group_by(ActivityDate) %>%
      summarise(Calories= sum(Calories, na.rm = TRUE))
      ```
    
  # Line plot for trends
    ```
    ggplot(time_trends, aes(x = ActivityDate, y = Calories)) +
      geom_col(fill = "blue") +
      theme_minimal() +
      labs(title = "Calories Over Time by Activity Date", x = "Activity Date", y = "Calories")
      ```
 ![lLineplot](https://github.com/Nallkevo/Case-study-Fitabase/blob/main/Screenshot%202024-11-18%20112535.png )
 # Notice the trends customer is burning more calories towards the end of the week
  # Group by category (e.g., activity type)
    ```
    segmented_data <- merged_data %>%
      group_by(Id) %>%
      summarise(AverageSteps = mean(TotalSteps, na.rm = TRUE))
    ```
    [
  
  # Bar plot of average steps by user
    ```
    ggplot(segmented_data, aes(x = Id, y = AverageSteps)) +
      geom_bar(stat = "identity", fill = "orange") +
      theme_minimal()
      ```
  ![Barplot](https://github.com/Nallkevo/Case-study-Fitabase/blob/main/Screenshot%202024-11-18%20113319.png) 
  ## Step 6: Act 
[Back to top](#introduction)
<br>
![womanrunning](https://github.com/Nallkevo/Case-study-Fitabase/blob/main/woman%20running.jpg)
Insights Summary:
About Bellabeat: Bellabeat is a cutting-edge company focused on creating tech-driven wellness products for women. By tracking and analyzing data related to activity, sleep, stress, and reproductive health, Bellabeat gives women the tools to better understand their health and habits. Since its founding in 2013, the company has rapidly emerged as a leader in wellness technology, helping users make informed decisions about their lifestyle.

After analyzing the FitBit Fitness Tracker data, I’ve identified several key opportunities to strengthen Bellabeat's marketing strategy based on patterns in user behavior:

Encourage More Active Lifestyles: The average daily step count across users is 7,638, which falls short of the CDC’s recommended 8,000 steps for optimal health. Studies show that taking 8,000 steps a day can reduce the risk of mortality by 51%, and 12,000 steps can lower the risk by 65%. Recommendation: Bellabeat can incentivize users to hit the 8,000-step goal by adding motivational prompts in the app, highlighting the benefits of maintaining a more active lifestyle.

Promote Increased Activity Levels: Most users are classified as lightly active, but there’s a significant opportunity for Bellabeat to motivate them to reach higher activity levels. By adding a system that helps users gradually progress to more intense activity, Bellabeat can elevate user engagement. Recommendation: Introduce a tiered fitness challenge that encourages users to become "fairly active," with rewards and personalized tips that guide them through their fitness journey.

Support Health and Nutrition Goals: Weight loss is a common goal for many users. To assist them, Bellabeat could offer tailored meal suggestions, such as low-calorie breakfast, lunch, and dinner options that align with users' activity levels and fitness objectives. Recommendation: Create a feature within the app that provides personalized meal plans or connects users to healthy eating resources to complement their workout routines.

Leverage Weekend Trends: Data shows that users are most active on Saturdays and less so on Sundays. Bellabeat could use this information to create weekend-specific content that keeps users engaged and encourages them to stay active throughout the weekend. Recommendation: On Saturdays, prompt users with motivational reminders for outdoor activities like walking or jogging, and on Sundays, suggest light exercise or relaxation routines to maintain a balanced weekend activity level.

These strategies will allow Bellabeat to not only encourage better health habits but also strengthen its relationship with users by providing them with actionable insights and personalized recommendations.







  
  
    





  


 








