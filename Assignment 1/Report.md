## Introduction:

MONEYBALL data set has a total of 2276 observations, and 17 variables (namely: TEAM_BATTING_H, TEAM_BATTING_2B, TEAM_BATTING_3B, TEAM_BATTING_HR, TEAM_BATTING_BB, TEAM_BATTING_SO, TEAM_BASERUN_SB, TEAM_BASERUN_CS, TEAM_BATTING_HBP,    TEAM_PITCHING_H,    TEAM_PITCHING_HR,    TEAM_PITCHING_BB, TEAM_PITCHING_SO, TEAM_FIELDING_E, and TEAM_FIELDING_DP). Each of these variables has either a positive or a negative impact on the overall wins. The attached data dictionary elaborates in detail the specifics of the variable’s impact on overall wins.

We divided this data study project into four major segments: understanding the data, preparing the data, building predictive models, and deploying predictive models. In order to build better predictive models, we need to learn the attributes of the data set. We performed an initial Exploratory Data Analysis (EDA) to understand the data. Upon learning about the overall structure of the data, and identifying major issues to tackle the problem in hand, we prepared the data by eliminating the “bad” data, weeding out the irrelevant data, cleaning up the structure of the data, addressing missing data entries, understanding the correlation of the variables, checking for multicollinearity, devising a plan to build the model, and deliberately selecting variables (and variable selection method), among other preparatory activities, to ensure an accurate model built.

After understanding the layout of the data set and completing the preparatory exercises, we were ready to build and test models. We built different models and tested them for accuracy and fit. We went through an iterative process of selecting different variables, and testing them against different models to evaluate the fit. Ultimately, we were able to decide on the “best” model based on the R-squared value, and Adjusted R-squared value, among other statistical matrices, produced by the respective models. The best fitting model was then deployed in the test data set to predict the number of target wins.
 
## Understanding the Data:

In order to understand the attributes of the data set, we ran a “Proc contents” analysis is SAS. We were able to learn that 15 out of 17 data sets were numerical in nature, and rests were character in nature. We also learned that 2 out of 17 variables, the same character variables, have majority, if not all, of data fields that are missing and/or cannot be used for further analysis. Therefore, we need to drop the two data sets for further analysis. Figure 1 below shows a part of the Proc Contents SAS output.

