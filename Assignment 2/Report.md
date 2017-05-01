## Introduction:

Insurance data set has a total of 8,000 observations, and 23 variables (namely: AGE, BLUEBOOK, CAR_AGE, CAR_TYPE, CAR_USE, CLM_FREQ, EDUCATION, HOMEKIDS, HOME_VAL, INCOME, JOB, KIDSDRIV, MSTATUS, MVR_PTS, OLDCLAIM, PARENT1, RED_CAR,
REVOKED, SEX, TIF, TRAVTIME, URBANICITY, and YOJ). Each of these variables has either a positive or a negative theoretical effect on the insurance premium, risk factor, subsequent pay outs, and overall profitability of the insurance business. The attached data dictionary elaborates in detail the specifics of the variables’ impact on the models below and their respective analysis.

We divided this data study and logistic model building project into four major segments: understanding the data, preparing the data, building predictive models, and deploying predictive models. In our previous assignment we build regression models, we attempt to take it a step further and build logistic models and compare it to the regression models. In order to build better predictive models, we need to learn the attributes of the data set first. We performed an initial Exploratory Data Analysis (EDA) to understand the data. Upon learning about the overall structure of the data, and identifying major issues to tackle the problem in hand, we prepared the data by eliminating the “bad” data, weeding out the irrelevant data, cleaning up the structure of the data, addressing missing data entries, understanding the correlation of the variables, checking for multicollinearity, devising a plan to build the model, and deliberately selecting variables (and variable selection method), among other preparatory activities, to ensure an accurate model built.

After understanding the layout of the data set and completing the preparatory exercises, we were ready to build and test models. We built different models and tested them for accuracy and fit. We went through an iterative process of selecting different variables, and testing them against different models to evaluate the fit. Ultimately, we were able to decide on the “best” model based on the AIC, Log Likelihood, and ROC value among other statistical indicators, produced by the respective models. The best fitting model was then deployed in the test data set to predict the crash probability and respective insurance amount.

## Understanding the Data:

In order to understand the attributes of the data set, we ran a “Proc contents” analysis is SAS. We were able to learn that 13 out of 23 data sets were numerical in nature, and rests were character in nature. Upon reviewing the data dictionary, we also learned that 2 out of 17 variables, PARENT1 and URBANICITY, have either an unknown or no effect on the data set and subsequent prediction of the probability of collusion. Therefore, we will drop the two data sets for further analysis during the preparation stage. Figure 1 below shows a part of the Proc Contents SAS output.


![image](https://cloud.githubusercontent.com/assets/26909910/25582712/a70f4ef0-2e5c-11e7-89b2-16c17f4a5668.png)
Figure 1: A “Birds-eye” view of all variables

In order to further understand the structure of the data and fields within the character variables, we ran a “Proc Freq” analysis on every single character variable. We identified basic frequency related measures such as Frequency, Percentage, Cumulative frequency, and Cumulative percentage for each of the 10 character variables. Figure 2 (Part A and Part B) below shows the result of the Proc Freq analysis for each of the 10 character variables.

![image](https://cloud.githubusercontent.com/assets/26909910/25582728/b5ed3fcc-2e5c-11e7-8485-946656ad74c6.png)
![image](https://cloud.githubusercontent.com/assets/26909910/25582735/b939cf2e-2e5c-11e7-98dc-c90b4fdbb338.png)

Figure 2: Proc FREQ procedure (Part A)

![image](https://cloud.githubusercontent.com/assets/26909910/25582750/c35e8490-2e5c-11e7-8eab-63a091f97aab.png)

Figure 2: Proc FREQ procedure (Part B)
