# power-outage-prediction
This is a homework for EECS 398 at U-M

## STEP 1: Introduction

The **Power Outages** dataset provides information on power outages occurring between 2000 and 2016, including details on their timing, location, causes, and effects. Initially, it contains 1,535 rows and 57 columns. After removing redundant columns (`variable` and `OBS`) and merging date and time fields (`OUTAGE.RESTORATION.DATE`, `OUTAGE.RESTORATION.TIME`, `OUTAGE.START.DATE`, and `OUTAGE.START.TIME`), we are left with 53 meaningful columns.

Our work focuses on estimating the number of customers affected by a power outage. We consider this metric especially meaningful for evaluating outage severity, as it directly influences the resources needed for recovery efforts. By understanding which factors correlate with higher numbers of affected customers, local authorities can better prepare for future outages.

In this initial stage, we have identified several columns that may be relevant to our question:

- **Categorical:**  
  - `CLIMATE.REGION` (e.g., `east north central`, `central`)  
  - `CLIMATE.CATEGORY` (e.g., `normal`, `cold`, `warm`)  
  - `CAUSE.CATEGORY` (e.g., `severe weather`, `intentional attack`, `system operability disruption`)

- **Numerical:**  
  - `MONTH`  
  - `YEAR`  
  - `ANOMALY.LEVEL` (values from -1.6 to 2.3, indicating degree and direction of anomaly)  
  - `OUTAGE.DURATION` (minutes)  
  - `DEMAND.LOSS.MW` (megawatts lost)  
  - `CUSTOMERS.AFFECTED` (number of customers impacted)

These selected columns, derived from the dataset and guided by the attached screenshot, will help us explore and predict the extent of customer impact during power outages.
