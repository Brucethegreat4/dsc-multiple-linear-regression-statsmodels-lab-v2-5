# Multiple Linear Regression in StatsModels - Lab

## Introduction
In this lab, you'll practice fitting a multiple linear regression model on the Ames Housing dataset!

## Objectives

You will be able to:

* Perform a multiple linear regression using StatsModels
* Visualize individual predictors within a multiple linear regression
* Interpret multiple linear regression coefficients from raw, un-transformed data

## The Ames Housing Dataset

The [Ames Housing dataset](http://jse.amstat.org/v19n3/decock.pdf) is a newer (2011) replacement for the classic Boston Housing dataset. Each record represents a residential property sale in Ames, Iowa. It contains many different potential predictors and the target variable is `SalePrice`.


```python
import pandas as pd
ames = pd.read_csv("ames.csv", index_col=0)
ames
```


```python
ames.describe()
```

We will focus specifically on a subset of the overall dataset. These features are:

```
LotArea: Lot size in square feet

1stFlrSF: First Floor square feet

GrLivArea: Above grade (ground) living area square feet
```


```python
ames_subset = ames[['LotArea', '1stFlrSF', 'GrLivArea', 'SalePrice']].copy()
ames_subset
```

## Step 1: Visualize Relationships Between Features and Target

For each feature in the subset, create a scatter plot that shows the feature on the x-axis and `SalePrice` on the y-axis.

import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
import statsmodels.api as sm
from sklearn.linear_model import LinearRegression
from sklearn.metrics import r2_score

# Load dataset
ames = ['LotArea', '1stFlrSF', 'GrLivArea', 'SalePrice']
ames_subset = pd.read_csv("ames.csv", usecols=names)
```python
# Your code here - import relevant library, create scatter plots
import
# Step 1: Visualizing Relationships
fig, axes = plt.subplots(1, 3, figsize=(15, 5))
for idx, col in enumerate(names[:-1]):
    sns.scatterplot(x=ames_subset[col], y=ames_subset['SalePrice'], ax=axes[idx])
    axes[idx].set_title(f"SalePrice vs {col}")
plt.show()
```


```python
# Your written answer here - do these seem like good candidates for linear regression?
```

## Step 2: Build a Simple Linear Regression Model

Set the dependent variable (`y`) to be the `SalePrice`, then choose one of the features shown in the subset above to be the baseline independent variable (`X`).

Build a linear regression using StatsModels, describe the overall model performance, and interpret its coefficients.


```python
# Your code here - define y and baseline X
```
y = ames_subset['SalePrice']
X_baseline = ames_subset[['GrLivArea']]  # Baseline independent variable
X_baseline = sm.add_constant(X_baseline)  # Add constant term for intercept



```python
# Your code here - import StatsModels, fit baseline model, display results
```
model_simple = sm.OLS(y, X_baseline).fit()
print(model_simple.summary())
```python
# Your written answer here - interpret model results
```
# Interpretation of Simple Linear Regression Results
print("\nInterpretation of Simple Linear Regression Results:")
print(f"R-squared: {model_simple.rsquared:.4f}")
print(f"Intercept: {model_simple.params[0]:.4f}")
print(f"Coefficient for GrLivArea: {model_simple.params[1]:.4f}")
print("This suggests that for each additional square foot in above-ground living area, the sale price increases by the coefficient value. The R-squared value indicates the proportion of variance in SalePrice explained by GrLivArea.")


## Step 3: Build a Multiple Linear Regression Model

For this model, use **all of** the features in `ames_subset`.


```python
# Your code here - define X
```


```python
# Your code here - fit model and display results
```
model_multiple = sm.OLS(y, X).fit()
print(model_multiple.summary())
```python
# Your written answer here - interpret model results. Does this model seem better than the previous one?
```
print("\nInterpretation of Multiple Linear Regression Results:")
print(f"R-squared: {model_multiple.rsquared:.4f}")
print(f"Intercept: {model_multiple.params[0]:.4f}")
print("Coefficients:")
print(model_multiple.params[1:])

print("\nComparison to Simple Linear Regression:")
if model_multiple.rsquared > model_simple.rsquared:
    print("The multiple linear regression model has a higher R-squared value, meaning it explains more variance in SalePrice compared to the simple linear regression model. This suggests that including multiple predictors improves the model's explanatory power.")
else:
    print("The simple linear regression model performed better, indicating that additional predictors may not significantly improve the model.")

## Step 4: Create Partial Regression Plots for Features

Using your model from Step 3, visualize each of the features using partial regression plots.


```python
# Your code here - create partial regression plots for each predictor
```
# Step 4: Partial Regression Plots
fig = plt.figure(figsize=(12, 8))
plot_partregress_grid(model_multiple, fig=fig)
plt.show()

```python
# Your written answer here - explain what you see, and how this relates
# to what you saw in Step 1. What do you notice?
```
# Interpretation of Partial Regression Plots
print("\nStep 4 Interpretation:")
print("The partial regression plots show the relationship between each predictor and SalePrice while accounting for the effects of the other variables in the model.")
print("- If a predictor shows a strong linear trend in the plot, it suggests that this variable contributes significantly to predicting SalePrice.")
print("- If a predictor has a weak or random distribution, it may not be a strong predictor of SalePrice.")
print("These plots reinforce the observations from Step 1, where we initially saw scatter plots of individual features. However, now we can see how each variable contributes after removing the effects of other predictors.")

## Level Up (Optional)

Re-create this model in scikit-learn, and check if you get the same R-Squared and coefficients.


```python
# Your code here - import linear regression from scikit-learn and create and fit model
```
lr = LinearRegression()
lr.fit(X.drop(columns=['const']), y)
y_pred = lr.predict(X.drop(columns=['const']))

```python
# Your code here - compare R-Squared
```
print(f"StatsModels R-squared: {model_multiple.rsquared:.4f}")
print(f"Scikit-learn R-squared: {r2_score(y, y_pred):.4f}")


```python
# Your code here - compare intercept and coefficients
```
print(f"Intercept (StatsModels): {model_multiple.params[0]:.4f}")
print(f"Intercept (Scikit-learn): {lr.intercept_:.4f}")
print("Coefficients (StatsModels):")
print(model_multiple.params[1:])
print("Coefficients (Scikit-learn):")
print(lr.coef_)

## Summary
Congratulations! You fitted your first multiple linear regression model on the Ames Housing data using StatsModels.
