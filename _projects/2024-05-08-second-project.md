---
title:  "Particulate Matter Exposure"
layout: single
classes: wide
---

In our analysis, we found that the particulate matter (PM) measurement of the stationary monitor alone does not do a good job of explaining PM exposure. Kids are exposed to higher PM measurements as they participate in different activities. The analysis also shows that the effects of stationary measurements and activities are child related, suggesting that a child’s living conditions impact the amount of PM they are exposed to. Children are exposed to the most PM when they play on the floor.

<script type="text/javascript" async
  src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.7/MathJax.js?config=TeX-MML-AM_CHTML">
</script>

# Introduction
Particulate matter is a mixture of airborne particles that are emitted from power plants, cars, and other sources of chemical reactions. Exposure to these particles can lead to a number of adverse health effects such as irregular heartbeat or aggravated asthma. The purpose of this study is to determine the best way to measure PM exposure among children so that researchers can better study the health effects of pollution. Our data consists of 100 children who were monitored for one hour. The children wore a PM monitoring vest and a stationary monitor was placed in their homes. Measurements from these devices were recorded every minute, along with the activity that the child was engaged in. From this analysis, we hope to determine:
- If the stationary measurement is a good measure of PM exposure
- If the activities explain more of the aerosol intake than stationary measurements alone
- If the effect of an activity or stationary measurements is specific to each child
- If there are any activities that lead to higher PM exposure.

<div style="text-align:center;">
  <img src="{{site.url}}/{{site.baseurl}}/assets/images/PMPlot1.jpeg" alt="" style="width:800px;"/>
</div>

<div style="text-align:center;">
  <img src="{{site.url}}/{{site.baseurl}}/assets/images/PMPlot2.jpeg" alt="" style="width:800px;"/>
</div>

When fitting our model, we need to ensure that the necessary assumptions are met. One of these assumptions is that there is independence between observations in the data. Each observation in the data should be unrelated to or unaffected by the other observations. One issue in our data is that there is correlation between minute observations of the same child. Using a model that does not account for this, the correlations between the residuals of first 10 minutes are shown below.

<div style="text-align:center;">
  <img src="{{site.url}}/{{site.baseurl}}/assets/images/PMPlot3.png" alt="" style="width:1000px;"/>
</div>

If this correlation is not addressed, our standard errors will be inaccurate. Inaccurate standard errors lead to biased coefficient estimates that do not capture the true relationship between the explanatory variables and the Log Aerosol measurements. We can account for correlation between minute observations of the same child by fitting a model that accounts for that correlation. This will improve the accuracy of our standard errors and obtain more reliable estimates of the model parameters.

To analyze this data, we plan to use a multiple linear regression model. Multiple linear regression seems like the best choice because it can include several explanatory variables in the process of explaining the response variable. We will include all of the variables provided in the data as well as interaction terms for each child and their stationary measurements and their activities. We will try a variety of correlation structures and use the structure that results in the best model in terms of AIC. This should maximize our model’s ability to capture the relationship between the explanatory variables and log Aerosol measurements.

# Statistical Model

$$\mathbf{y} = \mathbf{X\beta} + \mathbf{\epsilon} \quad \text{,} \quad \mathbf{y} \sim N(\mathbf{X\beta}, \sigma^2\mathbf{B})$$

$$
\begin{pmatrix}
log(Aerosol_1) \\
log(Aerosol_2) \\
\vdots \\
log(Aerosol_n)
\end{pmatrix}
=
\begin{pmatrix}
1 & Minute_1 & \dots & ID100:log(Stationary_1)\\
1 & Minute_2 & \dots & ID100:log(Stationary_2)\\
\vdots & \vdots & \dots & \vdots\\
1 & Minute_n & \dots & ID100:log(Stationary_n)\\
\end{pmatrix}
\begin{pmatrix}
\beta_0 \\
\beta_{Minute} \\
\vdots \\
\beta_{ID100:log(Stationary)} \\
\end{pmatrix}
+ 
\begin{pmatrix}
\epsilon_1 \\
\epsilon_2 \\
\vdots \\
\epsilon_n
\end{pmatrix}
$$

$$
\mathbf{B} = \begin{pmatrix} R & 0 & \dots & 0 \\ 0 & R & \dots & 0 \\
\vdots & \vdots & \ddots & \vdots \\ 0 & 0 & \dots & R \end{pmatrix}
, R = \begin{pmatrix} 1 & \dots & \rho_{1,59} \\ \vdots & \ddots & \vdots \\
\rho_{59,1} & \dots & 1 \end{pmatrix}
$$

* **𝐲** is a column vector of the response variable observations. In this case, it is the log PM measurement on the child’s vest. 

* **𝐗** is a matrix of the explanatory variables. Each row represents an observation and each column represents one of the variables.

* **β** is a column vector of coefficients. $\beta_0$ represents the intercept and the rest correspond to individual explanatory variables in the $\mathbf{X}$ matrix.

* **𝜖** is a column vector of residuals (the difference between the observed and predicted values for the log PM measurement on the child’s vest). 

