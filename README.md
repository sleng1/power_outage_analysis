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

<iframe src="assets/group.html" width="800" height="600" frameborder="0" ></iframe>
This grouped table shows the average outage duration, total electricity consumption, and total gross state product per US state. This helps us visualize the relationship between these three variables.\

<iframe src="assets/duration_year.html" width="800" height="600"></iframe>
This plot explores the relationship between year and the duration of a power outage. As you can see, power outages seem to have gottten longer as the years have progressed.\

<iframe src="assets/sales.html" width="800" height="600"></iframe>
This plot looks at the distribution of total sales in a given state  in a given year. There seems to be a bimodal distribution, with local means just below 10 million and 20 million. However, it seems the majority of states consumed less that 15 million megawatt-hours of electricity, with notable outliers past 40 million.

## Assessment of Missingness

Considering the data generation process and the methodology outlined by the authors of this dataset, we have rather simplified process of analyzing missingness.\
"Note: “NA” in the data file indicates that data was not available."\
This can allow us to conclude that from the perspective of collecting the data from the prescribed sources in the article, all missing data can be listed as MCAR due to the fact that the authors were not able to find the data from publicly available sources.\
From the perspective of why the data was missing in the first place, the reasoning for many featurs remains trivial. For features like 'MONTH,' there is no reasonable justification for the missingness of this value to be conditioned on any other column or inherent to itself.\
There are some columns that do have differing missingness.\
'CLIMATE.REGION' missingness seems to be conditioned on certain values in the 'U.S._STATE' columns, where only hawaii and alaska have missing climate region values
Seems like the missingness of "MONTH", "ANOMALY.LEVEL", and "CLIMATE.CATEGORY" seem to be missing at the same time.\
'HURRICANE.NAMES' is missing at random conditioned on the cause column. If the cause of outage was not a hurricane, then there is no hurricane name.
The rest of the statistics are all MCAR. It would be rather bold to infer the missingness of such columns being attributed to more discreet and nefarious motivations, such as energy companies refusing to disclose prices during times of major outage; we will not do such a thing..
### Analysis of non trivial missingness
The column we have selected for this analyis is titled: 'CUSTOMERS.AFFECTED'\
We first test to see if the missingness of this column is missing at random conditioned on 'COM.PRICE'
We conduct a permutation test:\
**null hypothesis:** The average COM.PRICE for incidents where CUSTOMERS.AFFECTED were recorded and incidents where CUSOMTERS.AFFECTED were missing are the same\
**alternate hypothesis:** The average COM.PRICE for incidents where CUSTOMERS.AFFECTED were recorded and incidents where CUSTOMERS.AFFECTED were missing are different\
Our test statistic is absolute difference in group means/K-S statistic and our significance level is 0.05\
quick note: CUSTOMERS.AFFECTED is highly MAR on DEMAND.LOSS.MW. can use just in case\
<iframe src="assets/missingness.html" width="800" height="600"></iframe>
<iframe src="assets/missingness2.html" width="800" height="600"></iframe>
Our significance level is around 0.31. We fail to reject the null hypothesis. There is not enough evidence in support for the column CUSTOMERS.AFFECTED is conditioned on COM.PRICE.\
Next, we test if 'CUSTOMERS.AFFECTED' is missing at random conditioned on 'OUTAGE.DURATION'\
We conduct a permutation test:\
**null hypothesis:** The average OUTAGE.DURATION for incidents where CUSTOMERS.AFFECTED were recorded and incidents where CUSOMTERS.AFFECTED were missing are the same.\
**alternate hypothesis:** The average OUTAGE.DURATION for incidents where CUSTOMERS.AFFECTED were recorded and incidents where CUSTOMERS.AFFECTED were missing are different.\
Our test statistic is absolute difference in group means and our significance level is 0.05\
<iframe src="assets/missingness3.html" width="800" height="600"></iframe>
Our significance level is around 0.02. We reject the null hypothesis. There is evidence in support for the column CUSTOMERS.AFFECTED is MAR conditioned on OUTAGE.DURATION.\
This concludes our analysis of missingness of CUSTOMERS.AFFECTED on two different columns where it is dependent on one and not dependent on the other

