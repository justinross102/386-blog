---
title:  "Project 1: Pedagogy"
#description: "Tips and Best Practices for Effective Data Graphing" 
image: /assets/images/computer_with_bar_chart.png
layout: posts
classes: wide
---

<script type="text/javascript" async
  src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.7/MathJax.js?config=TeX-MML-AM_CHTML">
</script>


# Executive Summary 
In our analysis, we found that student learning was most associated with performance on midterm exams. Homework scores were also a good indicator of student learning but were not determined to be statistically significant. We determined that there was no semester that was significantly worse or better in terms of student learning. The number of students in a section and quiz scores were not found to be statistically significant.

# Introduction 
The purpose of this study is to evaluate the effect of various learning activities on student learning in an introductory statistics class. Our data consists of average scores on 3 unique learning activities administered by a statistics department over several semesters. The learning activities include exams, homework, and quizzes, with student learning measured by their performance on the final exam. From our analysis, we hope to determine:
- Which of these three learning activities are most closely associated with student learning
- If these learning activities explain student learning, and
- If there are semesters that had better or worse student learning than average

## Graphical Summaries


When examining the data, we found that most of the learning activities have a very low correlation with the average final exam score. Exam 3 has the highest correlation with the final exam with a correlation of 0.84, but the next highest correlation with the final exam is Exam 2’s correlation of 0.44. This suggests that some of our explanatory variables may not have a strong linear relationship with average final exam score. We are also concerned about differences in class sizes. Class sizes in this data range from 280 to 873 students, and we are concerned that these differences in class size may affect the variability in the average final exam scores.

If these issues are not addressed, our standard errors will be inaccurate. Inaccurate standard errors lead to biased coefficient estimates that do not capture the true relationship between the explanatory variables and the average final exam score. We can account for differing class sizes by fitting a model that uses a variance function to produce a unique variance for each individual observation. The variance of an individual observation will be the sample variance divided by an observation’s value for NStudents.

To analyze this data, we plan to use a multiple linear regression model. Multiple linear regression seems like the best choice because it can include several explanatory variables in the process of explaining the response variable. We will include all of the variables provided in the data as well as interaction terms related to class size. This should improve our model’s ability to capture the relationship between learning activities and average final exam scores.

# Statistical Model

$$
\mathbf{y} = \mathbf{X\beta} + \mathbf{\epsilon} \quad \text{,} \quad \mathbf{y} \sim N(\mathbf{X\beta}, \sigma^2\mathbf{D})
$$

$$
\begin{pmatrix}
Final_1 \\
Final_2 \\
\vdots \\
Final_n
\end{pmatrix}
=
\begin{pmatrix}
1 & NStudents_1 & Exam1_1 & \ldots & Semester10_1\\
1 & NStudents_2 & Exam1_2 & \ldots & Semester10_2\\
\vdots & \vdots & \vdots & & \vdots\\
1 & NStudents_n & Exam1_n & \ldots & Semester10_n\\
\end{pmatrix}
\begin{pmatrix}
\beta_0 \\
\beta_{NStudents} \\
\beta_{Exam1} \\
\beta_{Exam2} \\
\beta_{Exam3} \\
\beta_{HW} \\
\vdots \\
\beta_{Semester10} \\
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
\text{Var}(\epsilon_i) = \frac{\sigma^2}{NStudents_i}, \quad D = \begin{pmatrix} d_{11} & 0 & 0 & \cdots & 0 \\ 0 & d_{22} & 0 & \cdots & 0 \\ \vdots & 0 & \ddots & \ddots & \vdots \\ 0 & 0 & \cdots & 0 & d_{nn} \\ \end{pmatrix}
$$

* **𝐲** is a column vector of the response variable observations (average final exam score).

* **𝐗** is a matrix of the explanatory variables. Each row represents an observation and each column represents one of the variables (NStudents, Exam1, Exam2, etc).

* **β** is a column vector of coefficients. **β₀** represents the intercept and the rest correspond to individual explanatory variables in the **𝐗** matrix.

* **𝜖** is a column vector of residuals (the difference between the observed and predicted values for average final exam score).

* **σ²** represents the variance of the average Final Exam score in the data.

* **𝐃** represents a diagonal matrix where each diagonal element corresponds to the unique variance of each observation in the data. Larger values of **dᵢᵢ** represent a larger variance for the ith observation. The off-diagonal elements are all zero because we assume that the errors are not correlated with each other.






