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


4.3 Loading and Reading CSV files
# Add Matplotlib inline magic command
%matplotlib inline
​
# Dependencies and Setup
import matplotlib.pyplot as plt
import pandas as pd
​
# File to Load 
city_data_to_load = "Resources/city_data.csv"
ride_data_to_load = "Resources/ride_data.csv"
​
# Read the City and Ride Data
city_data_df = pd.read_csv(city_data_to_load)
ride_data_df = pd.read_csv(ride_data_to_load)
Merge the DataFrames
# Combine the data into a single dataset
pyber_data_df = pd.merge(ride_data_df, city_data_df, how="right", on=["city", "city"])
​
# Display the data table for preview
pyber_data_df.head()
city	date	fare	ride_id	driver_count	type
0	Richardfort	2019-02-24 08:40:38	13.93	5628545007794	38	Urban
1	Richardfort	2019-02-13 12:46:07	14.00	910050116494	38	Urban
2	Richardfort	2019-02-16 13:52:19	17.92	820639054416	38	Urban
3	Richardfort	2019-02-01 20:18:28	10.26	9554935945413	38	Urban
4	Richardfort	2019-04-17 02:26:37	23.00	720020655850	38	Urban
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
Total Rides	Total Drivers	Total Fares	Average Fare per Ride	Average Fare per Driver
Rural	125	78	$4,327.93	$34.62	$55.49
Suburban	625	490	$19,356.33	$30.97	$39.50
Urban	1625	2405	$39,854.38	$24.53	$16.57
Deliverable 2. Create a multiple line plot that shows the total weekly of the fares for each type of city.
# 1. Read the merged DataFrame
pyber_data_df.head()
city	date	fare	ride_id	driver_count	type
0	Richardfort	2019-02-24 08:40:38	13.93	5628545007794	38	Urban
1	Richardfort	2019-02-13 12:46:07	14.00	910050116494	38	Urban
2	Richardfort	2019-02-16 13:52:19	17.92	820639054416	38	Urban
3	Richardfort	2019-02-01 20:18:28	10.26	9554935945413	38	Urban
4	Richardfort	2019-04-17 02:26:37	23.00	720020655850	38	Urban
# 2. Using groupby() to create a new DataFrame showing the sum of the fares 
#  for each date where the indices are the city type and date.
sum_fares_df = pyber_data_df.groupby(["type", "date"]).sum()["fare"]
sum_fares_df
type   date               
Rural  2019-01-01 09:45:36    43.69
       2019-01-02 11:18:32    52.12
       2019-01-03 19:51:01    19.90
       2019-01-04 03:31:26    24.88
       2019-01-06 07:38:40    47.33
                              ...  
Urban  2019-05-08 04:20:00    21.99
       2019-05-08 04:39:49    18.45
       2019-05-08 07:29:01    18.55
       2019-05-08 11:38:35    19.77
       2019-05-08 13:10:18    18.04
