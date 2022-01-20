---
layout: post
title: ☆Blog Post 1☆
---
Using plotly, sqlite3, fancy pandas to create nice interactive plots with the NOAA data!
###施工中####
```python
import sqlite3
import pandas as pd
import numpy as np
```


```python
conn = sqlite3.connect("temps.db")
```


```python
temperatures = pd.read_csv("temps.csv")
stations = pd.read_csv("https://raw.githubusercontent.com/PhilChodrow/PIC16B/master/datasets/noaa-ghcn/station-metadata.csv")
countries = pd.read_csv("countries.csv")
```


```python
countries.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>FIPS 10-4</th>
      <th>ISO 3166</th>
      <th>Name</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>AF</td>
      <td>AF</td>
      <td>Afghanistan</td>
    </tr>
    <tr>
      <th>1</th>
      <td>AX</td>
      <td>-</td>
      <td>Akrotiri</td>
    </tr>
    <tr>
      <th>2</th>
      <td>AL</td>
      <td>AL</td>
      <td>Albania</td>
    </tr>
    <tr>
      <th>3</th>
      <td>AG</td>
      <td>DZ</td>
      <td>Algeria</td>
    </tr>
    <tr>
      <th>4</th>
      <td>AQ</td>
      <td>AS</td>
      <td>American Samoa</td>
    </tr>
  </tbody>
</table>
</div>




```python
stations.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>ID</th>
      <th>LATITUDE</th>
      <th>LONGITUDE</th>
      <th>STNELEV</th>
      <th>NAME</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>ACW00011604</td>
      <td>57.7667</td>
      <td>11.8667</td>
      <td>18.0</td>
      <td>SAVE</td>
    </tr>
    <tr>
      <th>1</th>
      <td>AE000041196</td>
      <td>25.3330</td>
      <td>55.5170</td>
      <td>34.0</td>
      <td>SHARJAH_INTER_AIRP</td>
    </tr>
    <tr>
      <th>2</th>
      <td>AEM00041184</td>
      <td>25.6170</td>
      <td>55.9330</td>
      <td>31.0</td>
      <td>RAS_AL_KHAIMAH_INTE</td>
    </tr>
    <tr>
      <th>3</th>
      <td>AEM00041194</td>
      <td>25.2550</td>
      <td>55.3640</td>
      <td>10.4</td>
      <td>DUBAI_INTL</td>
    </tr>
    <tr>
      <th>4</th>
      <td>AEM00041216</td>
      <td>24.4300</td>
      <td>54.4700</td>
      <td>3.0</td>
      <td>ABU_DHABI_BATEEN_AIR</td>
    </tr>
  </tbody>
</table>
</div>




