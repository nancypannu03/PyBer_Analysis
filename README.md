# PyBer_Analysis

## Overview of the Analysis

- In this project, we will be assisting a ride-sharing app company in order to analyze all the ride share data from January to early May of 2019 and create a compelling visualization for the CEO. 
- Skills used - Python, Pandas library, Matplotlib library

### Purpose

We will deal with two technical analyis outcomes:
 - A ride-sharing summary DataFrame by city type - In this part, we will be dealing with the Pandas's groupby(), count(), sum(), merge() functions in order to explore    the provided data and achieve the desired results.
 
 - A multiple-line chart of total fares for each city type - Finally, we will create multiple line chart by using the plot() function from the matplotlib library.


## Results: 
Using images from the summary DataFrame and multiple-line chart, describe the differences in ride-sharing data among the different city types.
Ride-sharing data include the total rides, total drivers, total fares, average fare per ride and driver, and total fare by city type. 

Deliverable 1: Get a Summary DataFrame
#  1. Get the total rides for each city type
total_rides = pyber_data_df.groupby(["type"]).count()["ride_id"]
         total_rides
type
Rural        125
Suburban     625
Urban       1625
Name: ride_id, dtype: int64
# 2. Get the total drivers for each city type
total_drivers = city_data_df.groupby(["type"]).sum()["driver_count"]
total_drivers
type
Rural         78
Suburban     490
Urban       2405
Name: driver_count, dtype: int64
#  3. Get the total amount of fares for each city type
total_fares = pyber_data_df.groupby(["type"]).sum()["fare"]
total_fares
type
Rural        4327.93
Suburban    19356.33
Urban       39854.38
Name: fare, dtype: float64
#  4. Get the average fare per ride for each city type. 
average_fare_ride = pyber_data_df.groupby(["type"]).mean()["fare"]
average_fare_ride
type
Rural       34.623440
Suburban    30.970128
Urban       24.525772
Name: fare, dtype: float64
# 5. Get the average fare per driver for each city type. 
​
average_fare_driver = total_fares / total_drivers
average_fare_driver
type
Rural       55.486282
Suburban    39.502714
Urban       16.571468
dtype: float64
#  6. Create a PyBer summary DataFrame. 
pyber_summary_df = pd.DataFrame({"Total Rides": total_rides,"Total Drivers" : total_drivers , "Total Fares" : total_fares,"Average Fare per Ride": average_fare_ride ,"Average Fare per Driver":average_fare_driver })
pyber_summary_df
Total Rides	Total Drivers	Total Fares	Average Fare per Ride	Average Fare per Driver
type					
Rural	125	78	4327.93	34.623440	55.486282
Suburban	625	490	19356.33	30.970128	39.502714
Urban	1625	2405	39854.38	24.525772	16.571468
#  7. Cleaning up the DataFrame. Delete the index name
pyber_summary_df.index.name = None
pyber_summary_df
Total Rides	Total Drivers	Total Fares	Average Fare per Ride	Average Fare per Driver
Rural	125	78	4327.93	34.623440	55.486282
Suburban	625	490	19356.33	30.970128	39.502714
Urban	1625	2405	39854.38	24.525772	16.571468
#  8. Format the columns.
pyber_summary_df["Total Fares"] = pyber_summary_df["Total Fares"].map('${:,.2f}'.format)
​
pyber_summary_df["Average Fare per Ride"] = pyber_summary_df["Average Fare per Ride"].map('${:,.2f}'.format)
​
pyber_summary_df["Average Fare per Driver"] = pyber_summary_df["Average Fare per Driver"].map('${:,.2f}'.format)
​
pyber_summary_df



## Summary:
Based on the results, provide three business recommendations to the CEO for addressing any disparities among the city types.




