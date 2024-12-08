# Power Outage Influence Prediction
This is a homework for EECS 398 at U-M
By Serena Chen & Andy Guo

# Introduction

In this project, we are analysing and make prediction base on the "Power Outages" dataset. This dataset provides information on power outages in US from 2000 to 2016, including details about their timing, location, causes, and effects. Initially, it contained 1,535 rows and 57 columns. After removing redundant columns (`variable` and `OBS`) and merging date and time fields (`OUTAGE.RESTORATION.DATE`, `OUTAGE.RESTORATION.TIME`, `OUTAGE.START.DATE`, and `OUTAGE.START.TIME`), we reduced the dataset to 53 meaningful columns. We then extracted the day and time from these merged columns, resulting in a total of 55 meaningful columns.

Our work focuses on estimating the duration of power outage. We consider this a key metric for evaluating outage severity, as it directly influences the resources required for recovery. Understanding which factors are associated with a longer outage duration can help local authorities better prepare for future outages and reduce losses for power outage. Although megawatts lost (`DEMAND.LOSS.MW`) could be important, that column has more than half of its entries missing, making it less suitable for prediction.

At this initial stage, we identified several relevant columns: `MONTH`, `YEAR`, `CLIMATE.REGION`, `CLIMATE.CATEGORY`, `ANOMALY.LEVEL`, `CAUSE.CATEGORY`, `OUTAGE.DURATION`, `DEMAND.LOSS.MW`, and `CUSTOMERS.AFFECTED`. Among these, `CLIMATE.REGION`, `CLIMATE.CATEGORY`, and `CAUSE.CATEGORY` are categorical variables that provide context (e.g., climate conditions like `normal` or `cold`, regions like `east north central`, and causes like `severe weather` or `intentional attack`). The numerical variables include `MONTH` and `YEAR` for timing, `ANOMALY.LEVEL` (ranging from -1.6 to 2.3), `OUTAGE.DURATION` (minutes per outage), `DEMAND.LOSS.MW` (megawatts lost), and `CUSTOMERS.AFFECTED` (number of customers impacted). The relevant dataframe has 1535 rows and 11 columns in total.

| Variable name       | Description                                                                                  |
|---------------------|----------------------------------------------------------------------------------------------|
| MONTH              | The month when the outage event occurred                                                     |
| YEAR               | The year when the outage event occurred                                                      |
| CLIMATE.REGION     | U.S. Climate regions as specified by National Centers for Environmental Information (9 Regions) |
| CLIMATE.CATEGORY   | The climate episodes corresponding to the years                                              |
| CAUSE.CATEGORY     | Categories of all the events causing the major power outages                                 |
| OUTAGE.DURATION    | Duration of outage events (in minutes)                                                       |
| ANOMALY.LEVEL      | The oceanic El Niño/La Niña (ONI) index referring to the cold and warm episodes by season    |
| DEMAND.LOSS.MW     | Amount of peak demand lost during an outage event (in Megawatt)                              |
| CUSTOMERS.AFFECTED | Number of customers affected by the power outage event                                       |
| start_day          | The date of the outage's start                                                              |
| start_hour         | Capturing the hour of the outage's start        

This selection of columns will guide our investigation into understanding and predicting the extent of customer impact during power outages.

# Data Cleaning and Exploratory Data Analysis

*Data Cleaning, Univariate Analysis, and Imputation:*

## Data Cleaning

Our data cleaning include the following steps:

1, **we identified and removed two columns that lacked meaningful information: `Variables` (which contained only NaN values) and `OBS` (which duplicated the dataframe’s indices).**

2, **We extracted specific information from the `OUTAGE.START.DATE` and `OUTAGE.START.TIME` columns. Specifically:**

- `OUTAGE.START.DATE` was split to create a new column named `start_day`, capturing the date of the outage's start.
- `OUTAGE.START.TIME` was split to create a new column named `start_hour`, capturing the hour of the outage's start.
- The original date and time columns were dropped to avoid multicollinearity.


3, **Then, we drop irrelevant columns and only keeping the features we are inerested in** 

The interested columns include:

| **Variable Name**   | **Unit**                                                                                   |
|---------------------|-------------------------------------------------------------------------------------------|
| MONTH              | Month (1–12)                                                                               |
| YEAR               | Year (e.g., 2000–2016)                                                                     |
| CLIMATE.REGION     | Categorical (9 regions in the U.S.)                                                        |
| CLIMATE.CATEGORY   | Categorical (“Warm,” “Cold,” or “Normal”)                                                  |
| CAUSE.CATEGORY     | Categorical (e.g., “Severe Weather,” “Intentional Attack”)                                 |
| OUTAGE.DURATION    | Minutes                                                                                    |
| ANOMALY.LEVEL      | Oceanic Niño Index (ONI), measured as a 3-month running mean                               |
| DEMAND.LOSS.MW     | Megawatts                                                                                  |
| CUSTOMERS.AFFECTED | Number of customers                                                                        |
| start_day          | Date (1-31)                                                                                |
| start_hour         | Hour (0–23)                                                                                |


