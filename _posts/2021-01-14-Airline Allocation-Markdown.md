---
layout: post
title: Airline Fare Allocation 
subtitle: How should airlines allocate their reservation inventory to different fare classes?
thumbnail-img: /assets/img/Airlines.jpg
cover-img: /assets/img/Seats.jpg
tags: [Excel, WhatIf]
---

## Problem Statement

With the introduction of multiple fare classes, airlines needed to update their yield management strategies to determine how to allocate their perishable reservation inventory to each fare class. In this simple example, we will look at a single-leg, two class model which is a one-way direct flight with no connections and two fare classes. We will need to determine the booking limit for the lower fare class (number of seats sold to economy class) and the protection level for the higher fare class (number of seats saved for business class).  

To begin this analysis, we have the capacity of the plane, the cost of each fare class and the historical demand distribution of each fare class.

## Analysis 

To conduct this analysis we used Excel to run a What-if analysis as shown below:

![What-if Analysis](/assets/img/Whatif.jpg)

We ran a one-way data table for different protection levels to evaluate the level at which we would be most profitable based on 1000 trials. We used the historical demand distribution and the rand() function in Excel to simulate demand. 

## Results

According to our analysis, the ideal protection level is 81. This means that the airline should save 81 of its 146 seats for the higher fare class. The goal of this analysis is to maximize the expected revenue by adjusting the protection level.
