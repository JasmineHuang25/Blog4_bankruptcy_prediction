# How accurate is the accuracy rate?
## - Using company bankruptcy prediction as an example

## Introduction
What other abilities should a good Data Scientist have besides advanced technical skills?  In my opinion, knowing your business and stakeholder always comes first.  
After all, we are utilizing our skills to solve people's problems.  With that concept, we need to be very careful how we interpret and deliver our findings from 
the research.  Misleading or wrong interpretations might put the company or stakeholder at great risk.

This blog will use company bankruptcy prediction as an example to prove that we can’t just trust the accuracy rate.  Providing accuracy rate alone to the stakeholders 
is actually far from being accurate.


## About the dataset
The data were collected from the Taiwan Economic Journal for the years 1999 to 2009. Company bankruptcy was defined based on the business regulations of the Taiwan 
Stock Exchange.  The data has 6819 instances and 96 attributes all stored as integers.  Except the target [ ‘Bankrupt?’ ], all the other features have been 
pre-processed, scaled and ready for modeling.

The target [ ‘Bankrupt?’ ] is imbalanced.  Will need to try SMOTE to handle the situation later.

![Alt Image text](Data/webpages.jpg)


## Modeling and finding

### Model 1: Dummy Classifier Model
Firstly, build the dummy model as the baseline model.
Finding: The accuracy score on training data is 96.96% and 0% on the testing data.  Looking into the confusion matrix, this model can’t predict any positive label.

![Alt Image text](Data/webpages.jpg)

### Model 2: Logistic Regression Model
After the dummy model, try the Logistic Regression Model as the first simple model.
Finding: The accuracy score on training data is 96.67% and 2.16% on the testing data.  Looking into the confusion matrix, this model can pick up a couple 
positive labels but also increase False Positive and false Negative.

![Alt Image text](Data/webpages.jpg)

### Model 3: Random Forest Model
Finding: The accuracy score on training data is 99.98% and 99.28% on the testing data. The accuracy scores are incredible!  How true is that?  Let’s look 
into the confusion matrix.  This model is doing much better compared to the Dummy Model and Logistic Regression Model.  Especially at training, this model 
can pick up more positive labels along with only 1 False Negative.  However,  the test result is not as good as training.  So far, this model seems to perform well.

![Alt Image text](Data/webpages.jpg)

### Model 4: Gradient Booster Model
Finding: The accuracy score on training data is 99.58% and 86.33% on the testing data.  Also need to look into its confusion matrix.  Compared to the Random 
Forest Model, this model does a little better at testing.  Will consider to tune and tweak this model for better performance.

![Alt Image text](Data/webpages.jpg)

### Model 5: Decision Tree Model
Finding: The accuracy score on training data is 100% and 100% on the testing data.  Looks like we have found the BEST MODEL!  Is this true?  Let’s look into 
its confusion matrix.  This model is doing better than the Gradient Booster Model but it’s not 100% PERFECT.  This model will generate False Positives and 
False Negatives as well.  Will tune and tweak this model to see if it gets better results.

![Alt Image text](Data/webpages.jpg)

### Model 6: Tuning and cross-validating Gradient Booster Model
Finding: The accuracy score on training data is 97.26% and 16.54% on the testing data.  Look into its confusion matrix.  It doesn’t seem like this model is 
any better than the Decision Tree Model.

![Alt Image text](Data/webpages.jpg)

### Model 7: Tuning and cross-validation Decision Tree Model
Finding: The accuracy score on training data is 97.57% and 30.22% on the testing data.  Now, check its confusion matrix.  This model may perform a little better
than the tuned Gradient Booster Model.

![Alt Image text](Data/webpages.jpg)

### Model 8: SMOTE (sampling_strategy = “auto”) with Gradient Booster Model
Finding: The accuracy score on training data is 96.10% and 93.52% on the testing data.  The accuracy scores look good but we still need to check its confusion 
matrix.  Looks like SMOTE does perform better than the previous models.

![Alt Image text](Data/webpages.jpg)

### Model 9: SMOTE (sampling_strategy = 0.8) with Gradient Booster Model
Finding: The accuracy score on training data is 96.74% and 92.81% on the testing data.  The accuracy scores are slightly less than the previous model.  Look 
into its confusion matrix.  Changing the sampling strategy doesn’t seem to make much improvement.

![Alt Image text](Data/webpages.jpg)

### Model 10: SMOTE with Decision Tree Model
Finding: The accuracy score on training data is 86.38% and 88.49% on the testing data.  The accuracy scores are smaller than the previous 2 models.  Now, let’s 
check its confusion matrix.  This model seems to have the least False Negative, however the False Positive is larger than all the other models.

![Alt Image text](Data/webpages.jpg)


## Inspect accuracy scores for all models

![Alt Image text](Data/webpages.jpg)

By looking at accuracy, recall, precision and F1 sores, it’s extremely hard to determine which is the best model.  If we only look at the accuracy scores, 
Decision Tree Model seems to be the best model with the perfect scores followed by Random Forest Model and SMOTE with Gradient Booster Model.  Then we consider 
their F1 scores, we could see the Decision Tree Model is doing okay.  And, the SMOTE with Gradient Booster Model is better than the Random Forest Model with 
less overfitting problem.  However, with all the information provided, it is still hard to name the best model.


## Let’s bring in the miss rate

![Alt Image text](Data/webpages.jpg)

If we are doing this research for investors, their main goal is to make profit.  However, they have a bigger concern, which is to put the money into the 
company who is actually bankrupt but the prediction says they are not (False Negative).  With this concern, we shall bring in the miss rate.

The miss rate is also known as false negative rate.  It is the probability that a true positive will be missed by the test. It's calculated as FN/FN+TP, where 
FN is the number of false negatives and TP is the number of true positives (FN+TP being the total number of positives).  

The SMOTE with Decision Tree Model has the lowest miss rate of 12.3% with moderate accuracy rates.  However, its f1 scores are horrible which means this model 
is sacrificing and increasing False Positive while trying decreasing False Negative.  Please revisit its confusion matrix shown earlier for visualization and 
better understanding.

The alternative choice would be the SMOTE with Gradient Booster Model.  It has the second lowest miss rate of 33.3% with high accuracy rates, and f1 scores.


## Conclusion
After models have been constructed, a good Data Scientist should carefully check into all measurements before deciding which is the best model.  Using this 
company bankruptcy prediction as an example, if we just trust the accuracy rates without looking into their confusion matrix and other scores, we would pick 
the model that fails to predict positive labels.  The model would place the investors in a great danger of losing money if the model generates a False Negative
prediction.  This is why we can’t just the accuracy rates.  And, it is always very important for us, Data Scientists,  to understand the business and to know 
the stakeholders before we dive into the data. 


### Reference and data resource
[Company Bankruptcy Prediction | Kaggle](https://www.kaggle.com/datasets/fedesoriano/company-bankruptcy-prediction)
