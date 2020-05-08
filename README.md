Problem Statement
 

Problem Statement :
		To reduce the customer churn, we need to predict which customers are at high risk of churn.

Churn prediction is usually more in prepaid customers
Usage_based churn is used in our project(not done any usage--- incoming, outgoing, internet etc over a period of time)
You will define high value customers(revenue comes from top 20 customers) ----
Predict churn only on high value customers

Objective:  To predict the churn in the last month(9th) using the data from 3 months(6,7,8)

6,7  ---- months ---good phase
8     ---      month ----- action phase
9     ----  month ----  churn phase


Data Preparation  :
Handling missing values:

For this i would suggest that you impute the missing values very thoroughly as then the cleaned data will have less outliers. Below methods you can use :
1) Impute with zeroes for those columns where the min value is 1(use describe to get the data).
for eg : columns like, 'max_rech_data_6', 'max_rech_data_7', 'max_rech_data_8', 'max_rech_data_9', etc.
2) Replace the NaN values wisely for the case of categorical variables.
3) As discussed in the session we do not need values which have 70% data missing. So, we can define a threshold and then remove the values.
4) Use fancyimpute library(Need to explore more on this)
Hope this helps :) 


Derive Features
Filter high value customers --- those who have recharged with an amount x
      X = 70 percentile of avg recharge amount in first 2 months
       After filtering you get 29.9k  rows
Tag churn as 1 and non churn as 0---- using 4th month
Not made any calls and no internet usage in churn phase also
Total_in_mou_9
Total_ou_mou_9
Vol_2g_mb_9
Vol_3g_mb_9

Remove all attributes corresponding to churn phase i.e.., tagged 9

Reduce number of features using PCA:

Train varieties of models, tune hyperparameters
	

Identify the churns than non churns accurately ----- TPR ---- Sensitivity == TP/TP+FN
Finally choose the model…..


Pca:  principal component analysis is an unsupervised, non-parametric statistical technique primarily used for  dimensionality reduction in ML, can also filter noisy datasets.

P_value: used to test null hypothesis--- Hypothesis frequency called the p_value, also known as ‘observed significance level for the test hypothesis.
Chi_square : the dist between the data and the model predictions is measured using chi-squared statistics.

STEPS:  
Data cleaning
. read data
. treating missing values
→ drop columns with 60% and above missing values
→ convert the “ “ , ‘NULL’, 0 by nan values
→ replace the nan values by median
→ the nan in date formatted columns cannot be imputed so we take just the date and replace the nan by dates
→ last date of rech can be imputed by avg of other 3 columns  or dropped. 


EDA
→ deriving feature churn( in or og )== 0 and (2g or 3g) = = 0 ⇒  churn =1 else churn = 0
	→ check the columns
	→ get the avg total amount of 6,7 months == 374
Rows == 29.9k (didn’t achieve this)
	
	→ based on high value customers bar plots of total amt recharged, in, og, 2g,3g  features against churn data 
	
Using PCA reduce features
 



Techniques and model building using Jupyter / google colab

Data Cleaning and preparation: 

Read the data from the given csv file
Looking at the columns and different features in the data and analysing them
Handling the missing values: 
→ check for percentage of missing values in the given data
→ We observe lot of columns having more than 70 percentage of missing values
→ lets analyse the important variables and try to impute them though they have high missing values to save important information , which we need for prediction
→ total recharge data , total recharge amount, max recharge data , avg recharge data are important features, for these features we imagine the nan values as 0 (assuming there is no recharge done )
→ Lets again check for percentage of null values and drop the columns with missing values greater than 70%
→ lets again check for % of missing values , we observe we have low percentage of missing values left , Now lets impute them either with fancy imputer or use median as we have outliers
→ Even though we dropped and imputed the missing values, we can still observe some nan values, due to the categorical data i.e, date of last recharge , we try and impote them by 0 assuming that there is no recharge done.
→ to reduce the features lets drop the non significant rows.
→ confirm there are no more missing values in our data and proceed with feature engineering.

High value customers : (customers who tend to produce the max revenue for company top 20%)
→ we have total recharge amount for a customer 
→ we directly dont have the total recharge amount for data, which has to be derived from two other features …. Total amount data and avg rech data , by multiplying these two columns we can derive the total recharge amount for data
→ Now let’s go a head and calculate the total recharge amount == total_rech_amt + total_rech_amt_data
→ For high value customers ,we cal avg of the total recharge amount and then filter the total recharge amount for 6 and 7 months greater than or equal to that avg amount

Tagging churn for high value customers:
→ customers churn is calculated based on 9th month data , the customers with incoming and outgoing calls as 0 and 2g and 3g data usage as 0 in 9th month are tagged as churned customers i.e., churn = 1 else churn =0

Outliers treatment:
→ we plot box plot for important features i.e., total rech amt and total rech data for 6, 7 , 8 months
→ check the values that are at a great gap and try dropping them
Analyse the features in 6, 7, and 8 months :  on observing the barplots we can see a great dip in recharge amount and recharge data in 8th month when compared to 6,7th month which implies these customers are likely to churn and take preventive measures to stop churn.
Since we finished deriving important features, lets drop the features ending with -9
Normalise the data :   As we can observe that the features are of different scales lets normalize them(bringing the mean =0 and varience = 1)
Calculate the churn date
FEATURE REDUCTION:
→ Using PCA for feature reduction
→ we have an imbalanced data , so let’s balance it using SMOTE
→ lets create a correlation matrix for pcs
→ we observe multicollinearity   in the data and once the data is balanced and then we can get a correlation less than 0(implies that data balancing has reduced the multicolinearity in data)

Model building:

Using PCA we build model using logistic regression(as we cannot predict any metrics except area under curve), we build other models
Build linear and Non linear SVM and observe which model gives more sensitivity/ recall
Lets also build a decision tree with the same pcs.
After building all the three models , let’s check which model gives us high truepositive rate, sensitivity(in our case it is linear SVM)


But as PCA models are not helpful in feature interpretation, lets build models without pca

Build both non linear svm and random forest and check for sensitivity
Select the model which gives the best sensitivity
And also the features for predicting churn which is done using random forest.    
