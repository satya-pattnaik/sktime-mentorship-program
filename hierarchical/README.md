# Hierarchical time series

## Introduction

A Time Series which follows an hierarchical aggreagted structure for example across geographical areas.
From Store Level to District Level to State Level and then Country Level. The following picture depicts such a hierarchy(insipred from [M5 Walmart Data](https://www.kaggle.com/c/m5-forecasting-accuracy)). 

![HTS](HTS.png)<br>
The challenge lies in Forecasting across the geographical divisions with highest accuracy.

For any time **t**, the observations at the bottom level will sum up to series above,<br>

 ![matrix_1](matrix_1.png)<br>

 A more general form of representation will be:<br>
 ![matrix_2](matrix_2.png)

where **b<sub>t</sub>** are the base forecasts, **S** is the summation matrix and **$\tilde{y}_h$** the aggragated data.

The matrix form:<br>
![matrix_3](matrix_3.png)

## Approaches for Hierarchichal Forecasting

### The Bottom Up Approach

A simple method for generating coherent forecasts is the Bottom Up Approach.The coherent forecasts are produced by summing up the bottom level individual forecasts. To represent in Matrix notation:<br>
![matrix_4](matrix_4.png)

### The Top Down Approach
This involves generating forecasts at the top level and then disaggregating the forecasts to individual levels based on proportions.

A general notation for this will be

Based on the above example, for store CA_1 the forecast will be:<br>
![matrix_5](matrix_5.png)<br>
where **p** is the historical proportion of sales at **CA_1 store**.
The computation of **p** can be done in two ways:
#### 1.Average historical proportions.
A general notation for this is<br>
![matrix_6](matrix_6.png)<br>

where, **y<sub>j,t</sub>** is the historical value for bottom level series **j** at time **t** and<br> **y<sub>t</sub>** is the total aggregate, over the period t=1,2,...,T.<br><br>
For our example:<br>
![matrix_7](matrix_7.png)<br> 
#### 2.Proportions of the historical averages.<br>
![matrix_8](matrix_8.png)<br>

## The Optimal Reconciliation

Considering a more generalized form to represent Hierarchical Forecasts, the mapping matrices can be represented as:<br>
![matrix_9](matrix_9.png)<br>
where **S** is summation matrix and *$\hat{y}$* are the set of base forecasts. The optimal reconciliation approach is about finding the optimal **G** matrix which in turn provides the most accurate coherent forecasts $\tilde{y}$.<br>

[More Reading](https://otexts.com/fpp2/reconciliation.html)


## API Design-Example Usage(To be Modified based on feedback)
**Load Time Series**

```
ts = load_data()
ts
```
![time_series_data](e_prop_1.png)<br>

**Aggregate Data**(For each state i.e CA and TX)
```
ts["CA"] = ts["CA_1"] + ts["CA_2"]
ts["TX"] = ts["TX_1"] + ts["TX_2"]
ts
```
![time_series_data_agg](e_prop_2.png)<br>

**Create a Placeholder DataFrame for Forecasts**
```
forecasts = pd.DataFrame(columns=["CA_1","CA_2","TX_1","TX_2"],
                         index=["May 2017","Jun 2017","Jul 2017","Aug 2017"])
forecasts
```
**Use Sktime Forecasters for Base Forecasts**
```
from sktime.forecasting.all import *

for i in range(4):
    fh = ForecastingHorizon(forecasts.index, is_relative=False)
    forecaster = AutoARIMA()
    forecaster.fit(ts[:,i])
    forecasts.iloc[:,i] = forecaster.predict(fh) 

print(forecasts)
```

![forecasts](e_prop_3.png)<br>

**Hierarchical Forecast Design Sketch**
(This will generate the coherent forecasts across the hierarchy)

```
hierarchical_reconciler = HierarchicalReconcile(method="ols")
hierarchical_reconciler.fit(ts)
coherent_forecasts = hierarchical_reconciler.predict(forecasts)
print(coherent_forecasts)
```
![coherent_forecasts](e_prop_4.png)<br>

## References
1. https://otexts.com/fpp2/hierarchical.html
2. [R Package](https://cran.r-project.org/web/packages/hts/index.html) 
3. [Python Package](https://pypi.org/project/scikit-hts/)

## Examples(Python Package)
1. [ScikitHTS Example](https://github.com/carlomazzaferro/scikit-hts)
2. [Example Notebook in Python](https://colab.research.google.com/drive/1thHtaUS-8boRRVqZ1pYiog8zpljndxAu?usp=sharing)
