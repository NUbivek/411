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

