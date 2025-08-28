---
title:  "Data Visualization Made Simple: Creating Graphs in R"
header:
  image: /assets/images/computer_with_bar_chart.png
  caption: "Photo credit: [**Unsplash**](https://unsplash.com)"
description: "Tips and Best Practices for Effective Data Graphing" 
classes: wide
---

We all have heard the saying "A picture is worth a thousand words," which is certainly true in our increasingly data-driven world. If you've ever wondered how to create insightful visualizations from your data, you've come to the right blog!

In this beginner's guide, I will demonstrate the process of data visualization using R, one of the most popular programming languages for data science. This language has endless tools and libraries that will allow you to produce high-quality visuals with relative ease. After reading this blog, you will be well-equipped for any standard data visualization tasks you may have.

# Using R

When using R, the tidyverse package is incredibly useful for easily creating customizable visuals. If you don't already have it installed, you can install the tidyverse using this command: `install.packages("tidyverse")`. You can then load the package using the command `library(tidyverse)`.

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
Let's practice by using ggplot to create a scatterplot. For this example, I will use data I scraped from [www.baseball-reference.com](https://www.baseball-reference.com/teams/PHI/2023-schedule-scores.shtml){:target="_blank"} which contains data from the Philadelphia Phillies 2023 season through September 27. 

If you wish to follow along, you can download this data from my repository on [GitHub](https://github.com/justinross102/386-blog/blob/master/assets/datasets/phillies_games_2023.csv).
Following the basic syntax shown above, let's create a scatterplot of attendance throughout the season. Assume that you have read in the data and saved it into an object called `phillies_games`.
```r
ggplot(data = phillies_games, mapping = aes(x = Date, y = Attendance)) +
geom_point() # function for scatterplots
```
<img src="{{site.url}}/{{site.baseurl}}/assets/images/graphs_in_r_plots/basic_scatter.jpeg" alt="" style="width:500px;"/>

Let's make this graph a little more interesting! By supplying a variable to the `color` argument in the `aes()` function, we can gain additional insights from this graph.
```r
ggplot(data = phillies_games, mapping = aes(x = Date, y = Attendance, color = Home)) +
  geom_point()
```
<img src="{{site.url}}/{{site.baseurl}}/assets/images/graphs_in_r_plots/basic_scatter_w_color.jpeg" alt="" style="width:500px;"/>

We're almost done, but our graph still needs a title! We can add a title using the `labs()` function, which can also add other elements like axis labels or captions.
```r
phillies_games %>% 
  ggplot(mapping = aes(Date, Attendance, color = Home)) +
  geom_point() +
  labs(title = "Phillies Attendance Throughout the 2023 Season",
       color = "Location") +
  theme(aspect.ratio = 1)
```

<img src="{{site.url}}/{{site.baseurl}}/assets/images/graphs_in_r_plots/scatter_w_color_title.jpeg" alt="" style="width:500px;"/>

Note that I've made some changes to the structure of my code. Instead of using the `data =` argument, I supplied my dataset into the function using a pipe: `%>%`. Also, I did not specify `x =` and `y =` in the `aes()` function, knowing that whatever variable is specified first goes on the x-axis. 

In addition to adding a title, I used `labs()` to change the label of the legend. Before, the legend was labeled `Home` because that is the variable determining the color of a point. To change the legend label, the argument is `color` because that matches the argument in the `ggplot()` function responsible for adding the color and legend to the plot. If you so desire, you can rename the x and y axes by adding `x = "", y = ""` arguments to the `labs()` function. 

Lastly, it is good practice for graphs to be square, so I added a line of code to make the plot square.

### Creating a boxplot

Now that we've seen how to create a scatterplot, let's make a boxplot showing the runs scored by the Phillies in Home vs. Away games. In addition to specifying new variables, we will need to use `geom_boxplot()` instead of `geom_point()`.
```r
phillies_games %>% 
  ggplot(mapping = aes(Home, R)) +
  geom_boxplot() +
  labs(title = "Runs Scored by the Phillies in Home and Away Games",
       x = "Location",
       y = "Runs") +
  theme(aspect.ratio = 1)
```

<img src="{{site.url}}/{{site.baseurl}}/assets/images/graphs_in_r_plots/basic_boxplot.jpeg" alt="" style="width:500px;"/>

Just as we did with the scatterplot, we can add additional information to this plot by specifying a color. For boxplots, we specify a color using the `fill` argument in the `aes()` function. We can also switch the orientation of the plots by adding `coord_flip()` to our code.

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

<img src="{{site.url}}/{{site.baseurl}}/assets/images/graphs_in_r_plots/boxplot_w_color_title.jpeg" alt="" style="width:500px;"/>

To relabel the legend to `Time of Game`, I used the `fill` argument in `labs()`. We use `color` for scatter plots and `fill` for boxplots because the argument in `labs()` has to match the argument that controls the colors in the plot.

### Displaying Multiple Plots

If you want to display multiple plots together, I would recommend the `patchwork` package. You'll have to install it if you haven't installed it already. Once it is installed and loaded by running `library(patchwork)`, you can display plots together using `+` and `/` operators. The `+` operator places plots side by side, while the `/` operator places plots on top of eachother. One example is shown below. Assume my final scatterplot is assigned to a variable called `attendance_scatter` and my final boxplot is assigned to a variable called `runs_boxplot`.

```r
install.packages("patchwork")
library(patchwork)

attendance_scatter + runs_boxplot # side by side
```
<img src="{{site.url}}/{{site.baseurl}}/assets/images/graphs_in_r_plots/multiple_plots.jpeg" alt="" style="width:1000px;"/>


I hope you found this tutorial helpful and insightful! If you have any remaining questions related to ggplot, I would encourage you to read this official guide [here](https://ggplot2.tidyverse.org).

