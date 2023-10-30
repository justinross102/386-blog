---
title:  "Data Visualization Made Simple Part II: Creating Graphs in Python"
header:
  image: assets/images/isaac-smith-AT77Q0Njnt0-unsplash.jpg
  caption: "Photo credit: [**Unsplash**](https://unsplash.com)"
author: Justin Ross
description: "A Step-by-Step Guide"
classes: wide
---

In my previous blog post, I introduced basic methods of data visualization using R, demonstrating its ability to easily create useful visuals.

In this highly anticipated sequel post, I will demonstrate many of the same skills in Python, a language commonly used by many all over the world. Whether you are an experienced coder who has forgotten some syntax or has just begun learning to code in Python, this guide will teach you the basic tools and skills you need to create insightful graphs.

In this tutorial, I will use the same dataset as my previous post, which I scraped from www.baseball-reference.com. It contains data from the Philadelphia Phillies 2023 season through September 27. If you'd like to follow along, you can download this data from my Source Repository on GitHub (the link is at the bottom of the page).

# Using Python
To import the dataset into Python, we will need the `pandas` function `read_csv`. Once we load in this data and save it to a variable, we should run a head on our dataset to make sure it is imported correctly. (Note: This code assumes that the data file is saved in your Desktop folder.)
```python
import pandas as pd
phillies_games = pd.read_csv('~/Desktop/phillies_games_2023.csv')
print(phillies_games.head())
```

If you've loaded the data correctly, it should look like this:
```python
         Date Team  Home  Opp  ...   Save  Time    D_N Attendance
0  2023-03-30  PHI  Away  TEX  ...    NaN  3:04    Day    38387.0
1  2023-04-01  PHI  Away  TEX  ...    NaN  3:10    Day    31916.0
2  2023-04-02  PHI  Away  TEX  ...  Smith  2:24  Night    25823.0
3  2023-04-03  PHI  Away  NYY  ...    NaN  2:41  Night    37202.0
4  2023-04-04  PHI  Away  NYY  ...    NaN  2:45  Night    35392.0
```
Now let's use this data to make some graphs!

To make the graphing process easier, we will need to make use of two libraries called `matplotlib` and `seaborn`. We will need to import these libraries just as we did for `pandas` above. Using these libraries, we can now create graphs following this basic syntax:
```python
import seaborn as sns
import matplotlib.pyplot as plt # add these to wherever you imported pandas

sns.scatterplot(x = 'x-axis variable', y = 'y-axis variable', data = your_data)
plt.xlabel('x-axis label')
plt.ylabel('y-axis label')
plt.title('Plot Title')
plt.show()
```

### Creating a Scatterplot
Using the basic syntax shown above, let's create a scatterplot of the Phillies' attendance throughout the season.
```python
sns.scatterplot(x = 'Date', y = 'Attendance', data = phillies_games)
plt.xlabel('Date')
plt.ylabel('Attendance')
plt.title('Phillies 2023 Attendance')
plt.show()
```
<img src="{{site.url}}/{{site.baseurl}}/assets/images/python1.jpg" alt="" style="width:500px;"/>


As you can see, this graph doesn't look quite right. The dates on the x-axis are crowded together, making it impossible to read. This happens because Python doesn't realize our `Date` variable represents time; it's treating it like regular text. To fix this, we need to reformat the variable like this: `phillies_games['Date'] = pd.to_datetime(phillies_games['Date'])`. Now our graph should look much better!
```python
sns.scatterplot(x = 'Date', y = 'Attendance', hue='Home', data = phillies_games)
plt.xlabel('Date')
plt.ylabel('Attendance')
plt.title('Phillies 2023 Attendance')
plt.legend(title = 'Location')
plt.show()
```
<img src="{{site.url}}/{{site.baseurl}}/assets/images/python2.jpg" alt="" style="width:500px;"/>

Always be aware of the data types of the variables you are graphing! Also, notice that I added more information to the graph by specifying a `hue` variable. Now the points are colored based on where the game was played. I also changed the label of the legend (it would have been labeled “Home” otherwise).

If you're not happy with the standard look of these graphs, you can spice things up by using a style! One existing style allows graphs made in Python to look like they were made using ggplot in R (my personal preference). To do this, simply add this line of code above your plotting function:
```python
plt.style.use('ggplot')
```

Using this style, our graph will look like this:

<img src="{{site.url}}/{{site.baseurl}}/assets/images/python3.jpg" alt="" style="width:500px;"/>

Here are some other common styles you may find interesting:

| Commonly Used Styles                |
|-------------------------------------|
| `plt.style.use('classic')`          |
| `plt.style.use('Solarize_Light2')`  |
| `plt.style.use('bmh')`              |
| `plt.style.use('dark_background')`  |
| `plt.style.use('grayscale')`        |

For more information about matplotlib styles, check out this official [guide](https://matplotlib.org/stable/gallery/style_sheets/style_sheets_reference.html).

### Creating a boxplot

Now that we’ve seen how to create a scatterplot, let’s try making a boxplot showing the runs scored by the Phillies at Home vs. Away games.

```python
sns.boxplot(x='Home', y='R', data=phillies_games)
plt.xlabel('Home/Away')
plt.ylabel('Runs')
plt.title('Boxplot of Runs by Home/Away')
plt.show()
```

<img src="{{site.url}}/{{site.baseurl}}/assets/images/python4.jpg" alt="" style="width:500px;"/>

Just as we did with the scatterplot, we can add more information to this boxplot by specifying a `hue` variable. In this example, let's color the boxes based on the time of day that the game was played:
```python
sns.boxplot(x='Home', y='R', hue = 'D_N', data=phillies_games)
plt.xlabel('Location')
plt.ylabel('Runs')
plt.title('Boxplot of Runs by Home/Away')
plt.legend(title = "Game Time")
plt.show()
```
<img src="{{site.url}}/{{site.baseurl}}/assets/images/python5.jpg" alt="" style="width:500px;"/>

### Displaying Multiple Plots

To display multiple plots together, we'll first need to initialize an empty "canvas" to hold our plots. Then, we will need to write the code to create the graphs we want to make. After listing the plots we want to display, we can include this line of code to ensure proper spacing between subplots: `plt.tight_layout()`. In this example, I will display two plots side-by-side:

```python
fig, axs = plt.subplots(ncols=2, figsize=(12, 5)) # create the canvas

# first plot
sns.scatterplot(x = 'Date', y = 'Attendance', hue = 'Home', data = phillies_games, ax = axs[0])
axs[0].set_title('Phillies 2023 Attendance')
axs[0].legend(title = 'Location')

# second plot
sns.boxplot(x = 'Home', y = 'R', hue = 'D_N', data = phillies_games, ax = axs[1])
axs[1].set_title('Boxplot of Runs by Location')
axs[1].set_xlabel('Location')
axs[1].set_ylabel('Runs')
axs[1].legend(title = 'Game Time')

# Adjust the layout
plt.tight_layout()
plt.show()
```

<img src="{{site.url}}/{{site.baseurl}}/assets/images/python6.jpg" alt="" style="width:700px;"/>

Congratulations, you now have all of the basic knowledge needed to create graphs in python! I hope you found this tutorial helpful and insightful. You can find additional information about the process of plotting with seaborn [here](https://seaborn.pydata.org/tutorial/function_overview.html). 

