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

You run a small business that sells prepared meals. Every production run of a new meal lasts 3 weeks and then the meal is changed for the following 3 weeks. At the beginning of each production run, you must determine how many meals to produce per week. Any meals that are not sold can be salvaged and sold at a lower cost and you estimate that any customer who is turned away would cost you a fixed amount of money.

The information we have is as stated below:

|    |  In Dollars($)  | 
| :--- |:--- |
| Cost | 8 | 
| Price | 11 |
| Salvage Price | 6 | 
| Shortage Cost | 5 | 



|   Probability |  Demand Change | 
| :--- |:--- |
| 25% | 5 to 10% | 
| 50% | -5 to 5% |
| 25% | -10 to -5% | 



|   Probability |  Percent Salvage (Large Demand Growth) | 
| :----- |:--- |
| 33% | 10% | 
| 33% | 20% |
| 33% | 30% | 



|   Probability |  Percent Salvage (Small Demand Growth) | 
| :----- |:--- |
| 20% | 10% | 
| 60% | 20% |
| 20% | 30% | 



## Analysis

To conduct this analysis, I used Excel to simulate the business based on historical demands. The analysis is as show below:

![Simulation](/assets/img/Simulation.jpg)

The demand and percent salvaged are simulated based on the provided probabilities. Using this I calculated the expected total profit after 3 weeks, based on the assumption that the production level is 175. I then simulated this expected total profit 1000 times and looked at the probability that the business could sell out and make a profit of over $1,000.

## Conclusion

Based on this analysis, 95% of the time the expected total profits will fall between $480 and $580. Another analysis can be conducted using What-if analysis to determine the best production level for the business. 
