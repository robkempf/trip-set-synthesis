# trip-set-synthesis  
  
Creates a trip set of all traffic across a specific metro area on a given day.  

The total set of all vehicle trips across a metro region can be very  
useful when training transportation-based machine learning models.  
  
In this example, traffic across a metro area is synthesized using data  
estimates from local MPO (Metropolitan Planning Office) reports in the US.  
  
# Definitions  
  
## Trip Table  
A trip table is a square matrix with indicies identified by TAZ areas (Transportation Analysis Zones).  
The first dimension is by origin TAZ and the second is the destination TAZ.  
  
## Trip Sets  
Trip Sets are built from trip tables. There may be different types of trip tables,  
but the type used here estimates a trip count for a given time of day (ToD) period.  
  
## Trip Counts  
The values in trip tables represent trip counts, that is, the number  
of trips from one origin to one destination counted over the ToD period.  
Our goal is to distribute synthesized trips over the the seconds in the  
ToD period. The resultant synthesized trips combine to form a trip set.  

# Specification  
  
## Takes - (trip table, profile and TAZ files)  
A trip table is in the form of an orig/dest matrix, which is a square matrix  
of esitmated trip counts between transportation analysis zone (TAZ) pairs.  
So each index is the list of TAZ identifiers for the particular municipal planning area.  
  
## Returns - (trip set csv file)  
Trips are drawn from a poisson distribution based  
on trip rates derived from the trip table and profile.
The first line contains these labels:  
### depart, origLon, origLat, destLon, destLat  
All coordinates are in WGS84 standard decimal degrees. Each line is one trip.  
The trips are sorted chronologically by departure epoch time.  
  
# Vectorized Approach  
This approach eliminates zero-estimate origin/destination pairs *before* synthesizing trips.  
Given a typical trip table, this results in trip generation *speedups of at least 100 times*  
compared to an object oriented approach, and more than 10 times faster  
compared to carrying forward a complete trip matrix.  
