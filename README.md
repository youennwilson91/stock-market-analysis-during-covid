# stock-market-analysis-during-covid
An exploratory analysis on multiple stock market from different industries during covid (Chrisitan Dior, Moderna, Spotify, Netflix, Airbus).

You'll find on the following link the course I followed in order to make such an analysis: https://www.udemy.com/course/python-for-finance-and-trading-algorithms/#instructor-1

# IMPORTING DATA

```
input:

import datetime
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
import pandas_datareader
import pandas_datareader.data as reader
from pandas_datareader import data as web

#start and end date of the analysis
date_start = datetime.datetime(2020, 3, 1)
date_end = datetime.datetime(2020, 12, 1)
#Defining stocks to download (MODERNA,NETFLIX,Christian Dior ,Spotify )
stocks = ['MRNA','NFLX','CDI.PA','AIR.PA','SPOT']

#Downloading the stocks
df_stocks = reader.get_data_yahoo(stocks,date_start,date_end)
df_stocks.head()
```
```
output:
```
![1](https://user-images.githubusercontent.com/117467104/215828405-44083fd0-b1fb-4114-845f-a130173f4841.png)

# EXPLORATORY DATA ANALYSIS OF DIFFERENT STOCKS

On the plot bellow we may see the volume traded each day.

```
input:

df_stocks['Volume'].plot(figsize=(16,8),title='Volume Traded')
```
```
output:
```
![2](https://user-images.githubusercontent.com/117467104/215829267-41c5a754-bdc7-4356-b4c7-9f6450f05a0e.png)

Apparently, the Moderna stock has seen huge volume of traded stock in 2020. We may see the exact date with the following command :
```
input:

df_stocks['Volume'].idxmax()
```
```
output:
```
![3](https://user-images.githubusercontent.com/117467104/215829834-a0eb11ea-e711-4c95-bdde-ce06d1a8ed83.png)

On December 1st, 2020, there was a large trading volume for Moderna. The reason for the spike was due to Moderna's announcement that their COVID-19 vaccine was highly effective and they plan to seek emergency use authorization from the FDA and European regulators. The CEO stated that the vaccine could be available for high-risk populations by December 21st. As a result, Moderna's stock rose over 16% in after-hours trading. For more information, see the following link: https://nymag.com/intelligencer/2020/11/moderna-covid-vaccineshots-could-be-ready-by-december-21.html


Let's examine if there's any correlation between the stock prices of companies operating in different industries that were impacted differently by the COVID-19 crisis.
```
input:
sns.pairplot(df_stocks['Open'], kind='reg')
```
```
output:
```
![4](https://user-images.githubusercontent.com/117467104/215831018-a1264efa-5353-4202-ad1c-96b942bdebb7.png)

We may seethat the most significant correlations are between the Moderna and Netflix stocks, followed by the Moderna and Spotify stocks. This could be explained by the fact that lots of people were under lockdown during covid. Thus, they spent their time watching movies/series and listening to music.

# Basic Financial Analysis

## Daliy percentage change

The concept of percentage change measures the degree of change over time and is widely used in finance, particularly to track the price change of securities. The formula we use to calculate it is r_t = {p_t}/{p_{t-1}} - 1, where r_t represents the return at time t and p_t represents the price at time t. The Pandas module has a method to compute it which is pct_change()

returns = df_stocks['Close'].pct_change(1)

While this method provides valuable insights into a stock's volatility, it's not always sufficient for predicting future values. However, by calculating the percent returns and creating a histogram, we can determine which stock has been the most stable since 2015. To do this, we create a new column in each of our three dataframes that contains the return of the corresponding stock, and then use the formula to determine the percent returns.

Thus, we may see which formula has been the most volatile during 2020 :

```
input:

fig = plt.figure()
(returns + 1).cumprod().plot(figsize=(16,8),title='Stock returns of the c
plt.show()
```
```
output:
```
![1](https://user-images.githubusercontent.com/117467104/216958585-3fbc300c-8d85-4786-bd6f-7ecc9b7ba786.png)


Moderna seems the be the company which stock has seen the most fluctuations in 2020, followed by Spotify.

## Cumulative Daily Returns

Cumulative return on an investment refers to the total gain or loss that has been accumulated over a certain period, regardless of the duration of the investment. It takes into account all positive and negative returns from an investment to arrive at the overall result.

The goal of calculating daily cumulative returns is to determine the value of an investment of $1 in a company at the start of the time series until today. This calculation takes into account daily returns, unlike just looking at the stock price on a single day. It's important to note that this simple calculation does not consider dividends from the stock.

 The formula to compute CDR is : CRD = (1+r_t) * i_{t-1}

The Pandas module offers an easy way to calculate cumulative returns through its cumprod() method :

cumulative_ret = (returns + 1).cumprod().

Let's plot this.

```
input:
cumulative_ret.plot(figsize=(16,8),title='Cumulative return from march to december 2020 for $1 invested)
plt.legend()
```
```
output:
```
![2](https://user-images.githubusercontent.com/117467104/216961336-2d84d1da-c73d-49f5-8dd8-cbc160a707f9.png)

Without surprise we may see that Moderna has the highest CDR.


