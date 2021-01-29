---
layout: post
title: Financial Aid Programs for State Prisons
subtitle: How effective are financial aid programs in reducing recidivism?
thumbnail-img: /assets/img/recidivism.jpg
---

## What is recidivism?

## Analysis

I used logistic regression in R to develop a model that was used to evaluate the impact of different characteristics on recidivism and predict the probability of recidivism for a convict with given characteristics. 

Based on the model below, the only characteristic that significantly impacts recidivism is weeks of employment (EmpW) and the formula for probability of recidivism is as follows:

Probability of Recidivism = 0.665 – 0.095 (financial aid) – 0.011 (age) + 0.094 (race) + 0.055 (work exp.) + 0.016 (prior convictions) – 0.015 (week employed) – 0.203 (education) + 0.006 (parole) – 0.016 (marital status)

```R
correlation <-cor(arrest)
install.packages("corrplot")
library(corrplot)
corrplot(correlation, type = "upper", order = "hclust", tl.col = "black", tl.srt = 45)
pchisq(5,5,lower=FALSE)
Reg <- glm (arrest~fin+age+race+wexp+prio+EmpW+Educ+paro+mar, data=arrest, family=binomial(link="logit"))
summary(Reg)

pred <- predict(Reg , arrest, type = "response")
Output<-data.frame(actual = arrest$arrest,predicted = pred)
head(pred)
head(Output)
classification<-ifelse(pred>0.34,1,0)
actual=arrest$arrest
Output<-data.frame(actual, predprob=pred, classification)
head(Output)
install.packages("caret")
install.packages('e1071', dependencies=TRUE)
library(caret)
confusionMatrix(as.factor(classification),as.factor(actual))
```
Steps of Analysis:

1. Correlation Analysis

![Correlation](/assets/img/corr.jpg)

2. Logistic Regression

![Logistic Regression results](/assets/img/Arrest_reg.png)

3. Confusion Matrix 

![Confusion Matrix](/assets/img/Arrest_CM.png)

## Conclusions

Based on my analysis, the new financial aid program implemented by state prisons does not effectively prevent recidivism. When looking at different characteristics and their impact on recidivism, I found that whether or not a convict received financial aid following their release did not have a significant effect. Instead, I found that the number of weeks the convict was employed during the 52 weeks in which they were released, was a more effective preventative measure in comparison to financial aid. Therefore, my recommendation is to stop the financial aid program and implement a program that helps released convicts find gainful employment. The state prisons can do this by re-allocating the financial aid funds to help released convicts find employment, some potential ideas include:

•	Subsidizing fees for certifications required for employment. 

•	Creating partnerships with employers who will hire ex-convicts.

•	Creating programs that help convicts develop skills for employment.

The effects of financial aid in comparison to employment can be further illustrated in this chart where I took an average convict (age 25 with less than high school education and 3 prior convictions) and predicted the probability of them re-offending after being released depending on whether they received financial aid and the number of weeks they were employed. Note that predictions using this model are made with 73% accuracy.

| Probability of Recidivism | Financial Aid | Weeks Employed | 
| :------ |:------ | :------ | 
| 49% | Y | 0 | 
| 59% | N | 0 | 
| 44% | N | 10 | 
| 22% | N | 26 | 
| 16% | Y | 26 | 
