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

After hyperparameter tuning, a decision tree regressor with      was selected.
Decision trees are usually less accurate than random forests but are better for interpretability of the model.

### Random forest regressor ###

Random forests average over many decision trees on bootstrapped samples of the data to improve accuracy and prevent overfitting.
With hyperparameter tuning, the best random forest regressor had 

### AdaBoost regressor ###

This AdaBoost regressor starts with a decision tree and continually improves the model by giving weights to the data based on the prediction error.
Hyperparameter tuning gave 

### XGBoost regressor ###

XGBoost is another typing of boosting; the difference is that XGBoost trains on the errors.
After hyperparameter tuning, we had

Classification models
---------------------------------------------

### Logistic regression ###

The logistic regression model estimates the probability that a flight will be delayed by fifteen minutes or more using the logistic function.
If the value of the logistic function is greater than a certain cutoff point, it will predict that the flight is delayed.

### Random forest classifier ###

### XGBoost classifier ###


Results
---------------

Three models are given in this repository: the baseline model, and modified versions of InceptionV3 and MobileNetV2. 
On the testing set, the accuracies of the models are as follows: Baseline--88%, InceptionV3--94%, and MobileNetV2--95%.

The precision and recall for the MobileNetV2 model are displayed in the table below.

| Regression model       | Validation error | Test error     |
| :---        |    :----:        |      :---: |
|  Baseline model    |             |          |
|  Linear regression   | Title            | Here's this   |
|  with Lasso Regularization   | Text             | And more      |
|  Decision tree regressor     |                          |      |
|  Random forest regressor    |                        |         |
|  AdaBoost regressor     |                           |         |
|  XGBoost regressor       |                          |         |

| Classification model  |                   |              |
|     :-----         |  :------:         |   :-----:        |
|   Logistic regression           |                  |           |          
|   Random forest classifier      |                  |           |
|   XGBoost classifier           |                  |           |

[^1]: https://www.faa.gov/data_research/aviation_data_statistics/media/cost_delay_estimates.pdf
[^2]: https://www.kaggle.com/datasets/deepankurk/flight-take-off-data-jfk-airport

