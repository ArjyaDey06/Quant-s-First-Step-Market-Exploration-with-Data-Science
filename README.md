# Quant-s-First-Step-Market-Exploration-with-Data-Science
This repositroy holds the contents like codes, concepts and ppt of DSA Club's event : Quant's First Step



## Table of Contents
What are Financial Markets?

Types of Financial Data

Quant in Action: Core Concepts

Machine Learning for Prediction

Summary

# 1. What are Financial Markets?
Financial markets are platforms where people buy and sell financial assets, like stocks, bonds, currencies, and commodities. They connect investors who have money with companies or governments that need funds, ensuring the economy runs smoothly.

Real-Life Analogy: Think of it like Amazon for money:

Companies list their “products” (stocks, bonds, etc.).

Investors “shop” by buying them.

Prices fluctuate based on demand and supply.
--
##  Key Terms
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


# a) Daily Returns

Where:

- Pt = today’s closing price
- Pt−1 = previous day's closing price
- Rt = return for the day

**Why**: Measures how much a stock increased or decreased in one day — the foundation for all quant calculations.
Steps to follow:

1. Import Dependencies
   ```python
    import pandas as pd
    import yfinance as yf
    import matplotlib.pyplot as plt

2. Fetch historical stock data
   ```python
    import yfinance as yf
    import pandas as pd
    import matplotlib.pyplot as plt
    stocks = ["RELIANCE.NS", "INFY.NS"]  # NSE tickers
    data = yf.download(stocks, period="6mo")['Close']
    print("Sample Stock Data:")
    print(data.head())
           
# Calculate Daily Returns:

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

#b) Calculating Moving Avg

Where:

- n = period (e.g., 5-day, 20-day)
- Pt−i = closing price for day t-i

**Why**: Helps **smoothen noise** and identify trends , commonly used for buy/sell signals.

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


