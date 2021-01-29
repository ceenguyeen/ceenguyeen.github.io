---
layout: post
title: The 2020 stock market
subtitle: Using Python to construct and analyze portfolios across different industries.
cover-img: /assets/img/download.jpg
thumbnail-img: /assets/img/finance.jpg
share-img: /assets/img/download.jpg
tags: [finance, python]
---

# What is the best way to construct a Portfolio and minimize downside risks?

Every investors goal is to maximize upside reward and minimize downside risks, but what is the best way to do this? There are multiple theoretical techniques that can be used to construct a portfolio and minimize downside risks. 

## 1. Maximum Sharpe Ratio Portfolio

```python
def msr(riskfree_rate, er, cov):
    from scipy.optimize import minimize
    num_assets = er.shape[0]
    init_guess = np.repeat(1/num_assets, num_assets)
    bounds = ((0.0, 1.0),)*num_assets
    weight_is_1 = {
        'type': 'eq',
        'fun': lambda weights: np.sum(weights)-1
    }
    def neg_sharpe_ratio (weights, riskfree_rate, er, cov):
        r = portfolio_return(weights, er)
        v = portfolio_volatility(weights, cov)
        return -(r-riskfree_rate)/v
    
    results = minimize(neg_sharpe_ratio, init_guess,
                        args = (riskfree_rate, er, cov), method="SLSQP",
                        options = {"disp": False},
                        constraints = (weight_is_1),
                        bounds = bounds
                       )
    return results.x
```

## 2. Global Minimum Variance Portfolio

```python
def gmv(cov):
    n = cov.shape[0]
    return msr(0, np.repeat(1,n),cov)
```

## 3. Constant Proportion Portfolio

```python
def cppi(risky_r, safe_r=None, m=3, start=1000,floor=0.8,riskfree_rate=0.03, drawdown=None):
    dates = risky_r.index
    n_steps = len(dates)
    account_value = start
    floor_value = start*floor
    peak = start
    
    account_history =pd.DataFrame().reindex_like(risky_r)
    cushion_history =pd.DataFrame().reindex_like(risky_r)
    riskyweight_history =pd.DataFrame().reindex_like(risky_r)
    
    if isinstance (risky_r, pd.Series):
        risky_r = pd.DataFrame(risky_r, columns=["R"])
        
    if safe_r is None:
        safe_r = pd.DataFrame().reindex_like(risky_r)
        safe_r.values[:] = riskfree_rate/12
    
    for step in  range(n_steps):
        if drawdown is not None:
            previous_peak = np.maximum(peak, account_value)
            floor_value = previous_peak*(1-drawdown)
        cushion = (account_value-floor_value)/account_value
        riskyweight = m*cushion 
        riskyweight = np.minimum(riskyweight,1)
        riskweight = np.maximum(riskyweight,0)
        safeweight = 1-riskyweight
        risky_alloc = riskyweight*account_value
        safe_alloc = safeweight*account_value
        account_value = risky_alloc*(1+risky_r.iloc[step])+safe_alloc*(1+safe_r.iloc[step])
        account_history.iloc[step] = account_value
        cushion_history.iloc[step] = cushion
        riskyweight_history.iloc[step] = riskyweight
    
    risky_wealth = start * (1+risky_r).cumprod()
    
    result = {
        "Wealth": account_history,
        "Risky Wealth": risky_wealth,
        "Risk Budget": cushion_history,
        "Risky Allocation": riskyweight_history,
        "M": m,
        "start": start,
        "floor": floor,
        "Risky Return": risky_r,
        "Safe Return": safe_r
    }
    return result 
```


Here's a table of the monthly returns from the 6 industries that I analyzed in this project:

| Date | Health | Autos | Finance | Oil | Wholesale | Retail
| :---- |:---- | :---- | :---- |:---- | :---- | :---- |
| 2008-01 | -0.0455 | -0.0449 | -0.0123 | -0.0953 | -0.0409 | 0.0173
| 2008-02 | -0.0090 | -0.0692 | -0.1072 | 0.0711 | -0.0217 | -0.0614
| 2008-03 | -0.0179 | -0.0393 | -0.0379 | -0.02655 | -0.0202 | 0.0256
| 2008-04 | 0.0042 | 0.1411 | -0.0566 | 0.1040 | 0.0134 | 0.0590
| 2008-05 | 0.0163 | -0.0310 | -0.0412 | 0.0372 | 0.0708 | 0.0019


And here are the results of the analysis:


