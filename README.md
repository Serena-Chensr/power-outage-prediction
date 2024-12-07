# Power Outage Influence Prediction
This is a homework for EECS 398 at U-M
By Serena Chen & Andy Guo

# Introduction

The "Power Outages" dataset provides information on power outages from 2000 to 2016, including details about their timing, location, causes, and effects. Initially, it contained 1,535 rows and 57 columns. After removing redundant columns (`variable` and `OBS`) and merging date and time fields (`OUTAGE.RESTORATION.DATE`, `OUTAGE.RESTORATION.TIME`, `OUTAGE.START.DATE`, and `OUTAGE.START.TIME`), we reduced the dataset to 53 meaningful columns. We then extracted the day and time from these merged columns, resulting in a total of 55 meaningful columns.

Our work focuses on estimating the number of customers affected by a power outage. We consider this a key metric for evaluating outage severity, as it directly influences the resources required for recovery. Understanding which factors are associated with a higher number of affected customers can help local authorities better prepare for future outages. Although megawatts lost (`DEMAND.LOSS.MW`) could be important, that column has more than half of its entries missing, making it less suitable for prediction.

At this initial stage, we identified several relevant columns: `MONTH`, `YEAR`, `CLIMATE.REGION`, `CLIMATE.CATEGORY`, `ANOMALY.LEVEL`, `CAUSE.CATEGORY`, `OUTAGE.DURATION`, `DEMAND.LOSS.MW`, and `CUSTOMERS.AFFECTED`. Among these, `CLIMATE.REGION`, `CLIMATE.CATEGORY`, and `CAUSE.CATEGORY` are categorical variables that provide context (e.g., climate conditions like `normal` or `cold`, regions like `east north central`, and causes like `severe weather` or `intentional attack`). The numerical variables include `MONTH` and `YEAR` for timing, `ANOMALY.LEVEL` (ranging from -1.6 to 2.3), `OUTAGE.DURATION` (minutes per outage), `DEMAND.LOSS.MW` (megawatts lost), and `CUSTOMERS.AFFECTED` (number of customers impacted).

This selection of columns, derived from the dataset and the attached screenshot, will guide our investigation into understanding and predicting the extent of customer impact during power outages.

# Data Cleaning and Exploratory Data Analysis

*Data Cleaning, Univariate Analysis, and Imputation:*

## Data Cleaning

During our data cleaning, we identified and removed two columns that lacked meaningful information: `Variables` (which contained only NaN values) and `OBS` (which duplicated the dataframe’s indices). We also merged related date and time columns. Specifically, `OUTAGE.START.DATE` and `OUTAGE.START.TIME` were combined into a single column capturing both the date and time of the outage’s start. Similarly, `OUTAGE.RESTORATION.DATE` and `OUTAGE.RESTORATION.TIME` were merged into a single column capturing the date and time when the outage was restored. We named these new columns `starttime_str` and `endtime_str` and dropped the original date/time columns to avoid multicollinearity. Using `pd.DataFrame`, we extracted the day and time from these combined columns.

Next, we addressed missing values. We devised an imputation strategy based on the results of our exploratory analysis. After performing univariate analysis on categorical variables (e.g., `CLIMATE_REGION`, `CLIMATE.CATEGORY`), we found that their subcategories were relatively evenly distributed. Therefore, for categorical variables, we applied one-hot encoding and performed probabilistic imputation for missing values. For numerical variables (`MONTH`, `YEAR`, day, time), we also used probabilistic imputation. For other numerical variables, we first examined their distributions. If a variable was skewed, we used the median for imputation; if it was not skewed, we used the mean. We then created functions to classify numerical columns as neutral or skewed based on a skewness threshold of 1.0. Ultimately, our imputation approach involves selecting the top 30 features most correlated with `CUSTOMERS.AFFECTED` to guide imputation using the correlation matrix.

*Bivariate Analysis and Interesting Aggregates:*

We proceeded with the nine columns identified in the introduction. We separated them into categorical and numerical columns and applied one-hot encoding to the categorical variables. Afterward, we calculated correlation coefficients using a correlation matrix and selected the columns most strongly correlated with `CUSTOMERS.AFFECTED`. Surprisingly, `MONTH` was not among the top 10 factors correlated with `CUSTOMERS.AFFECTED`. To understand this, we conducted bivariate analysis, plotting scatter plots of the top correlated variables (`DEMAND.LOSS.MW`, `OUTAGE.DURATION`, and `CAUSE.CATEGORY_intentional attack`) against `CUSTOMERS.AFFECTED`, as well as a scatter plot of `MONTH` vs. `CUSTOMERS.AFFECTED`. We observed that `CUSTOMERS.AFFECTED` was evenly distributed throughout the year, confirming our correlation matrix findings. Moreover, we noticed that many of the top correlated variables, once encoded, represented specific cause categories. To explore this further, we created an aggregated groupby object by cause category and summed the number of affected customers, then visualized these results.

<iframe
  src="assets/map.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>