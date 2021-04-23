---
layout: post
title: Short Selling Stocks
subtitle: Using Python to evaluate stock prices in relation to company revenues for two stocks that led to major losses for short sellers.
cover-img: /assets/img/stocks.jpg
thumbnail-img: /assets/img/finance.jpg
share-img: /assets/img/stocks.jpg
tags: [finance, python]
---

# Popular Shorted Stocks

Short Selling is when an investor borrows a stock to sell, with plans to repurchase the stock at a later point in time to return to the lender. 
Investors do this when they believe that the price of the stock will decrease, allowing them to make money by selling the stock now at a higher 
price and buying at a lower price at a later point. The problem with short selling is that if the stock price increases, the investor will lose money. 
Usually, experienced investors such as hedge funds partake in short selling.

Two popular examples of short selling gone wrong include:

##### Tesla

Tesla is a stock that have been of interest to many investors over the past few years. A few years ago, many short sellers targeted Tesla, beliving that 
the company's stock will decrease. However, the company's profits began increasing causing the company stock to rise. This led to  big losses for short sellers.

##### GameStop

Recently, shorted stocks can increase for reasons that are not based on fundamentals. This means that a company's stock price can increase regardless of its current and future profits. This makes it much less sustainable to short sell stocks and make a profit. The most recent example is Gamestop. 
Individual investors using the forum WallStreetBets on Reddit, started buying into shares of GameStop, a video and computer-game retailer. 
The influx of demand caused GameStop shares to soar, leading to billions of dollars in losses for hedge funds who had sold the stock short. 

# Analysis

In Python, I scraped stock and revenue information on both companies to visualize their stock prices in relation to their revenues. An example of the code for one 
of the companies is as seen below:

```python
#Importing the necessary libraries
import yfinance as yf
import pandas as pd
import requests
from bs4 import BeautifulSoup
import plotly.graph_objects as go
from plotly.subplots import make_subplots

#Scraping stock and revenue data from the web
tesla = yf.Ticker("TSLA")
tesla_stock = tesla.history(period="max")
tesla_data = pd.DataFrame(tesla_stock)
tesla_data.reset_index(inplace=True)

html_data = requests.get('https://www.macrotrends.net/stocks/charts/TSLA/tesla/revenue?cm_mmc=Email_Newsletter-_-Developer_Ed%2BTech-_-WW_WW
-_-SkillsNetwork-Courses-IBMDeveloperSkillsNetwork-PY0220EN-SkillsNetwork-23455606&cm_mmca1=000026UJ&cm_mmca2=10006555&cm_mmca3=M12345678&cvosrc=email.Newsletter.M12345678&cvo_campaign=000026UJ&cm_mmc=Email_Newsletter-_-Developer_Ed%2BTech-_-WW_WW-_-SkillsNetwork-Courses-IBMDeveloperSkillsNetwork-PY0220EN-SkillsNetwork-23455606&cm_mmca1=000026UJ&cm_mmca2=10006555&cm_mmca3=M12345678&cvosrc=email.Newsletter.M12345678&cvo_campaign=000026UJ&cm_mmc=Email_Newsletter-_-Developer_Ed%2BTech-_-WW_WW-_-SkillsNetwork-Courses-IBMDeveloperSkillsNetwork-PY0220EN-SkillsNetwork-23455606&cm_mmca1=000026UJ&cm_mmca2=10006555&cm_mmca3=M12345678&cvosrc=email.Newsletter.M12345678&cvo_campaign=000026UJ&cm_mmc=Email_Newsletter-_-Developer_Ed%2BTech-_-WW_WW-_-SkillsNetwork-Courses-IBMDeveloperSkillsNetwork-PY0220EN-SkillsNetwork-23455606&cm_mmca1=000026UJ&cm_mmca2=10006555&cm_mmca3=M12345678&cvosrc=email.Newsletter.M12345678&cvo_campaign=000026UJ').text
html_data = BeautifulSoup(html_data,'html5lib')
tesla_revenue = pd.DataFrame(columns=["Date","Revenue"])
tables = html_data.find_all("table")
tables = tables[1]
for row in tables.find("tbody").find_all("tr"):
    col = row.find_all("td")
    date =col[0].text
    revenue =col[1].text
    revenue = revenue.replace("$","").replace(",","")
    tesla_revenue = tesla_revenue.append({"Date":date,"Revenue":revenue}, ignore_index=True)
tesla_revenue = tesla_revenue[tesla_revenue['Revenue']!= ""]

#Plotting revenues and stock prices
def make_graph(stock_data, revenue_data, stock):
    fig = make_subplots(rows=2, cols=1, shared_xaxes=True, subplot_titles=("Historical Share Price", "Historical Revenue"), vertical_spacing = .3)
    fig.add_trace(go.Scatter(x=pd.to_datetime(stock_data.Date, infer_datetime_format=True), y=stock_data.Close.astype("float"), name="Share Price"), row=1, col=1)
    fig.add_trace(go.Scatter(x=pd.to_datetime(revenue_data.Date, infer_datetime_format=True), y=revenue_data.Revenue.astype("float"), name="Revenue"), row=2, col=1)
    fig.update_xaxes(title_text="Date", row=1, col=1)
    fig.update_xaxes(title_text="Date", row=2, col=1)
    fig.update_yaxes(title_text="Price ($US)", row=1, col=1)
    fig.update_yaxes(title_text="Revenue ($US Millions)", row=2, col=1)
    fig.update_layout(showlegend=False,
    height=900,
    title=stock,
    xaxis_rangeslider_visible=True,
    xaxis=dict(
        rangeselector=dict(
            buttons=list([
                dict(count=1,
                     label="1m",
                     step="month",
                     stepmode="backward"),
                dict(count=6,
                     label="6m",
                     step="month",
                     stepmode="backward"),
                dict(count=1,
                     label="YTD",
                     step="year",
                     stepmode="todate"),
                dict(count=1,
                     label="1y",
                     step="year",
                     stepmode="backward"),
                dict(step="all")
            ]))))
    fig.show()
make_graph(tesla_data, tesla_revenue, 'Tesla')
```
# Results
Looking at the plotted graphs below, you can see the sharp increase in profits and stock prices for Tesla. As for Gamestop, regardless of the company's profits, you
can see the volatile increase and decrease in the company's stock prices.

![Tesla](/assets/img/Tesla.png)

![GameStop](/assets/img/Gamestop.png)
