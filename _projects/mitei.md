---
layout: page
title: mit energy initiative natural gas estimator
description: Jan 2021
importance: 4
img: assets/img/mitei.png
category: school/work
---

<h3>Overview</h3>

This was a monthlong project conducted during MIT's Independent Activities Period (IAP), as an undergraduate research assistant in the MIT Energy Initiative. The purpose of this project was to develop a **model of natural gas demand in the Northeast**. The resulting model estimated the **correlation between heating degree hours** (integral of difference between actual temperature and desired temperature over time) **and actual consumption** for different temperature ranges. 

<h3>Determining Model Type</h3>

There was initial exploration into different types of models, with substantial research done on tools that estimated consumption based on building characteristics (ResStock). Unfortunately, this modeling strategy, though promising, was unable to be implemented in the time frame (a few weeks). 

The project defaulted to an **empirical model of natural gas demand based on hourly temperature data, regional demographics, and past consumption**. 

<h3>Calculating Model Inputs</h3>
<h4>1. Adjusting Population Data</h4>
The population data (at the census tract level) was adjusted to be only the portion of the population that primarily used natural gas as a source of fuel. 

<h4>2. Heating Degree Hours (HDH)</h4>
The heating degree hours were calculated using temperature data (hourly data for each census tract). For a given hour, if the temperature was below 72 Fahrenheit, the difference between 72 Fahrenheit and the actual temperature was calculated (heating degree hour). It was then multiplied by the adjusted population. This was computed for every census tract in the Northeast for every hour. 

In other words:

{% include figure.html path="assets/img/hdh.png" title="hdh equation" class="img-fluid rounded z-depth-1" %}

<h4>3. Aggregating HDH</h4>
The inputs were aggregated on a monthly level, but into three categories

1. temperatures below 20 F
2. between 20 F and 40 F
3. between 40 F and 72 F (no heating required above 72 F)

Research has shown that although there is a linear correlation between heating degree hours and actual consumption, the **linear correlation varies based on the temperature range**. 

<h3>Regression</h3>

**Final Input:**
12 months, 3 values / month: total heating degree hours in each of the temperature ranges

**Final Output:**
Natural gas consumption in the Northeast on a monthly level taken from the US Energy Information Administration

The goal of the regression was to determine 3 coefficients - one for each of the temperature ranges.

<h3>Results</h3>
After running a linear regression, the coefficients were calculated, with the R squared value (level of correlation) at 0.997.

**Impact:** Using estimated future temperature data and the resulting coefficients, a prediction could be made for natural gas demand (in the Northeast) in the future.

<h3>Links</h3>
[Data Processing + Regression Source Code](https://github.com/jasmine2000/ri-bio-project)