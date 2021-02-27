# Hierarchical time series

## Introduction

A Time Series which follows an hierarchical aggreagted structure for example across geographical areas.
From Store Level to District Level to State Level and then Country Level. The following picture depicts such a hierarchy(insipred from [M5 Walmart Data](https://www.kaggle.com/c/m5-forecasting-accuracy)). 

![HTS](HTS.png)<br>
The challenge lies in Forecasting across the geographical divisions with highest accuracy.

For any time **t**, the observations at the bottom level will sum up to series above,<br>
$$
\begin{aligned}
 y_{Total} &= y_{CA} + Y_{TX} \\
 y_{CA} &= y_{CA_1} + Y_{CA_2} \\
 y_{TX} &= y_{TX_1} + Y_{TX_2}
\end{aligned}
$$
 A more general form of representation will be:
 $$\tilde{y}_h=S\hat{b}_t$$

where **b<sub>t</sub>** are the base forecasts, **S** is the summation matrix and **$\tilde{y}_h$** the aggragated data.

The matrix form:
$$\begin{bmatrix} y_{Total} \\ y_{CA}\\ y_{TX} \\  y_{CA_1} \\ y_{CA_2} \\  y_{TX_1} \\ y_{TX_2}\end{bmatrix} = \begin{bmatrix} 1 & 1 & 1 & 1 \\ 1 & 1 & 0 & 0 \\ 0 & 0 & 1 & 1 \\ 1 & 0 & 0 & 0 \\ 0 & 1 & 0 & 0 \\ 0 & 0 & 1 & 0 \\ 0 & 0 & 0 & 1 \end{bmatrix} \begin{bmatrix}y_{CA_1} \\ y_{CA_2} \\  y_{TX_1} \\ y_{TX_2}\end{bmatrix}$$

## Approaches for Hierarchichal Forecasting

### The Bottom Up Approach

A simple method for generating coherent forecasts is the Bottom Up Approach.The coherent forecasts are produced by summing up the bottom level individual forecasts. To represent in Matrix notation:<br>
$$\begin{bmatrix} \tilde{y}_{Total} \\ \tilde{y}_{CA}\\ \tilde{y}_{TX} \\  \tilde{y}_{CA_1} \\ \tilde{y}_{CA_2} \\  \tilde{y}_{TX_1} \\ \tilde{y}_{TX_2}\end{bmatrix} = \begin{bmatrix} 1 & 1 & 1 & 1 \\ 1 & 1 & 0 & 0 \\ 0 & 0 & 1 & 1 \\ 1 & 0 & 0 & 0 \\ 0 & 1 & 0 & 0 \\ 0 & 0 & 1 & 0 \\ 0 & 0 & 0 & 1 \end{bmatrix} \begin{bmatrix}\hat{y}_{CA_1} \\ \hat{y}_{CA_2} \\  \hat{y}_{TX_1} \\ \hat{y}_{TX_2}\end{bmatrix}$$

### The Top Down Approach
This involves generating forecasts at the top level and then disaggregating the forecasts to individual levels based on proportions.

A general notation for this will be

Based on the above example, for store CA_1 the forecast will be:
\begin{aligned}
 \tilde{y}_{CA} &= p.\hat{y}_{Total} 
\end{aligned}<br>
where **p** is the historical proportion of sales at **CA_1 store**.
The computation of **p** can be done in two ways:
#### 1.Average historical proportions.
A general notation for this is
\begin{aligned}
p_j=\frac{1}{T}\sum_{t=1}^{T}\frac{y_{j,t}}{{y_t}}
\end{aligned}<br>

where, **y<sub>j,t</sub>** is the historical value for bottom level series **j** at time **t** and<br> **y<sub>t</sub>** is the total aggregate, over the period t=1,2,...,T.<br><br>
For our example:
\begin{aligned}
p = \frac{1}{T}\sum_{t=1}^{T}\frac{y_{CA_1,t}}{{y_{Total}}}
\end{aligned}<br> 
#### 2.Proportions of the historical averages.
\begin{aligned}
p_j={\sum_{t=1}^{T}\frac{y_{j,t}}{T}}\Big/{\sum_{t=1}^{T}\frac{y_t}{T}}
\end{aligned}<br>

## The Optimal Reconciliation

Considering a more generalized form to represent Hierarchical Forecasts, the mapping matrices can be represented as:
\begin{equation}
  \tilde{y}_h=SG\hat{y}_h
\end{equation}
where **S** is summation matrix and *$\hat{y}$* are the set of base forecasts. The optimal reconciliation approach is about finding the optimal **G** matrix which in turn provides the most accurate coherent forecasts $\tilde{y}$.

## API Design
```
base_forecasts = ['CA_1','CA_2','TX_1','TX_2'] 
recnociled_forecasts = reconcile(base_forecasts,method='bu')
```

## References
1. https://otexts.com/fpp2/hierarchical.html
2. [R Package](https://cran.r-project.org/web/packages/hts/index.html) 
