## Introduction:

The Wine data set has a total of 12795 observations, and 14 variables (namely: AcidIndex, Alcohol, Chlorides, CitricAcid, Density, FixedAcidity, FreeSulfurDioxide, LabelAppeal, ResidualSugar, STARS, Sulphates, TotalSulfurDioxide, VolatileAcidity, and pH). The main goal of this data study assignment is to a). Predict the probability of a customer buying Wine, and b). Predict the number of wine cases the customer purchases.

We divided this data study project into four major segments: understanding the data, preparing the data, building predictive models, and deploying predictive models. In order to build better predictive models, we need to learn the attributes of the data set. We performed an initial Exploratory Data Analysis (EDA) to understand the data. Upon learning about the overall structure of the data, and identifying major issues to tackle the problem in hand, we prepared the data by eliminating the “bad” data, weeding out the irrelevant data, cleaning up the structure of the data, addressing missing data entries, understanding the correlation of the variables, checking for multicollinearity, devising a plan to build the model, and deliberately selecting variables (and variable selection method), among other preparatory activities, to ensure an accurate model built.

After understanding the layout of the data set and completing the preparatory exercises, we were ready to build and test models. We built five different models and tested them for accuracy and fit. We went through an iterative process of selecting different variables, and testing them against different models to evaluate the fit. Ultimately, we were able to decide on the “best” model based on the different predictors and statistical matrices, produced by the respective models. The best fitting model was then deployed in the test data set to predict the probability of a customer buying a wine, and the number of wine cases the customer may buy.



## Understanding the Data:

In order to understand the overall nature of the data set, we simply printed out 10 observations with all variables included. We were able to learn about the overall layout and nature of the dataset and variables. We then ran a PROC MEANS procedure to learn about the mean, median, mode, missing items, standard deviation, and lower and upper percentile information about each variable. We learned that eight variables have a lot of data sets that are missing and needed to be addressed. Figure 1 below shows the SAS output of 10 observations from the original dataset, and Figure 2 below shows the SAS output of the PROC MEANS procedure.

