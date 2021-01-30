---
layout: post
title: Flight Delay Predictor 
subtitle: Using R and Naive Bayes to predict flight delays.
cover-img: /assets/img/plane.jpg
thumbnail-img: /assets/img/airport.jpg
tags: [R, Naive Bayes]
---

## Introduction

 A simple application of the Naive Bayes classifier on flight data, which is used to predict the probability of different of each attribute (delayed or on-time) for each class (based on day of the week, origin, destination, carrier or departure time). 

## Analysis 

Using R and a dataset of 2,210 entries, I used 60% of the data to train the model and tested the model on the remaining data to test the accuracy of our model.

```R
delays.df <- read.csv("Desktop/FlightDelays.csv")
delays.df$DAY_WEEK <- factor(delays.df$DAY_WEEK)
delays.df$DEP_TIME <- factor(delays.df$DEP_TIME)
delays.df$CRS_DEP_TIME <- factor(round(delays.df$CRS_DEP_TIME/100))
train.index <- sample(c(1:dim(delays.df)[1]), dim(delays.df)[1]*0.6)
selected.var <- c(10, 1, 8, 4, 2, 13)
train.df <- delays.df[train.index, selected.var]
valid.df <- delays.df[-train.index, selected.var]
install.packages('e1071', dependencies=TRUE)
library(e1071)
delays.nb <- naiveBayes(as.factor(Flight.Status) ~ ., data = train.df)
delays.nb
pred.prob <- predict(delays.nb, newdata = valid.df, type = "raw")
pred.class <- predict(delays.nb, newdata = valid.df)
df<-data.frame(actual = valid.df$Flight.Status, predicted = pred.class, pred.prob)
df[valid.df$CARRIER == "DL"& valid.df$DAY_WEEK == 7 & valid.df$CRS_DEP_TIME == 10 & valid.df$DEST == "LGA" & valid.df$ORIGIN == "DCA",]   
```
## Results

One insight from our analysis is that flights are more likely to be delayed on Saturdays. Of course it is important to note that many other factors go into a flight being delayed, such as weather.

| | Monday | Tuesday | Wednesday | Thursday | Friday | Saturday | Sunday |
|:--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
Delayed | 0.196 | 0.12 | 0.13 | 0.14 | 0.18 | 0.73 | 0.16|
On-time | 0.13 | 0.15 | 0.15 | 0.18 | 0.17 | 0.13 | 0.10|

Using a confusion matrix, we were able to determine that the model has a 84% accuracy.
