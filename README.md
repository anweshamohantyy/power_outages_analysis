# Power Outage Analysis

#### By: Anwesha Mohanty

## Introduction

Power outages can disrupt businesses and critical services such as healthcare, transportation, and communication. Since outages in the U.S. can affect large populations, the consequences can go beyond temporary inconvenience and lead to significant economic and societal impacts. Therefore, it is important to understand the various factors that affect outages in order to improve the reliability of existing infrastructure that is used to combat outages.

In this project, I analyze a dataset that has information about **major power outages in the United States between January 2000 and July 2016**. The dataset was created by researchers at **Purdue University and the University of Illinois** containing **1534 rows**, which each correspond to an outage occurence and more than **50 variables** describing characteristics of each outage, such as outage duration, the cause of the outage, how many people were affected, environmental conditions, and more. Each observation represents a single outage event.

 Using this dataset, I analyze the following question: **What characteristics of an outage can be used to predict the duration of the outage?**. Specifically, I explore whether outages caused by **natural events such as severe weather tend to last longer than outages caused by infrastructure-related failures**. I also investigate if characteristics of an outage can predict whether an outage will be **long or short**. Identifying these patterns is vital for improving outage response strategies, minimizing downtime, and strengthening systems against future disruptions.

#### Relevant Data

The primary variable of interest is **`OUTAGE.DURATION`**, which measures the total duration of the outage in minutes. For the prediction task, I convert this variable into a binary outcome indicating whether an outage is **long** or **short**.

The following variables may also be of interest in my analysis: 

- **`CAUSE_CATEGORY`** – the reported cause of the outage (i.e. severe weather, equipment failure, etc). This variable is particularly relevant when comparing outages caused by natural events versus infrastructure-related failures.
- **`MONTH`** – month in which the outage occured; may capture seasonal patterns, such as increased outages during extreme weather months.
- **`OUTAGE_START_DATE`** - the day the outage started.
- **`OUTAGE_START_TIME`** - the time at which the outage started.
- **`OUTAGE_RESTORATION_DATE`** - the day the power was restored.
- **`OUTAGE_RESTORATION_TIME`** - the time the power was restored.
- **`CLIMATE_CATEGORY`** – the type of climate conditions associated with the outage event.
- **`CLIMATE_REGION`** – the broader climate region in which the outage occurred.
- **`CUSTOMERS.AFFECTED`** – the number of customers impacted by the outage.
- **`DEMAND_LOSS_MW`** – the estimated loss of electricity demand during the outage in megawatts.
- **`U.S._STATE`** – the state in which the outage occurred.


Collectively, these variables provide information about the context of outage events, which can potentially explain differences in outage duration.

## Data Cleaning and Exploratory Data Analysis

### Data Cleaning

Before performing exploratory analysis, I cleaned and prepared the dataset so it could be used reliably for analysis. 

First, I loaded the dataset using the correct row as the header. 

Next, I converted several columns that should contain numeric values, including variables such as year, month,outage duration, demand loss, customers affected, and population, into numeric data types. As a result, these variables can be used correctly in calculations and statistical analysis.

I then removed rows where `OUTAGE.DURATION` is missing, since outage duration is the primary variable of interest throughout this project. To narrow the scope of the project, I dropped several columns that are either irrelevant to the research question or contain little useful information. I also removed an additional non-data row that remained in the dataset and reset the index to maintain a clean and consistent structure.

After initial cleaning, I created a binary variable for the prediction problem called `DURATION_CLASS`. Outages with durations greater than the median are labeled as **long**, while those less than or equal to the median are labeled as **short**. This variable is key in the prediction model.

I also created a variable called `CAUSE_TYPE` for use in hypothesis testing. This variable classifies outages into broader categories: **natural** (for severe weather), **infrastructure** (for equipment failure and system operability disruption), and **other** for all remaining causes. This classification allows me to analyze how outages of each of the cause types differ.

Finally, I replaced common placeholder values such as `"NA"` and empty strings with `NaN` to handle missing data. I also standardized the column names by replacing extra spaces and periods with underscores, and converting all names to uppercase, so I can easily access them throughout the project.

Below are the first few rows of the cleaned dataset:

<div style="overflow-x:auto;">
<table border="1" class="dataframe table table-striped">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>YEAR</th>
      <th>MONTH</th>
      <th>U_S__STATE</th>
      <th>POSTAL_CODE</th>
      <th>NERC_REGION</th>
      <th>CLIMATE_REGION</th>
      <th>ANOMALY_LEVEL</th>
      <th>CLIMATE_CATEGORY</th>
      <th>OUTAGE_START_DATE</th>
      <th>OUTAGE_START_TIME</th>
      <th>OUTAGE_RESTORATION_DATE</th>
      <th>OUTAGE_RESTORATION_TIME</th>
      <th>CAUSE_CATEGORY</th>
      <th>CAUSE_CATEGORY_DETAIL</th>
      <th>OUTAGE_DURATION</th>
      <th>DEMAND_LOSS_MW</th>
      <th>CUSTOMERS_AFFECTED</th>
      <th>RES_PRICE</th>
      <th>COM_PRICE</th>
      <th>IND_PRICE</th>
      <th>TOTAL_PRICE</th>
      <th>RES_SALES</th>
      <th>COM_SALES</th>
      <th>IND_SALES</th>
      <th>TOTAL_SALES</th>
      <th>RES_PERCEN</th>
      <th>COM_PERCEN</th>
      <th>IND_PERCEN</th>
      <th>RES_CUSTOMERS</th>
      <th>COM_CUSTOMERS</th>
      <th>IND_CUSTOMERS</th>
      <th>TOTAL_CUSTOMERS</th>
      <th>RES_CUST_PCT</th>
      <th>COM_CUST_PCT</th>
      <th>IND_CUST_PCT</th>
      <th>UTIL_CONTRI</th>
      <th>PI_UTIL_OFUSA</th>
      <th>POPULATION</th>
      <th>POPPCT_URBAN</th>
      <th>POPPCT_UC</th>
      <th>POPDEN_URBAN</th>
      <th>POPDEN_UC</th>
      <th>POPDEN_RURAL</th>
      <th>AREAPCT_URBAN</th>
      <th>AREAPCT_UC</th>
      <th>PCT_LAND</th>
      <th>PCT_WATER_TOT</th>
      <th>PCT_WATER_INLAND</th>
      <th>DURATION_CLASS</th>
      <th>CAUSE_TYPE</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2011.0</td>
      <td>7.0</td>
      <td>Minnesota</td>
      <td>MN</td>
      <td>MRO</td>
      <td>East North Central</td>
      <td>-0.3</td>
      <td>normal</td>
      <td>2011-07-01</td>
      <td>5:00:00 PM</td>
      <td>2011-07-03</td>
      <td>8:00:00 PM</td>
      <td>severe weather</td>
      <td>NaN</td>
      <td>3060.0</td>
      <td>NaN</td>
      <td>70000.0</td>
      <td>11.6</td>
      <td>9.18</td>
      <td>6.81</td>
      <td>9.28</td>
      <td>2332915</td>
      <td>2114774</td>
      <td>2113291</td>
      <td>6562520</td>
      <td>35.54907261</td>
      <td>32.22502941</td>
      <td>32.20243138</td>
      <td>2.31e+06</td>
      <td>276286.0</td>
      <td>10673.0</td>
      <td>2.60e+06</td>
      <td>88.9448</td>
      <td>10.6440</td>
      <td>0.4112</td>
      <td>1.751391412</td>
      <td>2.2</td>
      <td>5.35e+06</td>
      <td>73.27</td>
      <td>15.28</td>
      <td>2279</td>
      <td>1700.5</td>
      <td>18.2</td>
      <td>2.14</td>
      <td>0.6</td>
      <td>91.59266587</td>
      <td>8.407334131</td>
      <td>5.478742983</td>
      <td>long</td>
      <td>natural</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2014.0</td>
      <td>5.0</td>
      <td>Minnesota</td>
      <td>MN</td>
      <td>MRO</td>
      <td>East North Central</td>
      <td>-0.1</td>
      <td>normal</td>
      <td>2014-05-11</td>
      <td>6:38:00 PM</td>
      <td>2014-05-11</td>
      <td>6:39:00 PM</td>
      <td>intentional attack</td>
      <td>vandalism</td>
      <td>1.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>12.12</td>
      <td>9.71</td>
      <td>6.49</td>
      <td>9.28</td>
      <td>1586986</td>
      <td>1807756</td>
      <td>1887927</td>
      <td>5284231</td>
      <td>30.03248722</td>
      <td>34.21038936</td>
      <td>35.72756376</td>
      <td>2.35e+06</td>
      <td>284978.0</td>
      <td>9898.0</td>
      <td>2.64e+06</td>
      <td>88.8335</td>
      <td>10.7916</td>
      <td>0.3748</td>
      <td>1.790001884</td>
      <td>2.2</td>
      <td>5.46e+06</td>
      <td>73.27</td>
      <td>15.28</td>
      <td>2279</td>
      <td>1700.5</td>
      <td>18.2</td>
      <td>2.14</td>
      <td>0.6</td>
      <td>91.59266587</td>
      <td>8.407334131</td>
      <td>5.478742983</td>
      <td>short</td>
      <td>other</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2010.0</td>
      <td>10.0</td>
      <td>Minnesota</td>
      <td>MN</td>
      <td>MRO</td>
      <td>East North Central</td>
      <td>-1.5</td>
      <td>cold</td>
      <td>2010-10-26</td>
      <td>8:00:00 PM</td>
      <td>2010-10-28</td>
      <td>10:00:00 PM</td>
      <td>severe weather</td>
      <td>heavy wind</td>
      <td>3000.0</td>
      <td>NaN</td>
      <td>70000.0</td>
      <td>10.87</td>
      <td>8.19</td>
      <td>6.07</td>
      <td>8.15</td>
      <td>1467293</td>
      <td>1801683</td>
      <td>1951295</td>
      <td>5222116</td>
      <td>28.09767152</td>
      <td>34.50101453</td>
      <td>37.36598344</td>
      <td>2.30e+06</td>
      <td>276463.0</td>
      <td>10150.0</td>
      <td>2.59e+06</td>
      <td>88.9206</td>
      <td>10.6870</td>
      <td>0.3924</td>
      <td>1.706265514</td>
      <td>2.1</td>
      <td>5.31e+06</td>
      <td>73.27</td>
      <td>15.28</td>
      <td>2279</td>
      <td>1700.5</td>
      <td>18.2</td>
      <td>2.14</td>
      <td>0.6</td>
      <td>91.59266587</td>
      <td>8.407334131</td>
      <td>5.478742983</td>
      <td>long</td>
      <td>natural</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2012.0</td>
      <td>6.0</td>
      <td>Minnesota</td>
      <td>MN</td>
      <td>MRO</td>
      <td>East North Central</td>
      <td>-0.1</td>
      <td>normal</td>
      <td>2012-06-19</td>
      <td>4:30:00 AM</td>
      <td>2012-06-20</td>
      <td>11:00:00 PM</td>
      <td>severe weather</td>
      <td>thunderstorm</td>
      <td>2550.0</td>
      <td>NaN</td>
      <td>68200.0</td>
      <td>11.79</td>
      <td>9.25</td>
      <td>6.71</td>
      <td>9.19</td>
      <td>1851519</td>
      <td>1941174</td>
      <td>1993026</td>
      <td>5787064</td>
      <td>31.99409925</td>
      <td>33.54333043</td>
      <td>34.43932882</td>
      <td>2.32e+06</td>
      <td>278466.0</td>
      <td>11010.0</td>
      <td>2.61e+06</td>
      <td>88.8954</td>
      <td>10.6822</td>
      <td>0.4224</td>
      <td>1.932088738</td>
      <td>2.2</td>
      <td>5.38e+06</td>
      <td>73.27</td>
      <td>15.28</td>
      <td>2279</td>
      <td>1700.5</td>
      <td>18.2</td>
      <td>2.14</td>
      <td>0.6</td>
      <td>91.59266587</td>
      <td>8.407334131</td>
      <td>5.478742983</td>
      <td>long</td>
      <td>natural</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2015.0</td>
      <td>7.0</td>
      <td>Minnesota</td>
      <td>MN</td>
      <td>MRO</td>
      <td>East North Central</td>
      <td>1.2</td>
      <td>warm</td>
      <td>2015-07-18</td>
      <td>2:00:00 AM</td>
      <td>2015-07-19</td>
      <td>7:00:00 AM</td>
      <td>severe weather</td>
      <td>NaN</td>
      <td>1740.0</td>
      <td>250.0</td>
      <td>250000.0</td>
      <td>13.07</td>
      <td>10.16</td>
      <td>7.74</td>
      <td>10.43</td>
      <td>2028875</td>
      <td>2161612</td>
      <td>1777937</td>
      <td>5970339</td>
      <td>33.9825762</td>
      <td>36.20585029</td>
      <td>29.77949828</td>
      <td>2.37e+06</td>
      <td>289044.0</td>
      <td>9812.0</td>
      <td>2.67e+06</td>
      <td>88.8216</td>
      <td>10.8113</td>
      <td>0.3670</td>
      <td>1.668704177</td>
      <td>2.2</td>
      <td>5.49e+06</td>
      <td>73.27</td>
      <td>15.28</td>
      <td>2279</td>
      <td>1700.5</td>
      <td>18.2</td>
      <td>2.14</td>
      <td>0.6</td>
      <td>91.59266587</td>
      <td>8.407334131</td>
      <td>5.478742983</td>
      <td>long</td>
      <td>natural</td>
    </tr>
  </tbody>