```python
stations["fips"] = stations["ID"].str[:2]
stations.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>ID</th>
      <th>LATITUDE</th>
      <th>LONGITUDE</th>
      <th>STNELEV</th>
      <th>NAME</th>
      <th>fips</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>ACW00011604</td>
      <td>57.7667</td>
      <td>11.8667</td>
      <td>18.0</td>
      <td>SAVE</td>
      <td>AC</td>
    </tr>
    <tr>
      <th>1</th>
      <td>AE000041196</td>
      <td>25.3330</td>
      <td>55.5170</td>
      <td>34.0</td>
      <td>SHARJAH_INTER_AIRP</td>
      <td>AE</td>
    </tr>
    <tr>
      <th>2</th>
      <td>AEM00041184</td>
      <td>25.6170</td>
      <td>55.9330</td>
      <td>31.0</td>
      <td>RAS_AL_KHAIMAH_INTE</td>
      <td>AE</td>
    </tr>
    <tr>
      <th>3</th>
      <td>AEM00041194</td>
      <td>25.2550</td>
      <td>55.3640</td>
      <td>10.4</td>
      <td>DUBAI_INTL</td>
      <td>AE</td>
    </tr>
    <tr>
      <th>4</th>
      <td>AEM00041216</td>
      <td>24.4300</td>
      <td>54.4700</td>
      <td>3.0</td>
      <td>ABU_DHABI_BATEEN_AIR</td>
      <td>AE</td>
    </tr>
  </tbody>
</table>
</div>




```python
temperatures.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>ID</th>
      <th>Year</th>
      <th>VALUE1</th>
      <th>VALUE2</th>
      <th>VALUE3</th>
      <th>VALUE4</th>
      <th>VALUE5</th>
      <th>VALUE6</th>
      <th>VALUE7</th>
      <th>VALUE8</th>
      <th>VALUE9</th>
      <th>VALUE10</th>
      <th>VALUE11</th>
      <th>VALUE12</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>ACW00011604</td>
      <td>1961</td>
      <td>-89.0</td>
      <td>236.0</td>
      <td>472.0</td>
      <td>773.0</td>
      <td>1128.0</td>
      <td>1599.0</td>
      <td>1570.0</td>
      <td>1481.0</td>
      <td>1413.0</td>
      <td>1174.0</td>
      <td>510.0</td>
      <td>-39.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>ACW00011604</td>
      <td>1962</td>
      <td>113.0</td>
      <td>85.0</td>
      <td>-154.0</td>
      <td>635.0</td>
      <td>908.0</td>
      <td>1381.0</td>
      <td>1510.0</td>
      <td>1393.0</td>
      <td>1163.0</td>
      <td>994.0</td>
      <td>323.0</td>
      <td>-126.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>ACW00011604</td>
      <td>1963</td>
      <td>-713.0</td>
      <td>-553.0</td>
      <td>-99.0</td>
      <td>541.0</td>
      <td>1224.0</td>
      <td>1627.0</td>
      <td>1620.0</td>
      <td>1596.0</td>
      <td>1332.0</td>
      <td>940.0</td>
      <td>566.0</td>
      <td>-108.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>ACW00011604</td>
      <td>1964</td>
      <td>62.0</td>
      <td>-85.0</td>
      <td>55.0</td>
      <td>738.0</td>
      <td>1219.0</td>
      <td>1442.0</td>
      <td>1506.0</td>
      <td>1557.0</td>
      <td>1221.0</td>
      <td>788.0</td>
      <td>546.0</td>
      <td>112.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>ACW00011604</td>
      <td>1965</td>
      <td>44.0</td>
      <td>-105.0</td>
      <td>38.0</td>
      <td>590.0</td>
      <td>987.0</td>
      <td>1500.0</td>
      <td>1487.0</td>
      <td>1477.0</td>
      <td>1377.0</td>
      <td>974.0</td>
      <td>31.0</td>
      <td>-178.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
def prepare_df(df):
    df = df.set_index(keys=["ID", "Year"])
    df = df.stack()
    df = df.reset_index()
    df = df.rename(columns = {"level_2"  : "Month" , 0 : "Temp"})
    df["Month"] = df["Month"].str[5:].astype(int)
    df["Temp"]  = df["Temp"] / 100
    return(df)