* **σ²** represents the variance of the log PM measurement on the child’s vest. 

* **B** represents a diagonal matrix where each diagonal element corresponds to an individual child in the data. The off-diagonal elements are all zero because we assume that there is no correlation between different patients.

* **R**, the diagonal elements of **B**, are 59x59 matrices unique to each child, wherein each element represents the unique correlation between minutes for a particular child. Each **R** has a compound symmetry correlation structure.

For our analysis, there are four assumptions that must be met in order for our inferences to be valid. The assumptions are:
- A linear relationship between the log vest PM measurement and the numeric explanatory variables in the data.
- Independence of the observations. The log vest PM measurement of one child should not impact the log vest PM measurement of another child.
- Normally distributed standardized residuals.
- Equal Variances. The variance of the residuals should be constant across all of the explanatory variables.


# Model Validation
<div style="text-align:center;">
  <img src="{{site.url}}/{{site.baseurl}}/assets/images/PM_AVPlots.jpg" alt="" style="width:1000px;"/>
</div>

After transforming the Aerosol and Stationary measurements the log scale, these added variable plots look sufficiently linear for the linearity assumption to be met. There are no obvious non-linear trends in any of the plots for numeric variables. In our analysis we are treating Activity and ID as categories, so we are not concerned about the linearity of those variables.

With our longitudinal model, we have accounted for correlation between minute measurements of the same child. The updated correlations between the residuals of first 10 minutes are shown below. There is also no reason to believe that one child’s measurements would be correlated with another child’s measurements. Thus, we can conclude that the independence assumption is met.
<div style="text-align:center;">
  <img src="{{site.url}}/{{site.baseurl}}/assets/images/PM decorrelated.png" alt="" style="width:1000px;"/>
</div>

<div style="text-align:center;">
  <img src="{{site.url}}/{{site.baseurl}}/assets/images/PM Histogram and FVR Plots.jpg" alt="" style="width:1000px;"/>
</div>

The histogram of decorrelated residuals looks approximately normal. Thus, we can conclude that the normality assumption is met.

The fitted values vs. residuals plot shows an approximately equal spread of residuals. The lack of obvious patterns or trends confirms our belief that the equal variance assumption is satisfied.

To determine how well our model fits the data, we calculated a pseudo R-Squared by dividing the variance of our model’s residuals by the variance of the residuals from a model with no explanatory variables. We then subtracted this from 1. The result was 0.930, which indicates that our model’s explanatory variables explain much of the variability in the log PM vest measurements.

# Analysis Results
To determine if the stationary measurement alone does a good job explaining PM exposure, we fit a model with the stationary measurements as the only explanatory variable. We then calculated a psuedo R-Squared squared for this model. The result of 0.001956604 indicates that stationary measurements alone do not do a good job explaining PM exposure.

To determine if the activities explain more of the aerosol intake than just stationary alone, we fit a model with the activities and stationary measurements as the only explanatory variables. Our psuedo R-Squared for this model was 0.008594545, which indicates that the activities explain more of the aerosol intake than just stationary alone.

We confirmed that the effects of activities and stationary are child-specific by conducting an F-test on two models. One model included all of the variables and interaction terms between activities/stationary measurements and each child, and the other model did not include any interaction terms. The p-value of 0.0001 indicated that there was a significant difference between these models.

To determine how much variability exists in the effects from child to child, we added the baseline effect for each activity to the interaction coefficients for each child and that activity. This gives us the adjusted coefficient for each child-activity pair. We did the same calculation for the stationary coefficient and interaction terms. Playing on the Floor had the highest variability with a standard deviation of 1.062668. The spread of effects for Playing on the Floor can be seen below. Other activities with high variability were Homework, Walking, and Playing On Furniture. Video Games had the lowest variability with a standard deviation of 0.8086411. Stationary measurements had very low variability, with a standard deviation of 0.1863893.

<div style="text-align:center;">
  <img src="{{site.url}}/{{site.baseurl}}/assets/images/PM_Histogram.png" alt="" style="width:1000px;"/>
</div>

To determine which activities lead to higher PM exposure, we calculated the mean of our adjusted coefficients for each child-activity pair:

<div style="text-align:center;">
  <img src="{{site.url}}/{{site.baseurl}}/assets/images/average_effects.png" alt="" style="width:1000px;"/>
</div>

Every activity except Homework had a positive average, indicating that on average, every activity except for Homework will lead to higher PM exposure. Playing on the Floor had the highest average effect.

# Conclusion

Our analysis found that the PM measurement of the stationary monitor alone does not do a satisfactory job of explaining PM exposure. Kids are exposed to higher PM measurements as they participate in different activities. The analysis also shows that the effects of stationary measurements and activities are child specific, suggesting that a child’s living conditions impact the amount of PM they are exposed to. Children are exposed to the most PM when they play on the floor. For researchers who hope to keep studying the effects of PM exposure on children, we suggest the continuation of employing the use of the vests that measure PM exposure instead of using stationary monitors to measure PM. We also suggest recording more data on the homes of the children, in order to determine what is causing the difference between children’s exposure to PM.