## Hypothesis Testing

### PERMUTATION TEST
The central focus of this project has a very economic flavor to it; elaborating on the question at hand, our analysis revolved around the simple relationship between prosperity of a state and severity/frequency of power outages, however these features may be quantified. From inception, we always hypothesized that states with greater levels of economic activity would most likely have a higher frequency of power outages. We performed a permutation test to measure this hypothesis.\
Our first step was to aggregate the data by state and perform the necessary operations to extract out the relevant features.\
We first grouped the data by 'U.S._STATE" and selected 3 columns per variable that well-quantified their respective measurements.\
The three columns we selected for severity/frequency of power outages: 'OUTAGE.DURATION,' 'DEMAND.LOSS.MW,' 'CUSTOMERS.AFFECTED'\
For 'OUTAGE.DURATION,' we aggregated this measurement across all incidents in each unique state by mean. This includes incidents from all years between 2000 - 2016.\
For 'DEMAND.LOSS.MW,' we aggregated this measurment in a way that requires some clarification. We did not actually use the values of this column in any way and we simply used this column as a placeholder to add together the total number of power outages within each state between the years 2000 - 2016. We did this because of the numerous missing values in the dataset and also because of arbitrary choice (we could have picked any other column).\
for 'CUSTOMERS.AFFECTED,' we used the same methodology as for 'OUTAGE.DURATION.'\
The three columns we selected for measuring the prosperity of a state: 'TOTAL.SALES,' 'TOTAL.CUSTOMERS,' 'PC.REALGSP.STATE'\
For 'TOTAL.SALES' and 'TOTAL.CUSTOMERS,' we simply used a mean aggregation like before.\
For 'PC.REALGSP.STATE,' we also used a mean aggregation. Our reasoning for this aggregate measure was simply for simplicity, but we are aware of the fact that this may not be the best method for encapsulating the per capita real gsp for a state especially over such a long duration of time. Our testing is predicated on the idea that over the 16 year period in which the data was collected, states reletively remained in order in terms of GSP from the beginning of the collection stage. Perhaps this artifact can be counteracted by the idea that number of incidents would increase proportionally, but this remains undecided.\
We also included the 'POPULATION' column just in case.\
After our aggregation of the relevant data, we extracted out columns in which we decided could best represent a test of our hypothesis. The columns were stored in grouped_relevant and the descriptions are all included below.\
The most important transformation we made to a column is contained in this step. For the 'PC.REALGSP.STATE' column we used the pd.qcut method to split the data into 4 equal quartiles each labeled in ascending order of upper-bounds: 25, 50, 75, 100. 25 would indicated <25th percentile, 50 would indicate between 25th and 50th percentile, and 100 would represent the upper eschelon of GSP (states with the highest prosperity).\
After all of these transformations, we conducted a test of our hypothesis:\
Null hypothesis: Among states in the upper and lower quartiles of averaged per-capital GSP between the years 2000 and 2016, there exists no difference between the total number of power outages.\
Alternative hypothesis: Comparing the states in the upper and lower quartiles of prosperity (quantified above) within the 16 year period, states in the top 25th percentile of per-capita GSP have a greater number of total power outages than states in the bottom 25th percentile of per-capita GSP.\
We frame this using a permutation test. We use the test statistic of difference in group means. p-value of 0.05\
we first compute the test statistic by grouping by quartiles in 'PC.REALGSP.STATE.' We then select the column 'DEMAND.LOSS.MW' which contains the total number of power outages throughout the years. We then find the mean of the average number of outages and then locate the groups '25' and '100,' indicating the top groups. Find the test statistic is done by subtracted the average of the 100 group by the 25 group.\
We run this simulation 10000 times and shuffle the 'PC.REALGSP.STATE' column using np.random.permutation. We compute the simulated statistic in each iteration and then store them all in an array.\
We find the p-value by counting the number of simulated statistics as extreme or even more extreme than our test statistics and we divide by 10000.

