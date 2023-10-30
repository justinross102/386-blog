---
title:  "Data Visualization Made Simple: Creating Graphs in R"
author: Justin Ross
description: "Tips and Best Practices for Effective Data Graphing" 
image: /assets/images/computer_with_bar_chart.png
classes: wide
---

We all have heard the saying "A picture is worth a thousand words," which is certainly true in our increasingly data-driven world. If you've ever wondered how to create insightful visualizations from your spreadsheets, you've come to the right blog!

In this beginner's guide, I will demonstrate the process of data visualization using R, one of the most popular programming languages for data science. This language has endless tools and libraries that will allow you to produce high-quality visuals with relative ease. After reading this blog, you will be well-equipped for any standard data visualization tasks you may have.

# Using R

When using R, the tidyverse package is incredibly useful for easily creating customizable visuals. If you don't already have it installed, you can install the tidyverse using this command: `install.packages("tidyverse")`. You can then load the package using the command: `library(tidyverse)`.

Now it's time to make some graphs! The tidyverse package will give you access to a function called `ggplot()`, which is what we will use in this tutorial. The basic syntax for ggplot is shown below:

```r
ggplot(data = your_data, mapping = aes(x = x_axis_values, y = y_axis_values)) + # specify what you are graphing
  graphing_function() # the function you use here determines what your graph will be
```
Here are some common graphing functions you may find useful:

| Function                   | Graph Type                |
|----------------------------|---------------------------|
| `geom_point()`              | Scatter plot              |
| `geom_line()`               | Line plot                 |
| `geom_bar(stat = "identity")`| Bar chart                |
| `geom_histogram()`          | Histogram                 |
| `geom_boxplot()`            | Boxplot                   |
| `geom_smooth()`             | Smoothed line plot        |
| `theme()`                   | Customize plot appearance |
| `labs()`                    | Modify axis and legend labels |

### Creating a scatterplot
Let's practice by using ggplot to create a scatterplot. For this example, I will use data I scraped from [www.baseball-reference.com](https://www.baseball-reference.com/teams/PHI/2023-schedule-scores.shtml){:target="_blank"} which contains data from the Philadelphia Phillies 2023 season through September 27. If you wish to follow along, you can download this data from my Source Repository on GitHub (the link is at the bottom of the page).

Following the basic syntax shown above, let's create a scatterplot of attendance throughout the season.
```r
ggplot(data = phillies_games, mapping = aes(x = Date, y = Attendance)) +
geom_point() # function for scatterplots
```
<img src="{{site.url}}/{{site.baseurl}}/assets/images/basic_plot.jpg" alt="" style="width:500px;"/>

Let's make this graph a little more interesting. By specifying a `color` variable in the `aes()` function, we can gain additional insights from this graph.
```r
ggplot(data = phillies_games, mapping = aes(x = Date, y = Attendance, color = Home)) +
  geom_point()
```
<img src="{{site.url}}/{{site.baseurl}}/assets/images/basic_colors.jpg" alt="" style="width:500px;"/>

We're almost done! Our graph needs a title. We can add a title using the `labs()` function.
```r
phillies_games %>% 
  ggplot(mapping = aes(Date, Attendance, color = Home)) +
  geom_point() +
  labs(title = "Phillies Attendance Throughout the 2023 Season",
       color = "Location") +
  theme(aspect.ratio = 1)
```

<img src="{{site.url}}/{{site.baseurl}}/assets/images/plot_with_title.jpg" alt="" style="width:500px;"/>

Note that I've made some changes to my existing code. Instead of using the `data =` argument, I can plug my dataset into the function using a pipe: `%>%`. Also, you do not need to specify `x =` and `y =` in the `aes()` function. Just know that whatever variable comes first will go on the x-axis. Using labs, I added a title and changed the label of the legend (it would have been labeled "Home" otherwise). You can also rename the x and y axes by adding `x = "", y = ""` arguments to the `labs()` function. Lastly, it is good practice for graphs to be square, so I added a line of code to make the plot a square.

### Creating a boxplot

Now that we've seen how to create a scatterplot, let's try making a boxplot showing the runs scored by the Phillies at Home vs. Away games.
```r
phillies_games %>% 
  ggplot(mapping = aes(Home, R)) +
  geom_boxplot() +
  labs(title = "Runs Scored by the Phillies in Home and Away Games",
       x = "Location",
       y = "Runs") +
  theme(aspect.ratio = 1)
```

<img src="{{site.url}}/{{site.baseurl}}/assets/images/boxplot.jpg" alt="" style="width:500px;"/>

Just as we did with the scatterplot, we can add additional information to this plot by specifying a color. For boxplots, we will specify a `fill` variable in the `aes()` function. We can also switch the orientation of the plots by adding `coord_flip()` to our code.

```r
phillies_games %>% 
  ggplot(mapping = aes(x = Home, y = R, fill = D_N)) +
  geom_boxplot() +
  labs(title = "Runs Scored by the Phillies in Home and Away Games",
       subtitle = "Distribution of Runs in Day and Night Games",
       x = "Location",
       y = "Runs",
       fill = "Time of Game") +
  theme(aspect.ratio = 1) +
  coord_flip()
```

<img src="{{site.url}}/{{site.baseurl}}/assets/images/boxplot_example_2.jpg" alt="" style="width:500px;"/>

### Displaying Multiple Plots

If you want to display multiple plots together, I would recommend the `patchwork` package. You'll have to install it if you haven't installed it already. Once it is installed, you can display plots together using `+` and `/` operators. The `+` operator places plots side by side, while the `/` operator places plots on top of eachother. One example is shown below. Assume my final scatterplot is assigned to a variable called `attendance_scatter` and my final boxplot is assigned to a variable called `boxplot_2`.

```r
install.packages("patchwork")
library(patchwork)

attendance_scatter + boxplot_2 # side by side
```
<img src="{{site.url}}/{{site.baseurl}}/assets/images/two_plots.jpg" alt="" style="width:1000px;"/>


I hope you found this tutorial helpful and insightful! If you have any remaining questions related to ggplot, I would encourage you to read this official guide [here](https://ggplot2.tidyverse.org).

