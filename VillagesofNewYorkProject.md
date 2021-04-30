# Coursara Capstone project - Villages of New York
## This study will compare the villages of New York by important venues and distance from hospitals.
## Introduction/Business Problem
Villages have many issues that are similar to larger cities without the resources of larger cities. Some villages are near major cities, others are very rural and may not have essential services like hospitals or even grocery stores.

We will use foursquare to determine the types of venues available in these villages or the distance to essential services. We will group the venues using k-means to determine the villages of similar caracteristics that may require similar services.

In addition we will identify all of the hospitals in New York and determine how far they are from each village.

### The Audience:
State and local governments can use the results of this study to allocate resources and develop plans to help the people and their villages. The specific result of this study is to identify those villages needing medical facilities.

## Data Sources
### Wikipedia
The primary source of data for this project will be "https://en.wikipedia.org/wiki/List_of_villages_in_New_York_(state)"
This site has a table with the following columns:

Village, county, population,land,water,latitude and longintude, geoID,ANSIcode, Town

With this data to identify each of the villages with their latitude and longitude. I can use foursquare to locate
nearby venues for each village.  From a table of the venues I will determine which villages are like each other.

###  Foursquare hospital information
I attempted to use Foursquare to get hospital infomration as follows:
Foursquare category ids can be found at https://developer.foursquare.com/docs/resources/categories
The category ID for hospital is "4bf58dd8d48988d196941735"

This command should give me the information I needed:
https://api.foursquare.com/v2/venues/search?ll=29.253219,-81.732377&categoryId=4bf58dd8d48988d196941735&client_id=MYCLIENTID&client_secret=MYCLIENTSECRET&limit=1&v=20180628

However Foursquare did not get me the information in a manor I could use so I investigated other sources for hospital information.

### Other data sources for Hospital information

    The following additional information about hospitals in New York is available:
https://en.wikipedia.org/wiki/List_of_hospitals_in_New_York_(state)  lists hospital names and cities
https://profiles.health.ny.gov/hospital/county_or_region/service:Emergency+Department  lists hospital, ratings or services, and county or region
https://www.ahd.com/states/hospital_NY.html  lists individual Hospital Statistics including City, beds and patient Revenue
http://www.hospitalinspections.org/state/ny/ lists hospitals, city, ownership type, inspection reports
https://www.health.ny.gov/regulations/hcra/provider/provhosp.htm lists hospitals, the hospital address and its current operating status
### Geopy
Latitudes and Longitudes came from geopy.geocoders Nominatim library

## Methodology
### Get village information
The first step of the project was to survey web sites to determine which had relevant information.  I found the wikipedia and ny.gov web sites to be the most useful.
I started with the wikipedia list of villages in New York State.  This table had village names and Latitude and Longitude.
To begin the program I used google developer tools to find a unique identifier for the table that was required.  Once this was identified I used BeautifulSoup to extract the table data and then built a dataframe.
I extracted the latitude and longitude for each village and used them to populate a map of New York State.   This allowed me to visuallize the location of the villages with the state.
### group villages by similar characteristics using K-means
I then used Foursquare to determine the 10 most visited venues in each village. Then build a dataframe with village and venue information.
I used K-means to cluster the villages together.  I tried creating 5, 6, 7, 8, and 9 clusters. I determined that 7 clusters gave me the most even distribution of villages among the clusters. 
Each villages was color coded by cluster and illustrated on the map.  I then printed data for each village/cluster
### Gather information about New York State Hospitals
After investigating a number of web sites the included Hospitals in New York State I determined that the website with the data most relevent to this project was https://www.health.ny.gov/regulations/hcra/provider/provhosp.htm. This site included a table of hospital names, addresses and current operating status.  

I analyzed the website using the developer tools in google chrome then used BeautifulSoup to extract the data table.

I created a dataframe from the table and removed all the hospital rows that were not "OPEN".