</table>
</div>
### Univariate Analysis

To begin exploring the dataset, I perform univariate analysis on several key variables. 

**Distribution of Outage Duration**

The following histogram shows the distribution of outage durations. 

<iframe
  src="assets/outage_dist_hist.html"
  width="800"
  height="400"
  frameborder="0"
></iframe>

The distribution of outage duration is extremely right-skewed as most outages last for a short amount of time from 0 to 20k minutes. A small number of outages last extremely long durations, with the highest being slightly over 100k. Therefore, there are significantly large outliers, while the majority is short.

**Frequency of Outage Causes**

Next, I examine the frequency of different outage causes using `CAUSE.CATEGORY`. 

<iframe
  src="assets/outages_per_cause.html"
  width="800"
  height="400"
  frameborder="0"
></iframe>

According to the plot, severe weather accounts for the largest number of outages by a large margin, followed by intentional attacks, while all other categories occur much less frequently. This suggests that natural events are the primary cause of power outages in the dataset.

**Number of Outages per Month**

Finally, I examine how outages are distributed across different months of the year. 

<iframe
  src="assets/month.html"
  width="800"
  height="400"
  frameborder="0"
></iframe>

Outages appear to be more frequent during the summer months, peaking around June and July, while fewer outages occur in the late fall and winter months. This suggests a seasonal pattern, possibly linked to increased severe weather or higher energy demand during warmer periods.

