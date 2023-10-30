---
title:  "Understanding Theme Park Wait Times: Collecting the Data"
header:
  image: assets/images/heather-maguire-QIUR-K3YgDk-unsplash.jpg
  caption: "Photo credit: [**Unsplash**](https://unsplash.com)"
author: Justin Ross
classes: wide
---

If you are anything like me, you love trips to theme parks. Incredible theming, soaring music, exhilarating rides, and delicious food combine to form a fantastic experience, so long as the wait times aren't too long. With there being so much to see and do, long wait times for attractions can (and often do) negatively impact the plans of excited park guests.

In this blog post, I will demonstrate my methods for collecting data on theme park attractions and their wait times. In a future post, I will use this data to determine how attraction wait times change throughout a given day. If there is an optimal time to go to a theme park, I will discover it!

## Collecting the Data

I gathered the data for my research from the [Theme Park Wait Times API](https://queue-times.com/en-US/pages/about). This unofficial API is free and easy to use and provides accurately updated wait times for attractions from theme parks all over the world. This project will only feature theme parks in the United States.

This API gives each park a unique ID, so to assemble a dataset I needed to know the IDs of the parks I wanted to include. I used to following code to extract the needed IDs:
```python
url = "https://queue-times.com/parks.json"
response = requests.get(url)

if response.status_code == 200:
    parks_data = response.json()

# the names of the parks I want to know the ID of
# need to match the way they are listed on the API webiste
parks = {
    'Animal Kingdom',
    ...
    'Disney Magic Kingdom',
    'Epcot',
}

# print out the park IDs

for group in parks_data:
    all_parks = group.get('parks', [])

    for park in all_parks:
        park_id = park.get('id', '')
        park_name = park.get('name', '')

        if park_name in parks:
            print(f"Park ID: {park_id}")
            print(f"Park Name: {park_name}")
```

This code will print out something like this for each park that we originally specified:

```
Park ID: 8
Park Name: Animal Kingdom
```

With all of these IDs, I collected the data and combined it into a dataset like this:







