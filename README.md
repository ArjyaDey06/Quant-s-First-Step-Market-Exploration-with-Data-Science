# Quant-s-First-Step-Market-Exploration-with-Data-Science
This repository holds the contents like codes, images and ppt of DSA Club's event : Quant's First Step



## Table of Contents
What are Financial Markets?

Types of Financial Data

Quant in Action: Core Concepts

Machine Learning for Prediction

Summary

#  What are Financial Markets?
Financial markets are platforms where people buy and sell financial assets, like stocks, bonds, currencies, and commodities. They connect investors who have money with companies or governments that need funds, ensuring the economy runs smoothly.

Real-Life Analogy: Think of it like Amazon for money:

Companies list their “products” (stocks, bonds, etc.).

Investors “shop” by buying them.

Prices fluctuate based on demand and supply.
--
#   Key Terms
A) Stocks (Equities): 

 - Represents ownership in a company. When you buy a share, you become a part-owner. Your profit comes from dividends or when the stock price goes up.
   
 **Example:**
- Buying **1 share of TCS** → you now own a small part of TCS.
- If TCS does well and the stock rises from ₹3,000 to ₹3,300, you earn ₹300.
  
B)Bonds: 
 - Like loans you give to governments or companies. They promise to pay you back later with interest. Bonds are less risky than stocks but also offer lower returns.
   
**Example:**

- The Indian government issues **Government Bonds** to fund infrastructure projects.
- If you buy a ₹10,000 bond with a 7% annual interest rate, you’ll earn ₹700 each year.


C)Derivatives:
-A financial instrument whose value is derived from another asset (e.g., a stock). They are often used for managing risks or speculating.

**Example:**
- A farmer uses a **wheat futures contract** to lock in a price for their crop today so they won’t be hurt if prices fall in the future.
- Stock traders use **options** to bet on whether Reliance’s stock will go up or down.


D)Forex (Foreign Exchange Market): The global marketplace for buying and selling currencies. It is the largest financial market in the world.

**Example:**

- You travel to the U.S. and convert **₹84,000 to $1,000**.
- If the USD/INR exchange rate changes from 84 to 86, you gain or lose money based on timing.



Steps to follow:

1. Import Dependencies
   ```python
    import pandas as pd
    import yfinance as yf
    import matplotlib.pyplot as plt
   ```
2. Fetch historical stock data
   ```python
    import yfinance as yf
    import pandas as pd
    import matplotlib.pyplot as plt
    stocks = ["RELIANCE.NS", "INFY.NS"]  # NSE tickers
    data = yf.download(stocks, period="6mo")['Close']
    print("Sample Stock Data:")
    print(data.head())
    ```   
#  a) Daily Returns

   <img width="254" height="124" alt="image" src="https://github.com/user-attachments/assets/627dc73b-88a1-4401-ad31-0d0bd099a2d7" />

   Where:

- Pt = today’s closing price
- Pt−1 = previous day's closing price
- Rt = return for the day
 
 - Why: Measures how much a stock increased or decreased in one day — the foundation for all quant calculations.
## Calculate Daily Returns:
   pct_change() calculates the **percentage change** between the current day's closing price and the previous day's closing price.
 
   ```python
    
      # Calculating Daily Returns

        daily_returns = data.pct_change() * 100  # gives value in percentage
        print("\nDaily Returns:")
        print(daily_returns.head())
        
        
        # Plotting daily returns for Reliance
        
        plt.figure(figsize=(12,6))
        plt.plot(daily_returns['RELIANCE.NS'], label="Reliance Daily Returns", color='blue')
        plt.title("Daily Returns - Reliance")
        plt.xlabel("Date")
        plt.ylabel("Return (%)")
        plt.grid(True, alpha=0.3)
        plt.legend()
        plt.show()
  ```
## A code walkthrough

* daily_returns = data.pct_change() * 100: This line calculates the daily returns.

   * data.pct_change() computes the percentage difference between the current day's closing price and the previous day's closing price.
         
   *  (* 100) converts this value from a decimal (e.g., 0.01) to a percentage (e.g., 1%).

* print(daily_returns.head()): This prints the first few rows of the new daily_returns DataFrame, allowing you to see the calculated values.

* Plotting the Results
   * plt.plot(daily_returns['RELIANCE.NS']): This plots the calculated daily returns for Reliance as a line graph.

   * plt.title(...), plt.xlabel(...), plt.ylabel(...): These lines add a title to the graph and labels to the axes, making the plot easy to understand.

   * plt.show(): This command displays the final graph.

#  b) Calculating Moving Avg

<img width="278" height="124" alt="image" src="https://github.com/user-attachments/assets/2a12f242-1db7-465a-91fd-7614fc3d7eb4" />

Where:

- n = period (e.g., 5-day, 20-day)
- Pt−i = closing price for day t-i

- Why: Helps **smoothen noise** and identify trends , commonly used for buy/sell signals.

   ```python
                       
                       # Calculating the moving average of Reliance's stocks for past 20 days
        
                        data['RELIANCE_SMA20'] = data['RELIANCE.NS'].rolling(window=20).mean()
                        
                        # Plotting the closing price vs moving average graph
                        plt.figure(figsize=(12,6))
                        plt.plot(data['RELIANCE.NS'], label="Reliance Closing Price", color='blue')
                        plt.plot(data['RELIANCE_SMA20'], label="20-Day SMA", color='red')
                        plt.title("Reliance Closing Price vs 20-Day Moving Average")
                        plt.xlabel("Date")
                        plt.ylabel("Price (₹)")
                        plt.legend()
                        plt.grid(True, alpha=0.3)
                        plt.show()
    ```
## A code walkthrough
*data['RELIANCE_SMA20'] = data['RELIANCE.NS'].rolling(window=20).mean(): This is the core calculation.

   *data['RELIANCE.NS'] selects the closing price data for Reliance.
   
   * .rolling(window=20) creates a "sliding window" that looks at the last 20 data points.
   
   * .mean() calculates the average of the prices within that 20-day window. This process repeats for every day, creating a new series of average prices.


# c) Volatility (Standard Deviation of Returns)

<img width="330" height="152" alt="image" src="https://github.com/user-attachments/assets/6fc1ee59-4a38-446c-b105-69a80447a74d" />


* Calculating Volatility
    * volatility = daily_returns.std(): This is the main calculation. It computes the standard deviation of the daily_returns data. The standard deviation is the numerical value that represents the stock's overall volatility during the entire period.
    
    * print(volatility): This displays the calculated volatility value in the console.

* Calculating & Plotting Rolling Volatility
   * rolling_vol = daily_returns['RELIANCE.NS'].rolling(window=20).std(): This calculates rolling volatility.
    
   * daily_returns['RELIANCE.NS'] selects the daily returns data for Reliance.
    
   * .rolling(window=20) creates a "sliding window" that calculates the standard deviation for every 20-day period. This helps you see how the stock's risk level changes over time.
    
   * .std() computes the standard deviation within each 20-day window.
    
   * plt.plot(rolling_vol, ...): This plots the rolling_vol data as a line graph. Instead of a single value, this graph shows how the stock's risk level has fluctuated day-by-day.
    
   * The remaining plt commands add a title, labels, and a legend to the graph, making it easy to read.
    
# d) Cumulative Returns


<img width="639" height="67" alt="image" src="https://github.com/user-attachments/assets/0ac6b168-78dc-4ccd-8ddb-e0f8e2f96222" />

Why: Shows total growth of investment over time.


```python
       # Cumilative Returns
       
       cumulative_returns = (1 + daily_returns/100).cumprod() - 1
       
       # Plotting the cumilative returns for both stocks
       
       plt.figure(figsize=(12,6))
       plt.plot(cumulative_returns['RELIANCE.NS'], label="Reliance", color='blue')
       plt.plot(cumulative_returns['INFY.NS'], label="Infosys", color='green')
       plt.title("Cumulative Returns (Last 6 Months)")
       plt.xlabel("Date")
       plt.ylabel("Cumulative Return")
       plt.legend()
       plt.grid(True, alpha=0.3)
       plt.show()
```
* Calculating Cumulative Returns
   *cumulative_returns = (1 + daily_returns/100).cumprod() - 1: This single line performs the entire calculation. It converts the daily percentage returns back to a decimal, compounds them using .cumprod(), and then converts the result to a final               cumulative percentage.     

-    a) daily_returns / 100
         This takes the daily return values, which are in percentage format (e.g., 1%), and converts them back to a decimal (e.g., 0.01). This is a crucial first step because percentages need to be in decimal form for mathematical operations.

