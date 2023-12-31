# Case Study 2: How Can a Wellness Technology Company Play It Smart?
# Bellabeat


## Table of Contents

* Introduction
* Ask Phase
* Prepare Phase
* Process Phase
* Analyze Phase
* Share Phase
* Act Phase(Conclusion)
  
These past few months i was doing a Google Data Analytics Certification Course from Coursera. As a part of it i have done Case study 2 as my Capstone Project.This case study follows 6 steps of the data analysis process: ask, prepare, process, analyze, share, and act Phase.

# Introduction
Bellabeat is a high-tech company that manufactures health-focused smart products for women.They offer different smart devices that collect data on activity, sleep, stress, and reproductive health to empower women with knowledge about their own health and habits.Although Bellabeat is a successful small company, they have the potential to become a larger player in the global smart device market. Urška Sršen, cofounder and Chief Creative Officer of Bellabeat, believes that analyzing smart device fitness data could help unlock new growth opportunities for the company.

The main focus of this case is to analyze smart devices fitness data and determine how it could help unlock new growth opportunities for Bellabeat. We will focus on one of Bellabeat’s products: Bellabeat app.

The Bellabeat app provides users with health data related to their activity, sleep, stress, menstrual cycle, and mindfulness habits. This data can help users better understand their current habits and make healthy decisions. The Bellabeat app connects to their line of smart wellness products

# Ask Phase
**Business Task :**

To Identify trends in how consumers use non-Bellabeat smart devices to apply insights into Bellabeat’s marketing strategy.

**Key Stakeholders:**

* Urška Sršen - Bellabeat cofounder and Chief Creative Officer
* Sando Mur - Bellabeat cofounder and key member of Bellabeat executive team
* Bellabeat Marketing Analytics team
  
# Prepare Phase
The data used for our case study is FitBit Fitness Tracker Data.This dataset is stored in Kaggle and was made available through Mobius.

**About Dataset:**

This Kaggle data set contains personal fitness tracker from thirty fitbit users. Thirty eligible Fitbit users consented to the submission of personal tracker data, including minute-level output for physical activity, heart rate, and sleep monitoring. It includes information about daily activity, steps, and heart rate that can be used to explore users’ habits.This data is from March 2016 through May 2016 The dataset contains 18 CSV files.

**Credibility of Dataset:**

Due to the limitation of size and not having any demographic information we could encounter a sampling bias. The problem we encounter is that the dataset is not current,meaning that user habits may have changed over the years.

## Installed and Loaded Packages
```
# installing packages

install.packages("tidyverse")
install.packages("lubridate")
install.packages("dplyr")
install.packages("ggplot2")
install.packages("tidyr")
install.packages("here")
install.packages("skimr")
install.packages("janitor")

#Loading Packages

library(tidyverse)
library(lubridate)
library(dplyr)
library(ggplot2)
library(tidyr)
library(here)
library(skimr)
library(janitor)

```


# Process Phase

**Importing datasets:**

The following datasets are imported.Due to the the small sample we won't consider for this analysis Weight (8 Users) and heart rate (7 users)

```
activity <- read.csv("C:/Fitness/Fitabase Data 4.12.16-5.12.16/dailyActivity_merged.csv")
calories <- read.csv("C:/Fitness/Fitabase Data 4.12.16-5.12.16/hourlyCalories_merged.csv")
intensities <- read.csv("C:/Fitness/Fitabase Data 4.12.16-5.12.16/hourlyIntensities_merged.csv")
sleep <- read.csv("C:/Fitness/Fitabase Data 4.12.16-5.12.16/sleepDay_merged.csv")
steps <- read.csv("C:/Fitness/Fitabase Data 4.12.16-5.12.16/hourlySteps_merged.csv")
```

Now i am good to clean the data.Before that I will take a look at each data frame to familiarize myself with the data and check errors in these datasets.


**Dataset Preview:**
```
head(activity)
tail(activity)
colnames(activity)
​
head(calories)
tail(calories)
colnames(calories)

head(sleep)
tail(sleep)
colnames(sleep)
​
head(steps)
tail(steps)
colnames(steps)
```

**Number of users:**

Before cleaning the dataset i have make sure how many unique users are per data frame.

```
n_distinct(activity$Id)
n_distinct(calories$Id)
n_distinct(sleep$Id)
n_distinct(steps$Id)
```
**Checking for Dulicates:**

```
sum(duplicated(activity))
sum(duplicated(sleep))
sum(duplicated(calories))
sum(duplicated(steps))
```
0

3

0

0


**Removing duplicates and N/A :**
```
activity <- activity %>%
  distinct() %>%
  drop_na()
​
calories <- calories %>%
  distinct() %>%
  drop_na()
​
sleep <- sleep %>%
  distinct() %>%
  drop_na()
​
steps <- steps %>%
  distinct() %>%
  drop_na()

```
**Checking if Duplicates is removed :**
```
sum(duplicated(sleep))
```
0