Geopy.geocoders Nominatim was used to get latitude and longitude from the hospital addresses.  If the geocoder could not translate the address then the address was shortened to just the city, state, and zipcode.  This returned the Latitude and Longitude of the city where the hospital is located.  As hospitals tend to be located near city centers, the error introduced through this approximation is minimal.
### Determine the distance from each village to each hospital
To determine the distance between a village and a hospital I used the geopy distance function.  I chose to use the Great_Circle method to determine the straight line distance between two locations.  Next a 2 dimensional array was created with the distance between each village and each hospital. 
Finally, I created a dataframe with each village and the distance to the closest hospital.  
### Group villages by distance from the nearest hospital
Lastly, I grouped the villages by their distance from the nearest hospital.  I arbitrarily picked 4 groups; less than 5 miles to the hospital, between 5 and 10 miles to the hospital, between 10 and 20 miles to a hospital, and greater then 20 miles from a hospital.   From these groups I identfied those villages that are more then 20 miles from the nearest hospital.

## Results
### Observations of village clusters
There are 533 incorporated villages in New York state that fall into 7 clusters:
Cluster number 0 has 44 villages and is made up of rural villages that are large enough to have parks.
Cluster number 1 has 92 villages that are mostly upstate and are not in the other clusters. 
Cluster number 2 has 6 villages and is distinguished by having a marina and is on the coast of large bodies of water.
Cluster number 3 has 6 villages, all located on the coasts of Long Island.
Cluster number 4 has 32 villages is mostly rural and tends to be far from large population centers.
Cluster number 5 has 14 villages is also very rural. 
Cluster number 6 has 293 villages is the largest cluster and is distributed evenly throughout the state. 

When mapping the clusters it was a supprise to see that the members of many of the clusters are evenly distributed throughout the state. 
Most of the villages are a distance away from urban centers. 
Although K-means clustering separted clusters 2 and 3 I would put them together as all of the villages are located along coastlines.
### Observations of Hospitals
There are 279 hospitals in New York of which only 166 are currently open.  Hospitals are usually closed for economic reasons and the closed hospitals may have largly served rural areas.

The villages are separted from hospitals as follows:
- Number of villages less than 5 miles from a hospital:  218
- Number of villages 5 to 10 milles from a hospital:  134
- Number of villages 10 to 20 milles from a hospital:  163
- Number of villages more than 20 milles from a hospital:  18

The villages that are more than 20 miles from a hospital are:
-        Village         Distance to Hospital
-        Afton              20.7
-        Cambridge          21.5
-        Cape Vincent       23.4
-        Dering Harbor      20.5
-        East Hampton       24.9
-        Gainesville        23.0
-        Granville          20.3
-        Hunter             22.0
-        Lacona             23.5
-        Nichols            22.8
-        Pulaski            20.8
-        Rouses Point       20.7
-        Sag Harbor         20.5
-        Sagaponack         21.2
-        Sandy Creek        23.8
-        Silver Springs     21.0
-        Speculator         35.8
-        Whitehall          20.8

## Discussion
### More Hospitals needed
Rapid access to hospitals is crucial for an indivduals survability from a medical emergancy.  This study identified 18 villages that are more than 20 miles from the nearest hospital.  20 miles of straight line distance will take more than 30 minutes on the roads many of these villagers will need to travel to get to the hospital. 
Some of these villages are near each other such as Pulaski and Sandy Creek.  A single emergency care facility located between them would significantly cut the travel time for these villagers.
I recommend placing emergency care facilies near these villages to increase the likely hood of survival from events like heart attacks or severe injuries.
### Rural Villages
Despite the notoriaty of New York State's large cities such as NYC and Buffalo, New York's villages are largely rural and can be far from hospitals, shopping and other infrastructure.  This tends to make the villages feel isolated and self sufficiant.  The politics of villages is different than that of cites and I recommend further study in this area.

## Conclusion
From this initial study we can see that there are 18 villages in disperate need of emergency health care facilities. Further study is required to determine the ideal placement of such facilities. It is also clear that these villages are largely rural and may not be able to financily support such facilities. Therefore state government may need to sponser them.

Further analysis may lead to better groupings of the villages so specific infrastructure needs can be identified and targeted to them.