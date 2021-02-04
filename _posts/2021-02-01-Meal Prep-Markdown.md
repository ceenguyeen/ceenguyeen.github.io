---
layout: post
title: Meal Prep Simulation
subtitle: Using Excel to help a small meal-prep business determine production quantity.
cover-img: /assets/img/Meal.jpg
thumbnail-img: /assets/img/Meals.png
share-img: /assets/img/Meals.jpg
tags: [Excel, Simulation]
---

## Problem Statement

Kevin runs a small business that sells pre-prepared meals. Every production run of a new meal lasts 3 weeks and then the meal is changed for the following 3 weeks. At the beginning of each production run, Kevin must determine how many meals to produce per week. Any meals that are not sold can be salvaged and sold at a lower cost and you estimate that any customer who is turned away would cost you a fixed amount of money.

**General Information

|    |  In Dollars($)  | 
| :---: |:---: |
| Cost | 8 | 
| Price | 11 |
| Salvage Price | 6 | 
| Shortage Cost | 5 | 


**Changes in demand

|   Probability |  Demand Change | 
| :---: |:---: |
| 25% | 5 to 10% | 
| 50% | -5 to 5% |
| 25% | -10 to -5% | 

**Salvage potential

|   Probability |  Percent Salvage (Large Demand Growth) | 
| :-----: |:---: |
| 33% | 10% | 
| 33% | 20% |
| 33% | 30% | 


|   Probability |  Percent Salvage (Small Demand Growth) | 
| :-----: |:---: |
| 20% | 10% | 
| 60% | 20% |
| 20% | 30% | 


## Analysis

To conduct this analysis, I used vlookup and simulation in Excel to determine total expected profits for the business based on historical demands. 

![Simulation](/assets/img/Simulation.jpg)

This analysis was based on the assumption that the production level is set at 175. I then simulated this expected total profit 1,000 times and looked at the probability that the business could sell out and make a profit of over $1,000.

## Additional Analysis

I then used what-if analysis to determine the optimal production level for Kevin based on historical data. I created a two-way data table that simulated the total exptected profits 1,000 times for production levels ranging from 100-200.

![What-if Analysis](/assets/img/Ravwhatif.png)

## Conclusion

Based on my analysis, the optimal production level for Kevin is between 145 and 155 meals each week for his 3 week production cycle. In this situation, changing his production level will allow him to increase his total expected profits by approximately $300. It should also be noted that the type of meals offered may affect demand.


