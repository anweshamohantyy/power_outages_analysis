# Power Outage Analysis


Large-scale power outages can disrupt essential services such as healthcare, transportation, communication networks, and businesses. When outages affect large populations, the consequences can extend beyond temporary inconvenience and lead to significant economic and societal impacts. Understanding the factors that influence outages is therefore important for improving the reliability and resilience of power infrastructure.

In this project, I analyze a dataset containing information about **major power outages in the continental United States between 2000 and 2016**. The dataset was compiled by researchers at **Purdue University and the University of Illinois** and is publicly available through the study *“Major Power Outage Events in the Continental U.S.”* by **Mukherjee et al. (2018)**. It contains over **1,500 outage events** and more than **50 variables** describing characteristics of each outage, including information about outage duration, the number of customers affected, the cause of the outage, and environmental or regional conditions surrounding the event.

Each observation represents a single outage event. Using this dataset, I investigate whether characteristics of an outage can be used to **predict the duration of the outage**, specifically whether an outage will be **long or short**. In addition to prediction, I explore whether outages caused by **natural events such as severe weather tend to last longer than outages caused by infrastructure-related failures**, which may reflect differences in the complexity of recovery efforts.



#### Relevant Data

Although the dataset contains many variables, only a subset are relevant for predicting outage duration and exploring patterns in outage causes.

The primary variable of interest is **`OUTAGE.DURATION`**, which measures the total duration of the outage in minutes. For the prediction task, I convert this variable into a binary outcome indicating whether an outage is **long** or **short**.

Several variables are used as predictors in the baseline model:

- **`CAUSE.CATEGORY`** – identifies the reported cause of the outage (such as severe weather, equipment failure, or other causes). This variable is particularly relevant when comparing outages caused by natural events versus infrastructure-related failures.
- **`MONTH`** – indicates when the outage occurred and may capture seasonal patterns, such as increased outages during extreme weather months.
- **`YEAR`** – provides temporal context and may reflect long-term trends in outage frequency or grid reliability.
- **`OUTAGE_START_DATE`** - 
- **`OUTAGE_START_TIME`** -
- **`OUTAGE_RESTORATION_DATE`** -

In addition to these predictors, several other variables provide useful context about outage severity and may be explored during the analysis:
- **`CLIMATE.CATEGORY`** – categorizes the type of climate conditions associated with the outage event.
- **`CLIMATE.REGION`** – identifies the broader climate region in which the outage occurred.
- **`CUSTOMERS.AFFECTED`** – the number of customers impacted by the outage.
- **`DEMAND.LOSS.MW`** – the estimated loss of electricity demand during the outage in megawatts.
- **`U.S._STATE`** – the state in which the outage occurred.


Together, these variables provide information about the **cause, timing, scale, and environmental context of outage events**, which may help explain differences in outage duration and improve the ability to predict whether an outage will be long or short.
