&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;![example](images/pexels-pixabay-40568.png)



# Phase 3 Project

**Author:** Freddy Abrahamson<br>
**Date created:** 3-27-2022<br>
**Discipline:** Data Science
## Overview
For this project, I will be comparing different models:

* Knn
* Decision Trees
* Random Forest
* XGBoost

I will run these models with different hyperparameters, to see which one can best predict if a person will, or will not take the h1n1 flu vaccine. I will use the F1 score as my metric.

## Business Problem

As a consequence of the Covid 19 pandemic, there is a renewed interest in the vaccination rates for the seasonal flu. A governmental agency is performing an exploratory analysis to identify who is more lilely to take the seasonal flu vaccine.



## Data Understanding
***
The data comes from the National 2009 H1N1 Flu Survey ('H1N1_Flu_Vaccines.csv'). The data is comprised of 36 columns, including the target variable, and 26,707 rows. Each row represents a respondent in the survey. The target variable, 'h1n1_vaccine', is a binary variable which represents whether the respondent took the vaccine or not. The distribution of the labels is the following:

Raw Counts<br>
0:&emsp;&emsp;21033<br>
1:&emsp;&emsp;5674<br>
Name: h1n1_vaccine, dtype: int64

Percentages<br>
0:&emsp;&emsp;0.787546<br>
1:&emsp;&emsp;0.212454<br>
Name: h1n1_vaccine, dtype: float64<br>


The survey is primarily comprised of binary and multiple choice questions such as:

* 'h1n1_knowledge':&emsp; 0 = No knowledge&emsp;1 = A little knowledge&emsp; 2 = A lot of knowledge
 
*  'health_worker'(binary):&emsp; 0 = no&emsp; 1 = yes


For complete information on the dataset:
https://www.drivendata.org/competitions/66/flu-shot-learning/page/211/<br><br><br>

**<font size='3'>Original Dataframe Before Any Modification:</font>**<br>
![image7](./images/df_info_orig.png)<br><br><br>

df.head()
## Preprocessing Data

<font size='3'>1.&emsp;I will drop certain features:<br>
&emsp;&emsp;1.&emsp;'respondent_id' - since it is a unique identifier<br>
&emsp;&emsp;2.&emsp;'employment_industry','employment_occupation', and 'health_insurance' - about 50% or more records are missing<br> 
2.&emsp;Split the dataframe between the predictors and the dependent variable. The dependent variable is "h1n1_vaccine".<br> 
3.&emsp;Perform a stratified train/test split (default 75%/25%), so that the training and test data sets have the same ratio of minority labels to majority labels.<br>
4.&emsp;Impute all missing values based on most common value in their respectivs columns, on both the training and test sets.<br>
5.&emsp;Applying one-hot encoding, and ordinal encoding to the necessary columns on the training set.
6.&emsp;Applying SMOTE(sampling_strategy=0.40), so that the minority class is 40% of the majority class.<br>
7.&emsp;Applying one-hot encoding, and ordinal encoding to the necessary columns on the test set.
</font><br><br><br>

**<font size='3'>The following is a description of the scaled dataframe after the preprocessing was completed:</font>**<br>
![image5](./images/df_info_after_preproc.png)<br><br><br>

# Classification models:<br>
<font size='3'>I will create 4 different models: knn, decsion tree, random forest, and XGBoost. I will create a baseline model of each, as well as variations on the same model using GridsearchCV. Iwill use the f1 score as a metric. <br>
1.&emsp; Knn basline score: 64.39%<br>
&emsp; &emsp;knn gridsearch parameters:<br>
![image7](./images/knn_params.png)<br><br><br>
2.&emsp; Decision Tree basline score: 60.26%<br>
&emsp; &emsp;Decision Tree gridsearch parameters:<br>
![image7](./images/dec_tree_params.png)<br><br><br>
3.&emsp; Random Forest basline score: 66.96%<br>
&emsp; &emsp;Random Forest gridsearch parameters:<br>
![image7](./images/rand_forest_params.png)<br><br><br>
4.&emsp; XGBoost basline score: 62.14%<br>
&emsp; &emsp;XGBoost gridsearch parameters:<br>
![image7](./images/xgboost_params.png)<br><br><br>

