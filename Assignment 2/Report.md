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

Proc Freq analysis of the 10 character variables gave us a good idea of the composition of the data set, frequency of each of the sub-fields, and missing numbers on the character variables. However, we need similar information of the numeric variables. We also need to explore the basic statistical measures on all variables, and identify the key mean, median, mode, standard deviation, minimum, lower and upper 95%, and missing values. Therefore, we run a Proc Means procedure to further evaluate the data set.

![image](https://cloud.githubusercontent.com/assets/26909910/25582838/3465ee1c-2e5d-11e7-886d-2d1259caeadd.png)

Figure 3: The Means procedure before preparing data

From the Proc Means procedure in Figure 3, we are able to identify four variables (YOJ, INCOME, HOME_VAL, and CAR_AGE) with a lot of missing entries. We need to address the missing data before relying on these numbers in its entirety. We will address the missing data in the data preparation section, but it is important to note that the mean, median, mode, and standard deviation presented above may change as we address the missing data. In fact, we will runt he Proc Means procedure in the data preparation section again to see how the same Means Procedure produces a totally different result table when we clean the data.

## Preparing the Data:

This is the step where we address all of the issues identified and discussed in the Understanding the data stage. We begin by filling up the missing values. As identified above, there are four variables with missing data -- YOJ, INCOME, HOME_VAL, and CAR_AGE. We start by finding the mean for YOJ, and fill the missing fields with the YOJ mean (45).
Similarly, we identify the mean for all of the remaining variables and fill in the missing fields with their respective means. We use the decision tree (combination of If and else statements) to fill in the missing values.
After addressing the missing values, we address other inconsistencies/outliers in the dataset. Some of the inconsistencies/outliers we identified and treated as shown below:

  1.	Home value of more than $700,000 (We deleted these values)

  2.	Car value of more than $70,000 (We deleted these values)

  3.	Travel time higher than 130 (We deleted these values)

  4.	Income or more than $180,000 were equated to $180,000

  5.	Jobs were categorized into two major groups – Blue Collar and White Collar 
  
As we curated the inconsistencies and outliers as shown above, the missing values were flagged to make sure that we are able to identify the missing values later on.
 
We then run a Proc Means procedure again to check to see if all major evident issues were addressed. Figure 4 below shows the Mean procedure with no missing values.

![image](https://cloud.githubusercontent.com/assets/26909910/25582903/8661666a-2e5d-11e7-8752-f131ddc5ab2b.png)

Figure 4: The Means procedure after preparing the data -- with no missing values

As evident in the Means procedure output above, we have also replaced many of the variables with the imputed dummy variables, and flagged the missing values. We do not have any values missing, and the data appears to be a lot cleaner than what we initially had. Before jumping to building models, we want to test to see if the key character variables represent the data they way we hypothesized early on. Figure 5 below presents the crash prediction for the TARGET_FLAG.


![image](https://cloud.githubusercontent.com/assets/26909910/25582918/9863ef0e-2e5d-11e7-8c8c-4848a5506818.png)
![image](https://cloud.githubusercontent.com/assets/26909910/25582935/a8421446-2e5d-11e7-8cd6-d0173a46dc63.png)
![image](https://cloud.githubusercontent.com/assets/26909910/25583040/1856e41e-2e5e-11e7-8270-996ba5a9e4e4.png)

Figure 5 (Part A): Freq procedure results for TARGET_FLAG for character variables





