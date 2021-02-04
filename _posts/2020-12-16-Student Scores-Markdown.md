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



