---
title:  "Understanding Theme Park Wait Times: An Exploratory Data Analysis"
header:
  image: assets/images/heather-maguire-QIUR-K3YgDk-unsplash.jpg
  caption: "Photo credit: [**Unsplash**](https://unsplash.com)"
author: Justin Ross
classes: wide
---

As I mentioned in my previous post, I always look forward to trips to a theme park. Most people would agree that the worst part of these trips is always waiting in line. Thus, my goal is to determine the best time to go to a theme park. I have already collected data from during the Morning, Afternoon, and Evening on a Friday and Saturday. This blog post will showcase insightful visualizations made from this data, and will hopefully shed some light on the best time of the weekend to visit a theme park!

# Questions and Visualizations

## Number of Attractions
As we begin an analysis of this data, the first question I have is: **Which theme park has the most attractions?**

<img src="{{site.url}}/{{site.baseurl}}/assets/images/graph1.png" alt="" style="width:800px;"/>

From this graph, we see that Cedar Point and Six Flags Great Adventure have significantly more attractions than every other park. Animal Kingdom and Hollywood Studios from Disney World have the lowest number of attractions. The large difference in attraction counts across parks could be useful for trip planners to know, and future visualizations can help us see how wait times in small (few attractions) parks compare to wait times in large (many attractions) parks.

## Rides with the Highest Wait Times

Another question I found myself asking was: **What attractions have the highest wait times in each park?** As I sought to identify the attractions with the highest wait times during different times of the day, I focused on the top 5 for each period. Interestingly, two parks consistently emerged as the frontrunners—Hollywood Studios and EPCOT—despite being among the smallest parks in terms of the number of attractions. Even Animal Kingdom, with the fewest attractions, managed to be among the top contenders during the morning and evening hours. Perhaps the smaller size of these parks contributes to higher wait times.

#### Morning

| park                       | attraction                | wait time |
|----------------------------|---------------------------|-----------|
| *Animal Kingdom*           | Na'vi River Journey      | 120 |
| Disneyland                 | Star Wars: Rise of the Resistance| 115 |
| **Hollywood Studios**      | Slinky Dog Dash          | 115 |
| **EPCOT**                  | Remy's Ratatouille Adventure| 85 |
| Islands of Adventure       | Hagrid's Magical Creatures Motorbike Adventure™| 85 |

#### Afternoon

| park                       | attraction                | wait time |
|----------------------------|---------------------------|-----------|
| Universal Studios Orlando  | Harry Potter and the Escape from Gringotts™ | 120 |
| **Hollywood Studios**      | Star Wars: Rise of the Resistance           | 110 |
| **EPCOT**                  | Test Track                                  | 90 |
| Disneyland                 | Star Wars: Rise of the Resistance           | 85 |
| Magic Kingdom              | Peter Pan's Flight                          | 85 |

#### Evening

|                   park     |                     attraction | wait time |
|----------------------------|--------------------------------|-------------|
  **Hollywood Studios**      |               Slinky Dog Dash  |     120     |
|      *Animal Kingdom*      |      Avatar Flight of Passage  |      105    |
|  Islands of Adventure      |  Jurassic World VelociCoaster  |      105    |
|         Magic Kingdom      | Buzz Lightyear's Space Ranger Spin |  80     |
|             **EPCOT**      |             Frozen Ever After  |       75    |

## Wait Times and Attraction Counts

This scatter plot allows us to further examine the relationship between a park's number of attractions and average wait time. From the data I collected for this analysis, there appears to be an extremely weak negative correlation between these two variables. While the correlation is extremely weak, the overall patterns suggest that parks with a higher number of attractions tend to have slightly shorter average wait times. However, it's essential to note that correlation does not imply causation, and various other factors could contribute to these observations.

<img src="{{site.url}}/{{site.baseurl}}/assets/images/plot7.png" alt="" style="width:800px;"/>

## Ride Closures
Another highly important question for theme park visitors could be: **What is the difference in ride closures between Friday and Saturday?** It can be incredibly depressing to show up at a theme park only to find that you cannot ride your favorite ride! I thought it would be more useful to look at ride closures at specific parks, so for this visualization, I filtered the data to only include attractions from Universal Studios Orlando and Islands of Adventure.

<img src="{{site.url}}/{{site.baseurl}}/assets/images/plot2.png" alt="" style="width:1000px;"/>

Interestingly, Friday and Saturday appear to be essentially the same. Perhaps Friday afternoon had a couple more ride closures than Saturday afternoon, but clearly, the evening is when most ride closures are in effect. If I were headed to Universal Studios or Islands of Adventure on the weekend, I would probably try to ride all of my favorite rides before the evening!

## Wait Times Throughout the Day

In addition to ride closures, another incredibly important question for theme park visitors is: **How do wait times change over the course of a day?**

<img src="{{site.url}}/{{site.baseurl}}/assets/images/plot3.png" alt="" style="width:1100px;"/>

From these side-by-side boxplots we see that across Friday and Saturday, the median wait time is consistently around 20 minutes. The only exceptions to that are Friday evening and Saturday Afternoon.

As a budding statistician, I sometimes think it is worth looking at violin plots in addition to boxplots. Both plots display essentially the same information, but the violin plot is a useful tool because it depicts summary statistics and the range of the data while also showing the density of the response variable. In this case, the boxplots show that the median is usually pretty close to 20, but the violin plots below show that the majority of the data has a wait time of about 10 minutes in most cases.

<img src="{{site.url}}/{{site.baseurl}}/assets/images/plot4.png" alt="" style="width:800px;"/>

## Average Wait Times

To further explore how wait times change throughout the day, I created this point plot which shows the average wait times along with a confidence interval for each of the Disney World parks on Friday and Saturday. From these plots, we see that Animal Kingdom and Hollywood Studios begin each day with the highest average wait time. Only Hollywood Studios ends both days with the highest average wait time. Just like the tables shown above, these plots seem to suggest that smaller parks with fewer attractions have higher wait times. 

<img src="{{site.url}}/{{site.baseurl}}/assets/images/plot5.png" alt="" style="width:1000px;"/>
<img src="{{site.url}}/{{site.baseurl}}/assets/images/plot6.png" alt="" style="width:1000px;"/>

# Conclusion

After conducting this EDA, I am most interested in determining if there is a relationship between the number of attractions in a park and the average wait time for that park. To answer this question I would need to collect more data over a longer period. This analysis only scratches the surface of what we can learn from analyzing theme park data. As one who loves both statistics and theme parks, I am grateful to those who collect data like this regularly and analyze it even more in-depth than I did in this post! I am also grateful to Zachary Bull, who gave me permission to use his [API](https://queue-times.com/en-US/pages/about) for this project. If you would like, feel free to visit the [Github Repository](https://github.com/justinross102/ThemeParkWaitTimes_EDA) which contains the data and code I used to create the visualizations in this post. Thanks for reading!
 
