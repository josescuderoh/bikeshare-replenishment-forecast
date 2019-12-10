# Toronto Bikeshare Replenishment

## Introduction

Bike Share Toronto was designed to allow users to make short trips around town. The sturdy-framed bikes are available at any docking station in the city. The bikes can be taken from any station and returned to any station in the bike share system. 

According to the CBC, the project has required more than $ 27.5 million in public funds on service operations since 2014, and more funding is every time on the works. This has drawn several criticisms given the fact that the service is still not profitable for the city and has been **cash flow negative** and losing money for a few years now, according to CBC. Furthermore, several figures demonstrate that the service is not efficient using its resources, one big example of this is the fact that as of Nov. 2018, the service had more than 3.750 bikes for a 1.8 million ridership, a mere 480 rides per bike a year, a little over **one ride per day per bike**. This is even more worrisome today since the service offers 5.000 bikes according to their site, and rumors about service expansion have spread out. Also, and even more important it's been a proof of non-sustainable eco-friendly alternative transportation service. 

Moving 5.000 bikes around 465 stations throughout Toronto is a major engineering and planning challenge to tackle. Therefore, predicting where, when and how many bikes should be moved from a station to another using their own data would be a major help for their operations department. *This report presents a methodology for modeling the bike demand along with examples of how it could be used for prediction in this system*. It also analyzes more in depth high demand months (June, July and August) and approximates the amount of bikes to be moved using existing data.

## Data Availability

Since **public historical information about station status is not available**, in order to perform this analysis, we rely on different sources of information to approximate the variables of interest.

1. Open data for 2016 (Q3 and Q4) and 2017 (Q1 to Q4) ridership available [here](https://open.toronto.ca/dataset/bike-share-toronto-ridership-data/). This data was used to as input for predictions of bike demand at the stations of interest.

2. Online data for stations general information available through an API [here](https://tor.publicbikesystem.net/ube/gbfs/v1/en/station_information). This was used to update the information from 2017 with information from today regarding station capacity, coordinates, location, name, ID, among others.

## Required dependencies

* **Data wrangling**: pandas, numpy, datetime, fuzzywuzzy, math, requests, json
* **Viz**: matplotlib, seaborn
* **Modeling**: scikit-learn

## Remarks
* Historic data for state of stations between 2011 and 2018 is available upon request [here](https://data.cdrc.ac.uk/dataset/toronto-bss), this application process is underway right now to further update the model and analysis.
* Real-time data for stations' status (including bikes parked and available spots) available [here](https://tor.publicbikesystem.net/ube/gbfs/v1/en/station_status)