---
layout: post
title: Portfolio Construction 
subtitle: Using Python to construct and analyze portfolios across different industries.
cover-img: /assets/img/download.jpg
thumbnail-img: /assets/img/finance.jpg
share-img: /assets/img/download.jpg
tags: [finance, python]
---

# What is the best way to construct a Portfolio and minimize downside risks?

Every investor's goal is to maximize upside reward and minimize downside risk, but what is the best way to do this? There are multiple theoretical techniques that can be used to construct a portfolio and minimize downside risks.

The data used for this analysis is the monthly returns across 30 industries from 1926 to 2018. 

## 1. Maximum Sharpe Ratio Portfolio

This is also called the tangency portfolio. It is meant to provide the high reward per unit of risk so that there is no exposure to unrewarded risks (specific risks). In other words, this is the portfolio with the maximum Sharpe Ratio (a measure of risk = (return - risk-free) / voltaility). This portfolio is constructed by plotting the returns, establishing the Efficient Frontier, creating the Capital Market Line (connecting the MSR Portfolio to the risk-free rate). Portfolios along this CML line can provide more profits than portfolios on the Efficient Frontier.

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

When looking at the Food and Steel industries, the MSR portfolios is shown by the orange line on the below graph:

![Max Sharpe Ratio Portfolios](/assets/img/MSR.jpg)

## 2. Global Minimum Variance Portfolio

This is an extension of the MSR portfolio and a more realistic approach as we would not have access to return information prior to investing and therefore could not accurately construct a MSR portfolio. To use the MSR technique we would need to estimate expected returns and covariance which would lead to estimation errors.

The Global Minimum Variance Portfolio is reduces estimation errors by taking the portfolio on the efficient frontier with the lowest variance (risk) regardless of expected return. This is because there are often more estimation errors with returns thatn covariance.

```python
def gmv(cov):
    n = cov.shape[0]
    return msr(0, np.repeat(1,n),cov)
```

When looking at the Food and Steel industries, the GMV portfolios is shown by the green dot below in comparison to the MSR portfolio shown by the orange line on the below graph:

![Global Minimum Variance Portfolio](/assets/img/GMV.jpg)


## 3. Constant Proportion Portfolio

The final portfolio we will discuss is a dynamic portfolio that requires maintenance to evaluate the allocation to safe and risky assets in order to maintain a floor (wealth preservation level that is established by the investor). Unlike diversification (which fails to address market risks or protect investors when the marker crashes) and hedging (which protects investors against downside risks while proportionately decreasing upside rewards), CPPI allows for downside protection wil also allowing for upside potential. In order for this technique to be successful investors need to re-balance their portfolio and determine their risky asset allocation depending on the voltaility of the market. The downside to this portfolio is the need for constant re-balancing to ensure efficacy.

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

When looking at the Auto industry, we can see that using the CPPI strategy (blue line) would protect investors from large downside risks during the financial crisis in 2008-2009. The horizontal line represents the floor, which protects investors from losses beyond this constraint.

![CPPI Strategy](/assets/img/CPPI.jpg)

## 4. Asset-Liability Management

The last option is for investors to construct two portfolios: a Liability Hedging Portolio and Performance Seeking Portfolio. It is important for investors to consider both their assets and liabilities when investing by looking at the funding ratio as opposed to solely focusing on the value of your assets. 