**Cleaning the datetime column:**
```
​
#calories
calories$ActivityHour=as.POSIXct(calories$ActivityHour, format="%m/%d/%Y %I:%M:%S %p", tz=Sys.timezone())
calories$time <- format(calories$ActivityHour, format = "%H:%M:%S")
calories$date <- format(calories$ActivityHour, format = "%m/%d/%y")
print(calories)
​
#sleep
sleep$SleepDay=as.POSIXct(sleep$SleepDay, format="%m/%d/%Y %I:%M:%S %p", tz=Sys.timezone())
sleep$date <- format(sleep$SleepDay, format = "%m/%d/%y")
print(sleep)
​
# activity
activity$ActivityDate=as.POSIXct(activity$ActivityDate, format="%m/%d/%Y", tz=Sys.timezone())
activity$date <- format(activity$ActivityDate, format = "%m/%d/%y")
print(activity)
​
# steps
steps$ActivityHour=as.POSIXct(steps$ActivityHour, format="%m/%d/%Y %I:%M:%S %p", tz=Sys.timezone())
steps$time <- format(steps$ActivityHour, format = "%H:%M:%S")
steps$date <- format(steps$ActivityHour, format = "%m/%d/%y")
print(steps)
```

# Analyze Phase
```
# activity
activity %>%  
  select(TotalSteps,
         TotalDistance,
         SedentaryMinutes, Calories) %>%
  summary()
​
#  num of active minutes per category
activity %>%
  select(VeryActiveMinutes, FairlyActiveMinutes, LightlyActiveMinutes) %>%
  summary()
​
#sleep
sleep %>%
  select(TotalSleepRecords, TotalMinutesAsleep, TotalTimeInBed) %>%
  summary()
​
#calories
calories %>%
  select(Calories,time) %>%
  summary()
​
#steps
steps %>%
  select(StepTotal,time) %>%
  summary()
```
**Observations from the above Summaries:**

* Sedetary minutes on average is 16.5 hours.
* The average participant burns 97 calories per hour
* The majority of the avaerage participants are lightly active for 3 hours.
* The average number of steps per day is 7638.According to Centers for Disease Control and Prevention(CDC),recommends people to take 10,000 steps daily for healthy adults to achieve health benefits.
* an average calories a participant burn per hour is 97.3.
* an average participant sleep for 7 hours.

# Share Phase
From the above observations, let the analysis be done in depth with the help of visualization for easy understanding. Before visualization, I will merge two of the datasets. I have joined activity and sleep datasets using an inner join on columns id and date.


**Merging two datasets:**

```
merged_data <- merge(sleep, activity, by=c ('Id', 'date'))
glimpse(merged_data)
```

# Total Steps vs Calories

![pic](total_steps_vs_calories.png)
From the above analysis there is a correlation between total number of steps taken and calories burned. The more steps each participant takes, the more calories they burn.


# Total time asleep vs Total time in bed

![pic](total_time_asleep_vs_time_in_bed.png)
We can see a positive correlation between total time asleep vs total time in bed. To improve sleep quality for its users, bellabeat should consider having a section where users can customize their sleep schedule to ensure consistency


# Sleep and Inactive Time

![pic](sleep_and_inactive_time.png)

```
 cor(merged_data$TotalMinutesAsleep,merged_data$SedentaryMinutes)
```
 -0.6010731
 
 From looking at the graph above, we can see there is a negative correlation between total inactive time and TotalMinutesAsleep. This means that the less active a participant is, the less sleep they tend to get. Now I will look at whether the each day of the week affects our activity levels and sleep and also find number of hourly steps a participant take throughout the day.
```
merged_data <- mutate(merged_data, day = wday(SleepDay, label = TRUE))
summarized_activity_sleep <- merged_data %>% 
  group_by(day) %>% 
  summarise(AvgDailySteps = mean(TotalSteps),
            AvgAsleepMinutes = mean(TotalMinutesAsleep),
            AvgAwakeTimeInBed = mean(TotalTimeInBed), 
            AvgSedentaryMinutes = mean(SedentaryMinutes),
            AvgLightlyActiveMinutes = mean(LightlyActiveMinutes),
            AvgFairlyActiveMinutes = mean(FairlyActiveMinutes),
            AvgVeryActiveMinutes = mean(VeryActiveMinutes), 
            AvgCalories = mean(Calories))
head(summarized_activity_sleep)

ggplot(data = summarized_activity_sleep, mapping = aes(x = day, y = AvgDailySteps)) +
  geom_col(fill = "Blue") + labs(title = "Daily Step Count")
```
![pic](daily_steps.png)
From this bar garph , it is clear that most of the participants are active in saturdays and less in sundays.
 # hourly steps throughout the day

 ```
steps %>% group_by(time) %>% summarize(average_steps = mean(StepTotal)) %>%
  ggplot() + geom_col(mapping = aes(x=time, y = average_steps, fill = average_steps)) + 
  labs(title = "Hourly steps throughout the day", x="", y="") + 
  scale_fill_gradient(low = "blue", high = "orange")+
  theme(axis.text.x = element_text(angle = 90))
```
![pic](

We can see that users are more active between 8am and 7pm. Walking more steps during lunch time from 12pm to 2pm and evenings from 5pm and 7pm.