### After the data cleaning, the dataframe is like:

|   MONTH |   YEAR | CLIMATE.REGION     | CLIMATE.CATEGORY   |   ANOMALY.LEVEL | CAUSE.CATEGORY     |   OUTAGE.DURATION |   DEMAND.LOSS.MW |   CUSTOMERS.AFFECTED |   start_day |   start_hour |
|--------:|-------:|:-------------------|:-------------------|----------------:|:-------------------|------------------:|-----------------:|---------------------:|------------:|-------------:|
|       7 |   2011 | East North Central | normal             |            -0.3 | severe weather     |              3060 |              nan |                70000 |           1 |           17 |
|       5 |   2014 | East North Central | normal             |            -0.1 | intentional attack |                 1 |              nan |                  nan |          11 |           18 |
|      10 |   2010 | East North Central | cold               |            -1.5 | severe weather     |              3000 |              nan |                70000 |          26 |           20 |
|       6 |   2012 | East North Central | normal             |            -0.1 | severe weather     |              2550 |              nan |                68200 |          19 |            4 |
|       7 |   2015 | East North Central | warm               |             1.2 | severe weather     |              1740 |              250 |               250000 |          18 |            2 |

## Univariate Analysis and Interesting Aggregates:

- Here is the cause category recorded in the dataset, we can see the two major reasons of the power outage is 'severe weather' and 'intentional attack'.

  <iframe
    src="assets/CAUSE.CATEGORY VS Outage Duration.html"
    width="800"
    height="600"
    frameborder="0"
    style="margin-left: 10px;"
  ></iframe>

- Next we want to explore the distribution of OUTAGE.DURATION variable, here is a histogram of power outage duration in hours:

 <iframe
    src="assets/outage_distribution.html"
    width="800"
    height="600"
    frameborder="0"
    style="margin-left: 10px;"
  ></iframe>

About 80% of the power outages ended within 75 hours.

- Here we have the number of Outage appear for each year and month.

  <iframe
    src="assets/year_count.html"
    width="800"
    height="400"
    frameborder="0"
    style="margin-right: 10px;"
  ></iframe>

  <iframe
    src="assets/month_count.html"
    width="800"
    height="400"
    frameborder="0"
    style="margin-left: 10px;"
  ></iframe>

We can see a peak of outage in 2011, and the peak month for power outage is June.

- And we want to find whether there is any geographical pattern for the number of power outage.

<iframe
  src="assets/map.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

**- What's the shape for numerical variables?**
### Skewness Table for Key Variables

| Variable Name         | Skewness     |
|-----------------------|--------------|
| MONTH                | Neutral      |
| YEAR                 | Neutral      |
| CLIMATE.REGION       | Neutral      |
| CLIMATE.CATEGORY     | Neutral      |
| CAUSE.CATEGORY       | Neutral      |
| OUTAGE.DURATION      | Right-skewed |
| ANOMALY.LEVEL        | Neutral      |
| DEMAND.LOSS.MW       | Right-skewed |
| CUSTOMERS.AFFECTED   | Right-skewed |
| start_day            | Neutral      |
| start_hour           | Neutral      |

## Bivariate Analysis and Interesting Aggregates:

We proceeded with the nine columns identified in the introduction. We separated them into categorical and numerical columns and applied one-hot encoding to the categorical variables. Afterward, we calculated correlation coefficients using a correlation matrix and selected the columns most strongly correlated with `OUTAGE.DURATION`. 

**-  here we want to decide the relationship between Customers Affected and Outage Duration**
<iframe
  src="assets/Customers Affected VS Outage Duration.html"
  width="800"
  height="500"
  frameborder="0"
></iframe>

The scatter plot reveals a concentration of data points where outage durations are short (below 20k minutes) and the number of customers affected is relatively small (under 500k). There are fewer instances of prolonged outages (exceeding 60k minutes), and these tend to involve varying numbers of customers, up to a maximum of approximately 3 million.

**- here we want to decide the relationship between Month and Outage Duration**
<iframe
  src="assets/Month VS Outage Duration.html"
  width="800"
  height="500"
  frameborder="0"
></iframe>

