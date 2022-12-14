# PyBer_Analysis

## Overview of the Analysis

- In this project, we will be assisting a ride-sharing app company in order to analyze all the ride share data from January to early May of 2019 and create a compelling visualization for the CEO. 
- Skills used - Python, Pandas library, Matplotlib library

[City data CSV File](Resources/city_data.csv)

[Ride data CSV File](Resources/ride_data.csv)


[PyBer Challenge Code](PyBer_Challenge.ipynb)

[PyBer Fare Summary PNG](analysis/PyBer_fare_summary.png)


### Purpose

We will deal with two technical analyis outcomes:
 - A ride-sharing summary DataFrame by city type - In this part, we will be dealing with the Pandas's groupby(), count(), sum(), merge() functions in order to explore    the provided data and achieve the desired results.
 
 - A multiple-line chart of total fares for each city type - Finally, we will create multiple line chart by using the plot() function from the matplotlib library.

 - By merging both csv files, we will create the graphical representation of the        total fares (USD) versus the months for each city type .

## Results: 
Using images from the summary DataFrame and multiple-line chart, describe the differences in ride-sharing data among the different city types.

### Deliverable 1: Get a Summary DataFrame
####  1. Get the total rides for each city type
total_rides = pyber_data_df.groupby(["type"]).count()["ride_id"]
       
#### 2. Get the total drivers for each city type
total_drivers = city_data_df.groupby(["type"]).sum()["driver_count"]
total_drivers

####  3. Get the total amount of fares for each city type
total_fares = pyber_data_df.groupby(["type"]).sum()["fare"]
total_fares

#### 4. Get the average fare per ride for each city type. 
average_fare_ride = pyber_data_df.groupby(["type"]).mean()["fare"]
average_fare_ride

#### 5. Get the average fare per driver for each city type. 
​
average_fare_driver = total_fares / total_drivers
average_fare_driver

####  6. Create a PyBer summary DataFrame. 
pyber_summary_df = pd.DataFrame({"Total Rides": total_rides,"Total Drivers" : total_drivers , "Total Fares" : total_fares,"Average Fare per Ride": average_fare_ride ,"Average Fare per Driver":average_fare_driver })

pyber_summary_df

####  7. Cleaning up the DataFrame. Delete the index name

pyber_summary_df.index.name = None
pyber_summary_df

####  8. Format the columns.
pyber_summary_df["Total Fares"] = pyber_summary_df["Total Fares"].map('${:,.2f}'.format)

pyber_summary_df["Average Fare per Ride"] = pyber_summary_df["Average Fare per Ride"].map('${:,.2f}'.format)

pyber_summary_df["Average Fare per Driver"] = pyber_summary_df["Average Fare per Driver"].map('${:,.2f}'.format)

pyber_summary_df

![Test Image](/Image/pyber_summary.png)

### Deliverable 2. Create a multiple line plot that shows the total weekly of the fares for each type of city.

#### 1. Read the merged DataFrame
pyber_data_df.head()

#### 2. Using groupby() to create a new DataFrame showing the sum of the fares ,  for each date where the indices are the city type and date.

sum_fares_df = pyber_data_df.groupby(["type", "date"]).sum()["fare"]
sum_fares_df

#### 3. Reset the index on the DataFrame you created in #1. This is needed to use the 'pivot()' function.
sum_fares_df = sum_fares_df.reset_index()
​
#### 4. Create a pivot table with the 'date' as the index, the columns ='type', and values='fare' 
#### to get the total fares for each type of city by the date. 

pivot_df = sum_fares_df.pivot(index="date", columns ="type", values = "fare")
pivot_df.head(10)

#### 5. Create a new DataFrame from the pivot table DataFrame using loc on the given dates, '2019-01-01':'2019-04-29'.
​
loc_df = pivot_df.loc["2019-01-01":"2019-04-28"]
loc_df.head(10)

#### 6. Set the "date" index to datetime datatype. This is necessary to use the resample() method in Step 8.
loc_df.index = pd.to_datetime(loc_df.index)
loc_df.index

#### 7. Check that the datatype for the index is datetime using df.info()
loc_df.info()

#### 8. Create a new DataFrame using the "resample()" function by week 'W' and get the sum of the fares for each week.
resample_df = loc_df.resample("W").sum()
resample_df.head(10)

![Test Image](/Image/pyber_resample.png)


#### 9. Using the object-oriented interface method, plot the resample DataFrame using the df.plot() function. 
​
##### Import the style from Matplotlib.
from matplotlib import style
​
##### Use the graph style fivethirtyeight.
style.use('fivethirtyeight')
​
plot_fig = resample_df.plot(figsize = (15,5))
​
plt.title("Total Fare by City Type")
plt.ylabel("Fare ($USD)")
​
##### Remove x-axis label
x_axis = plot_fig.axes.get_xaxis()
x_label = x_axis.get_label()
x_label.set_visible(False)
​
​
##### Legend formatting
lgnd = plt.legend(loc="center", title="type")
​
#save the image
​
plt.savefig("analysis/PyBer_fare_summary.png")
​
plt.show()

![Test Image](/analysis/PyBer_fare_summary.png)

### Conclusion :- 
- The Summary DataFrame provides an overview of the rise-sharing facilities in three city types : Urban, Rural and Suburban. The multi line chart graph depicts that the fare($USD) for the Urban riders is more than rest of the city types(Rural and Suburban). 
  - Urban city type has the maximum number of total rides(1,625) followed by            Suburban(625) and the Rural has the lowest number of the total rides.
  - After the month of April the amount of fare decreases for Rural and Urban.
  - All three city types started with different amount of fares :
      - Urban fares started from approximately $1600 and it increases in mid of February and March
      - Rural fares are the lowest, starting from approx $250 and the fare amount keep on fluctuating with the time and experience the maximun fare value                   in April.
      - Suburban fares are starting from approx $700 and fluctuates till the mid of February and March where it experiences the highest amount almost $1400 and             then later dropped in March and mid April.
   
## Summary:
-Three business recommendations to the CEO for addressing any disparities among the city types.
   -  We can expand the range of the data by providing additional factors such as riding distance(miles covered by each driver), population in order to                   analyze the data in a more precise manner.
   - This Analysis is limited as this trend is just for the months of January, February, March and April of the year 2019. Therefore, we can expand this range of         the time(year) which will give a better explanation of having the highest average fare per driver in Rural cities despite of experiencing the lesser number of       the rides.
   - Since the Urban cities have the maximum number of total rides but still the drivers receive less fare than the Rural and Suburban cities. This would further         discourage the drivers in the the Urban cities.
 




