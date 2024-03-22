# The Analysis and Modelling of Power Outages
### By Sebastian Leng and Cosun Zhou

## Introduction
The dataset our project is centered around is entitled "Major Power Outage Risks in the U.S." and is sourced from The Laboratory for Advancing Sustainable
Critical Intrastructure at Purdue University. 

The dataset contains information on every major power outage in the United States between the years of 2000 and 2016. In total, the dataset contains 1534 observations
and 55 features containing information about each power outage.

The question we explored was whether states with greater levels of economic activity would most likely have a higher frequency of power outage.
We hypothesized that states with a greater amount of economic activity would have a more power outages, as we reasoned more economic activity
would put more strain on a state's electric grid.

The three columns we selected for severity/frequency of power outages: 'OUTAGE.DURATION,' 'DEMAND.LOSS.MW,' 'CUSTOMERS.AFFECTED'
'OUTAGE.DURATION' contains the duration, in minutes, of the power outage.
'DEMAND.LOSS.MW' contains the amount of peak demand lost during an outage event, measured in Megawatts.
'CUSTOMERS.AFFECTED' contains the total customers affected by the outage.
The three columns we selected for measuring the prosperity of a state: 'TOTAL.SALES,' 'TOTAL.CUSTOMERS,' 'PC.REALGSP.STATE'
'TOTAL.SALES' contains the total electricity consumption in the U.S. state, measured in megawatts per hour.
'TOTAL.CUSTOMERS' contains the total amount of customers served in the state per year.
'PC.REALGSP.STATE' contains the per capita real gross state product (GSP) in the U.S. state, measured in 2009 dollars.
We also included the 'POPULATION' column just in case.

## Data Cleaning
"The dataset is rigorously preprocessed and checked for inconsistencies to minimize the measurement errors leveraging different methods such as data visualization, analyzing the descriptive statistics as well as manual cross-checking of the observations."
this indicates a rather straightfoward process of cleaning the data as the breadth of features along with the precision of collection allows us to readily utilize the data for further analysis
1. Dropped the first few rows of the dataset that contained empty data and renamed the columns with the column names stored in the 5th row.
2. Combined the columns 'OUTAGE.START.DATE' and 'OUTAGE.START.TIME' into a single column named 'OUTAGE.START' of Pandas Timestamp objects. Simillar procedure for 'OUTAGE.RESTORATION.DATE' and 'OUTAGE.RESTORATION.TIME' into 'OUTAGE.RESTORATION'. Former columns were dropped and later columns were added. NaN values in the original colums are now NaT values

## Exploratory Data Analysis

