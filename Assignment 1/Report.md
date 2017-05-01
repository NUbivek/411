# Introduction:

MONEYBALL data set has a total of 2276 observations, and 17 variables (namely: TEAM_BATTING_H, TEAM_BATTING_2B, TEAM_BATTING_3B, TEAM_BATTING_HR, TEAM_BATTING_BB, TEAM_BATTING_SO, TEAM_BASERUN_SB, TEAM_BASERUN_CS, TEAM_BATTING_HBP,    TEAM_PITCHING_H,    TEAM_PITCHING_HR,    TEAM_PITCHING_BB, TEAM_PITCHING_SO, TEAM_FIELDING_E, and TEAM_FIELDING_DP). Each of these variables has either a positive or a negative impact on the overall wins. The attached data dictionary elaborates in detail the specifics of the variable’s impact on overall wins.

We divided this data study project into four major segments: understanding the data, preparing the data, building predictive models, and deploying predictive models. In order to build better predictive models, we need to learn the attributes of the data set. We performed an initial Exploratory Data Analysis (EDA) to understand the data. Upon learning about the overall structure of the data, and identifying major issues to tackle the problem in hand, we prepared the data by eliminating the “bad” data, weeding out the irrelevant data, cleaning up the structure of the data, addressing missing data entries, understanding the correlation of the variables, checking for multicollinearity, devising a plan to build the model, and deliberately selecting variables (and variable selection method), among other preparatory activities, to ensure an accurate model built.

After understanding the layout of the data set and completing the preparatory exercises, we were ready to build and test models. We built different models and tested them for accuracy and fit. We went through an iterative process of selecting different variables, and testing them against different models to evaluate the fit. Ultimately, we were able to decide on the “best” model based on the R-squared value, and Adjusted R-squared value, among other statistical matrices, produced by the respective models. The best fitting model was then deployed in the test data set to predict the number of target wins.
 
# Understanding the Data:

In order to understand the attributes of the data set, we ran a “Proc contents” analysis is SAS. We were able to learn that 15 out of 17 data sets were numerical in nature, and rests were character in nature. We also learned that 2 out of 17 variables, the same character variables, have majority, if not all, of data fields that are missing and/or cannot be used for further analysis. Therefore, we need to drop the two data sets for further analysis. Figure 1 below shows a part of the Proc Contents SAS output.

![image](https://cloud.githubusercontent.com/assets/26909910/25581951/baa0a7a6-2e58-11e7-9451-15086698feb6.png)

   Figure 1: Variables’ list part of Proc Contents SAS output
 
 We then ran “Proc Univariate” analysis on each of the remaining 15 variables. We identified key statistical measures such as Mean, Median, Mode, Standard Deviation, Variance, Range, and interquartile Range for each of the 15 variables. We also plotted a normal distribution chart, normal quantile chart, and qq plot for each of the variables to visualize the data. Figure 2 below shows an example the proc univariate analysis for one of the 15 selected variables.


![image](https://cloud.githubusercontent.com/assets/26909910/25581981/e21e210a-2e58-11e7-8db2-994f8e9f5524.png)
![image](https://cloud.githubusercontent.com/assets/26909910/25581983/e5c641e8-2e58-11e7-92f2-fa5195b018e2.png)
![image](https://cloud.githubusercontent.com/assets/26909910/25581987/e9e940fe-2e58-11e7-985c-72860f93af8c.png)

   Figure 2: Proc Univariate example – TEAM_BATTING_H (Part A)

![image](https://cloud.githubusercontent.com/assets/26909910/25582094/6c29adc4-2e59-11e7-839f-6b8fcfeb0dcc.png)
![image](https://cloud.githubusercontent.com/assets/26909910/25582097/6ffba06a-2e59-11e7-9df9-9ca3b5441377.png)
![image](https://cloud.githubusercontent.com/assets/26909910/25582101/7294be42-2e59-11e7-938a-b2c88a7dc5d2.png)
