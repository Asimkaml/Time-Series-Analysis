# Time-Series-Analysis-
These are the concepts i have learned in my time-Series analysis learing
(the details are mostly AI generated)

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
Loading and Preparing Time Series Data
A time series dataset typically contains timestamps and values (e.g., stock prices).
```python
import pandas as pd  

# Load stock data  
df = pd.read_csv('stock.csv', parse_dates=['Date'], index_col='Date')  

# Display first few rows  
print(df.head())
```  
Why Use parse_dates and index_col?
✅ parse_dates=['Date'] → Converts the "Date" column into datetime format for time-based analysis.
✅ index_col='Date' → Sets the "Date" column as the index, which is essential for resampling and shifting.


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

# Check for missing values  
print(df.isnull().sum())  

# Forward fill: Fill missing values with the last known value  
df.ffill(inplace=True)  

# Backward fill: Fill missing values with the next available value  
df.bfill(inplace=True)  
Linear interpolation: Estimate missing values based on trend  
df.interpolate(method='linear', inplace=True)
```
Why Use Different Filling Methods?
✅ ffill() → Works well for stock prices, assuming the last value remains valid.
✅ bfill() → Fills gaps when future data is available.
✅ interpolate() → Useful when values change gradually, like temperature data.


## **3. Shifting Data (Past and Future)**

Shifting allows us to compare current values with past/future values, useful in trend analysis & forecasting.

```python
# Shifting data forward (future shift)
df['future_value'] = df['value'].shift(-1)

# Shifting data backward (past shift)
df['past_value'] = df['value'].shift(1)
```
Why Use Shifting?
✅ Helps in predictive modeling by comparing today's price with tomorrow’s.
✅ Useful for calculating returns and detecting trends.

## **4. Percentage Change (**`pct_change()`**)**

`pct_change()` calculates the percentage change between consecutive data points:
shows the rate of change in stock prices over time.
```python
df['Daily_Return'] = df['Close'].pct_change()
```

🔹 **Use Case:** Identifying trends, growth rates in stock prices, sales, etc.

## **5. Normalizing Data for Comparison**
Normalization scales values for easier comparison across different stocks.

```python
# Min-Max Normalization  
df['Normalized_Close'] = (df['Close'] - df['Close'].min()) / (df['Close'].max() - df['Close'].min())  


df['Norm_Sub'] = df['Close'].sub(df['Close'].min()).div(df['Close'].max() - df['Close'].min())  
df['Norm_Mul'] = df['Close'].mul(2)  # Example: Double all values
```
Why Normalize?
✅ Allows comparison between stocks with different price ranges.
✅ Helps machine learning models process data effectively.

## **6. Changing Frequency (Resampling, Upsampling, Downsampling)**

### ** Resampling(changing)
Financial data is often recorded daily, but we may need weekly or monthly trends.
```python
df_weekly = df.resample('W').mean()  # Convert daily to weekly
```
### **Downsampling (Reducing Frequency)**

Aggregates data over larger intervals (e.g., daily → weekly):
```python
df_downsampled = df.resample('W').mean()
df_weekly = df.resample('W').mean()  # Convert daily to weekly  
```

### **Upsampling (Increasing Frequency)**

Fills missing values after increasing frequency:

```python
df_upsampled = df.resample('H').asfreq()  # Convert daily to hourly (creates NaNs)  
df_upsampled = df_upsampled.interpolate(method='linear')  # Fill missing values
```

### **Custom Resampling with Aggregation**

```python
df_resampled = df.resample('2D').agg({'value': 'sum'})
print(df_resampled)
```
Why Change Frequency?
✅ Upsampling → Creates finer granularity (e.g., converting daily data to hourly).
✅ Downsampling → Reduces noise & improves trend detection (e.g., monthly stock analysis).
## **7. Interpolation for Missing Data**

Interpolation estimates missing values based on surrounding data:

```python
# Linear Interpolation
df_interpolated = df.interpolate(method='linear')

# Polynomial Interpolation
df_poly = df.interpolate(method='polynomial', order=2)
print(df_interpolated)
```
Why Use Interpolation?
✅ Stock market closure days (e.g., weekends, holidays).
✅ Filling missing sensor data in IoT applications.
✅ Smoothens data for plotting & analysis.
## **Conclusion**

This repository serves as a quick reference for handling **time series data** in Python. By mastering these techniques, you can effectively **analyze trends, predict future values, and clean missing data** in time-dependent datasets.

