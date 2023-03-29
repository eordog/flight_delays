# Predicting flight delays from JFK

This repository contains models that can both predict whether a flight will be delayed by fifteen minutes or more and predict how long a flight will be delayed given information about the flight (scheduled departure time, distance, destination, etc.) and the local weather.

Background
----------

The Federal Aviation Administration estimated that in 2019, airline delays had a cost of $8.3 billion dollars to airlines and $18.1 billion to customers. [^1]
The ability to predict flight delays is of value to both the airline and the consumer, and an understanding of the strongest predictors of delays can help them.

This repository contains models both to predict the number of minutes a flight will be delayed and to predict whether a flight will be delayed by fifteen minutes or more. 
As the models only rely on flight information (scheduled departure time, distance, destination, etc.) and local weather information (temperature, wind speed, condition) that can often be known well in advance, these models can be used to give airlines and customers a good prediction of whether or not their flight will be delayed.
Both the airline and the customer can factor this prediction into their plans.

Data
-------

The dataset contains approximately 28,000 samples of data for flights departing from JFK airport between November 30, 2019 and January 30, 2020. 
It contains the following features:
* weather data:
  * temperature
  * dew point
  * humidity
  * wind
  * wind speed
  * wind gust
  * pressure
  * condition
* date
* flight information:
  * carrier code
  * destination
  * scheduled departure time
  * scheduled arrival time
  * scheduled duration
  * distance
* airport information:
  * number of flights scheduled to arrive that day
  * number of flights scheduled to depart that day
 
The flight information is from the Bureau of Transportation Statistics, and the dataset as a whole is from Kaggle.[^2]

Data preprocessing was performed in the notebook for linear regression.

Regression Models
---------------------

### Baseline Model ###

The baseline model uses the mean flight delay time (measured in minutes). 

### Linear regression with lasso regularization ###

Multiple linear regression with lasso regularization is performed using all of the features as either linear terms or dummy variables. 
Analysis of the conditions of linear regression shows that the normality of the residuals is not satisfied; the omnibus is very high.
Applying a log-transformation and a square root-transformation did not fix the problem.
This suggests that linear regression may not be an appropriate model to use; however, normality of the residuals is not as important a condition to satisfy as some of the others.

Lasso regularization is used to reduce the complexity of the model and perform feature selection.
This helps to get rid of redundant and less useful data.

With more time, I would see if the continuous variables have a polynomial relationship to the target; the basic multiple linear regression model assumes that there is a linear relationship, which is not necessarily the case.
I would also add interaction terms to the model.

### Decision tree regressor ###

Decision trees make splits in the data to minimize the error after averaging the data placed in each group.
Decision trees are usually less accurate than random forests but are better for interpretability of the model.
The hyperparameters tuned were max depth, max features, minimum samples per leaf, minimum samples split, minimum weight fraction per leaf, and splitter.

### Random forest regressor ###

Random forests average over many decision trees on bootstrapped samples of the data to improve accuracy and prevent overfitting.
With hyperparameter tuning, the best random forest regressor had 200 estimators and no maximum depth or maximum number of features.

### AdaBoost regressor ###

This AdaBoost regressor starts with a decision tree and continually improves the model by giving weights to the data based on the prediction error.
Hyperparameter tuning gave a learning rate of 0.012, 7 estimators, and the decision tree baseline estimator had a max depth of 20.

### XGBoost regressor ###

XGBoost is another type of boosting; the difference is that XGBoost trains on the errors.
The best model had a learning rate of 0.3, a maximum depth of 6, number of estimators 200, and all features used for each node in the tree.


Classification models
---------------------------------------------

### Logistic regression ###

The logistic regression model estimates the probability that a flight will be delayed by fifteen minutes or more using the logistic function.
If the value of the logistic function is greater than a certain cutoff point, it will predict that the flight is delayed.
The best cutoff point for logistic regression was 0.177.

### Random forest classifier ###
After hyperparameter tuning, the best random forest classifier had 200 estimators, a maximum depth of 100, entropy as the splitting criterion, and the square root of the number of features used to determine each split.

### XGBoost classifier ###
After hyperparameter tuning, the best XGBoost classifier had a learning rate of 0.3, maximum depth of 7, and number of estimators 500.

Results
---------------

The validation and test set error for the regression models are below. 
The best model was the XGBoost regressor, with a test set RMSE of 25.802.

| Regression model       | Validation RMSE | Test RMSE    |
| :---        |    :----:        |      :---: |
|  Baseline model    |       37.025      |      36.235    |
|  Linear regression   | 36.477            | 35.478   |
|  Decision tree regressor     |              33.278            |   32.411   |
|  Random forest regressor    |             27.609           |    27.926     |      
|  AdaBoost regressor     |           29.978                |    30.775     |
|  XGBoost regressor       |              23.496            |     25.802    |

The following scores for classification models are given for the test set. 
The best model was the XGBoost classifier, with an F1 score of 0.607.

| Classification model  |             Precision        | Recall | F1 score | 
|     :-----         |  :------:         |   :-----:        |   :----:|
|   Logistic regression           |          0.257        |  0.405         | 0.315|          
|   Random forest classifier      |         0.841         |  0.145         | 0.248|
|   XGBoost classifier           |          0.817        |    0.483       |  0.607 |

Below is the normalized confusion matrix for the XGBoost classifier.

![confusion_matrix_xgboost](https://user-images.githubusercontent.com/97986688/228402553-facd3323-a545-4797-b0b5-2a46dd9e860b.png)

[^1]: https://www.faa.gov/data_research/aviation_data_statistics/media/cost_delay_estimates.pdf
[^2]: https://www.kaggle.com/datasets/deepankurk/flight-take-off-data-jfk-airport