# Choosing the Best Model:<br>
**<font size='3'>The best model is Random Forest (based on the auc score):</font>**<br>
![image5](./images/best_model_df.png)<br><br><br>


# Recreating the  the Best Model:<br>
**<font size='3'>After recreating the best model (Random Forest), with the parameters specified on the dataframe, we recieved an f1 score of 0.6227045075125209.</font>**<br>
**<font size='3'>Confusion Plot for the best model::</font>**<br>
![image5](./images/conf_plot.png)<br><br><br>

## Finding best threshold to optimize F1 score:
**<font size='3'>Based on an optimal threshold of 0.571795, I get an optimal f1 score of 0.646012 .</font>**<br>

# Recommendations
<b>metric</b>: F1 score

<b>model</b>: Random Forest

<b>model parameters</b>: class_weight = 'balanced', criterion = 'gini', max_depth = 5,<br>
                         max_features = 32, min_samples_split = 2, n_estimators = 150,<br>
                         random_state = 42, n_jobs = -1<br>
                              
<b>how to handle class imbalance</b> &emsp;: SMOTE(sampling_strategy=0.40) 

<b>Recommend to place a special emphasis on the following features, since they account for more
than 99% of toal feature importance metric (decrease in the gini impurity score):</b>
* race_White
* opinion_h1n1_sick_from_vacc
* employment_status_Employed
* opinion_seas_sick_from_vacc
* h1n1_knowledge
* age_group
* race_Black
* h1n1_concern
* opinion_seas_vacc_effective
* opinion_seas_risk
* health_worker_0.0
* health_worker_1.0
* doctor_recc_seasonal_1.0
* opinion_h1n1_risk
* doctor_recc_seasonal_0.0
* opinion_h1n1_vacc_effective
* doctor_recc_h1n1_1.0
* doctor_recc_h1n1_0.0
* seasonal_vaccine_1
* seasonal_vaccine_0<br><br><br>






# Project Conclusion: Main Take-aways


**<font size='3'> 1. It is critical to have some understanding of the subject matter one is dealing with, in order to be able to select features, more effectively, and root out non-sensical results.</font>**<br>
**<font size='3'>2. If the ordinal data in one's data set does not have a monotonic relationship, one may wind up with non-sensical results. For example, recommending a downgrade from 'grd_10 Very Good' to 'grd_9 Better', in order to increase the price of the home.</font>**<br>
**<font size='3'>3. High multi-colinearity between features, may lead to unexpected results. For example, one may have two features that are highly correlated, where both have positive Pearson correlations, but one has a negative regression coefficient.</font>**<br>
**<font size='3'>4. The features with the higher Perason correlation coefficient, do not necessarily have a higher regression coefficient. I found this particularly in the 'dummy' variables. I think this may have something to do with these variables not having an equal number of occurences, since only one can be chosen at a time, for each row of data.</font>**<br>
**<font size='3'>5. I understand that the distribution of the residuals do not necessarily have to be perfectly normal, but I am not clear as to what is defined as 'normal enough'.</font>**<br>
**<font size='3'>6. I may have been able to further improve the R-squared score by adding 'sqft_bsmnt' as a binary variable.</font>**<br>
**<font size='3'>7. I originally planned to associate the zip codes with their corresponding average household incomes (per the U.S census bureau),then binning by income groups, and creating interactions, but ultimately decicded against it because it generated too many columns, and didn't lend itself to learning.</font>**<br>
**<font size='3'>8. Log transforming the dependent variable can help approximate a normal residual distribution.</font>**<br>
**<font size='3'>9. Remove rows with outliers in the dependent variable can help approximate a normal residual distribution.</font>**<br><br><br><br>




## For More Information

Please review my full analysis in [my Jupyter Notebook](./student.ipynb) or my[presentation](./DS_Project_Presentation.pdf).<br>
For any additional questions, please contact **Freddy Abrahamson at fred0421@hotmail.com**,<br><br>

## Repository Structure

```
├── README.md                                    <- The top-level README for reviewers of this project
├── student.ipynb                                <- Narrative documentation of analysis in Jupyter notebook
├── Phase_3_Project_Presentation.pdf             <- PDF version of project presentation
└── images                                       <- Images used for this project
```
