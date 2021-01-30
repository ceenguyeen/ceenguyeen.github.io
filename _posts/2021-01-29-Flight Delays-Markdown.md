---
layout: post
title: Flight Delay Predictor 
subtitle: Using R and Naive Bayes to predict flight delays.
cover-img: /assets/img/plane.jpg
thumbnail-img: /assets/img/airport.jpg
tags: [R, Naive Bayes]
---

## Analysis 

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