### Analysis of hypothesis test
Our resulting p value is roughly 0.039 after lots of testing. As such, we reject the null hypothesis. Our result is statistically significant.\
The significance level of this test plays a role in our determining of the question we have for our data but there are still other categories needed to be tested in order for us to have a full conclusion. From the significant of this result, we have some stronger evidence for a positive correlation between the variables in question\
Analyzing the methodology, we followed a standard protocol for permutation tests and we did not re-invent the wheel in anyway shape or form. Some potential issues with our procedure could possibly arise from our aggregation techniques and our omission of states in the middle half of the GSP.

## Framing a Prediction Problem
We decided to see if we can predict the cause of a power outage given some of its features. To do this, we will build a multiclass classification model with a response variable CAUSE.CATEGORY, because this is the variable that reports the reason why an outage was started. We will use f1 score as our model's evaluation metric instead of accuracy. This is because most of the causes in the dataset are severe weather (about 2/3). So if our model were to preedict 'severe weather' for every prediction, our model would have an accuracy of over 66%, which seems high but is actually misleading. Since it is a multiclass classifier, we will use a weighted average of the f1 scores for each category.

## Baseline Model

Our model will attempt to classify the cause of a power outage based on its features. Our baseline model will use one-hot encoded versions of the outage region and climate category, as well as scaled versions of number of customers affected and percent of population living in urban areas. We decided to start with a gradient boosted ensemble for our baseline model.\
We dropped the rows with empty features that we are using, as imputation does not really make sense.\
In total, we used two nominal features (region and climate category) and two quantitative columns (percent of state living in urban areas and number of customers affected. There were no ordinal features.\
We performed standard scaling on the two quantitative features to make the coefficients more interpretable.\
We performed one-hot encoding on the two categorical nominal features\
Our baseline model attaind an f1 score of 0.802. Our baseline model seems to do well identifying the most common outage causes, but struggles to correctly predict the less common ones. For example, it seems to commonly mistake system operability disruption caused power outages with those caused by sever weather.

## Final Model

We decided to engineer a feature called START.TIME.DAY, which classifies the time an outage starts as Morning, Midday, Afternoon, Evening, or Night. This feature was
useful because certain types of attack may bne more likely to occur during certain times of the day (e.g. intentional attacks may be more likely to happen at night).\
According to the National Centers of Environmental Information, anomaly levels of > 0.5 are indicative of El Nino conditions and anomaly levels of < -0.5 are indivative of La Nina conditions. Our next engineered feature classifies an outage as occurring during an El Nino season, a La Nina season, or a season with neither.
This helped our model because outages that occurred in areas with a present El Nino or La Nina are probably more likely to have been started by a weather-related event, as both of these patterns tend to lead to more frequent instances of severe eather.\
Finally, we wanted to use the duration of a power outage as a feature, but duration itself was too clustered at the left, as most power outages last only a very short amount of time. So we engineered a new feature to use in our final model, which is the log of outage duration. Using the log of duration instead of duration itself allowed us to capture a more linear trend of the feature, avoiding clustering at the low end.\
After trying out multiple classifier models, we found gradient boosted ensembles still work the best. We performed a GridSearchCV on the hyperparameters max_depth (maximum depth of the tree) and min_samples_split (minimum datapoints required to split a node). However, we found that the optimal values for these two hypterparameters were the default ones, so our score did not improve.\
We were able to improve our model's f1 score to 0.8606 (over 6% increase from baseline). Our model got better at prediction categories with fewer data.\
Below is a confusion matrix for our final model:\
<iframe src="assets/cm.html" width="800" height="600"></iframe>

## Fairness Analysis

For our fairness analysis, we will test to see whether our model performs the same for outages in large and small states. We will consider a state large if its population its population is 10 million or greater, and small if its population is less than 10 million.\
**Null hypothesis:** our model performs the same for outages in large and small states.\
**Alternative hypothesis:** our model is performs differently for large and small states.\
**Test statistic:** the absolute difference between the f1 score for large states and the f1 score for small states.\
As seen above, the observed test statistic is just over 0.02.\
We obtained a p-value of 0.063, meaning there is a 6.3% chance that the differnce between our models score in large and small stats is greater than or equal to what we observed. We can visualize this in the histogram below.\
<iframe src="assets/fairness.html" width="800" height="600"></iframe>
As 0.063 > 0.05, we fail to reject the null at the 5% significance level.
