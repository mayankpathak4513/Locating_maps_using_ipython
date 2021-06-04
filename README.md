# Locating_maps_using_ipython and Jupyter Notebook

## By **Mayank Pathak👨‍💻**
Various map plotting has been done using basic python coding in the jupyter notebook.
Follow to try and practice for best results.

### Run all the commands and coding snippets in the python jupyter notekook and get the suprisingly outputs.


# let's Go step by step to implement and Execute the Coding part

> Note : Run and Execute each code block in new cell of Jupyter notebook.
#### Importing Pandas and Numpy
```
import numpy as np  
import pandas as pd
```

#### Let's install Folium
```
!conda install -c conda-forge folium=0.5.0 --yes #installing folium through conda library
import folium

print('Folium installed and imported!')
```

#### Display simple map
```
# define the world map
world_map = folium.Map()

# display world map
world_map
```

#### Let's create a map centered around India and play with the zoom level to see how it affects the rendered map
```
# define the world map centered around India with a low zoom level
world_map = folium.Map(location=[20, 78], zoom_start=4)

# display world map
world_map
```

#### Let's create a Stamen Toner map of India with a zoom level of 4.
```
# create a Stamen Toner map of the world centered around India
world_map = folium.Map(location=[20, 77], zoom_start=4, tiles='Stamen Toner')

# display map
world_map
```

#### Let's create a Stamen Terrain map of Canada with zoom level 4.
```
# create a Stamen Toner map of the world centered around India
world_map = folium.Map(location=[20, 77], zoom_start=4, tiles='Stamen Terrain')

# display map
world_map
```

#### Now let us import a data from online source using the url and displat the Choropleth map
```
df_can = pd.read_excel('https://s3-api.us-geo.objectstorage.softlayer.net/cf-courses-data/CognitiveClass/DV0101EN/labs/Data_Files/Canada.xlsx',
                     sheet_name='Canada by Citizenship',
                     skiprows=range(20),
                     skipfooter=2)

print('Data downloaded and read into a dataframe!')
```

#### Show the dataframe as downloaded
```
df_can.head()
```

#### finding Dimension of the dataframe 
```
# print the dimensions of the dataframe
print(df_can.shape)
```

##### Clean up data. We will make some modifications to the original dataset to make it easier to create our visualizations. Refer to Introduction to Matplotlib and Line Plots and Area Plots, Histograms, and Bar Plots notebooks for a detailed description of this preprocessing.
```
# clean up the dataset to remove unnecessary columns (eg. REG) 
df_can.drop(['AREA','REG','DEV','Type','Coverage'], axis=1, inplace=True)

# let's rename the columns so that they make sense
df_can.rename(columns={'OdName':'Country', 'AreaName':'Continent','RegName':'Region'}, inplace=True)

# for sake of consistency, let's also make all column labels of type string
df_can.columns = list(map(str, df_can.columns))

# add total column
df_can['Total'] = df_can.sum(axis=1)

# years that we will be using in this lesson - useful for plotting later on
years = list(map(str, range(1980, 2014)))
print ('data dimensions:', df_can.shape)
```

#### Let's take a look at the first five items of our cleaned dataframe.
```
df_can.head()
```

#### In order to create a Choropleth map, we need a GeoJSON file, let's download it
```
# download countries geojson file
!wget --quiet https://s3-api.us-geo.objectstorage.softlayer.net/cf-courses-data/CognitiveClass/DV0101EN/labs/Data_Files/world_countries.json -O world_countries.json
    
print('GeoJSON file downloaded!')
```
##### Now that we have the GeoJSON file, let's create a world map, centered around [0, 0] latitude and longitude values, with an intial zoom level of 2, and using Mapbox Bright style.
```
world_geo = r'world_countries.json' # geojson file

# create a plain world map
world_map = folium.Map(location=[0, 0], zoom_start=2, tiles='Mapbox Bright')
```
```
# generate choropleth map using the total immigration of each country to Canada from 1980 to 2013
world_map.choropleth(
    geo_data=world_geo,
    data=df_can,
    columns=['Country', 'Total'],
    key_on='feature.properties.name',
    fill_color='YlOrRd', 
    fill_opacity=0.7, 
    line_opacity=0.2,
    legend_name='Immigration to Canada'
)

# display map
world_map
```

### Thank you for viewing this notebook

#### This notebook is created by [Mayank Pathak](https://www.linkedin.com/in/mayank-pathak4513/). I hope you found this lab interesting and educational. Feel free to contact me if you have any questions!

### If you liked the repository and find it useful then please **Drop a star ⭐**