```


```python
prepare_df(temperatures)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>ID</th>
      <th>Year</th>
      <th>Month</th>
      <th>Temp</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>ACW00011604</td>
      <td>1961</td>
      <td>1</td>
      <td>-0.89</td>
    </tr>
    <tr>
      <th>1</th>
      <td>ACW00011604</td>
      <td>1961</td>
      <td>2</td>
      <td>2.36</td>
    </tr>
    <tr>
      <th>2</th>
      <td>ACW00011604</td>
      <td>1961</td>
      <td>3</td>
      <td>4.72</td>
    </tr>
    <tr>
      <th>3</th>
      <td>ACW00011604</td>
      <td>1961</td>
      <td>4</td>
      <td>7.73</td>
    </tr>
    <tr>
      <th>4</th>
      <td>ACW00011604</td>
      <td>1961</td>
      <td>5</td>
      <td>11.28</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>13992657</th>
      <td>ZIXLT622116</td>
      <td>1970</td>
      <td>8</td>
      <td>15.40</td>
    </tr>
    <tr>
      <th>13992658</th>
      <td>ZIXLT622116</td>
      <td>1970</td>
      <td>9</td>
      <td>20.40</td>
    </tr>
    <tr>
      <th>13992659</th>
      <td>ZIXLT622116</td>
      <td>1970</td>
      <td>10</td>
      <td>20.30</td>
    </tr>
    <tr>
      <th>13992660</th>
      <td>ZIXLT622116</td>
      <td>1970</td>
      <td>11</td>
      <td>21.30</td>
    </tr>
    <tr>
      <th>13992661</th>
      <td>ZIXLT622116</td>
      <td>1970</td>
      <td>12</td>
      <td>21.50</td>
    </tr>
  </tbody>
</table>
<p>13992662 rows × 4 columns</p>
</div>




```python
df_iter = pd.read_csv("temps.csv", chunksize = 100000)
for df in df_iter:
    df = prepare_df(df)
    df.to_sql("temperatures", conn, if_exists = "append", index = False)
```