![image](https://cloud.githubusercontent.com/assets/26909910/25592872/63bbc738-2e88-11e7-94c0-7df1ba9a7254.png)

   Figure 1: Sample observations to understand the data	
   
![image](https://cloud.githubusercontent.com/assets/26909910/25592876/66a1d276-2e88-11e7-820c-dec575d7a408.png)

   Figure 2: PROC MEANS procedure to understand the data	
   
Figure 2 above shows the mean procedure data for all variables in the dataset. It is interesting to see how many variables have a wide spread between their mean and maximum and minimum value. Such a wide spread often means that there are entries in the dataset that are extremes and anomalies. These data entries will, therefore, be addressed in the data preparation step. For example, FreeSulfurDioxide has a minimum value of -555 and a positive value of 623, and a mean of 30.85; this entry will be addressed in the data preparation step and the extreme values will be ‘normalized.’

![image](https://cloud.githubusercontent.com/assets/26909910/25592918/9e299ff8-2e88-11e7-8f99-6b5a0b75c39a.png)

  Figure 3: Proc Univariate Histogram example – TARGET


After learning about the basic statistics of the dataset, we tried to explore the data more by using proc univariate histogram for the Target variable. As shown in Figure 3, the proc univariate histogram allowed us to find out that the zero column was at a peak, and that we needed to address the issue at the data preparation stage. We will discuss more in the data preparation stage as to how we addressed this situation. The proc univariate histogram process led us to learn about this occurring. Therefore, with Poisson distribution, we think it is crucial to visualize your initial data and learn about the outliers, so that we can take care of it sooner than later.

## Preparing the Data:

We started with manual observation of the variables and its spread as indicated in the data exploration stage above. Upon learning the basic attributes of the data, we decided to select the “best fitting” variables to build preliminary models. We could not manually pick the variables due to the lack of sufficient information and subject matter expertise in the area of wine production and consumption. Aside from the couple obvious theoretical effect stated by the data dictionary and the general knowledge on the product, we were not able to deduce the most important variables for the modeling purpose. Hence, we resorted to automatic variable selection methods. We tested all of the available variables on a regression model by using forward, backward, stepwise, Max R, adjusted R-squared, and Mallow’s CP variable selection methods. The adjusted R-squared variable selection method, on the given Regression model, gave us the lowest error and a usable number of variables.

![image](https://cloud.githubusercontent.com/assets/26909910/25592968/d71ec950-2e88-11e7-9b85-ac1d1a006fe7.png)

   Figure 4: Using Proc Reg for initial variable selection using Adj. R-squared	
   
Figure 4 above shows the SAS output of the initial regression model using the adjusted R-squared variable selection method. This process allowed us to vet the variables that produce the lowest error, and use those variables to build and test our models. Therefore, we selected VolatileAcidity, ResidualSugar, Chlorides, TotalSulfurDioxide, Density, pH, Alcohol, LabelAppeal, AcidIndex, and STARS as the initial variables to build and test our models. Before jumping straight into the model building process, there’s so much more that we need to do to make the variables “model ready.”

We ran Proc Means again on the above selected variables, and identified the extreme and missing values. First, we tacked the problem of missing values, and replaced all missing fields with corresponding missing values for all fields except for the variable STARS. As discussed in the data exploration stage, STARS variable is evidently highly predictable variable, and therefore makes more sense to plug-in zero value as opposed to using the mean or median. Therefore, we flagged the imputed STARS variable with a new variable name M_STARS and changed the missing STARS variable to IMP_STARS by replacing all missing values with a zero. Figure 5 below shows the results after we replaced all missing values.

![image](https://cloud.githubusercontent.com/assets/26909910/25592982/e509c07e-2e88-11e7-917a-389b66c063b1.png)

  Figure 5: SAS output after replacing missing values with corresponding Means
  
Similarly, we also normalized the extreme values by using conditions to match the extreme values to the mean. Before normalizing, in order to learn the normalizing criteria, we plotted all of the selected variables in a histogram chart. Then, we changed the conditions to match the criteria we identified; for example: if IMP_TotalSulfurDioxide< -330 then IMP_TotalSulfurDioxide=-330, and if IMP_TotalSulfurDioxide> 630 then IMP_TotalSulfurDioxide=630, among others. In totality, we normalized TotalSulfurDioxide , VolatileAcidity, IMP_ResidualSugar, Density, IMP_Chlorides, IMP_pH, IMP_Alcohol, and AcidIndex; a total of eight variables. We then tested our variable set again by using Proc Univariate and Proc Means. Figure 6 presents the “before normalizing” and “after normalizing” perspective of the variable Alcohol via histogram.

Similarly, we also normalized the extreme values by using conditions to match the extreme values to the mean. Before normalizing, in order to learn the normalizing criteria, we plotted all of the selected variables in a histogram chart. Then, we changed the conditions to match the criteria we identified; for example: if IMP_TotalSulfurDioxide< -330 then IMP_TotalSulfurDioxide=-330, and if IMP_TotalSulfurDioxide> 630 then IMP_TotalSulfurDioxide=630, among others. In totality, we normalized TotalSulfurDioxide , VolatileAcidity, IMP_ResidualSugar, Density, IMP_Chlorides, IMP_pH, IMP_Alcohol, and AcidIndex; a total of eight variables. We then tested our variable set again by using Proc Univariate and Proc Means. Figure 6 presents the “before normalizing” and “after normalizing” perspective of the variable Alcohol via histogram.


![afdaf](https://cloud.githubusercontent.com/assets/26909910/25593105/450b5cda-2e89-11e7-8fd9-9cd8ce460e2d.PNG)

   Figure 6: Comparative visual to present the effect of using conditions to replace extreme values

We now believe, we are ready to build models using the variables selected above. However, it is important to note that as we build models, depending on the type of model we develop, the variable selection will vary. What we have selected is the maximum number of variables to be used by any given model, regardless of the type. Without further ado, let’s dive into developing models.

### Model 1 (Regression):

We first built a regression model using a stepwise variable selection method on the variables we picked from our previous data preparation steps. Upon using Proc Reg to create a model, Model 1 output R-square of 0.5476 and the parameter estimates and other details presented in the SAS output in Figure 8.

Figure 7 presents the actual model we received by using the parameter estimates from the SAS output to multiply with their respective variables. Upon plugging in the values from the parameter estimates to the model, we received new values stored in a variable X_Regression. Figure 8 presents a sample of 10 values from the variable X_Regression. Looking at the output presented in the Figure 8, X_Regression values do match closely to the actual values in TARGET variable.

      P_Regression = 4.47092 + VolatileAcidity * (-0.10494) + IMP_Chlorides * (-0.12802) + IMP_TotalSulfurDioxide 
      * (0.00024613) + Density * (-0.83372) + IMP_pH * (-0.04250) + IMP_Alcohol * (0.01280) + LabelAppeal 
      * (0.46674) + AcidIndex * (-0.20421) + IMP_STARS * (0.77860) + M_STARS * (-0.68959);

   Figure 7: Model 1 -- Regression
   
![image](https://cloud.githubusercontent.com/assets/26909910/25593222/ba887ec0-2e89-11e7-88ac-2b6557de36c2.png)

 Figure 8: SAS output of Proc Reg for Model 1	

![image](https://cloud.githubusercontent.com/assets/26909910/25593226/bd37c752-2e89-11e7-8ee6-870e262082f5.png)

 Figure 9: Side-by-side comparison

We expand further on the results from Model 1, and compare the Target values and X_Regression with the P_Regression. P_Regression is the probability that a customer will by the wine cases. Figure 10 below makes it clear that the predicted values using the regression model did provide a fairly accurate prediction. Therefore, we would not like to eliminate the results from this model, and rather, use it to compare with other models to see how it holds its value.

![image](https://cloud.githubusercontent.com/assets/26909910/25593284/f18c69e0-2e89-11e7-8cf9-0d856da9d85a.png)

 Figure 10: Side-by-side comparison of actual, predicted, and scored values.	
 
### Model 2 (Negative Binomial):

The regression model above gave us a pretty good prediction on if the customer will purchase wine. However, now that we’ve learned other techniques to build and test models, we will attempt to use the same selected and refined set of variables, and build a second model, and compare against the first to see how it stands against the first.
We use Proc GENMOD to build our second model. We selected VolatileAcidity, IMP_ResidualSugar, IMP_Chlorides, IMP_TotalSulfurDioxide, Density, IMP_pH, IMP_Alcohol, LabelAppeal, AcidIndex, IMP_STARS, and M_STARS to build the second model. We log transformed the variables, and received an output model presented in Figure 11:

      P_GENMOD_NB = 1.1950 + VolatileAcidity * (-0.0136)+ IMP_ResidualSugar * (-0.0001)+ IMP_Chlorides 
      * (-0.0233) + Density * (-0.3903) + IMP_pH * (0.0092) + IMP_Alcohol * (0.0094) + LabelAppeal 
      * (0.2955) + AcidIndex * (-0.0204) + IMP_STARS * (0.1207) + M_STARS * (0.0344);

 Figure 11: Model 2

Figure 11 represents the model we received using the Proc Genmod process. A sample SAS output of the model using the training dataset is presented in Figure 12 below. The output is then compared to the regression model (Model 1) in Figure 13. When comparing Model 2 against Model 1 in Figure 14, the difference is subtle, and it is really difficult to tell which model is better. However, the regression model still seems to provide a better estimate here.

![image](https://cloud.githubusercontent.com/assets/26909910/25593381/4ccfba82-2e8a-11e7-9b05-b9f282420d21.png)


 Figure 12: Genmod procedure for Model 2	

![image](https://cloud.githubusercontent.com/assets/26909910/25593383/5091a018-2e8a-11e7-9021-79465d24a475.png)

 Figure 13: Parameter Estimates for Model 2
 
![image](https://cloud.githubusercontent.com/assets/26909910/25593438/87c5d324-2e8a-11e7-9689-cc91a9a8116f.png)

 Figure 14: Comparing General Regression and Negative Binomial 
 
### Model 3 (Poisson):

In the same fashion as in Model 2, we run a proc genmod with POI distribution to develop a Poisson model. Upon using the pre-selected variables (VolatileAcidity, IMP_ResidualSugar, IMP_Chlorides, IMP_TotalSulfurDioxide, Density, IMP_pH, IMP_Alcohol, LabelAppeal, AcidIndex, IMP_STARS, and M_STARS), we were able to develop a more accurate predictive poisson model presented in Figure 15 thru 17 below:

![image](https://cloud.githubusercontent.com/assets/26909910/25593524/edbef9da-2e8a-11e7-9274-bbdf0ad0ad50.png)

 Figure 15: SAS output for Model 3 (Part A)	
 
![image](https://cloud.githubusercontent.com/assets/26909910/25593580/395d657a-2e8b-11e7-930c-461bb61bf604.png)
 
 Figure 15: SAS output for Model 3 (Part B) – Parameter Estimates	
   
      P_GENMOD_POI=1.7978 + VolatileAcidity * (-0.0342) + IMP_ResidualSugar * (0.0001) + IMP_Chlorides 
      * (-0.0400) + IMP_TotalSulfurDioxide * (0.0001) + Density * (-0.2861) + IMP_pH * (-0.0169) + IMP_Alcohol 
      * (0.0036) + LabelAppeal * (0.1591) + AcidIndex * (-0.0814) + IMP_STARS * (0.1875) + M_STARS * (-0.6494);

  Figure 16: Model 3

![image](https://cloud.githubusercontent.com/assets/26909910/25593601/52366d3a-2e8b-11e7-9154-64a358ea3377.png)

  Figure 17: Comparing Model 1, Model 2, and Model 3	   
 
Model 3, the Poisson model, so far is the best model. Aligning all models alongside each other in Figure 17 above has made it evident that Model 3 is, so far, able to predict closest to the Target value. We now will evaluate the Zero inflated versions of Model 2 and Model 3 to see if they would yield in a better prediction. However, thus far, the Model 3 seems to stand out as the best model yet.

### Model 4 (Logistic Hurdle):

Logistic hurdle model is one among many techniques to build zero inflated Poisson and zero inflated negative binomial models. We started with building a logistic model to predict the probability of the wine being sold using the same variables we selected in the data preparation steps. Figure 18 shows the logistic model and the Figure 19 shows the SAS output of the model. We will compare the individual output of the logistic model with other models later in the narrative.

      P_LOGIT_PROB = 3.2612 + VolatileAcidity *(-0.1974+ IMP_ResidualSugar *(0.00137) + IMP_Chlorides 
      *(-0.1693)+ IMP_TotalSulfurDioxide *(0.000958)+ Density *(-0.4951)+ IMP_pH * (-0.2120)+ IMP_Alcohol 
      * (-0.0223) + LabelAppeal * (-0.4643) + AcidIndex *(-0.3941)+ IMP_STARS * (2.5504) + M_STARS * (0.7303);
      
  Figure 18: Logistic model (STEP 1) of the Logistic Hurdle model process

![image](https://cloud.githubusercontent.com/assets/26909910/25593705/d1537a18-2e8b-11e7-9db1-c0d0a74b5909.png)

 Figure 19: Model Output for Logistic model part of the Logistic Hurdle Model
 
Upon building a logistic model to predict the likelihood of a customer to purchase Wine, we looked into the quantity of Wine the customer is likely to purchase. To determine the number of cases of wine to be sold if/when the wine is sold, we used TARGET_AMT variable we created early on in the process. We had subtracted 1 from the original TARGET variable to create the TARGET_AMT variable. PROC GENMOD is then used to run a model using a poison distribution (I tried using both, Poisson and Negative binomial, distributions, but ended up with the exact same parameter estimates). Upon running the PROC GENMOD, we received the model presented in Figure 20 below. The process yielded in the results shown in Figure 21.

      P_GENMOD_HURDLE = 1.1950 + VolatileAcidity *(-0.0136) + IMP_ResidualSugar *(-0.0001) + IMP_Chlorides 
      *(-0.0233)+ IMP_TotalSulfurDioxide *(0)+ Density *(-0.3903)+ IMP_pH * (0.0092)+ IMP_Alcohol 
      * (0.0094) + LabelAppeal * (0.2955)+ AcidIndex *(-0.0204)+ IMP_STARS * (0.1207) + M_STARS * (0.0344)

  Figure 20: PROC GENMOD model (STEP 2) of the Logistic Hurdle model process

![image](https://cloud.githubusercontent.com/assets/26909910/25593762/18a96350-2e8c-11e7-83ec-b02f16193ef3.png)

  Figure 21: Model 4 Sample Output
  
 We also used [P_LOGIT_PROB = exp(P_LOGIT_PROB) / (1+exp(P_LOGIT_PROB));] to add the 1 back to the model (the one we initially removed from the variable). And we used [P_TARGET = (P_LOGIT_PROB) * (P_GENMOD_HURDLE);] to combine the two models above. Finally, we used [P_GENMOD_HURDLE = exp(P_GENMOD_HURDLE);] for results exponentiation.


### Model 5 (An Average model of the Model 1 thru Model 4 – Ensemble Model):

After analyzing all of the four models above, we thought it might be better to combine the models and take an average of the models to see how the predicted values fit with the target values. Therefore, we took an average of all four models above – Regression, Poisson, Negative Binomial, and Zero Inflated Poisson/Negative binomial models (hurdle), and came up with an entirely new model below. The results were then compared alongside other models to understand and compare the fit. We used [P_Ensemble = (P_Regression + P_GENMOD_NB + P_GENMOD_POI + P_TARGET)/3;] formula to generate Model 5.

## Model Selection

We have now developed and analyzed all five models above. All models seem to be very close to each other in terms of output, and not that far from the target values. Therefore, it is a very close call to decide on a model. However, looking at the overall AIC, BIC, R square, and sum of each model, and comparing their output to the target output, we select the first Model – MODEL 1 as our BEST MODEL. Yes, ultimately, the regression model turned out to be the best predictor here.
Figure 22 below shows a neat comparison of the models’ overall Mean, Median, Minimum, Maximum, and Sum. The Means Procedure below indicates that the sum of our Best model is very close, almost equal to the Target sum. The difference between the sum of our best model and target variable is 0.66, which is negligible. Therefore, we are fairly confident on our model. Besides running the Means Procedure, we also performed a side-by- side comparison of 10 observations in each model. This exercise enabled us to see how each observation performed on different models. A detailed output is presented in Figure 23:
 
![image](https://cloud.githubusercontent.com/assets/26909910/25593834/518ba9bc-2e8c-11e7-9b07-204cc0af2750.png)
 
 Figure 22: Means Procedure for all Five Models
 
![image](https://cloud.githubusercontent.com/assets/26909910/25593838/559beb52-2e8c-11e7-9435-459a140fc8ae.png)

 Figure 23: Side-by-side output comparison of all Five Models
 
 
## Stand-Alone Predictive Model Application:

Upon identifying the best model equation, we apply the model equation data to predict the target number of Wine purchases for the test data set. However, because of the specific requirement of this assignment, I have deployed both, the best (Regression model) and the Logistic Hurdle Model. It is important to note that the exact same criteria and methodology used in training data set was incorporated to build with the predictive test data set and subsequent scorecard files. We have only kept Index and P_Target variables in the files.

## Conclusion:

This has been a long and intensive process, but the results look good. We identified five different models, and picked the one that we believe fits the best based on the measures we described in the model selection part of this document. Following the steps one through four – Understanding the data to deploying the model, we were able to come up with a fairly accurate prediction indicated by the statistical measures stated above.

Some of the important things to note in this model development and deployment exercise includes the variable selection methods we incorporated and tested in multiple different environment for accuracy and utility. However, there’s only so much an automated variable selection technique can do. It is important to note that the subject matter knowledge leads the variable selection process, and having a qualified SME (Subject Matter Expert) in the team to guide through the variable selection process might improve the current prediction. Overall, the selected model seem to model probability of sales and amount of sales in a fairly accurate way. Even though the conventional wisdom may prefer Poisson models for this dataset, our exercise proved that the basic regression model was the best predictor of all. While this may not always be the case, it is always a great idea to try out different models and modelling techniques and see what works the best for the given dataset. For there may be times, just like this, where the least expected one turns out to be the best predictive model. A detailed output and scorecard along with the complete SAS code is attached with this document.

 
 
