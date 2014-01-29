#Introduction to MesoWest API Web Services

##Overview

MesoWest is now providing experimental API web services to
publicly-accessible weather data.
Expanded, more robust, and operational API web services will become
available during Spring 2014 arising from the University of Utah license of
MesoWest software to a commercial firm. 

API web services are intended for researchers and software developers needing access
to large volumes of current or retrospective data. The University of Utah
(<http://mesowest.utah.edu>MesoWest web site</a> provides access to weather information
in graphical, tabular, and downloadable formats for individual needs.

Available now from the experimental API is access to millions of observations every day
retrieved from hundreds of sources. There is no charge for access to
these observations in the public domain. Software developers can
develop, test, and deploy applications using the experimental MesoWest
API. We hope to foster the development of an expanding community of
researchers and software developers interested in developing open
source tools to access environmental data via the experimental
MesoWest API and its licensed operational counterpart.

##Data Usage Disclaimer

Data available from the experimental MesoWest API are considered provisional. 
As with other MesoWest web services, the data provided to MesoWest arise from cooperative 
with many different educational institutions, public agencies and commercial firms. 
See <http://mesowest.utah.edu/html/help/main_index.html#usage> for further details. 


##Service Disclaimer

The MesoWest API web services are considered experimental. 
We do not guarantee that this web service will be available 24/7. 
During heavy usage, data delivery may be subject to some additional latency, but we are minimizing delays whenever possible. 
We are testing notification of outages of the MesoWest API services to the registered API key owners depending on the nature of the issue. 
Production-grade services will be available soon from a collaborative project underway.

##Getting Started

Using the API requires a valid API key which is used to request a Token from the API. 
That Token is then used as the authentication method to facilitate all other web service API requests. 
A Token does not expire and can be used for any purpose in applications or server side scripting.
Contact <atmos-mesowest@lists.utah.edu> to obtain an API key.

##Data Requests

This API provides access to provisional observations as they are received from data providers.
Access is available to millions of raw observations per day. 
At this time, we do not provide climatological summaries such as minimum or maximum values within a time interval.

##Metadata Requests

As our development of API services are funded in part by the National Mesonet Project of the National Weather Service, we are
striving to obtain as much "metadata" (data about the data) as possible. Basic metadata (station name, network data provider, latitude, longitude, elevation) are available
for all stations. Capabilities are in place to provide additional metadata to users, but much of this information is incomplete at this time.

#API Reference Documentation

##Interfacing with the API

All API requests must be HTTP GET requests. The API can be found at <http://api.mesowest.net/>. This interface is exposed as a web service only. All requests must be formed according to the API guidelines below. All requests are logged and excessive usage will be blocked when necessary to allow equal access to services for all Beta users.

##Making API Requests


Requests are

#Web Services


##/stations

Returns stations matching the query string parameters provided.

###Stations Metadata Query String Parameters

List of query string parameters to request metadata.




Query String | Description and Usage| Example
------------ | ---------------------|----------
   stid      |  String. Single or comma separated list of MesoWest station IDs. To find stations IDs, you can use other query parameters.| `&stid=wbb,kslc,fps`
 state       |   String. Single or comma separated list of abbreviated 2 character states (US and Canada Only).| `&state=ut,wy,dc`
nwszone      |   String. Single or comma separated list of National Weather Service Zones. <http://www.nws.noaa.gov/organization.php> .| ` &nwszone=UT003,CA041`
nwsfirezone  |   String. Single name of National Weather Service Fire Zones. <http://www.nws.noaa.gov/organization.php>. |`&nwsfirezone=LOX241`
cwa          |   String. Single name of National Weather Service County Warning Area. <http://www.nws.noaa.gov/organization.php>.| `&cwa=LOX`
gacc         |   String. Single name of Geographic Area Coordination Center. <http://gacc.nifc.gov/>.|` &gacc=EBCC`
subgacc      |   String. Single name of Sub Geographic Area Coordination Center. <http://gacc.nifc.gov/>.| ` &subgacc=EB07`
county       |   String. Single name of state county. Use the &amp;state parameter to filter by state in the case of duplicate county names (such as King).| `&county=king&state=wa`
vars         |   String. Single or comma separated list of sensor variables found [here](#Sensor_Variable_List). The request will return all stations matching at least one of the variables provided. This is useful for filtering all stations that sense only certain variables, such as wind speed, or pressure.| `&vars=wind_speed,pressure`
network      |   Number. Single network IDs. The ID can be found be using the /networks web service.| ` &network=153`
radius       |   Number. A comma separated list of three values of the type \[latitude,longitude,miles\]. Coordinates are in decimal degrees. Returns all stations within radius of the point and miles provided.| `&radius=41.5,-120.25,20`
bbox         |   Number. A bounding box defined by the lower left and upper right corners in decimal degrees latitude and longitude coordinates, in the form of \[lonmin,latmin,lonmax,latmax\].| `&bbox=-120,40,-119,41`
nearstation  |   *Not implemented at this time.* |
status       |   String. A value of either `active` or `inactive` returns only stations that are currently set as active in the archive. By default, omitting this parameter will return all stations.| ` &status=active`
complete     |   Number. A value of `1` or `0`. When set to 1 an extended list of metadata attributes for each returned station is provided. This is useful for exploring the zones and regions stations are within.| ` &complete=1`
groupby      |   String. Requests can be counted by grouping using one of the following key words: state, country, county, cwa, nwszone, nwsfirezone, gacc, subgacc.| `&groupby=state`
jsonformat   |   Number. A feature to return the JSON response back in human readable format. A value of 2 will indent nested objects by 2 tab widths. By default, omitting this field will return the JSON response back as a single line.| `&jsonformat=2`



###Stations Data Query String Parameters

List of query string parameters to request data. These are used in combination with the metadata query string parameters.

Query String     | Description and Usage | Example Query
------------     | ----------------------|---------------
latestobs        | Number. Latest observations. Set this to `1` to request the last reported observations for stations. This MUST be used with the `within` query string parameter. | `&latestobs=1&within=15` 
within           | Number. The number of minutes back from Now to return observations for stations when the latestobs parameter is used. A value of `15` means to return all observations within the past 15 minutes for the given stations. | `&latestobs=1&within=15`
start            | Number. The start date and time in the form of `YYYYMMDDhhmm` to return observations, where `YYYY` is year, `MM` is month, `DD` is day, `hh` is hour, and `mm` is minutes. The start parameter must be used with the end parameter. All times are requested in UTC, but may be returned in either UTC or Local time format for each station. See the `obtimezone` parameter.  |` &start=201306011800 &end=201306021215`
end              | Number. The end date and time in the form of `YYYYMMDDhhmm` to return observations, where `YYYY` is year, `MM` is month, `DD` is day, `hh` is hour, and `mm` is minutes. The start parameter must be used with the end parameter. All times are requested in UTC, but may be returned in either UTC or Local time format for each station. See the `obtimezone` parameter.  |` &start=201306011800 &end=201306021215`
obtimezone       | String. A value of either `UTC` or `local`. If omitted the default is UTC.  |` &timezone=local`
showemptystations| Number. Show stations even when there are no observations that match the time period. The JSON OBSERVATION object will be empty. A value of "1" is used to turn this one. By default, stations without observations are omitted from the response.  |`&showemptystations=1`



##/networks

Returns network metadata matching the query string parameters provided.

###Networks Query String Parameters

List of query string parameters to request network information


Query String     | Description and Usage  | Example Query
------------     | -----------------------|---------------
id|Number. Single or comma separated list of network IDs. |`&id=1,2,3,4`
shortname|String. Single or comma separated list of network short names.  |`&shortname=uunet,raws`
sortby|String. A value of either `station_count` or `alphabet` for determining the sorting order. By default networks are sorted by ID.  |`&sortby=station_count`
jsonformat|Number. A feature to return the JSON response back in human readable format. A value of 2 will indent nested objects by 2 tab widths. By default, omitting this field will return the JSON response back as a single line.  |`&jsonformat=2`



##/networktypes

Returns network type metadata matching the query string parameters provided.

###Network Types Query String Parameters

List of query string parameters to request metadata


Query String     | Description and Usage    | Example
-----------------| --------------------------|---------
id|Number. Single or comma separated list of network ID types. | `&id=1,2,3,4`
jsonformat|Number. A feature to return the JSON response back in human readable format. A value of 2 will indent nested objects by 2 tab widths. By default, omitting this field will return the JSON response back as a single line.  | ` &jsonformat=2`

## /variables

Returns a list of the available variables within the system. No additional arguments (besides `token`) are accepted.



#Examples

##/stations Web Service Examples

###Metadata queries:

1.  query by states

    http://api.mesowest.net/stations?&state=dc,de&jsonformat=1&token=YourToken
    
2.  query by networks

    http://api.mesowest.net/stations?&network=150,170&jsonformat=1&token=YourToken
    
3.  query by county

    http://api.mesowest.net/stations?&county=Salt%20Lake,Summit&jsonformat=1&token=YourToken&complete=1
    
4.  query with a bounding box

    http://api.mesowest.net/stations?state=ut&status=active&bbox=-112,37.6,-111.8,40&jsonformat=2&token=YourToken
    
5.  query for a given radius

    http://api.mesowest.net/stations?state=ut&status=active&radius=40.75,-111.8833,10&jsonformat=2&token=YourToken
    
6.  query by gacc, subgacc

    http://api.mesowest.net/stations?state=ut&status=active&gacc=ebcc&subgacc=EB08&jsonformat=2&complete=1&token=YourToken

###Data queries:

1.  latestobs

    http://api.mesowest.net/stations?stid=wbb&jsonformat=2&latestobs=1&vars=air_temp,pressure&obtimezone=local&token=YourToken
    
2.  query for latest observation within 30 minutes

    http://api.mesowest.net/stations?state=dc&jsonformat=2&latestobs=1&within=30&vars=air_temp,pressure&obtimezone=local&token=YourToken
    
3.  query for data in a given time range:

    http://api.mesowest.net/stations?state=dc&jsonformat=2&start=201307010000&end=201307020000&vars=air_temp,pressure&obtimezone=local&token=YourToken

###/networks Web Service Examples

1.  http://api.mesowest.net/networks?id=1,2,3&jsonformat=2&token=YourToken
2.  http://api.mesowest.net/networks?shortname=raws,dugway&jsonformat=2&token=YourToken
3.  http://api.mesowest.net/networks?sortby=station_count&jsonformat=2&token=YourToken

###/networktypes Web Service Examples

1.  http://api.mesowest.net/networktypes?id=1,2,3&jsonformat=2&token=YourToken

### Response Example

Example of simple  metadata response for two stations
`http://api.mesowest.net/stations?stid=WBB,FPS&token=YourTokenKey`  
Returns

    {
        "STATION": [
            {
                "STATUS": "ACTIVE",
                "MNET_ID": "153",
                "ELEVATION": "4806",
                "NAME": "WBB/U UTAH",
                "STID": "WBB",
                "LONGITUDE": "-111.84755",
                "STATE": "UT",
                "LATITUDE": "40.76623",
                "TIMEZONE": "US/Mountain",
                "ID": "1"
            },
            {
                "STATUS": "ACTIVE",
                "MNET_ID": "153",
                "ELEVATION": "5202",
                "NAME": "Flight Park South",
                "STID": "FPS",
                "LONGITUDE": "-111.90383",
                "STATE": "UT",
                "LATITUDE": "40.45707",
                "TIMEZONE": "US/Mountain",
                "ID": "2524"
            }
        ],
        "SUMMARY": {
            "NUMBER_OF_OBJECTS": 2,
            "RESPONSE_CODE": 1,
            "RESPONSE_MESSAGE": "OK",
            "RESPONSE_TIME": "4.48298454285 ms"
        }
    }


#Sensor Variable List

The following is an exhaustive list of the variables which can be requested
through the API.

Variable     | Description | Unit
-------------|-------------|------
air_temp|Temperature | Fahrenheit 
dew_point_temperature|Dew Point | Fahrenheit                      
relative_humidity|Relative Humidity | %                      
wind_speed|Wind Speed | Knots                          
wind_direction|Wind Direction | Degrees                    
wind_gust|Wind Gust | Knots                          
solar_radiation|Solar Radiation | W/m\*\*2                    
soil_temp|Soil Temperature | Fahrenheit              
sea_level_pressure|Sea_level pressure | Mb                    
pressure_1500_meter|1500 m Pressure | Mb                        
altimeter|Altimeter | inches Hg                      
pressure|Pressure | Mb                              
water_temp|Water Temperature | Fahrenheit  
cloud_layer_1_code|Cloud_layer_1 height/coverage | code        
cloud_layer_2_code|Cloud_layer_2 height/coverage | code        
cloud_layer_3_code|Cloud_layer_3 height/coverage | code        
visibility|Visibility | Statute miles                  
cloud_low_symbol|Low_cloud symbol | code                    
cloud_mid_symbol| Mid_cloud symbol | code                    
cloud_high_symbol| High_cloud symbol | code                    
weather_cond_code| Weather conditions | code                        
pressure_tendency| Pressure Tendency | code                    
qc| Quality check flag | code  
remark| Remarks | text                              
raw_ob| Raw observation | text                      
sun_hours| Hours of sun | Hours                        
road_sensor_num| Road sensor number |                         
road_temp| Road Temperature | Fahrenheit      
road_subsurface_tmp| Road Subsurface Temperature | Fahrenheit        
road_freezing_temp| Road_Freezing Temperature | Fahrenheit      
road_surface_condition| Road_Surface Conditions | code              
air_temp_high_6_hour| 6 Hr High Temperature | Fahrenheit          
air_temp_low_6_hour| 6 Hr Low Temperature | Fahrenheit            
air_temp_high_24_hour| 24 Hr High Temperature | Fahrenheit        
air_temp_low_24_hour| 24 Hr Low Temperature | Fahrenheit          
peak_wind_speed| Peak_Wind Speed | Knots          
peak_wind_direction| Peak_Wind Direction | Degrees                  
fuel_temp| Fuel Temperature | Fahrenheit                
fuel_moisture_ten_hour| 10\_hr\_Fuel Moisture | gm                      
ceiling| Ceiling | feet                              
sonic_wind_speed| Sonic_Wind Speed | Knots                    
pressure_change_code| Pressure change | code                      
precip_smoothed| Precipitation smoothed | Inches            
soil_temp_ir| IR_Soil Temperature | Fahrenheit            
temp_in_case| Temperature in_case | Fahrenheit            
soil_moisture| Soil Moisture | %                          
volt| Battery voltage | volts                    
created_time_stamp| Data Insert Date/Time | minutes            
last_modified| Data Update Date/Time | minutes            
snow_smoothed| Snow smoothed | Inches                      
precip_accum_ten_minute| Precipitation 10min | Inches                
precip_accum_three_hour| Precipitation 3hr | Inches                  
precip_accum_fifteen_minute| Precipitation 15min | Inches                
precip_accum_one_hour| Precipitation 1hr | Inches
precip_accum_five_minute|Precipitation 5min | Inches                        
precip_accum_six_hour| Precipitation 6hr | Inches                  
precip_accum_24_hour| Precipitation 24hr | Inches                
precip_accum_30_minute| Precipitation 30 min | Inches              
precip_accum| Precipitation accumulated | Inches    
precip_accum_one_minute| Precipitation 1min | Inches                    
snow_depth| Snow depth | Inches                        
snow_accum| Snowfall | Inches                        
precip_storm| Precipitation storm | Inches                
precip| Precipitation manual | Inches              
precip_accum| Precipitation 1hr manual | Inches          
precip_accum_five_minute| Precipitation 5min manual | Inches          
precip_accum_ten_minute| Precipitation 10min manual | Inches        
precip_accum_fifteen_minute| Precipitation 15min manual | Inches        
precip_accum_three_hour| Precipitation 3hr manual | Inches          
precip_accum_six_hour| Precipitation 6hr manual | Inches          
precip_accum_24_hour| Precipitation 24hr manual | Inches          
snow_accum_manual| Snow manual | Inches                        
snow_interval| Snow interval | Inches                      
T_water_temp| Water Temperature | Fahrenheit              
evapotranspiration| Evapotranspiration | inches                
snow_water_equiv| Snow water equivalent | inches              
precipitable_water_vapor| Precipitable water vapor | Inches          
precip_accum| Precipitation (weighing_gauge) | Inches    
net_radiation| Net_Radiation (all_wavelengths) | W/m**2    
soil_moisture_tension| Soil Moisture tension | centibars          
air_temp_wet_bulb| Wet bulb temperature | Fahrenheit          
air_temp_2m| Air_Temperature at_2_meters | Fahrenheit    
air_temp_10m| Air_Temperature at_10_meters | Fahrenheit  
soil_temp1_18| 18_Inch Soil_Temperature | Fahrenheit      
soil_temp2_18| 18_Inch Soil_Temperature2 | Fahrenheit      
soil_temp_20| 20_Inch Soil_Temperature | Fahrenheit      
net_radiation_sw| Net_Radiation (short_wave) | W/m\*\*2        
net_radiation_lw| Net_Radiation (long_wave) | W/m\*\*2          
sonic_air_temp| Sonic Temperature | Fahrenheit              
sonic_wind_direction| Sonic_Wind Direction | Degrees              
sonic_vertical_vel| Vertical_Velocity Sonic | m/s              
ground_temp| Ground Temperature | Fahrenheit            
sonic_zonal_wind_stdev| Zonal_Wind Standard_Deviation | m/s        
sonic_meridonial_wind_stdev| Meridional_Wind Standard_Deviation | m/s    
sonic_vertical_wind_stdev| Vertical_Wind Standard_Deviation | m/s      
sonic_air_temp_stdev| Temperature Standard_Deviation | Centigrade
vertical_heat_flux| Vertical Heat_Flux | m/s C                  
friction_velocity| Friction Velocity | m/s                    
w_ratio| SIGW/USTR | nondimensional                  
sonic_ob_count| Sonic_Obs Total | nondimensional            
sonic_warn_count| Sonic Warnings | nondimensional            
moisture_stdev| Moisture Standard_Deviation | g/m**3        
vertical_moisture_flux| Vertical Moisture_Flux | m/s g/m**3        
M_dew_point_temperature| Dew Point | Fahrenheit                      
virtual_temp| Virtual Temperature | Fahrenheit            
gepotential_height| Geopotential Height | Feet                  
outgoing_radiation_sw| Outgoing_Radiation (short_wave) | W/m**2    
