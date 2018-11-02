## Nelson Turbine Maintenance Model

This project is a training exercise for a 3-day AWS SageMaker training. The project goal is to predict turbine engine maintenance needs, eliminating the downtime caused by reactive maintenance, and reducing the cost of preventative maintenance.

### Contents

These Jupyter notebooks are used: 

* etl.ipynb - This covers reading in the CSV data files and preparing them for modeling. 
* modeling.ipynb - Partitions the data, and runs through SageMaker's built-in XGBoost model. 
* hyperparameters.ipynb - Code for hyperparameter tuning of the XGBoost model. 

### Datasets

Maintenance Data Sets: https://ti.arc.nasa.gov/tech/dash/groups/pcoe/prognostic-data-repository/#turbofan 

Data fields: 

* Unit number
* Time, in cycles
* Operational setting 1
* Operational setting 2
* Operational setting 3
* Sensor measurement 1
* ...
* Sensor measurement 26

The training data set contains one record per unit until failure. The testing data set truncates each unit some time prior to failure. Remaining Useful Life (RUL) in cycles for the test set units is given in separate files. 

### Modeling Strategy

* We will predict remaining operational cycles before failure (remaining useful life) based on high pressure compressor (HPC) degradation.
* We are planning to start with an xgboost model as a regression-type excercise. 
    * Positives of xgboost algorithm include flexibility for non-linear relationships, robustness to outliers.
    * We also considered DeepAR model, but our use case does not include a forecasted time series target. We instead want a predicted single value of RUL, so we did not see DeepAR as applicable.
* We will look for hyper-parameter tuning opportunities for optimal performance of the model.
  *Model options:
    *1 model using 1 condition test data plus 1 model usig 6-condition test data
    *1 combined model w/feature for combined data set
  *Predict RUL based on current cycle without context of other cycles; time permitting, create additional model features to capture time-series history within unit

Summary of modeling approach: 

| TARGET     | Remaining cycles until HPC Degradation failure, calculated in the Training  set as max cycle for the unit, minus current cycle.                                                                                                                                                                                                                                                                   |
|------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| POPULATION | We will focus for this exercise on HPC Degradation, not Fan Degradation.  We are therefore focusing in on data sets FD001 and FD002, which are HPC  Degradation only, while FD003 and FD004 are both HPC Degradation and  Fan Degradation. (If there were more time, we would explore incorporating the 3rd and 4th data sets, in a combined model or a separate model, and compare performance.) |
| DATA PREP  | For xgboost model, we do not need to standardize/normalize our inputs.  We will need to ensure only numeric inputs.  Our data as-is does not capture time series history of each unit, because the xgboost model will treat each observation as independent; therefore we will need to do feature engineering to capture changes in sensor readings over time.                                    |


### End Goal

Improve operational safety of aircraft through improved prognostics. Alert operations staff that an engine needs maintenance since a key component is nearing end of useful life. This will implement a maintenance strategy that avoids failure reactions as well as expensive but unnecessary routine maintenance. These improvements will allow operators to:

1. Change operational characteristics (such as load) which in turn may prolong the life of the component
2. Plan upcoming maintenance
3. Initiate logistics to transition from faulty equipment to fully functional

If time permitted, this project would:
  * Predict remaining useful life (RUL) for fan degradation.
  * Explore other models. Some papers that have looked at this data set have used recurrent neural networks, for example in the Keras framework. 
