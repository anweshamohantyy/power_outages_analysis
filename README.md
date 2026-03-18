# Power Outage Analysis

#### By: Anwesha Mohanty

### Introduction

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
