## Nelson Turbine Maintenance Model

Predict turbine engine maintenance tasks, eliminating the downtime caused by reactive maintenance, and reducing the cost of preventative maintenance.

### Datasets

Maintenance Data Sets: https://ti.arc.nasa.gov/tech/dash/groups/pcoe/prognostic-data-repository/#turbofan Includes:

* Unit number
* Time, in cycles
* Operational setting 1
* Operational setting 2
* …
* Operational setting 26

### Modeling Strategy

* Predict remaining operational cycles before failure (remaining useful life) based on high pressure compressor (HPC) degradation

1. Apply XGboost algorithm
2. Positives of xgboost algorithm include flexibility for non-linear relationships, robustness to outliers
3. We also considered DeepAR model, but our use case does not include a forecasted time series target; we instead want a predicted single value of RUL, so we did not see DeepAR as applicable
  *Look for hyper-parameter tuning opportunities for optimal performance of the model
  *Model options:
    *1 model using 1 condition test data plus 1 model usig 6-condition test data
    *1 combined model w/feature for combined data set
  *Predict RUL based on current cycle without context of other cycles; time permitting, create additional model features to capture time-series history within unit
 

### End Goal

Improve operational safety of aircraft through improved prognostics. Alert operations staff that an engine needs maintenance since a key component is nearing end of useful life. This will implement a maintenance strategy that avoids failure reactions as well as expensive but unnecessary routine maintenance. These improvements will allow operators to:

1. Change operational characteristics (such as load) which in turn may prolong the life of the component
2. Plan upcoming maintenance
3. Initiate logistics to transition from faulty equipment to fully functional

If time permitted, this project would:
  * Predict remaining useful life (RUL) for fan degradation
  * Explore other models