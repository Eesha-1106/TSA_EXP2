# Ex.No: 02 LINEAR AND POLYNOMIAL TREND ESTIMATION
Date:28-04-26
### AIM:
To Implement Linear and Polynomial Trend Estiamtion Using Python.

### ALGORITHM:
Import necessary libraries (NumPy, Matplotlib)

Load the dataset

Calculate the linear trend values using least square method

Calculate the polynomial trend values using least square method

End the program
### PROGRAM:
```py
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

data=pd.read_csv('/content/Chocolate Sales (2).csv',parse_dates=['Date'],index_col='Date')

if not isinstance(data.index, pd.DatetimeIndex):
    data.index = pd.to_datetime(data.index, format='%d-%m-%Y')
resampled_data = data['Boxes Shipped'].resample('YE').sum().to_frame()
resampled_data.head()

resampled_data.index = resampled_data.index.year

resampled_data.reset_index(inplace=True)
resampled_data.rename(columns={'Date': 'Year'}, inplace=True)
resampled_data.head()
years = resampled_data['Year'].tolist()
box_shipped = resampled_data['Boxes Shipped'].tolist()

X = [i - years[len(years) // 2] for i in years]
x2 = [i ** 2 for i in X]
xy = [i * j for i, j in zip(X, box_shipped)]
n = len(years)
b = (n * sum(xy) - sum(box_shipped) * sum(X)) / (n * sum(x2) - (sum(X) ** 2))
a = (sum(box_shipped) - b * sum(X)) / n
linear_trend = [a + b * X[i] for i in range(n)]

x3 = [i ** 3 for i in X]
x4 = [i ** 4 for i in X]
x2y = [i * j for i, j in zip(x2, box_shipped)]
coeff = [[len(X), sum(X), sum(x2)],
[sum(X), sum(x2), sum(x3)],
[sum(x2), sum(x3), sum(x4)]]
Y = [sum(box_shipped), sum(xy), sum(x2y)]
A = np.array(coeff)
B = np.array(Y)
solution = np.linalg.solve(A, B)
a_poly, b_poly, c_poly = solution
poly_trend = [a_poly + b_poly * X[i] + c_poly * (X[i] ** 2) for i in range(n)]

print(f"Linear Trend: y={a:.2f} + {b:.2f}x")
print(f"\nPolynomial Trend: y={a_poly:.2f} + {b_poly:.2f}x + {c_poly:.2f}x²")
resampled_data['Linear Trend'] = linear_trend
resampled_data['Polynomial Trend'] = poly_trend

resampled_data['Boxes Shipped'].plot(kind='line',color='blue',marker='o') #alpha=0.3 makes 
resampled_data['Linear Trend'].plot(kind='line',color='black',linestyle='--')

fig, ax = plt.subplots()
ax.scatter(resampled_data.index, resampled_data['Boxes Shipped'],
           color='black', marker='x', s=100, label='Boxes Shipped')
resampled_data['Polynomial Trend'].plot(
    kind='line', color='red', ax=ax, label='Polynomial Trend'
)
```
### A - LINEAR TREND ESTIMATION
Linear Trend: y=180145.67 + 2506.00x

### B- POLYNOMIAL TREND ESTIMATION
Polynomial Trend: y=181411.00 + 2506.00x + -1898.00x²


### OUTPUT
A - LINEAR TREND ESTIMATION
<img width="584" height="432" alt="download" src="https://github.com/user-attachments/assets/1d26e70e-57d5-47b4-b5cb-bdaab1f31ad3" />

B- POLYNOMIAL TREND ESTIMATION
<img width="584" height="432" alt="download" src="https://github.com/user-attachments/assets/6e05f5fb-d5a5-47db-b52f-cf82ac573655" />

### RESULT:
Thus the python program for linear and Polynomial Trend Estiamtion has been executed successfully.
