# Toronto Bikeshare Replenishment

## Executive Summary

(...)

## Introduction

Bike Share Toronto was designed to allow users to make short trips around town. The sturdy-framed bikes are available at any docking station in the city. The bikes can be taken from any station and returned to any station in the bike share system. 

According to the CBC, the project has required more than $ 27.5 million in public funds on service operations since 2014, and more funding is every time on the works. This has drawn several criticisms given the fact that the service is still not profitable for the city and has been **cash flow negative** and losing money for a few years now, according to CBC. Furthermore, several figures demonstrate that the service is not efficient using its resources, one big example of this is the fact that as of Nov. 2018, the service had more than 3.750 bikes for a 1.8 million ridership, a mere 480 rides per bike a year, a little over **one ride per day per bike**. This is even more worrisome today since the service offers 5.000 bikes according to their site, and rumors about service expansion have spread out. Also, and even more important it's been a proof of non-sustainable eco-friendly alternative transportation service. 

Moving 5.000 bikes around 465 stations throughout Toronto is a major engineering and planning challenge to tackle. Therefore, predicting where, when and how many bikes should be moved from a station to another using their own data would be a major help for their operations department. *This report presents a methodology for modeling the bike demand along with examples of how it could be used for prediction in this system*. It also analyzes more in depth high demand months (June, July and August) and approximates the amount of bikes to be moved using existing data.

## Data Availability

Since **public historical information about station status is not available**, in order to perform this analysis, we rely on different sources of information to approximate the variables of interest.

1. Open data for 2016 (Q3 and Q4) and 2017 (Q1 to Q4) ridership available [here](https://open.toronto.ca/dataset/bike-share-toronto-ridership-data/). This data was used to as input for predictions of bike demand at the stations of interest.
2. Online data for stations general information available through an API [here](https://tor.publicbikesystem.net/ube/gbfs/v1/en/station_information). This was used to update the information from 2017 with information from today regarding station capacity, coordinates, location, name, ID, among others.
3. Weather data for 2017 including hourly records of weather variables from the Government of Canada available [here](https://climate.weather.gc.ca/historical_data/search_historic_data_e.html).

## Required dependencies

* **Data wrangling**: `pandas`,` numpy`, `datetime`, `fuzzywuzzy`, `math`, `requests`,` json`, `holidays`
* **Viz**: `matplotlib`, `seaborn`
* **Modeling**: `sklearn`


## Data [preprocessing](Code/Preprocessing.ipynb) highlights

- Timestamps formats did not match across the dataset and contained multiple string errors, these were standardized and fixed.
- Time continuity of records was tested.
- Station IDs were missing for Q3 and Q4 of both 2016 and 2017, these were included by matching station names with names from current stations available through API endpoint,  and using brute-force string matching and `fuzzywuzzy` for partial string matching.
- A new and more reliable `trip_duration` variable was created from timestamps.
- Outliers were detected using two criteria (1) *false trips* criteria for short trips (trips with less than 1 minute duration) which is about 1.3% of the data and (2) *IQR* interquartile range for long trips (about 5% of the data).

## [EDA](Code/EDA.ipynb) highlights

- The top 10 origin and destination stations were found. These 10 stations out of 266 (3.6%) account for the 13% and 14% of all trips, respectively. To create a first modeling approach, we will only analyze bike supply and demand for the top station, which is **Union Station**, accounting for a 2% and 2.6% as origin and destination, respectively.
- During 2017, more than 90K bikes were taken from and to Union Station in total. Considering the 5.000 bikes available today, and Union Station's average of 212 bikes a day, with a maximum of nearly 500 bikes, *up to 10% of all bikes could be used* at this station alone. Furthermore, this mean goes up to 220 bikes during weekdays, which means trips are mainly skewed towards weekdays instead of weekends.
- A surrogate variable was created in order to approximate the demand of bikes at Union Station for every hour of every day of the year 2017. This variable is called **rate of change** and, is calculated as the amount of trips leaving the station minus the amount of trips arriving to the station. We will have a signed number that will describe the deficit or surplus of bikes for every hour, respectively.
- Union Station has a -1.2 average rate of change, meaning that on average *more bikes will arrive to the station than those leaving the station*. In terms of operations, this station will have a surplus of bikes that need to be taken from the station. 
- Peak hours present 49 bikes arriving to the station (almost twice the station's capacity) and 26 bikes leaving the station (full station capacity).
- For weekends, the average rate of change tends to zero (with low variance), which means a balanced supply and demand. This means that on average, *we will not need resources for bike replenishment at Union Station on weekends*.
- For weekdays, morning hours have a positive rate of change on average and, until 6 AM a neutral rate of change. Demand peaks at 7 AM, in which the capacity is enough to satisfy the demand of about 10 bikes. This capacity seems to be optimal and we will not need replenishment.
- For weekdays' afternoons the opposite is the case. With negative rates of change from 3 PM until 10 PM, *we will need to remove a significant amount of bikes during afternoon hours*, even more during peak hours (from 2 to 6) the capacity of the station will be exceeded many times as users will arrive and have nowhere to park their bikes. Users could even incur in overage.
- For every hour of the day, the mean and standard deviation of the rate of change will be within the capacity of the station, which is optimal for operations.
- Extreme values are present at 4 PM, the "ride back home", in which the station's capacity is exceeded. However, the "ride to work" is not symmetric to this number. This station is used for the trip back home more than the trip to work.
- Wednesday is the day that presents the *most negative peaks* for rate of change, followed in similar magnitudes by Tuesday, Thursday and Monday. On a Friday is rare to have a bike surplus.

## [Modeling](Code/Modeling.ipynb) highlights

- ...

## Remarks
* Historic data for state of stations between 2011 and 2018 is available upon request [here](https://data.cdrc.ac.uk/dataset/toronto-bss), this application process is underway right now to further update the model and analysis.
* Real-time data for stations' status (including bikes parked and available spots) available [here](https://tor.publicbikesystem.net/ube/gbfs/v1/en/station_status)