### Bivariate Analysis

After examining individual variables, I explore relationships between pairs of variables to identify potential associations.

**Outage Duration by Cause Category**

To better understand how outage duration varies across different types of disruptions, I examine the relationship between `CAUSE.CATEGORY` and `OUTAGE.DURATION`.

<iframe
  src="assets/outage_cause_boxplot.html"
  width="800"
  height="400"
  frameborder="0"
></iframe>

Outage duration varies significantly by cause category. For example, fuel supply emergencies and severe weather generally have the longest and most variable durations. On the other hand, intentional attacks and islanding tend to result in shorter outages, although all categories show high variability in outliers.

**Outage Duration by Cause Type**

To investigate whether outage duration differs between outages caused by natural events and those caused by infrastructure-related failures, I compare the distribution of outage durations across these two types.

<iframe
  src="assets/types.html"
  width="800"
  height="400"
  frameborder="0"
></iframe>

Outages caused by natural events tend to have longer durations and greater variability compared to infrastructure-related outages. Infrastructure-related outages are typically shorter and more tightly distributed, although both categories contain some extreme outliers.

### Interesting Aggregates

**Median Outage Duration by Month and Cause Type**

To examine how outages vary throughout the year, I created a pivot table showing the median outage duration for each `CAUSE_TYPE` across different months. I was able to see whether certain outage types tend to last longer during specific months of the year. For instance, outages caused by natural events may have longer durations in months with extreme weather conditions. This pivot table helps highlight seasonal patterns in outage severity.

<div style="overflow-x:auto;">
<table border="1" class="dataframe table table-striped">
  <thead>
    <tr style="text-align: right;">
      <th>CAUSE_TYPE</th>
      <th>infrastructure</th>
      <th>natural</th>
      <th>other</th>
    </tr>
    <tr>
      <th>MONTH</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1.0</th>
      <td>372.0</td>
      <td>1950.0</td>
      <td>90.0</td>
    </tr>
    <tr>
      <th>2.0</th>
      <td>215.0</td>
      <td>2656.5</td>
      <td>108.0</td>
    </tr>
    <tr>
      <th>3.0</th>
      <td>105.0</td>
      <td>2370.0</td>
      <td>137.5</td>
    </tr>
    <tr>
      <th>4.0</th>
      <td>130.0</td>
      <td>2620.0</td>
      <td>118.0</td>
    </tr>
    <tr>
      <th>5.0</th>
      <td>327.5</td>
      <td>2515.0</td>
      <td>71.5</td>
    </tr>
    <tr>
      <th>6.0</th>
      <td>150.5</td>
      <td>2015.5</td>
      <td>97.0</td>
    </tr>
    <tr>
      <th>7.0</th>
      <td>222.5</td>
      <td>1545.0</td>
      <td>195.0</td>
    </tr>
    <tr>
      <th>8.0</th>
      <td>249.5</td>
      <td>1987.5</td>
      <td>225.0</td>
    </tr>
    <tr>
      <th>9.0</th>
      <td>247.5</td>
      <td>4305.0</td>
      <td>152.5</td>
    </tr>
    <tr>
      <th>10.0</th>
      <td>235.0</td>
      <td>3000.0</td>
      <td>14.0</td>
    </tr>
    <tr>
      <th>11.0</th>
      <td>759.0</td>
      <td>2939.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>12.0</th>
      <td>234.0</td>
      <td>2700.0</td>
      <td>350.0</td>
    </tr>
  </tbody>