![image](https://cloud.githubusercontent.com/assets/26909910/25581951/baa0a7a6-2e58-11e7-9451-15086698feb6.png)

Figure 1: Variables’ list part of Proc Contents SAS output
 
 We then ran “Proc Univariate” analysis on each of the remaining 15 variables. We identified key statistical measures such as Mean, Median, Mode, Standard Deviation, Variance, Range, and interquartile Range for each of the 15 variables. We also plotted a normal distribution chart, normal quantile chart, and qq plot for each of the variables to visualize the data. Figure 2 below shows an example the proc univariate analysis for one of the 15 selected variables.


![image](https://cloud.githubusercontent.com/assets/26909910/25581981/e21e210a-2e58-11e7-8db2-994f8e9f5524.png)
![image](https://cloud.githubusercontent.com/assets/26909910/25581983/e5c641e8-2e58-11e7-92f2-fa5195b018e2.png)

Figure 2: Proc Univariate example – TEAM_BATTING_H (Part A)


![image](https://cloud.githubusercontent.com/assets/26909910/25582094/6c29adc4-2e59-11e7-839f-6b8fcfeb0dcc.png)
![image](https://cloud.githubusercontent.com/assets/26909910/25582101/7294be42-2e59-11e7-938a-b2c88a7dc5d2.png)
  
Figure 2: Proc Univariate example – TEAM_BATTING_H (Part B)

Upon conducting a thorough analysis of the statistical measures, we were able to conclude that the “TEAM_PITCHING_H” variable had the highest mean (1779.21), standard deviation (1406.84), Mode (1494), and Median (1518), and “TEAM_BASERUN_CS” variable had the lowest standard deviation (22.96), and Mean (52.80). There were, however, four variables with more missing data, and we need to address the missing data before relying on these numbers in its entirety. We will address the missing data in the data preparation section, but it is important to note that the mean, median, mode, and standard deviation presented above may change as we address the missing data. Figure 3 below shows the SAS output of the Means procedure.

![image](https://cloud.githubusercontent.com/assets/26909910/25582160/ca3cba3c-2e59-11e7-8ec2-25922d96e300.png)

Figure 3: The Means procedure part of the SAS output

We also studied a sample data set of 10 observations out of 2276 observations with all 17 variables to learn the nature, specifics, and pattern of the data set. While running proc contents we were also able to run the target win mean (80.79), median (82), mode (83), and standard deviation (15.75).
 
## Preparing the Data:

In order to set a base line, we run a regression procedure to identify the R-square (0.4078), Adj. R-Square (0.4027), and overall fit (via residual analysis), among others, with the data we have in hand. It is important to note that the data in hand, as is, has many missing values stated in previous section. This step is solely for Exploratory Data Analysis (EDA) and base-line creation purposes.
Before we replace the missing values with mean or median, we performed a quick step-wise variable selection in SAS to see what variables will be eliminated and what will remain. The new R-Square is 0.3621 and the new Adj-R-Square is 0.3583. We dropped TEAM_BASERUN_CS variable. Figure 4 below shows the Proc Reg results after eliminating the TEAM_BASERUN_CS variable. Now, we are ready to replace the missing values with mean or median, depending on the nature of the missing variables.

![image](https://cloud.githubusercontent.com/assets/26909910/25582183/e5c7ff1e-2e59-11e7-9c99-2c98deb41518.png)
![image](https://cloud.githubusercontent.com/assets/26909910/25582199/f522a108-2e59-11e7-8674-7d3ba768a99a.png)

Figure 4: Reg procedure results after the elimination of all possible variables

Upon eliminating all possible unnecessary variables, we are now ready to fill in the missing values in the necessary variables. Therefore, evaluating the nature of the three missing variables (TEAM_BATTING_SO, TEAM_BATTING_SO, and TEAM_PITCHING_SO), filling in the missing fields of these variables with their respective median value seems the best way to go. Therefore, we populate the missing fields of the above variables with their respective median value and receive the output shown in Figure 5 below.


![image](https://cloud.githubusercontent.com/assets/26909910/25582230/19744d9a-2e5a-11e7-8c7f-1d8b690b1fba.png)

Figure 5: Means procedure results after filling in the missing fields of the variables

## Models Building and Comparison:

During the data preparation stage, we tested various different variable selection methods and checked their respective regression model outputs to identify the best models. We will discuss the findings in detail in this section.


### Model 1:

We first built our first regression model by eliminating the two character variables that had a lot of missing pieces to it, and were not much relevant to our study. This exercise alone resulted in an adjusted r-squared of 0.4027 and r squared of 0.4078 as stated above. Figure 6 below shows the details of Model 1. Since there was a lot more we could do with variable selection, we went ahead and tested different variable selection strategies to build the second model.

![image](https://cloud.githubusercontent.com/assets/26909910/25582245/2edf2c90-2e5a-11e7-9d57-9448b40e92ea.png)

Figure 6: First model, Model 1, with only 15 variables

### Model 2:

Upon testing forward, backward, step-wise, and adjusted r-squared variable selection methods, we felt much more comfortable with the result of r-squared and adjusted r- squared numbers from the step-wise selection method. This resulted in eliminating three variables and evaluating the regression model results from the step-wise selection strategy. Figure 7 below shows the details and fit-analysis of the output of the Model 2. The r-squared was 0.4068 and Adjusted r-squared was 0.4024 for the second model, which was a little better than the first model but we felt we could do a lot better.

![image](https://cloud.githubusercontent.com/assets/26909910/25582266/45a6c38e-2e5a-11e7-9ac2-d81bda7b12c4.png)

Figure 7: Second model, Model 2, using step-wise variable selection

### Model 3:

Upon learning about the narrowed down 15 variables, we tested through an iterative process of eliminating a combination of individual single variables, and multiple variables at one time. We were able to identify that dropping the TEAM_BASERUN_CS and TEAM_ BATTING_2B variables would yield in the lowest R-squared and lowest Adjusted R-squared. After filling in the missing fields, we run the regression model and find out that the new R- squared for the model is 0.2864, and the new Adjusted-R-Squared for the model is 0.2829. This is the best model we were able to come up with. Figure 8, 9, 10, and 11 below illustrate the details of the model with a complete residual analysis of regressor.

![image](https://cloud.githubusercontent.com/assets/26909910/25582282/5f0c0f82-2e5a-11e7-99f6-9b1551fe71bc.png)

Figure 8: Third model, Model 3, the best model yet.

### Best Model Equation

    P_TARGET_WINS = 12.85469 + 0.04205  * TEAM_BATTING_H + 0.07591* TEAM_BATTING_3B + 0.03605 * TEAM_BATTING_HR 
    + 0.00398 *   TEAM_BATTING_BB + (-0.00632)* TEAM_BATTING_SO + 0.03185 * TEAM_BASERUN_SB + (-0.00084374) * TEAM_PITCHING_H 
    + 0.01273 * TEAM_PITCHING_HR + 0.00012757 * TEAM_PITCHING_BB + 0.00245 * TEAM_PITCHING_SO + (-0.01950) * TEAM_FIELDING_E

![image](https://cloud.githubusercontent.com/assets/26909910/25582306/8a0f151c-2e5a-11e7-99e3-069dcd69ed54.png)

Figure 9: Third model, Model 3, fit analysis.

![image](https://cloud.githubusercontent.com/assets/26909910/25582318/9ac68778-2e5a-11e7-89dc-18a42f7d88a6.png)


Figure 10: Third model, Model 3, Residual by regressor analysis.

![image](https://cloud.githubusercontent.com/assets/26909910/25582324/aa368f82-2e5a-11e7-9a7b-dba17246fc50.png)

Figure 11: Third model, Model 3, Residual by regressor analysis.


## Stand-Alone Predictive Model Application:

Upon identifying the best model equation, we apply the model equation data to predict the target number of wins for the test data set. We use the Scorefile command and the best model linear regression equation in SAS to perform the operation. It is important to note that the exact same criteria and methodology used in training data set should be incorporated to build with the predictive test data set.
We then produced the best possible prediction on the number of wins for the test data set. Figure 12 below presents the tentative number of wins predicted based on the Model 3 above.

![image](https://cloud.githubusercontent.com/assets/26909910/25582431/4f3a36a0-2e5b-11e7-8180-2762be0ca4b8.png)

Figure 12: Predicted number of wins for the test data set

## Conclusion:

Following the steps one through four – from understanding the data to deploying the model, we were able to come up with a fairly accurate prediction indicated by the statistical measures stated above. However, it is important to note that two variables (TEAM_PITCHIN_BB and TEAM_PITCHING_HR) with positive coefficients indicated by our best fitting model might not be painting an entirely correct picture. In contrary to what is indicated by the model, these two variables, in theory, negatively impact the overall win rates. The positive sign of these variables does raise a question as to how these coefficients affect the overall validity of the model. Having said that, the model can still be accurate and reliable. The two variables need to be paid special attention throughout the interpretation phase, but the positive sign alone is not sufficient to question the validity of the model. 9 out of the 11 variables used in the best fitting model match and support the theoretical correlation with the target number of wins. Hence, this model can be trusted and used.
