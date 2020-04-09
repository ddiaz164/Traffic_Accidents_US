![](https://imagevars.gulfnews.com/2019/10/24/accident-sign_16dfd47e1da_medium.jpg)
# United States Accidents: A Look Into Countrywide Traffic Accidents
More than 38,000 people die yearly in crashes throughout the roadways of the United States; the fatality rate is 12.4 deaths for every 100,000 people. On top of this an additional 4.4 million get injured to the point that they require some sort of medical attention. Road crashes are the leading cause of death for inhabitants of the United States ages 1-54 and the socioeconomic impact of these crashes costs citizens about $871 billion.
## Data Source
Through exploration of available data on accidents occurring in the United States I found one particular dataset on Kaggle that had plenty to look through. Sobhan Moosavi's "US Accidents (3.0 million records)" really stood out in that it contained voluminous information on the topic. The database had entries starting February of 2016 and the last of them were as recent as December of 2019. And as his title claimed-- it possesses 3 million entries each containing 49 columns worth of information.
## Data Preparation
As previously mentioned, this dataset was very large and thus I figured it would prove troublesome to handle at first. Since some places have restrictions on the size of the files you can upload, I stored the data into an AWS S3 bucket so that I could then access it from anywhere. I imported boto3 and pandas so that I could bring the data into a notebook to further analyze and discarded any of the entries containing too many null values.
## Data Analysis
Having loaded the data into a pandas dataframe and cleaning it up for analysis, I first wanted to get a grasp of where these accidents were occurring and how many were occurring in each state. I imported the python library known as folium in order to create maps and better visualize the answers to my questions.
### Initial Look
![](https://github.com/ddiaz164/capstone_1/blob/master/images/choro_map.png)
Initially not only did the map resulting from the accident counts per state mostly resemble a map of the population per state, but most of the information was overshadowed by the density of accidents in California. I knew I wanted to change this data, and though I thought of log-scaling it to get a better distribution, I decided to instead normalize it by the number of cars registered to each state.
![](https://github.com/ddiaz164/capstone_1/blob/master/images/choro_rates.png)
This second map was still somewhat like a map of the population would be but the distribution of accidents across the nation was a lot more apparent. There was, however, one state that was very much a standout: South Carolina. 

Looking at the different accident rates it was almost doubling the next highest state!
![](https://github.com/ddiaz164/capstone_1/blob/master/images/image.png)
I decided to create a heat map to further zoom in and see if there was anything special going on.
<img src="https://github.com/ddiaz164/capstone_1/blob/master/images/heat_sc.png" width="450" height="400">

Nevertheless I couldn't glean anything extraordinary from this map other than noting that there were a lot of accidents occurring around major cities-- as one would expect. I hypothesized some other factors were contributing to having such a large accident rate but in order to get to that I had to first explore those factors on a larger scale. I decided to come back to South Carolina once I had a clearer picture on what contributed to these accidents.
### Accidents Across Weather Conditions, Traffic Signaling Devices, and Time of Day

<img src="https://github.com/ddiaz164/capstone_1/blob/master/images/count_cond.png" width="350" height="200"><img src="https://github.com/ddiaz164/capstone_1/blob/master/images/traffic_counts.png" width="350" height="200"><img src="https://github.com/ddiaz164/capstone_1/blob/master/images/count_time.png" width="200" height="300">

<img src="https://github.com/ddiaz164/capstone_1/blob/master/images/sev_cond.png" width="350" height="200"><img src="https://github.com/ddiaz164/capstone_1/blob/master/images/sev_traffic.png" width="350" height="200"><img src="https://github.com/ddiaz164/capstone_1/blob/master/images/sev_time.png" width="200" height="300">

### Statistically Different?

``` snow p-value:0.000 t-value:38.782
rain p-value:0.000 t-value:42.230
fog p-value:0.000 t-value:-25.022
wind p-value:0.825 t-value:-0.222
smoke p-value:0.005 t-value:2.794
dust p-value:0.000 t-value:-6.026
freezing p-value:0.000 t-value:11.677
clear p-value:0.000 t-value:-8.157
cloud p-value:0.000 t-value:4.163


Traffic_Calming p-value:0.000 t-value:-14.763
Traffic_Signal p-value:0.000 t-value:-337.392
Roundabout p-value:0.000 t-value:-8.596
Railway p-value:0.000 t-value:-45.239
No_Exit p-value:0.000 t-value:-16.281
Junction p-value:0.000 t-value:174.778
Give_Way p-value:0.000 t-value:-22.630
Crossing p-value:0.000 t-value:-242.410
Bump p-value:0.000 t-value:-9.184
Stop p-value:0.000 t-value:-117.353

Comparing Nighttime and Daytime t-score:63.7, p-value:0.000 
```
### Lowest and Highest Severity
<img src="https://github.com/ddiaz164/capstone_1/blob/master/images/traffic_low.png" width="450" height="300"><img src="https://github.com/ddiaz164/capstone_1/blob/master/images/traffic_high.png" width="450" height="300">

<img src="https://github.com/ddiaz164/capstone_1/blob/master/images/cond_low.png" width="450" height="300"><img src="https://github.com/ddiaz164/capstone_1/blob/master/images/cond_high.png" width="450" height="300">

### Weather Condition Accident Distributions Across the Country
Freezing:
![](https://github.com/ddiaz164/capstone_1/blob/master/images/heat_freezing.png)
Dust:
![](https://github.com/ddiaz164/capstone_1/blob/master/images/heat_dust.png)
Smoke:
![](https://github.com/ddiaz164/capstone_1/blob/master/images/heat_smoke.png)
Wind:
![](https://github.com/ddiaz164/capstone_1/blob/master/images/heat_wind.png)
Snow:
![](https://github.com/ddiaz164/capstone_1/blob/master/images/heat_snow.png)

![](https://github.com/ddiaz164/capstone_1/blob/master/images/snow_states.png)
![](https://github.com/ddiaz164/capstone_1/blob/master/images/snow_norm.png)
Rain:
![](https://github.com/ddiaz164/capstone_1/blob/master/images/heat_rain.png)

![](https://github.com/ddiaz164/capstone_1/blob/master/images/rain_counts.png)
Fog:
![](https://github.com/ddiaz164/capstone_1/blob/master/images/heat_fog.png)

![](https://github.com/ddiaz164/capstone_1/blob/master/images/fog_counts.png)
![](https://github.com/ddiaz164/capstone_1/blob/master/images/fog_accident_rate.png)

### South Carolina

### Future Steps

### Data Sources
[Kaggle Database](https://www.kaggle.com/sobhanmoosavi/us-accidents)

[Car Data](https://www.statista.com/statistics/196010/total-number-of-registered-automobiles-in-the-us-by-state/)

[Snow Data](http://www.usa.com/rank/us--average-snow-days--state-rank.htm)