Name: fare, Length: 2375, dtype: float64
# 3. Reset the index on the DataFrame you created in #1. This is needed to use the 'pivot()' function.
sum_fares_df = sum_fares_df.reset_index()
​
# 4. Create a pivot table with the 'date' as the index, the columns ='type', and values='fare' 
# to get the total fares for each type of city by the date. 
​
pivot_df = sum_fares_df.pivot(index="date", columns ="type", values = "fare")
pivot_df.head(10)
type	Rural	Suburban	Urban
date			
2019-01-01 00:08:16	NaN	NaN	37.91
2019-01-01 00:46:46	NaN	47.74	NaN
2019-01-01 02:07:24	NaN	24.07	NaN
2019-01-01 03:46:50	NaN	NaN	7.57
2019-01-01 05:23:21	NaN	NaN	10.75
2019-01-01 09:45:36	43.69	NaN	NaN
2019-01-01 12:32:48	NaN	25.56	NaN
2019-01-01 14:40:14	NaN	NaN	5.42
2019-01-01 14:42:25	NaN	NaN	12.31
2019-01-01 14:52:06	NaN	31.15	NaN
# 5. Create a new DataFrame from the pivot table DataFrame using loc on the given dates, '2019-01-01':'2019-04-29'.
​
loc_df = pivot_df.loc["2019-01-01":"2019-04-29"]
loc_df.head(10)
type	Rural	Suburban	Urban
date			
2019-01-01 00:08:16	NaN	NaN	37.91
2019-01-01 00:46:46	NaN	47.74	NaN
2019-01-01 02:07:24	NaN	24.07	NaN
2019-01-01 03:46:50	NaN	NaN	7.57
2019-01-01 05:23:21	NaN	NaN	10.75
2019-01-01 09:45:36	43.69	NaN	NaN
2019-01-01 12:32:48	NaN	25.56	NaN
2019-01-01 14:40:14	NaN	NaN	5.42
2019-01-01 14:42:25	NaN	NaN	12.31
2019-01-01 14:52:06	NaN	31.15	NaN
# 6. Set the "date" index to datetime datatype. This is necessary to use the resample() method in Step 8.
loc_df.index = pd.to_datetime(loc_df.index)
loc_df.index
DatetimeIndex(['2019-01-01 00:08:16', '2019-01-01 00:46:46',
               '2019-01-01 02:07:24', '2019-01-01 03:46:50',
               '2019-01-01 05:23:21', '2019-01-01 09:45:36',
               '2019-01-01 12:32:48', '2019-01-01 14:40:14',
               '2019-01-01 14:42:25', '2019-01-01 14:52:06',
               ...
               '2019-04-28 09:25:03', '2019-04-28 10:54:14',
               '2019-04-28 11:40:49', '2019-04-28 11:49:26',
               '2019-04-28 12:48:34', '2019-04-28 14:28:36',
               '2019-04-28 16:29:16', '2019-04-28 17:26:52',
               '2019-04-28 17:38:09', '2019-04-28 19:35:03'],
              dtype='datetime64[ns]', name='date', length=2196, freq=None)
# 7. Check that the datatype for the index is datetime using df.info()
loc_df.info()
<class 'pandas.core.frame.DataFrame'>
DatetimeIndex: 2196 entries, 2019-01-01 00:08:16 to 2019-04-28 19:35:03
Data columns (total 3 columns):
 #   Column    Non-Null Count  Dtype  
---  ------    --------------  -----  
 0   Rural     114 non-null    float64
 1   Suburban  573 non-null    float64
 2   Urban     1509 non-null   float64
dtypes: float64(3)
memory usage: 68.6 KB
# 8. Create a new DataFrame using the "resample()" function by week 'W' and get the sum of the fares for each week.
resample_df = loc_df.resample("W").sum()
resample_df.head(10)
type	Rural	Suburban	Urban
date			
2019-01-06	187.92	721.60	1661.68
2019-01-13	67.65	1105.13	2050.43
2019-01-20	306.00	1218.20	1939.02
2019-01-27	179.69	1203.28	2129.51
2019-02-03	333.08	1042.79	2086.94
2019-02-10	115.80	974.34	2162.64
2019-02-17	95.82	1045.50	2235.07
2019-02-24	419.06	1412.74	2466.29
2019-03-03	175.14	858.46	2218.20
2019-03-10	303.94	925.27	2470.93
# 8. Using the object-oriented interface method, plot the resample DataFrame using the df.plot() function. 
​
# Import the style from Matplotlib.
from matplotlib import style
​
# Use the graph style fivethirtyeight.
style.use('fivethirtyeight')
​
plot_fig = resample_df.plot(figsize = (15,5))
​
plt.title("Total Fare by City Type")
plt.ylabel("Fare ($USD)")
​
#Remove x-axis label
x_axis = plot_fig.axes.get_xaxis()
x_label = x_axis.get_label()
x_label.set_visible(False)
​
​
# Legend formatting
lgnd = plt.legend(loc="center", title="type")
​
#save the image
​
plt.savefig("analysis/PyBer_fare_summary.png")
​
plt.show()



## Summary:
Based on the results, provide three business recommendations to the CEO for addressing any disparities among the city types.




