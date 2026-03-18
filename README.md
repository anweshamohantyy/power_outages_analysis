# Power Outage Analysis

#### By: Anwesha Mohanty

## Introduction

Power outages can disrupt businesses and critical services such as healthcare, transportation, and communication. Since outages in the U.S. can affect large populations, the consequences can go beyond temporary inconvenience and lead to significant economic and societal impacts. Therefore, it is important to understand the various factors that affect outages in order to improve the reliability of existing infrastructure that is used to combat outages.

In this project, I analyze a dataset that has information about **major power outages in the United States between January 2000 and July 2016**. The dataset was created by researchers at **Purdue University and the University of Illinois** containing **1534 rows**, which each correspond to an outage occurence and more than **50 variables** describing characteristics of each outage, such as outage duration, the cause of the outage, how many people were affected, environmental conditions, and more. Each observation represents a single outage event.

 Using this dataset, I analyze the following question: **What characteristics of an outage can be used to predict the duration of the outage?**. Specifically, I explore whether outages caused by **natural events such as severe weather tend to last longer than outages caused by infrastructure-related failures**. I also investigate if characteristics of an outage can predict whether an outage will be **long or short**. Identifying these patterns is vital for improving outage response strategies, minimizing downtime, and strengthening systems against future disruptions.

#### Relevant Data

The primary variable of interest is **`OUTAGE.DURATION`**, which measures the total duration of the outage in minutes. For the prediction task, I convert this variable into a binary outcome indicating whether an outage is **long** or **short**.

The following variables may also be of interest in my analysis: 

- **`CAUSE.CATEGORY`** – the reported cause of the outage (i.e. severe weather, equipment failure, etc). This variable is particularly relevant when comparing outages caused by natural events versus infrastructure-related failures.
- **`MONTH`** – month in which the outage occured; may capture seasonal patterns, such as increased outages during extreme weather months.
- **`OUTAGE_START_DATE`** - the day the outage started.
- **`OUTAGE_START_TIME`** - the time at which the outage started.
- **`OUTAGE_RESTORATION_DATE`** - the day the power was restored.
- **`OUTAGE_RESTORATION_TIME`** - the time the power was restored.
- **`CLIMATE.CATEGORY`** – the type of climate conditions associated with the outage event.
- **`CLIMATE.REGION`** – the broader climate region in which the outage occurred.
- **`CUSTOMERS.AFFECTED`** – the number of customers impacted by the outage.
- **`DEMAND.LOSS.MW`** – the estimated loss of electricity demand during the outage in megawatts.
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


