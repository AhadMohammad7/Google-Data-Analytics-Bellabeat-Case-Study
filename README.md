# Google-Data-Analytics-Bellabeat-Case-Study
Author: Ahad Mohammad

Date: 23/02/2025

**(IN PROGRESS)** 

Last Updated: 26/02/2025



[Google Data Analytics Course](https://www.coursera.org/professional-certificates/google-data-analytics)

## Introduction / Background 

In this case study, I will be working as a junior data analyst to solve the key **business task** at hand while utilizing data analysis processes: [Ask](https://github.com/AhadMohammad7/Google-Data-Analytics-Bellabeat-Case-Study/blob/main/README.md#ask), [Prepare](https://github.com/AhadMohammad7/Google-Data-Analytics-Bellabeat-Case-Study/blob/main/README.md#prepare), [Process](https://github.com/AhadMohammad7/Google-Data-Analytics-Bellabeat-Case-Study/blob/main/README.md#process), [Analyze](), and [Act]().

Bellabeat, a high-tech manufacturer of health-focused products for women. Bellabeat is a successful small company, but they have the potential to become a larger player in the global smart device market. Urška Sršen, cofounder and Chief Creative Officer of Bellabeat, believes that analyzing smart device fitness data could help unlock new growth opportunities for the company. I will be focusing on Bellabeat’s products and analyze smart device data to gain insight into how consumers are using their smart devices. I will look for key insights to help guide marketing strategy for the company. I will present my analysis to the Bellabeat executive team.


## Ask
In this section, I will identify the business task and guiding questions for what I will be looking for throughout my analysis.

### Business Task

To analyze smart device usage and fitness data to help unlock new growth opportunities for Bellabeat including marketing strategy and product offering.

### Analysis Questions
1. What are some trends between how fitbit users use their smart device?
2. What are some features or improvements Bellabeat can incorporate into their products?
3. How do users engage with their smart device? What do they look for?


## Prepare

**Data Source:** 

The dataset was obtained from Kaggle and was published on Zenodo. It contains the information of 30 fitbit users who consentually submitted the data of their personal tracker data. This dataset includes physical activity(minute level output), heart rate, and sleep monitoring. The data was generated by a distributed survey via Amazon Mechanical Turk between 03.12.2016-05.12.2016.

**Data Organization:**

The data proves to be a bit out dated as it was captured in 2016. The gap in the advancement of technology could prove to be slightly inaccurate. 
The data is organized into 2 periods 3.12.16-4.11.16 and 4.12.16-5.12.16. The data is in wide format.

**Data Bias/Credibility:**

The data is a sample of 30 users, which may prove to be too small of a sample to determine accurate and effective insights. Furthermore, the data has no mention of demographics. Bellabeat offers products to female only customers, without knowing the gender of the fitbit users it could impact the findings of the analysis. Lastly, the data is credible as it is sourced by Zenodo and is an original dataset, the only downside being that the data spans over 2 months. This could leave out valuable information as there could be seasonal changes in the key metrics used in the analysis.

* **Reliability:** The dataset is posted on Zenodo and Kaggle with all information related to the source of the data, and its origin.
* **Original:** The dataset is original in terms of its posting on Zenodo. There are no copies of this dataset on Zenodo.
* **Comprehensive:** The data in the datasets are labelled and seperated in rows and columns. The data proves to be in structured format, allowing for us to be able to perform analysis.
* **Current:** The data is not considered current as it was captured in 2016. This could limit the relevance of the data.
* **Cited:** The dataset on Zenodo offers citation, showing who was responsible for the creation of the data.

**Licensing, Privacy, Security, and Accessibility:**

The data does have a CC0 Public Domain license, and the dataset does not disclose the identity of the respondents of the survey. In terms of accessibility, the data is *public* and can be found on Kaggle, referenced to the original source: Zenodo.

**Data Integrity:**

The data does contain blanks, and incorrect date formats, which can be cleaned during the data processing cycle. However, the data is from a reliable source (Zenodo), and does contain private keys (ID) to differentiate the data between users.

**Data Relevance**

While the data is original, cited, and comprehensive, there may be some limitations due to the short period of data (spanning 2 months) and the fact that the data was captured in 2016. However, there are still insights that could be discovered from cleaning, and processing the data. 


## Process

In this section, I will be processing/cleaning the data prior to analyzing.

### Setting up my environment

* tidyverse: A collection of R packages designed for data science, including ggplot2, dplyr, tidyr, readr, and more. It helps with data manipulation, visualization, and importing/exporting data.
* janitor: Helps clean and format data, particularly useful for handling messy column names and duplicate values. Functions like clean_names() automatically standardize column names.
* skimr: Used for quickly summarizing datasets. The skim() function provides an overview of missing values, data types, and summary statistics in a more detailed way than summary().

```{r}
install.packages("tidyverse")
install.packages("janitor")
install.packages("skimr")
```

### Loading Packages

```{r}
library(tidyverse)
library(janitor)
library(skimr)
```

### Importing Data

Files will be read and assigned to an object so that they can be viewed and transformed.

```{r}
p1_daily_activity <- read.csv("mturkfitbit_export_3.12.16-4.11.16/Fitabase Data 3.12.16-4.11.16/dailyActivity_merged.csv")
p2_daily_activity <- read.csv("mturkfitbit_export_4.12.16-5.12.16/Fitabase Data 4.12.16-5.12.16/dailyActivity_merged.csv")
p1_hourly_calories <- read.csv("mturkfitbit_export_3.12.16-4.11.16/Fitabase Data 3.12.16-4.11.16/hourlyCalories_merged.csv")
p2_hourly_calories <- read.csv("mturkfitbit_export_4.12.16-5.12.16/Fitabase Data 4.12.16-5.12.16/hourlyCalories_merged.csv")
p1_minute_sleep <- read.csv("mturkfitbit_export_3.12.16-4.11.16/Fitabase Data 3.12.16-4.11.16/minuteSleep_merged.csv")
p2_minute_sleep <- read.csv("mturkfitbit_export_4.12.16-5.12.16/Fitabase Data 4.12.16-5.12.16/minuteSleep_merged.csv")
p1_hourly_steps <- read.csv("mturkfitbit_export_3.12.16-4.11.16/Fitabase Data 3.12.16-4.11.16/hourlySteps_merged.csv")
p2_hourly_steps <- read.csv("mturkfitbit_export_4.12.16-5.12.16/Fitabase Data 4.12.16-5.12.16/hourlySteps_merged.csv")
```
### Merging Datasets

Note how there is a p1 and p2 file for each metric. This is due to there being two seperate datasets as there are two periods over which the data was collected. The datasets are structured in the same format, therefore I will be merging them using the rbind function.

```{r}
daily_activity <- rbind(p1_daily_activity,p2_daily_activity)
hourly_calories <- rbind(p1_hourly_calories,p2_hourly_calories)
minute_sleep <- rbind(p1_minute_sleep,p2_minute_sleep)
hourly_steps <- rbind(p1_hourly_steps,p2_hourly_steps)
```

### Checking Data 

Checking column names and data types

```{r}
str(daily_activity)
str(hourly_calories)
str(minute_sleep)
str(hourly_steps)
```

Using janitor function skim_without_charts to do a final check to see if any inconsistencies in the datasets.

```{r}
skim_without_charts(daily_activity)
skim_without_charts(hourly_calories)
skim_without_charts(minute_sleep)
skim_without_charts(hourly_steps)
```

Quick check for total number of nulls in each dataset

```{r}
sum(is.na(daily_activity))
sum(is.na(hourly_calories))
sum(is.na(minute_sleep))
sum(is.na(hourly_steps))
```

After checking the data, we are able to see that the date column is a char type which will need to be changed to date. There are 0 null values, and we need to ensure that all column names are consistent. Duplicate values will also have to be cleaned.

### Cleaning Data

Note: Column names are cleaned using clean_names function to ensure consistency

```{r}
daily_activity <- clean_names(daily_activity)
hourly_calories <- clean_names(hourly_calories)
minute_sleep <- clean_names(minute_sleep)
hourly_steps <- clean_names(hourly_steps)
```

Removing duplicate data. In this step the distinct function will remove any duplicate entries (rows) ensuring that the same data is not used in the analysis process.

```{r}
daily_activity <- distinct(daily_activity)
hourly_calories <- distinct(hourly_calories)
minute_sleep <- distinct(minute_sleep)
hourly_steps <- distinct(hourly_steps)
```
Changing variable type (char to date and date/time).

```{r}
daily_activity <- daily_activity %>%
  mutate(activity_date = as.Date(activity_date, format = "%m/%d/%Y"))

hourly_calories <- hourly_calories %>%
  mutate(activity_hour = as.POSIXct(activity_hour, format = "%m/%d/%Y %I:%M:%S %p"))

hourly_steps <- hourly_steps %>%
  mutate(activity_hour = as.POSIXct(activity_hour, format = "%m/%d/%Y %I:%M:%S %p"))

minute_sleep <- minute_sleep %>%
  mutate(date = as.POSIXct(date, format = "%m/%d/%Y %I:%M:%S %p"))
```