</table>
</div>

## Assessment of Missingness

### MNAR Analysis

Out of the columns in the dataset that contain missing values, the OUTAGE_DURATION column may be Missing Not At Random (MNAR). The missingness of outage duration is most likely due to the actual value of the duration itself. For instance, outages with extremely short durations may not be recorded, and very long and large outages may have incomplete records due to ongoing investigations or reporting delays. As a result, it is possible that the probability of missingness depends on the true unobserved outage duration, which makes it MNAR. 
In order to better understand the data and make it Missing at Random (MAR), I would need to access outage reporting practices, data collection protocols, and other characteristics of outages such as demand loss or number of customers affected. This information could potentially help explain why certain durations are missing, and make it MAR. 

### Missingness Dependency 

In this section, I investigate whether the missingness of the `DEMAND_LOSS_MW` column depends on other variables in the dataset. To do this, I create a missingness indicator for `DEMAND_LOSS_MW` and use permutation tests to determine whether the proportion of missing values changes across different groups. I first test whether missingness depends on outage severity (`OUTAGE_DURATION`), and then test whether it depends on `MONTH`.

#### Outage Duration

First, I analyzed whether the distribution of outage duration differs when `DEMAND_LOSS_MW` is missing versus when it is not missing.

**Null Hypothesis:** The distribution of `OUTAGE_DURATION` is the same when `DEMAND_LOSS_MW` is missing vs not missing.

**Alternate Hypothesis:** The distribution of `OUTAGE_DURATION` is different when `DEMAND_LOSS_MW` is missing vs not missing.

<iframe
  src="assets/duration_missing.html"
  width="800"
  height="400"
  frameborder="0"
></iframe>

**Results**

I found an observed **TVD of 0.074** with a **p-value of 0.016**. 

Since the p-value is small, I reject the null hypothesis in favor of the alternate hypothesis. This suggests that the distribution of `OUTAGE_DURATION` differs depending on whether `DEMAND_LOSS_MW` is missing, indicating that the missingness of demand loss likely depends on outage duration.

<iframe
  src="assets/duration_tvd.html"
  width="800"
  height="400"
  frameborder="0"
></iframe>

#### Month

Next, I analyze whether the missingness of `DEMAND_LOSS_MW` depends on the column `MONTH`.

**Null Hypothesis:** The distribution of `MONTH` is the same when `DEMAND_LOSS_MW` is missing vs not missing.

**Alternate Hypothesis:** The distribution of `MONTH` is different when `DEMAND_LOSS_MW` is missing vs not missing.

<iframe
  src="assets/month_missing.html"
  width="800"
  height="400"
  frameborder="0"
></iframe>

**Results**

I found an **observed TVD of 0.111** with a **p-value of 0.009**. 

Since the p-value is quite large, I fail to reject the null hypothesis. This suggests that the distribution of `MONTH` is not significantly different when `DEMAND_LOSS_MW` is missing versus when it is not missing, meaning that the missingness of demand loss does not depend on the month of the outage.

<iframe
  src="assets/month_tvd.html"
  width="800"
  height="400"
  frameborder="0"
></iframe>

## Hypothesis Testing

#### Do outage durations differ depending on the cause type?

I examine whether the average outage duration differs between outages caused by **natural events** and those caused by **infrastructure issues**.

**Null Hypothesis:**  
The average `OUTAGE_DURATION` is the same for natural outages and infrastructure outages.

**Alternative Hypothesis:**  
The average `OUTAGE_DURATION` is different for natural outages and infrastructure outages.

**Test Statistic:** To test this hypothesis, I perform a permutation test using the difference in mean outage duration between the two groups as the test statistic.