-    b) 1 + ...
         This adds 1 to the decimal daily return. For a 1% gain, the value becomes 1.01. For a 1% loss, it becomes 0.99. This step is necessary to prepare the returns for multiplication, as it represents the total value after each day's change.


-    c) .cumprod()
         This is a pandas command that calculates the cumulative product. It starts with the first value and multiplies it by the second, then multiplies that result by the third, and so on. This accurately reflects the effect of compounding, where gains           and losses build on each other over time.


-    e)  -1
         The final step subtracts 1 from the result. After the .cumprod() operation, the result represents the total growth factor (e.g., 1.15 for a 15% gain). Subtracting 1 converts it back to a percentage or decimal return, making it easy to see the              total gain or loss from the starting point.

# e) Histogram of Returns (Risk Visualization)

Shows distribution of daily returns — useful to **visualize risk** and skewness.
```python
         # Histogram of Reliance Daily Returns
     
     plt.figure(figsize=(10,6))
     plt.hist(daily_returns['RELIANCE.NS'].dropna(), bins=40, color='skyblue', edgecolor='black')
     plt.title("Reliance - Distribution of Daily Returns")
     plt.xlabel("Daily Return (%)")
     plt.ylabel("Frequency")
     plt.grid(True, alpha=0.3)
     plt.show()
```
# d)Linear Regression for Stock Price Prediction.

## Step 1: Import dependencies

 *📌 Import libraries needed for data handling, fetching financial data, building the model, and visualizing results.
 ```python
   import pandas as pd          # For handling tabular data
   import numpy as np           # For numerical calculations
   import matplotlib.pyplot as plt  # For plotting graphs
   import yfinance as yf        # For fetching stock price data
   from sklearn.model_selection import train_test_split  # For splitting data into training/testing
   from sklearn.linear_model import LinearRegression     # For building regression model
   from sklearn.metrics import mean_squared_error, r2_score  # For model evaluation
 ```
## Step 2: Load stock price data**

* 📌Here, we download Apple (AAPL) stock data using Yahoo Finance for a given date range.
  ```python
    data = yf.download("AAPL", start="2023-01-01", end="2023-09-01")
    print(data.head())  # Show first few rows to understand data structure
  ```
## Step 3: Prepare features and target**

 * 📌We create a new column `Prev_Close` which shifts yesterday's closing price to predict today's price.
    ```python
     data['Prev_Close'] = data['Close'].shift(1)  # Add previous day's close
     data = data.dropna()  # Drop missing values caused by shifting
    ```
  * 📌 Define independent variable (X = previous close) and dependent variable (y = today's close).
     
## Step 4: Split into training & testing data
   ```
     X = data[['Prev_Close']]  # Feature (yesterday's price)
     y = data['Close']         # Target (today's price)
   ```
 * 📌We split data into training (80%) and testing (20%) to evaluate the model on unseen data.
   ```python
     X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, shuffle=False)
   ```
## Step 5: Initialize and train the model
   
  * 📌Create a Linear Regression model and fit it to the training data to find the best-fit line.
  ```python

     model = LinearRegression()
     model.fit(X_train, y_train)  # Train the model
  ```
## Step 6: Make predictions

 * 📌Use the trained model to predict stock prices for the test dataset.
   ```python
     predictions = model.predict(X_test)
   ```
## Step 7: Evaluate model performance**

 * 📌Calculate error metrics to understand how accurate our predictions are.
     ```python
      mse = mean_squared_error(y_test, predictions)  # Lower MSE is better
      r2 = r2_score(y_test, predictions)             # R² closer to 1 is better
      
      print(f"Mean Squared Error: {mse}")
      print(f"R² Score: {r2}")
   ```

## Step 8: Visualize actual vs predicted prices

 * 📌Plot actual closing prices vs predicted closing prices to visually check model performance.
   ```python
    plt.figure(figsize=(10,5))
    plt.plot(y_test.values, label='Actual Price', color='blue')      # Actual stock prices
    plt.plot(predictions, label='Predicted Price', color='red')      # Predicted prices by model
    plt.legend()
    plt.title('Actual vs Predicted Stock Price')  # Title of the chart
    plt.xlabel('Days')                            # X-axis label
    plt.ylabel('Price')                           # Y-axis label
    plt.show()
   ```

Linear regression is a foundational machine learning model that transforms historical data into a predictive tool, helping us find and act on the simple, underlying trends in financial markets.
