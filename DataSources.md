## Data Sources
### Wikipedia
The primary source of data for this project will be "https://en.wikipedia.org/wiki/List_of_villages_in_New_York_(state)"
This site has a table with the following columns:

Village, county, population,land,water,latitude and longintude, geoID,ANSIcode, Town

With this data to identify each of the towns with the latitude and longitude I can use foursquare to locate
nearby venues for each town.  From a table of the venues I will determine which villages are like each other.
I will also use foursquare to locate hospitals then determine how close the hosiptals are from the villages.
Villages that are more than 25 miles from the nearest hospital will be designated as needing a clinic.

###  Foursquare information
Foursquare category ids can be found at https://developer.foursquare.com/docs/resources/categories
The category ID for hospital is "4bf58dd8d48988d196941735"

This command will give me the information I need:
https://api.foursquare.com/v2/venues/search?ll=29.253219,-81.732377&categoryId=4bf58dd8d48988d196941735&client_id=MYCLIENTID&client_secret=MYCLIENTSECRET&limit=1&v=20180628

### Other data sources for Hospital information

The following additional information about hospitals in New York is available:
https://en.wikipedia.org/wiki/List_of_hospitals_in_New_York_(state)  lists hospital names and cities
https://profiles.health.ny.gov/hospital/county_or_region/service:Emergency+Department  lists hospital, ratings or services, and county or region
https://www.ahd.com/states/hospital_NY.html  lists individual Hospital Statistics including City, beds and patient Revenue
http://www.hospitalinspections.org/state/ny/ lists hospitals, city, ownership type, inspection reports
