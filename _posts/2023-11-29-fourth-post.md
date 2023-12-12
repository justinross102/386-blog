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

As we begin an analysis of this data, the first question I have is: **Which theme park has the most attractions?**

<img src="{{site.url}}/{{site.baseurl}}/assets/images/graph1.png" alt="" style="width:800px;"/>

From this graph, we see that Cedar Point and Six Flags Great Adventure have significantly more attractions than every other park. Animal Kingdom and Hollywood Studios from Disney World have the lowest number of attractions. The large difference in attraction counts across parks could be useful for trip planners to know, and future visualizations can help us see how wait times in small (few attractions) parks compare to wait times in large (many attractions) parks.

Another highly important question for theme park visitors could be: **What is the difference in ride closures between Friday and Saturday?**

<img src="{{site.url}}/{{site.baseurl}}/assets/images/graph2.png" alt="" style="width:1000px;"/>

Interestingly, open attractions consistently outnumbered closed attractions on Saturday. On Friday, closed attractions outnumbered open attractions in both the morning and evening, suggesting that Friday afternoon is the best time to go if you're worried that your favorite ride will be closed!

In addition to ride closures, another incredibly important question for theme park visitors is: **How do wait times change over the course of a day?**

<img src="{{site.url}}/{{site.baseurl}}/assets/images/plot3.png" alt="" style="width:1100px;"/>

From these side-by-side boxplots we see that across Friday and Saturday, the median wait time is consistently around 20 minutes. The only exceptions to that are Friday evening and Saturday Afternoon.


<img src="{{site.url}}/{{site.baseurl}}/assets/images/plot4.png" alt="" style="width:1100px;"/>