Outage durations are scattered across all months, with no clear concentration in any particular month. Most outages last less than 20k minutes, and there are only a few extreme outliers with durations exceeding 80k minutes. Surprisingly, the distribution of outage durations across the months suggests that outages occur consistently throughout the year, without any strong seasonal trend affecting outage duration.


**- here we want to decide the relationship between Cause of outage and Mean Outage Duration**
<iframe
  src="assets/Cause VS Outage Duration.html"
  width="800"
  height="500"
  frameborder="0"
></iframe>

The bar chart highlights that outages caused by weather have the highest mean duration, followed by equipment failure. Other causes such as maintenance, vandalism, and other categories result in significantly shorter outage durations on average.

**- What is the correlation between One-hot Encoded variables and `OUTAGE.DURATION`?**
   - We utilized a correlation matrix to identify the top 30 features most correlated with `OUTAGE.DURATION`(a potential response variable that we are interested in) after one-hot encoding of the categorical variables. 

**- Here is the correlation matrix of variable with the top-5 correlation value**

<div style="display: flex; justify-content: center; align-items: center; width: 100%; height: 50vh; overflow: hidden;">
  <iframe
    src="assets/corr_matrix.svg"
    style="width: 100%; height: 100%; border: none; object-fit: contain;"
    title="Correlation Matrix">
  </iframe>
</div>

We might utilize this for future feature selection to improve the baseline model.

## aggregates & pivot table

**-Here is a pivot table for Region and Outage Duration**

|    | CLIMATE.REGION     |   OUTAGE.DURATION |
|---:|:-------------------|------------------:|
|  0 | Central            |          2701.13  |
|  1 | East North Central |          5352.04  |
|  2 | Northeast          |          2991.66  |
|  3 | Northwest          |          1284.5   |
|  4 | South              |          2846.1   |
|  5 | Southeast          |          2217.69  |
|  6 | Southwest          |          1566.14  |
|  7 | West               |          1628.33  |
|  8 | West North Central |           696.562 |

Seemingly East North Central have the highest mean OUTAGE.DURATION among the other regions

**-Here is a pivot table for Climate category and Outage Duration**

|    | CLIMATE.CATEGORY   |   OUTAGE.DURATION |
|---:|:-------------------|------------------:|
|  0 | cold               |           2656.96 |
|  1 | normal             |           2530.98 |
|  2 | warm               |           2817.32 |

There is not obvious pattern for these two variables

## Handle missing values

we developed an imputation strategy informed by our exploratory analysis:

   *Categorical Variables*  
   - Variables like `CLIMATE_REGION` and `CLIMATE.CATEGORY` were analyzed using univariate analysis, which showed relatively even distributions across their subcategories(i.e., no single category dominated the data significantly).

<div style="display: flex; justify-content: space-around; align-items: center;">

  <iframe
    src="assets/CLIMATE.CATEGORY VS Outage Duration.html"
    width="550"
    height="300"
    frameborder="0"
    style="margin-right: 10px;"
  ></iframe>

  <iframe
    src="assets/CLIMATE.REGION VS Outage Duration.html"
    width="550"
    height="300"
    frameborder="0"
    style="margin-left: 10px;"
  ></iframe>

</div>

   - As a result for these categorical variables, we applied probabilistic imputation to address missing values.

   *Numerical Variables*  
   - For variables such as `MONTH`, `YEAR`, `start_day`, and `start_hour`, probabilistic imputation was used.  

   - For other numerical variables, we examined their distributions:  

     - If a variable was **skewed**, the **median** was used for imputation.  

     - If a variable was **not skewed**, the **mean** was used for imputation.  

# Prediction Problem

We plan to predict the duration of each individual power outage occurrence in 2016, framing this as a regression problem. The response variable for this analysis is OUTAGE.DURATION, which measures the total duration of an outage in minutes. We chose this focus because the length of an outage is a critical factor for recovery planning, resource allocation, and minimizing the impact on affected communities, making it a vital metric for assessing outage severity. We choose this also because we find it correlats with many of the variables and have few missing values in the EDA section, which potentially allow us to create a better model.

To evaluate the performance of our model, we will use Mean Squared Error (MSE) as the primary metric. MSE was selected because it penalizes larger errors more heavily, which is important in the context of predicting outage durations where significant underestimations or overestimations could have substantial real-world consequences. By focusing on minimizing MSE, we aim to prioritize accuracy for severe or prolonged outages.

In line with the requirements, we will carefully ensure that all features included in the model are available at the time of prediction, avoiding any data leakage. Notably, we will include the variable number of customers affected as a predictor. This feature can be reasonably estimated based on the population of the outage area, which is typically known or accessible at the time of the outage.

# Baseline Model

