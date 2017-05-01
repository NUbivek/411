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


![image](https://cloud.githubusercontent.com/assets/26909910/25583082/42f45ad0-2e5e-11e7-87e1-cf984bd00314.png)
![image](https://cloud.githubusercontent.com/assets/26909910/25583088/475bfa74-2e5e-11e7-88a4-f65072e85d98.png)
![image](https://cloud.githubusercontent.com/assets/26909910/25583096/520a8300-2e5e-11e7-88d2-b9386148ecd9.png)
![image](https://cloud.githubusercontent.com/assets/26909910/25583091/4b59f2fc-2e5e-11e7-90d9-2face6660551.png)
![image](https://cloud.githubusercontent.com/assets/26909910/25583092/4eabae82-2e5e-11e7-9f7d-98749dc02086.png)

Figure 5 (Part B): Proc Freq procedure results for TARGET_FLAG for key character variables

Above result from Proc Freq procedure clearly show that we are on a right track. We have weighed in key variables that significantly affect the crash rates. This step now allows us to incorporate these variables into our model building process. We will now be able to make a more educated estimate on what variables to consider and what to avoid.
 
## Models Building and Comparison:

During the data preparation stage, we tested various different variables and their respective influence in predicting the crash rates. We will now incorporate the findings to build and test eight different models in this section. We will only present the details and visualization for 3 competitive models, but the summary table below will summarize the key findings of all eight models.


![4q453q](https://cloud.githubusercontent.com/assets/26909910/25583288/29adc0ec-2e5f-11e7-80d4-463a8b5b88a3.PNG)

Figure 6: Side-by-side comparison of eight models prepared as a part of this

### Model 1:

Amongst the eight models we built, Model 1 is built using an automatic variable selection technique using the stepwise selection method for all numeric and one (JOB_WHITE_COLLAR) dummy character variable. The stepwise selection method selected 10 variables and resulted in AIC of 9049.609 and log likelihood of 9045.609.


![image](https://cloud.githubusercontent.com/assets/26909910/25583310/42029ad2-2e5f-11e7-9650-b5d030498a71.png)
![image](https://cloud.githubusercontent.com/assets/26909910/25583315/456dd736-2e5f-11e7-994c-87b911a1e14b.png)

Figure 7: Key data for Model 1

![image](https://cloud.githubusercontent.com/assets/26909910/25583336/5af4c902-2e5f-11e7-858d-de7ae9bb39ad.png)
![image](https://cloud.githubusercontent.com/assets/26909910/25583339/5d2ab2cc-2e5f-11e7-8e32-f501fc41bfe9.png)

Figure 8: ROC curve for selected and all model-building steps for Model 1

### Model 5:

After building the first step-wise model, we quickly used other automatic variable selection techniques and build 4 other models. We used Mallow’s CP and two manual selection methods and built Model 2,3, and 4 discussed in the summary table above.
We would like to skip to the Model 5 and discuss the details of Model 5 here.

Model 5 was built using a manual selection method by first testing all 23 variables, and then based on their performance and effect on the predictability of the crash, we selected 13 high-impact variables. The 13 variables included a mix of character, numeric and imputed variables. Upon running the model, we obtained an AIC of 7605.927 and log likelihood of 7569.927.

![image](https://cloud.githubusercontent.com/assets/26909910/25583447/cc631ecc-2e5f-11e7-90e4-5c4a9fbd5f4a.png)
![image](https://cloud.githubusercontent.com/assets/26909910/25583450/d0b5a5bc-2e5f-11e7-9397-059f6aeebdb2.png)
![image](https://cloud.githubusercontent.com/assets/26909910/25583456/d34964d0-2e5f-11e7-8eca-b2c8ecb2e44c.png)

Figure 9: Key data and ROC for Model 5

### Model 7:

Model 7 is also built using an automatic variable selection technique using the stepwise selection method. However, we have taken all 23 variables – both, character and numeric -- for this selection process. We created a class stage and added all imputed numeric and character and remaining categorical variables. The stepwise selection process did not eliminate any of the 23 variables and kept all variables for the model building process. The selection method resulted in an AIC of 7360.154 and log likelihood of 7296.15.

![image](https://cloud.githubusercontent.com/assets/26909910/25583540/37df8d20-2e60-11e7-82d1-01f5158899e6.png)
![image](https://cloud.githubusercontent.com/assets/26909910/25583535/34b9dd76-2e60-11e7-9fab-74eeac764145.png)
![image](https://cloud.githubusercontent.com/assets/26909910/25583542/3abd65c6-2e60-11e7-84c6-0dc1193ce000.png)
![image](https://cloud.githubusercontent.com/assets/26909910/25583544/3cffac4a-2e60-11e7-956d-9868bf2af3a0.png)

Figure 10: Key data and ROC curve for Model 7

## Model Selection:

In each of the models above, the estimate column next to the variable names reflects the effect and the magnitude of the effect of the respective variables on the overarching goal of predicting the crash probability. The positive numbers represent that the variable favors the crash probability; meaning increases the chances of crash. Whereas, the negative number denotes that the respective variable actually reduces the chances of crash. These variables and their respective numbers are the predictor variables and coefficients, respectively. The intercept number on the top of the estimates is the y-intercept of the model.

We now look at the indicators deduced from the models above to determine which model is the best model for our analysis. We will then develop a model formula and deploy it in subsequent exercise. The AIC, Log Likelihood, and ROC value of the above models are key measures to help us make a decision on the best model to predict the probability of a crash. The variable we are trying to predict is the TARGET_FLAG variable.

AIC measures the degree of an estimate of the loss of information during the data generation and modeling stages. Looking at the above measures for all eight models, Model 5 seems to minimize the information lost and/or the impact of the lost information. In addition, the impact of individual variables in Model 5 is significantly higher and there is a very few wasted variables in the analysis process. Therefore, this promotes the goodness of fit, and helps us deliver better predictions. Based on these reasons, we decide to proceed with Model 5.

Therefore the equation representing Model 5 is as follows:

    YHAT = -1.4354+0.8897*(CAR_USE in ("Commercial")) -0.8842*(CAR_TYPE in ("Minivan")) -0.7780
    *(CAR_TYPE in ("Panel Truck")) -0.3713*  (CAR_TYPE in ("Pickup"))+0.2449*(CAR_TYPE in ("Sports Car"))-0.4877
    *(CAR_TYPE in ("Van"))-0.6275*(MSTATUS in ("Yes"))-0.9080*(REVOKED in ("Yes"))+0.2193
    *(SEX in ("M"))+2.1119*(URBANICITY in ("Yes"))-0.3135*(PARENT1 in ("N"))+0.0902
    *HOMEKIDS - 0.00001*IMP_INCOME -0.00001*OLDCLAIM +0.2025*CLM_FREQ +0.3540*KIDSDRIV+0.1217*MVR_PTS

Figure 11: Best fitting model deployed and scored

## Standalone Predictive Model Application:

Upon selecting the best logistic model to fit the dataset, we used a test data set to deploy the above model, Model 5. We first loaded the test dataset, and then used the exact same data preparation steps to prepare the test dataset. The test dataset has to match exactly to the training dataset. Therefore, we prepared the dataset using the same criterion, and then first used YHAT to store the output generated from deploying the model. Upon storing the values in YHAT, we then converted the YHAT to P_TARGET_FLAG to store the probability of crashing the vehicles. Figure 12 below shows a sample of 15 observations and their respective P_TARGET_FLAG values.

![image](https://cloud.githubusercontent.com/assets/26909910/25583715/ffde720a-2e60-11e7-82ac-e4f301f71ca3.png)

Figure 12: Sample (15) observations from P_TARGET_FLAG deployment


Upon determining the P_TARGET_FLAG values, we developed a second model to predict the value of P_TARGET_AMT. P_TARGET_AMT predicts the respective cost of insurance for each individual observation. We used a mix of two variables to build the model.

    P_TARGET_AMT = 1504.32+(P_TARGET_FLAG*OLDCLAIM);

Figure 13: P_TARGET_AMT model

We took the mean value of TARGET_AMT in the training data set and added the product of the crash probability (P_TARGET_AMT) and number of claims made by the individual in last 5 years (OLDCLAIM). This model is useful, particularly, in determining and offsetting the cost of high-risk observations. The logic of charging above average cost of insurance for high-risk customers based on their driving records and probability to crash provides a good estimate of the premium to be charged. Figure 14 below shows a side-by- side comparison of the P_TARGET_AMT and P_TARGET_FLAG output of 15 sample observations. The complete list of the model output and deployment is attached separately with the final submission.

![image](https://cloud.githubusercontent.com/assets/26909910/25583743/27045b60-2e61-11e7-83cd-1ad5bb9ed018.png)

Figure 14: Sample (15) observations from P_TARGET_FLAG and P_TARGET_AMT deployment

## Conclusion:

Following the steps one through four – Understanding the data to deploying the model, we were able to come up with a fairly accurate logistic prediction model and output indicated by the statistical measures stated above. We explored eight possible models, and selected the one that fits the best for the dataset.

It is important to note that, while our “best” model takes into account many different important facets of determining the probability for a crash from the available datasets, it might not be the ultimate truth. There are many variables, and we have taken the ones that best reflected the opportunity to accurately predict the crash probability. Therefore, while comparing against the models that included somewhere from every single variable to only one variable, this model is significantly more relevant and accurate.

In addition, the best fitting model also helps to remove the fog and provide a relevant correlative insight into the two variables – URBANICITY and PARENT1 –whose theoretical significance were unknown. The model proves that both of these variables are strong predictors of the crash probability. Finally, all 13 variables used in the best fitting model match and support the theoretical correlation with the reality of the insurance industry. Hence, this model can be trusted and used.
