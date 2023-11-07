---
title:  "Understanding Theme Park Wait Times: Collecting the Data"
header:
  image: assets/images/heather-maguire-QIUR-K3YgDk-unsplash.jpg
  caption: "Photo credit: [**Unsplash**](https://unsplash.com)"
# excerpt: "Featuring tutorials and projects from my Statistics classes at Brigham Young University."
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
    'Epcot'
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

## Formatting the Data

To streamline the process of gathering and formatting data, I created a function called `get_park_data()`. This function interacts with the API using a park's unique ID, retrieves the information, and organizes it into a pandas DataFrame. Each row of this DataFrame corresponds to an attraction from the specified park, with columns for each parameter provided by the API. The code for this function is shown below:  

```python
def get_park_data(ID):
    url = f"https://queue-times.com/parks/{ID}/queue_times.json"
    response = requests.get(url)

    if response.status_code == 200:
        park_data = response.json()
    else:
        print(f"Failed to fetch data for park {ID}")
        return None

    lands_data = park_data.get('lands', [])

    # create empty lists
    lands = []
    attractions = []
    is_open_list = []
    wait_times = []
    last_updated_list = []

    # iterate over each land
    for land in lands_data:
        land_name = land.get('name')

        # make sure the parks have a 'rides' id
        rides = land.get('rides', [])

        # iterate over the rides in each land
        for ride in rides:
            ride_name = ride.get('name')
            is_open = ride.get('is_open')
            wait_time = ride.get('wait_time')
            last_updated = ride.get('last_updated')

            # append data to lists
            lands.append(land_name)
            attractions.append(ride_name)
            is_open_list.append(is_open)
            wait_times.append(wait_time)
            last_updated_list.append(last_updated)

    # create df from the lists
    park_df = pd.DataFrame({
        'land': lands,
        'attraction': attractions,
        'is_open': is_open_list,
        'wait_time': wait_times,
        'last_updated': last_updated_list
    })

    return park_df
```

To collect information from multiple parks, I created a list that contained each park's ID and its name. Using a for loop, I applied the `get_park_data()` function to each ID, creating a DataFrame for each park. To make it easy to distinguish each park after merging all the DataFrames, I added the park name as a new column in each DataFrame. Finally, I combined all park DataFrames into a single DataFrame named `all_parks`.

```python
parks_info = [
    (8, 'Animal Kingdom'),
    (17, 'California Adventure'),
    (7, 'Hollywood Studios'),
    (6, 'Magic Kingdom'),
    (16, 'Disneyland'),
    (5, 'EPCOT'),
    (64, 'Islands of Adventure'),
    (65, 'Universal Studios Orlando'),
    (33, 'Six Flags Discovery Kingdom'),
    (36, 'Six Flags St. Louis'),
    (37, 'Six Flags Great Adventure'),
    (279, 'Legoland California'),
    (50, 'Cedar Point'),
    (69, 'Dorney Park')
]

all_parks = []

for park_id, park_name in parks_info:
    park_data = get_park_data(park_id)
    park_data['park'] = park_name
    all_parks.append(park_data)

all_parks = pd.concat(all_parks, ignore_index=True)
```

After running all of this code and printing the resulting dataframe, we get a dataframe that looks like this:
```python
                                            attraction  is_open  wait_time              last_updated            park    land
0                            Festival of the Lion King     True          0 2023-10-30 14:11:34-06:00  Animal Kingdom  Africa
1                      Gorilla Falls Exploration Trail     True          0 2023-10-30 14:11:34-06:00  Animal Kingdom  Africa
2                                  Kilimanjaro Safaris     True          5 2023-10-30 14:11:34-06:00  Animal Kingdom  Africa
3                               Wildlife Express Train     True          0 2023-10-30 14:11:34-06:00  Animal Kingdom  Africa
4    Expedition Everest - Legend of the Forbidden M...     True         15 2023-10-30 14:11:34-06:00  Animal Kingdom    Asia
..                                                 ...      ...        ...                       ...             ...     ...
[551 rows x 6 columns]
```

# Combining Data for Friday and Saturday

Once I got the API working, I collected data from it during the Morning, Afternoon, and Evening on a Friday and Saturday. I combined all 6 dataframes together, adding a column to represent the day of the week. I had to do some difficult wrangling to create 3 columns for both the `wait_time` and `is_open` variables, depending on the time of day.

The finished dataframe shows every attraction twice, once for Friday and once for Saturday. Each row has three `is_open` columns and three `wait_time` columns, for Morning, Afternoon, and Evening. When printed, it looks like this:
```python
                           attraction        land                       park       day is_open_M is_open_A is_open_E  wait_time_M  wait_time_A  wait_time_E
0           Festival of the Lion King      Africa             Animal Kingdom    Friday      True      True      True          0.0          0.0          0.0
1           Festival of the Lion King      Africa             Animal Kingdom  Saturday      True      True      True          0.0          0.0          0.0
2     Gorilla Falls Exploration Trail      Africa             Animal Kingdom    Friday      True      True     False          0.0          0.0          0.0
3     Gorilla Falls Exploration Trail      Africa             Animal Kingdom  Saturday      True      True     False          0.0          0.0          0.0
4                 Kilimanjaro Safaris      Africa             Animal Kingdom    Friday      True      True     False         65.0         30.0          0.0
...                               ...         ...                        ...       ...       ...       ...       ...          ...          ...          ...
1098        Kang & Kodos Twirl n Hurl  World Expo  Universal Studios Orlando  Saturday      True      True     False         35.0         35.0          0.0
1099     MEN IN BLACK™ Alien Attack!™  World Expo  Universal Studios Orlando    Friday     False      True      True          0.0         35.0         10.0
1100     MEN IN BLACK™ Alien Attack!™  World Expo  Universal Studios Orlando  Saturday      True      True      True         50.0         35.0         10.0
1101               The Simpsons Ride™  World Expo  Universal Studios Orlando    Friday      True      True     False         20.0         25.0          0.0
1102               The Simpsons Ride™  World Expo  Universal Studios Orlando  Saturday      True      True     False         25.0         60.0          0.0
```

I am extremely grateful to the creator of the [Theme Park Wait Times API](https://queue-times.com/en-US/pages/about) Zachary Bull, who gave me permission to use his API for this project. It was difficult to collect the data and wrangle it into its final form, but it was a perfect opportunity for me to practice the skills we've been learning in class. I look forward to exploring this data in my next blog post!