The significance level used for this test is **α = 0.05**.

<iframe
  src="assets/hyp_test_fig.html"
  width="800"
  height="400"
  frameborder="0"
></iframe>

**Results**

The observed difference in mean outage duration was **2818.923**, with a **p-value of 0.0**.

Since the p-value is less than the significance level of 0.05, I reject the null hypothesis. Therefore, these results imply that outage durations may differ depending on the cause type. In particular, outages caused by natural events tend to have different average durations than outages caused by infrastructure-related issues.

## Framing a Prediction Problem

The goal of my model is to predict whether a power outage will be **long or short**, based on information available at the time the outage occurs.

This is a **binary classification problem**, where the response variable is `LONG_OUTAGE`, defined as whether `OUTAGE_DURATION` is greater than the median outage duration.

I chose this response variable because it provides a clear measure of outage impact, which aligns with earlier analysis focused on outage duration and impact patterns.

The features used for prediction are `CAUSE_CATEGORY`, `CLIMATE_REGION`, `MONTH`, and `NERC_REGION`. These variables are suitable for this model because they are known at the time of the outage and are relevant to predicting outage impact.

I do not include `OUTAGE_DURATION` as it is the target variable and  `DEMAND_LOSS_MW` because it is typically recorded after the outage has occurred, so including it would result in data leakage.

To evaluate model performance, I use **accuracy** as the primary metric. Accuracy is a suitable metric because it provides a clear measure of how often the model correctly classifies outages as long or short.

## Baseline Model

For my baseline model, I trained a **logistic regression classifier** to predict `DURATION_CLASS`, which labels each outage as either long or short.

The features used in the model are:
- `CAUSE_CATEGORY` (nominal)
- `CLIMATE_REGION` (nominal)
- `NERC_REGION` (nominal)
- `MONTH` (quantitative)

The categorical features were transformed using **one-hot encoding**, while the numerical feature `MONTH` was unchanged. These preprocessing steps were combined with the classifier in a single sklearn `Pipeline`.

I evaluated the model using **accuracy** on a held-out test set because accuracy is appropriate for this binary classification problem when the classes are approximately balanced.

The baseline model achieved an accuracy of **0.79** on test data. Therefore, this suggests that the model is able to successfully identify some relationship between outage characteristics and outage impact, but there is still room for improvement.

## Final Model

For my final model, I use a **RandomForestClassifier** to predict `DURATION_CLASS`.

In addition to the baseline features, I created and added two new features:

- `SEASON`, derived from `MONTH`
- `CAUSE_CLIMATE`, which combines `CAUSE_CATEGORY` and `CLIMATE_REGION`

I added `SEASON` because outage impact may vary across different times of year due to seasonal weather patterns as suggested by my EDA. I added `CAUSE_CLIMATE` because the effect of outage cause may depend on the climate region in which the outage occurs.

For hyperparameter tuning, I use **GridSearchCV** to search over values of `max_depth` and `n_estimators`. I chose these hyperparameters because they directly affect model complexity and how well the random forest can capture patterns without overfitting.

The confusion matrix below illustrates my model's performance:

<img src="confusion_matrix.png" width="600">

The confusion matrix demonstrates that the model accurately predicts 160 long outages and 138 short outages, meaning it correctly classifies most outages. While there are 31 long outages predicted as short and 39 short outages predicted as long, the overall performance of the model is reasonably balanced across both classes.


## Fairness Analysis

To evaluate fairness, I compared model performance across two groups based on **climate region**:

- **Group X:** Northeast  
- **Group Y:** South  

#### Evaluation Metric  
I used **precision for predicting "long" outages** as the evaluation metric, since correctly identifying long outages is vital to understanding severe events.

#### Hypotheses  
**Null Hypothesis (H₀):** The model has equal precision for long outages in both regions. Any observed difference is due to random chance.  
**Alternative Hypothesis (H₁):** The model’s precision for long outages differs between the two regions.

#### Test Statistic  
The test statistic is the **difference in precision** between the two groups:
Northeast - South

#### Significance Level  
I used a significance level of **α = 0.05**.

#### Results  
- **Observed test statistic:** -0.0082  
- **p-value:** 0.953  

#### Conclusion  
Since the p-value of 0.953 is much higher than 0.05, we fail to reject the null hypothesis. There is no sufficient evidence to conclude that the model performs differently across the Northeast and South regions.  

Therefore, the model’s performance is consistent across these climate regions, and any observed differences in precision are likely due to random variation rather than systematic bias.


