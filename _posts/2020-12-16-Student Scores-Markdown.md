---
layout: post
title: One dataset, Two stories
subtitle: An analysis of one dataset from two different perspectives, with two different objectives
thumbnail-img: /assets/img/Test.jpg
cover-img: /assets/img/Student.png
tags: [Tableau]
comments: false
---

## The Dataset 

The dataset was obtained on Kaggle and is comprised of 1000 the following information on 1000 students in a U.S. public school: Ethnicity, Parental Level of Education, Lunch Status, Test Preparatory Courses, Math Scores, Reading Scores, Writing Scores.

## The Objectives

The first analysis was done from the perspective of a school that is considering whether to subsidize test preparatory courses to improve the school's overall test scores. In this analysis the school needs to indeitfy whether to subsidize test preparatory courses and what students to prioritize based on their budget and the cost of the courses. The second analysis was done from a social perspective, evaluating what characteristics impact student academic success to indetify disparities amongst different groups of students. This analysis is to view the impact of ethnicity in comparison to financial circumstances, two factors that are known to create disparities in the U.S.

## Analysis 1

First, we ran a regression to confirm that the test preparatory courses were effective. I then calculated the summary statistics based on the historical dataset of student grades as seen below:

![Summary Statistics](/assets/img/studentstats.png)

Next, I used these summary statistics to run a simulation of the three possible subsidization allocation methods: students who express financial need, students with the lowest test scores and students who do not express financial need.

![Simulation](/assets/img/studentsim.png)

Finally, I created a historgram of the school's average test scores based on the simulation for each scenario.

![Histogram](/assets/img/studentdist.png)

## Analysis 2

First, I conducted a correlation analysis to observe the relationship between the variables. This allows us to indentify possible sources of multi-collinearity.

![Correlation Analysis](/assets/img/Studentcorr.png)

I then conducted a regression analysis on the test scores as shown below. The R-squared statistics across our regression analyses range from 0.22 to 0.32. This indicates that 24% of variance in math score, 22% of variance in reading score, 32% of variance in writing score, and 23% of variance in average testing scores can be explained by the independent variables used collectively. Although the R-square statistics are not very high, this is expected since the dependent variable is human academic performance and human behavior is difficult to predict. Therefore, the R-squared statistics are sufficient for our model.   

![Regression Analysis](/assets/img/Studentreg.png)

![Regression Analysis](/assets/img/Studentreg1.png)

![Regression Analysis](/assets/img/Studentreg2.png)

To verify the results from the regression analyses. The plots show no pattern and are randomly dispersed around the horizontal axis, which indicates the linear regression model is valid and can be used for analysis.  

![Residual Plot](/assets/img/Studentres.png)

## The Conclusion

**Analysis 1**
The school should subsidize test preparatory courses for students with the lowest test scores to ensure a sufficient impact on the school's test scores and fair subsidization regardless of financial need. 

**Analysis 2**
Based on the analysis, gender, financial need and ethnicity all have an impact on student academic success. Financial need is determined by the student's lunch program (standard or reduced), completion of test preparatory courses and parental level of education.