```python
stations.to_sql("stations", conn, if_exists = "replace", index = False)
countries.to_sql("countries", conn, if_exists = "replace", index = False)
```

    /Users/kadomiii/opt/anaconda3/envs/PIC16B/lib/python3.8/site-packages/pandas/core/generic.py:2872: UserWarning: The spaces in these column names will not be changed. In pandas versions < 0.14, spaces were converted to underscores.
      sql.to_sql(



```python
cursor = conn.cursor()
cursor.execute("SELECT name FROM sqlite_master WHERE type = 'table'")
print(cursor.fetchall())
```

    [('temperatures',), ('stations',), ('countries',)]



```python
conn.close()
```


```python
def query_climate_database(country, year_begin, year_end, month):
        conn = sqlite3.connect("temps.db")
        cmd = \
        """
        SELECT S.name, S.latitude, S.longitude, C.name, T.year, T.month, T.temp
        FROM temperatures T
        LEFT JOIN stations S on S.id = T.id
        LEFT JOIN countries C ON C.'fips 10-4' = S.fips
        WHERE C.name = ? AND (T.year BETWEEN ? AND ?) AND T.month = ?
        """
        df = pd.read_sql_query(cmd, conn, params=(country, year_begin, year_end, month))
        conn.close()
        return df

```


```python
query_climate_database(country = "India", 
                       year_begin = 1980, 
                       year_end = 2020,
                       month = 1)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>NAME</th>
      <th>LATITUDE</th>
      <th>LONGITUDE</th>
      <th>Name</th>
      <th>Year</th>
      <th>Month</th>
      <th>Temp</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>PBO_ANANTAPUR</td>
      <td>14.583</td>
      <td>77.633</td>
      <td>India</td>
      <td>1980</td>
      <td>1</td>
      <td>23.48</td>
    </tr>
    <tr>
      <th>1</th>
      <td>PBO_ANANTAPUR</td>
      <td>14.583</td>
      <td>77.633</td>
      <td>India</td>
      <td>1981</td>
      <td>1</td>
      <td>24.57</td>
    </tr>
    <tr>
      <th>2</th>
      <td>PBO_ANANTAPUR</td>
      <td>14.583</td>
      <td>77.633</td>
      <td>India</td>
      <td>1982</td>
      <td>1</td>
      <td>24.19</td>
    </tr>
    <tr>
      <th>3</th>
      <td>PBO_ANANTAPUR</td>
      <td>14.583</td>
      <td>77.633</td>
      <td>India</td>
      <td>1983</td>
      <td>1</td>
      <td>23.51</td>
    </tr>
    <tr>
      <th>4</th>
      <td>PBO_ANANTAPUR</td>
      <td>14.583</td>
      <td>77.633</td>
      <td>India</td>
      <td>1984</td>
      <td>1</td>
      <td>24.81</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>18907</th>
      <td>DARJEELING</td>
      <td>27.050</td>
      <td>88.270</td>
      <td>India</td>
      <td>1983</td>
      <td>1</td>
      <td>5.10</td>
    </tr>
    <tr>
      <th>18908</th>
      <td>DARJEELING</td>
      <td>27.050</td>
      <td>88.270</td>
      <td>India</td>
      <td>1986</td>
      <td>1</td>
      <td>6.90</td>
    </tr>
    <tr>
      <th>18909</th>
      <td>DARJEELING</td>
      <td>27.050</td>
      <td>88.270</td>
      <td>India</td>
      <td>1994</td>
      <td>1</td>
      <td>8.10</td>
    </tr>
    <tr>
      <th>18910</th>
      <td>DARJEELING</td>
      <td>27.050</td>
      <td>88.270</td>
      <td>India</td>
      <td>1995</td>
      <td>1</td>
      <td>5.60</td>
    </tr>
    <tr>
      <th>18911</th>
      <td>DARJEELING</td>
      <td>27.050</td>
      <td>88.270</td>
      <td>India</td>
      <td>1997</td>
      <td>1</td>
      <td>5.70</td>
    </tr>
  </tbody>
</table>
<p>18912 rows × 7 columns</p>
</div>




```python
from sklearn.linear_model import LinearRegression

def coef(data_group):
    x = data_group[["Year"]] # x is a dataframe
    y = data_group["Temp"]   
    LR = LinearRegression()
    LR.fit(x, y)
    return LR.coef_[0]
```


```python
def temperature_coefficient_plot(country, year_begin, year_end, month, min_obs, **kwargs):
    df = query_climate_database(country = country, 
                       year_begin = year_begin, 
                       year_end = year_end,
                       month = month)
    
    #add a new column to count the number of years
    df["YEARS"] = df.groupby(["NAME"])['NAME'].transform(lambda x: x.shape[0])
    #exclude stations that doesn't achieve the minimum requirement of years
    df1 = df[df['YEARS'] >= min_obs]
    
    #estimate yearly temperature increases using the coef function
    coefs = df1.groupby(["NAME", "Month"]).apply(coef)
    coefs = coefs.reset_index()
    
    
        #merge the estimation result with stations
    incr = pd.merge(coefs, stations, on = "NAME")
    
    #rename coef column to "Estimated Yearly Increase(°C)" and round the yearly increases
    incr = incr.rename(columns = {0:"Estimated Yearly Increase(°C)"})
    incr = incr[["Estimated Yearly Increase(°C)", 'NAME', 'LATITUDE','LONGITUDE']]
    incr['Estimated Yearly Increase(°C)'] = incr['Estimated Yearly Increase(°C)'].round(4)
    
    #creat a dict for the title
   # monthDict={1:'Janurary', 2:'Feburary', 3:'March', 4:'April', 5:'May', 6:'June', 7:'July', 8:'August', 
              # 9:'September', 10:'October', 11:'November', 12:'December'}
    #month = monthDict[int(month)]
    
    #draw the plotly plot add titles and hover overs
    fig = px.scatter_mapbox(incr, 
                        lon = "LONGITUDE", 
                        lat = "LATITUDE",
                        hover_name = "NAME", 
                        color = "Estimated Yearly Increase(°C)", 
                        title="Estimates of yearly increase in Temperature in " + str(month) + " for stations in "
                            + country +", years " + str(year_begin) + " - " + str(year_end),
                        **kwargs)
    return fig
    
```


```python
from plotly import express as px
color_map = px.colors.diverging.RdGy_r # choose a colormap

fig = temperature_coefficient_plot("India", 1980, 2020, 1, 
                                   min_obs = 10,
                                   zoom = 2,
                                   mapbox_style="carto-positron",
                                   color_continuous_scale=color_map)

fig.show()
```


<div>                            <div id="db27d47a-26f4-4edc-8f6a-0d0e18fbcdd8" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("db27d47a-26f4-4edc-8f6a-0d0e18fbcdd8")) {                    Plotly.newPlot(                        "db27d47a-26f4-4edc-8f6a-0d0e18fbcdd8",                        [{"hovertemplate":"<b>%{hovertext}</b><br><br>LATITUDE=%{lat}<br>LONGITUDE=%{lon}<br>Estimated Yearly Increase(\u00b0C)=%{marker.color}<extra></extra>","hovertext":["AGARTALA","AGRA","AHMADABAD","AKOLA","AKOLA","ALLAHABAD","ALLAHABAD_BAMHRAULI","AMBALA","AMRITSAR","AURANGABAD_CHIKALTH","BALASORE","BANGALORE","BANGALORE_HINDUSTAN","BAREILLY","BEGUMPETOBSY","BELGAUM_SAMBRA","BHOPAL_BAIRAGARH","BHUBANESWAR","BHUJ_RUDRAMATA","BIKANER","BOMBAY_COLABA","BOMBAY_SANTACRUZ","CALCUTTA_ALIPORE","CALCUTTA_DUM_DUM","CHERRAPUNJI","CHERRA_POONJEE","CHITRADURGA","COIMBATORE_PEELAMED","CUDDALORE","CUTTACK","DALTONGANJ","DARBHANGA","DARJEELING","DEHRADUN","DIBRUGARH_MOHANBAR","DUMKA","DWARKA","GADAG","GANGANAGAR","GAUHATI","GAYA","GAYA","GOA_PANJIM","GORAKHPUR","GUNA","GWALIOR","HISSAR","HONAVAR","IMPHAL","INDORE","JABALPUR","JAGDALPUR","JAIPUR_SANGANER","JAISALMER","JHARSUGUDA","JODHPUR","KAKINADA","KARAIKAL","KODAIKANAL","KOTA_AERODROME","KOZHIKODE","KURNOOL","LUCKNOW_AMAUSI","LUDHIANA","MACHILIPATNAM","MADRAS_MINAMBAKKAM","MANGALORE_BAJPE","MINICOY","MINICOYOBSY","MO_AMINI","MO_RANCHI","MUKTESWAR_KUMAON","NAGPUR_SONEGAON","NELLORE","NEW_DELHI_PALAM","NEW_DELHI_SAFDARJUN","NORTH_LAKHIMPUR","PAMBAN","PATIALA","PATNA","PBO_ANANTAPUR","PENDRA_ROAD","POONA","PORT_BLAIR","RAIPUR","RAJKOT","RAMGUNDAM","RATNAGIRI","SAGAR","SANDHEADS","SATNA","SHILONG","SHIMLA","SHOLAPUR","SILCHAR","SRINAGAR","SURAT","SURAT","TEZPUR","THIRUVANANTHAPURAM","THIRUVANANTHAPURAM","TIRUCHCHIRAPALLI","TRIVANDRUM","UDAIPUR_DABOK","VARANASI_BABATPUR","VERAVAL","VISHAKHAPATNAM"],"lat":[23.883,27.1667,23.067,20.7,20.7,25.441,25.5,30.383,31.71,19.85,21.517,12.967,12.95,28.367,17.45,15.85,23.283,20.25,23.25,28.0,18.9,19.117,22.533,22.65,25.25,25.25,14.233,11.033,11.767,20.467,24.05,26.1667,27.05,30.317,27.483,24.267,22.3667,15.417,29.917,26.1,24.75,11.883,15.483,26.75,24.65,26.233,29.167,14.283,24.667,22.717,23.2,19.083,26.817,26.9,21.917,26.3,16.95,10.917,10.2333,25.15,11.25,15.8,26.75,30.9333,16.2,13.0,12.917,8.3,8.3,11.117,23.317,29.4667,21.1,14.45,28.567,28.583,27.233,9.267,30.333,25.6,14.583,22.767,18.533,11.667,21.217,22.3,18.767,16.983,23.85,20.85,24.567,25.6,31.1,17.667,24.82,34.083,-27.159,21.2,26.617,8.483,8.467,10.767,8.5,24.617,25.45,20.9,17.717],"legendgroup":"","lon":[91.25,78.0333,72.633,77.033,77.067,81.735,81.9,76.767,74.797,75.4,86.933,77.583,77.633,79.4,78.47,74.617,77.35,85.833,69.667,73.3,72.8167,72.85,88.333,88.45,91.7333,91.73,76.433,77.05,79.767,85.933,84.067,85.9,88.27,78.033,95.017,87.25,69.0833,75.633,73.917,91.583,84.95,3.45,73.817,83.367,77.317,78.25,75.733,74.45,93.9,75.8,79.95,82.033,75.8,70.917,84.083,73.017,82.233,79.833,77.4667,75.85,75.783,78.067,80.883,75.8667,81.15,80.183,74.883,73.15,73.0,72.733,85.317,79.65,79.05,79.983,77.117,77.2,94.117,79.3,76.467,85.1,77.633,81.9,73.85,92.717,81.667,70.783,79.433,73.333,78.75,88.25,80.833,91.89,77.167,75.9,92.83,74.833,149.0703,72.833,92.783,76.95,76.95,78.717,77.0,73.883,82.867,70.367,83.233],"marker":{"color":[-0.0062,-0.0954,0.0067,-0.0081,-0.0081,-0.0294,-0.0155,0.0814,-0.007,0.0124,-0.0085,0.036,0.05,-0.0169,0.0107,-0.0036,-0.0221,-0.0161,0.0699,0.0172,0.0114,0.0347,-0.0103,-0.0162,0.0465,-0.022,0.0156,0.0344,0.0208,-0.0101,-0.0046,-0.0716,-0.0401,0.0436,0.052,-0.0821,0.017,0.0089,-0.0076,0.0141,-0.0181,-0.0181,0.0224,0.0039,-0.0092,-0.0184,-0.0034,-0.0492,0.0633,0.003,-0.0252,-0.0001,0.0067,0.0026,-0.0185,0.0097,0.026,0.0213,0.0691,-0.0186,0.0334,0.0257,-0.0372,0.1318,0.0123,0.0237,0.0233,0.0252,0.0141,-0.0062,0.0053,0.0155,-0.0246,0.0235,0.0056,-0.0024,0.0955,0.0248,-0.0116,-0.0219,0.0263,-0.0096,-0.0004,0.0307,0.0014,0.0157,-0.0089,0.0032,0.0015,-0.0524,-0.0226,0.0135,0.004,0.0038,0.0161,0.0488,0.0232,0.0232,0.0394,0.0162,0.0162,0.0146,0.0229,0.0724,-0.013,0.0248,-0.034],"coloraxis":"coloraxis"},"mode":"markers","name":"","showlegend":false,"subplot":"mapbox","type":"scattermapbox"}],                        {"coloraxis":{"colorbar":{"title":{"text":"Estimated Yearly Increase(\u00b0C)"}},"colorscale":[[0.0,"rgb(26,26,26)"],[0.1,"rgb(77,77,77)"],[0.2,"rgb(135,135,135)"],[0.3,"rgb(186,186,186)"],[0.4,"rgb(224,224,224)"],[0.5,"rgb(255,255,255)"],[0.6,"rgb(253,219,199)"],[0.7,"rgb(244,165,130)"],[0.8,"rgb(214,96,77)"],[0.9,"rgb(178,24,43)"],[1.0,"rgb(103,0,31)"]]},"legend":{"tracegroupgap":0},"mapbox":{"center":{"lat":20.67351775700934,"lon":79.7092457943925},"domain":{"x":[0.0,1.0],"y":[0.0,1.0]},"style":"carto-positron","zoom":2},"template":{"data":{"bar":[{"error_x":{"color":"#2a3f5f"},"error_y":{"color":"#2a3f5f"},"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"bar"}],"barpolar":[{"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"barpolar"}],"carpet":[{"aaxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"baxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"type":"carpet"}],"choropleth":[{"colorbar":{"outlinewidth":0,"ticks":""},"type":"choropleth"}],"contour":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"contour"}],"contourcarpet":[{"colorbar":{"outlinewidth":0,"ticks":""},"type":"contourcarpet"}],"heatmap":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"heatmap"}],"heatmapgl":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"heatmapgl"}],"histogram":[{"marker":{"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"histogram"}],"histogram2d":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"histogram2d"}],"histogram2dcontour":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"histogram2dcontour"}],"mesh3d":[{"colorbar":{"outlinewidth":0,"ticks":""},"type":"mesh3d"}],"parcoords":[{"line":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"parcoords"}],"pie":[{"automargin":true,"type":"pie"}],"scatter":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatter"}],"scatter3d":[{"line":{"colorbar":{"outlinewidth":0,"ticks":""}},"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatter3d"}],"scattercarpet":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scattercarpet"}],"scattergeo":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scattergeo"}],"scattergl":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scattergl"}],"scattermapbox":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scattermapbox"}],"scatterpolar":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatterpolar"}],"scatterpolargl":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatterpolargl"}],"scatterternary":[{"marker":{"colorbar":{"outlinewidth":0,"ticks":""}},"type":"scatterternary"}],"surface":[{"colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"type":"surface"}],"table":[{"cells":{"fill":{"color":"#EBF0F8"},"line":{"color":"white"}},"header":{"fill":{"color":"#C8D4E3"},"line":{"color":"white"}},"type":"table"}]},"layout":{"annotationdefaults":{"arrowcolor":"#2a3f5f","arrowhead":0,"arrowwidth":1},"autotypenumbers":"strict","coloraxis":{"colorbar":{"outlinewidth":0,"ticks":""}},"colorscale":{"diverging":[[0,"#8e0152"],[0.1,"#c51b7d"],[0.2,"#de77ae"],[0.3,"#f1b6da"],[0.4,"#fde0ef"],[0.5,"#f7f7f7"],[0.6,"#e6f5d0"],[0.7,"#b8e186"],[0.8,"#7fbc41"],[0.9,"#4d9221"],[1,"#276419"]],"sequential":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"sequentialminus":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]},"colorway":["#636efa","#EF553B","#00cc96","#ab63fa","#FFA15A","#19d3f3","#FF6692","#B6E880","#FF97FF","#FECB52"],"font":{"color":"#2a3f5f"},"geo":{"bgcolor":"white","lakecolor":"white","landcolor":"#E5ECF6","showlakes":true,"showland":true,"subunitcolor":"white"},"hoverlabel":{"align":"left"},"hovermode":"closest","mapbox":{"style":"light"},"paper_bgcolor":"white","plot_bgcolor":"#E5ECF6","polar":{"angularaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"bgcolor":"#E5ECF6","radialaxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"scene":{"xaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","gridwidth":2,"linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white"},"yaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","gridwidth":2,"linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white"},"zaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","gridwidth":2,"linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white"}},"shapedefaults":{"line":{"color":"#2a3f5f"}},"ternary":{"aaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"baxis":{"gridcolor":"white","linecolor":"white","ticks":""},"bgcolor":"#E5ECF6","caxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"title":{"x":0.05},"xaxis":{"automargin":true,"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","zerolinewidth":2},"yaxis":{"automargin":true,"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","zerolinewidth":2}}},"title":{"text":"Estimates of yearly increase in Temperature in 1 for stations in India, years 1980 - 2020"}},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('db27d47a-26f4-4edc-8f6a-0d0e18fbcdd8');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>



```python

```


```python

```
