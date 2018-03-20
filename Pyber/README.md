

```python
import os
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn
```


```python
# Open the citys data file
city = os.path.join("generated_data", "city_data.csv")
city_df = pd.read_csv(city)
city_df.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>city</th>
      <th>driver_count</th>
      <th>type</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Davisberg</td>
      <td>2</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>1</th>
      <td>East Joshua</td>
      <td>16</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Port Jennytown</td>
      <td>43</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>3</th>
      <td>West Jessicamouth</td>
      <td>24</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Cartertown</td>
      <td>72</td>
      <td>Urban</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Open the ride data file
ride = os.path.join("generated_data", "ride_data.csv")
ride_df = pd.read_csv(ride)
ride_df.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>city</th>
      <th>date</th>
      <th>fare</th>
      <th>ride_id</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Bergermouth</td>
      <td>2018-01-20 01:01:28</td>
      <td>43.02</td>
      <td>683665871149</td>
    </tr>
    <tr>
      <th>1</th>
      <td>West Jessicamouth</td>
      <td>2018-01-14 11:34:08</td>
      <td>6.52</td>
      <td>5409319008083</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Karatown</td>
      <td>2018-01-10 12:01:16</td>
      <td>22.33</td>
      <td>9240161770669</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Port Ann</td>
      <td>2018-01-14 22:27:10</td>
      <td>40.23</td>
      <td>6581910956754</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Markchester</td>
      <td>2018-02-15 07:23:25</td>
      <td>4.26</td>
      <td>8481316754252</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Average fares by city
average_fare = ride_df.groupby(["city"])['fare'].mean().reset_index()
total_fare = ride_df.groupby(["city"])['fare'].sum().reset_index()
# Merge into city data table
city_df = city_df.merge(average_fare, on = 'city', how = "outer")
city_df = city_df.merge(total_fare, on = 'city', how = "outer")

city_df.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>city</th>
      <th>driver_count</th>
      <th>type</th>
      <th>fare_x</th>
      <th>fare_y</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Davisberg</td>
      <td>2</td>
      <td>Urban</td>
      <td>26.743043</td>
      <td>615.09</td>
    </tr>
    <tr>
      <th>1</th>
      <td>East Joshua</td>
      <td>16</td>
      <td>Urban</td>
      <td>24.764000</td>
      <td>495.28</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Port Jennytown</td>
      <td>43</td>
      <td>Urban</td>
      <td>20.618696</td>
      <td>474.23</td>
    </tr>
    <tr>
      <th>3</th>
      <td>West Jessicamouth</td>
      <td>24</td>
      <td>Urban</td>
      <td>18.545238</td>
      <td>389.45</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Cartertown</td>
      <td>72</td>
      <td>Urban</td>
      <td>24.591935</td>
      <td>762.35</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Total rider per city
total_rides = ride_df.groupby(["city"])['date'].count().reset_index()
total_rides.rename_axis({'rider_id' : 'Rider Count'}, axis=1, inplace=True)

# Merge into city data table
city_df = city_df.merge(total_rides, on = 'city', how = "outer")
city_df = city_df.rename(columns={"date": "Ride Count",
                                 "city": "City",
                                 "driver_count": "Driver Count",
                                 "type": "City Type",
                                 "fare_x": "Average Fare",
                                 "fare_y": "Total Fares"})
city_df.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>City</th>
      <th>Driver Count</th>
      <th>City Type</th>
      <th>Average Fare</th>
      <th>Total Fares</th>
      <th>Ride Count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Davisberg</td>
      <td>2</td>
      <td>Urban</td>
      <td>26.743043</td>
      <td>615.09</td>
      <td>23</td>
    </tr>
    <tr>
      <th>1</th>
      <td>East Joshua</td>
      <td>16</td>
      <td>Urban</td>
      <td>24.764000</td>
      <td>495.28</td>
      <td>20</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Port Jennytown</td>
      <td>43</td>
      <td>Urban</td>
      <td>20.618696</td>
      <td>474.23</td>
      <td>23</td>
    </tr>
    <tr>
      <th>3</th>
      <td>West Jessicamouth</td>
      <td>24</td>
      <td>Urban</td>
      <td>18.545238</td>
      <td>389.45</td>
      <td>21</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Cartertown</td>
      <td>72</td>
      <td>Urban</td>
      <td>24.591935</td>
      <td>762.35</td>
      <td>31</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Set parameters for pie charts
colors = ["gold", "lightskyblue", "lightcoral"]
explode = (0.1, 0.1, 0.1)
```


```python
# Get data for % of Total Fares by City Type
city_types = city_df.groupby(["City Type"])["Total Fares"].sum().reset_index()
city_types
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>City Type</th>
      <th>Total Fares</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Rural</td>
      <td>4452.41</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Suburban</td>
      <td>19054.47</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Urban</td>
      <td>40506.11</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Create pie chart for % of Total Fares by City Type
plt.title("% of Total Fares by City Type")
plt.pie(city_types["Total Fares"], explode=explode, labels=city_types["City Type"], colors=colors,
        autopct="%1.1f%%", shadow=True, startangle=120)
plt.axis("equal")
plt.show()
```


![png](output_7_0.png)



```python
# Get data for % of Total Rides by City Type
city_types = city_df.groupby(["City Type"])["Ride Count"].sum().reset_index()
city_types
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>City Type</th>
      <th>Ride Count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Rural</td>
      <td>125</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Suburban</td>
      <td>625</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Urban</td>
      <td>1667</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Create pie chart for % of Total Rides by City Type
plt.title("% of Total Rides by City Type")
plt.pie(city_types["Ride Count"], explode=explode, labels=city_types["City Type"], colors=colors,
        autopct="%1.1f%%", shadow=True, startangle=120)
plt.axis("equal")
plt.show()
```


![png](output_9_0.png)



```python
# Get data for % of Total Drivers by City Type
city_types = city_df.groupby(["City Type"])["Driver Count"].sum().reset_index()
city_types
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>City Type</th>
      <th>Driver Count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Rural</td>
      <td>110</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Suburban</td>
      <td>610</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Urban</td>
      <td>2151</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Create pie chart for % of Total Drivers by City Type
plt.title("% of Total Drivers by City Type")
plt.pie(city_types["Driver Count"], explode=explode, labels=city_types["City Type"], colors=colors,
        autopct="%1.1f%%", shadow=True, startangle=140)
plt.axis("equal")
plt.show()
```


![png](output_11_0.png)



```python
# Bubble Chart
# Set gray grid
seaborn.set(style='darkgrid')
# Set data for plot colors
_types= ['Urban', 'Suburban', 'Rural']

# Set palette - color names do not match but the actual colors are close
pal = dict(Rural = "#FFCC33", Suburban = "#76D7EA", Urban = "#ff6163")


fg = seaborn.FacetGrid(data=city_df, hue='City Type', palette=pal, hue_order=_types, aspect=1.61)
fg.map(plt.scatter, 'Ride Count', 'Average Fare', 
       s=city_df["Driver Count"]*1.3, alpha=0.9, edgecolors="black", linewidth=1) # .add_legend()

plt.xlabel('Total Number of Rides (Per City)')
plt.ylabel('Average Fare ($)')
plt.title('Pyber Ride Sharing Data')
plt.legend()
plt.show()
```


![png](output_12_0.png)

