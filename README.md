# Time-Series-Analysis-
These are the concepts i have learned in my time-Series analysis learing
(the details are AI generated)

# **Time Series Analysis in Python**

## **Overview**

This repository provides a comprehensive guide to working with time series data in Python using **pandas**. It covers essential concepts like **generating time series data**, **handling missing values**, **shifting data**, **calculating percentage change**, **normalization**, **changing frequency**, **resampling**, and **interpolation**.

## **Getting Started**

### **Installation**

Ensure you have Python installed along with the required libraries:

```bash
pip install pandas numpy matplotlib
```

### **Importing Required Libraries**

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
```

## **1. Generating Time Series Data**

Time series data is often indexed by **datetime**. You can create a datetime index using `date_range()`:

```python
# Creating a DateTime Index
date_rng = pd.date_range(start='2023-01-01', periods=10, freq='D')
df = pd.DataFrame({'value': np.random.randint(1, 100, size=(10,))}, index=date_rng)
print(df)
```

## **2. Handling Missing Values**

Time series data often has missing values. We can handle them using different methods:

```python
# Introduce missing values
df.loc[df.sample(frac=0.2).index, 'value'] = np.nan

# Filling missing values
df_filled = df.fillna(method='ffill')  # Forward Fill
df_filled = df.fillna(method='bfill')  # Backward Fill
df_interpolated = df.interpolate(method='linear')  # Interpolation
print(df_interpolated)
```

## **3. Shifting Data (Past and Future)**

Shifting is useful for creating lag features or forecasting:

```python
# Shifting data forward (future shift)
df['future_value'] = df['value'].shift(-1)

# Shifting data backward (past shift)
df['past_value'] = df['value'].shift(1)
print(df)
```

## **4. Percentage Change (**``**)**

`pct_change()` calculates the percentage change between consecutive data points:

```python
# Percentage Change
df['pct_change'] = df['value'].pct_change()
print(df)
```

🔹 **Use Case:** Identifying trends in stock prices, sales, etc.

## **5. Normalization Techniques**

Normalization scales data between a range to make different features comparable.

### **Using Min-Max Normalization**

```python
df['normalized'] = (df['value'] - df['value'].min()) / (df['value'].max() - df['value'].min())
print(df)
```

### **Using **``**, **``**, and **``

```python
# Subtracting mean
df['sub_normalized'] = df['value'].sub(df['value'].mean())

# Dividing by standard deviation
df['div_normalized'] = df['value'].div(df['value'].std())

# Multiplication scaling
df['mul_scaled'] = df['value'].mul(0.1)
print(df)
```

## **6. Changing Frequency (Resampling, Upsampling, Downsampling)**

### **Downsampling (Reducing Frequency)**

Aggregates data over larger intervals (e.g., daily → weekly):

```python
df_downsampled = df.resample('W').mean()
print(df_downsampled)
```

### **Upsampling (Increasing Frequency)**

Fills missing values after increasing frequency:

```python
df_upsampled = df.resample('H').asfreq().fillna(method='ffill')
print(df_upsampled)
```

### **Custom Resampling with Aggregation**

```python
df_resampled = df.resample('2D').agg({'value': 'sum'})
print(df_resampled)
```

## **7. Interpolation for Missing Data**

Interpolation estimates missing values based on surrounding data:

```python
# Linear Interpolation
df_interpolated = df.interpolate(method='linear')

# Polynomial Interpolation
df_poly = df.interpolate(method='polynomial', order=2)
print(df_interpolated)
```

## **Conclusion**

This repository serves as a quick reference for handling **time series data** in Python. By mastering these techniques, you can effectively **analyze trends, predict future values, and clean missing data** in time-dependent datasets.

