# power-outage-prediction
This is a homework for EECS 398 at U-M

The "Power Outages" dataset provides information on power outages occurring between 2000 and 2016, including details about their timing, location, causes, and effects. It originally contains 1,535 rows and 57 columns. After removing redundant columns (‘variable’ and ‘OBS’) and merging date and time fields for ‘OUTAGE.RESTORATION.DATE’, ‘OUTAGE.RESTORATION.TIME’, ‘OUTAGE.START.DATE’, and ‘OUTAGE.START.TIME’, we are left with 53 meaningful columns.

Our work focuses on estimating the number of customers affected by a power outage. We believe this is one of the most meaningful metrics to evaluate outage severity, as it directly influences the resources needed for recovery. Understanding which factors are associated with higher numbers of affected customers can help local authorities prepare for future outages.

At this initial stage, we have identified several columns that may be relevant to our question: ‘MONTH’, ‘YEAR’, ‘CLIMATE.REGION’, ‘CLIMATE.CATEGORY’, ‘ANOMALY.LEVEL’, ‘CAUSE.CATEGORY’, ‘OUTAGE.DURATION’, ‘DEMAND.LOSS.MW’, and ‘CUSTOMERS.AFFECTED’. Among these, ‘CLIMATE.REGION’, ‘CLIMATE.CATEGORY’, and ‘CAUSE.CATEGORY’ are categorical variables providing context (e.g., climate conditions like ‘normal’ or ‘cold’, regions like ‘east north central’, and causes such as ‘severe weather’ or ‘intentional attack’). The numerical variables include ‘MONTH’ and ‘YEAR’ for timing, ‘ANOMALY.LEVEL’ (ranging from -1.6 to 2.3 to indicate the degree and direction of deviation from the norm), ‘OUTAGE.DURATION’ (minutes per outage), ‘DEMAND.LOSS.MW’ (megawatts lost), and ‘CUSTOMERS.AFFECTED’ (number of customers impacted).

This selection of columns, as derived from the dataset and the attached screenshot, will guide our investigation into understanding and predicting the extent of customer impact during power outages