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
I divided my data to look at accidents involving different weather conditions, traffic signaling devices, and time of day and created bar graphs to show the distribution of accidents occuring under these scenarios.

<img src="https://github.com/ddiaz164/capstone_1/blob/master/images/count_cond.png" width="350" height="200"><img src="https://github.com/ddiaz164/capstone_1/blob/master/images/traffic_counts.png" width="350" height="200"><img src="https://github.com/ddiaz164/capstone_1/blob/master/images/count_time.png" width="200" height="300">

I expected to see the highest counts for weather as clear or cloudy since on average that would be the most common weather condition.  The highest counts for signal devices I predicted to be traffic signals since that would be where most cars would congregate into a stop. And since the majority of people drive during the day I expected to see most accidents occurring at daytime. Since the first few graphs did not show anything out of the norm I decided to look at the average severity of these scenarios.

<img src="https://github.com/ddiaz164/capstone_1/blob/master/images/sev_cond.png" width="350" height="200"><img src="https://github.com/ddiaz164/capstone_1/blob/master/images/sev_traffic.png" width="350" height="200"><img src="https://github.com/ddiaz164/capstone_1/blob/master/images/sev_time.png" width="200" height="300">

The dataset defines severity as how much of an impact the accident had on traffic (scale of 1-4). Thus I expected to see snow and freezing as top candidates for weather conditions and junctions being the leading device on these severity charts. However, with the time of day graph, I expected to see daytime severity as higher than night since there would not be a lot of traffic at nighttime. I predict this finding has to do with nighttime accidents not receiving the same kind of attention and therefore would take longer to clear.

### Statistically Different?
I had these severity averages that were all seemingly different but were somewhat close to each other. So I decided to perform statistical tests on each and compare them to the average severity of the entire data.

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
The chances all but one of these severities occurring by chance was close to 0. Therefore these were statistically different from each other despite it being a small difference.

### Lowest and Highest Severity
Now knowing that the difference in severity between these was statistically significant I decided to zoom in on those with the lowest and highest severities.

<img src="https://github.com/ddiaz164/capstone_1/blob/master/images/traffic_low.png" width="450" height="300"><img src="https://github.com/ddiaz164/capstone_1/blob/master/images/traffic_high.png" width="450" height="300">
The low severity traffic signal devices had high rates for no exit and stop signs which makes sense seeing as these signals are in areas with very little traffic. Similarly the high severity devices showed junction and give way as having the highest rates since an accident on these would mean multiple lanes would be blocked rather than just one.

<img src="https://github.com/ddiaz164/capstone_1/blob/master/images/cond_low.png" width="450" height="300"><img src="https://github.com/ddiaz164/capstone_1/blob/master/images/cond_high.png" width="450" height="300">
As for weather conditions, the low severity chart had clear on top. But the high severity chart illustrated that, for the highest impact on traffic, the weather condition with the highest accident rate was wind which was not even present on any low severity accidents. These findings got me thinking about how the accident distribution of these weather conditions would look across the United States.

### Weather Condition Accident Distributions Across the Country
I decided to create heat maps of each of the weather conditions to get a better look at how these accidents were distributed across the country.

Freezing:
!["Freezing"](https://github.com/ddiaz164/capstone_1/blob/master/images/heat_freezing.png)
Dust:
![](https://github.com/ddiaz164/capstone_1/blob/master/images/heat_dust.png)
Smoke:
![](https://github.com/ddiaz164/capstone_1/blob/master/images/heat_smoke.png)
Wind:
![](https://github.com/ddiaz164/capstone_1/blob/master/images/heat_wind.png)
Snow:
![](https://github.com/ddiaz164/capstone_1/blob/master/images/heat_snow.png)
The snow map was interesting in that it was mostly clustered around places where it snows often so I decided to dive a little deeper. 

![](https://github.com/ddiaz164/capstone_1/blob/master/images/snow_states.png)
I looked at accidents involving snow across all the states and found that for the most part the higher rates were in the states that get more snow. But I really wanted to know if some of the states that don't get as much snow got into a lot of accidents when it did snow as I predicted that they would be poorly equipped to handle the conditions. Thus I normalized the snow accident rates against how many days it snows yearly on average for each state.

![](https://github.com/ddiaz164/capstone_1/blob/master/images/snow_norm.png)
The resulting graph had very different results that backed up my theory that those states that don't receive a lot of snow have a higher accident rate than those that are more used to the snow.

Rain:
![](https://github.com/ddiaz164/capstone_1/blob/master/images/heat_rain.png)

I initially wanted to look into rain in the same manner as I did with snow but after seeing the map I noticed how aggregated it was in South Carolina and thought I might've found my contributing factor. 

![](https://github.com/ddiaz164/capstone_1/blob/master/images/rain_counts.png)
However after plotting the accidents involving rain for each state, although it was relatively high for South Carolina, it wouldn't explain the rate being so much higher for them so I decided to keep looking.

Fog:
![](https://github.com/ddiaz164/capstone_1/blob/master/images/heat_fog.png)
The fog map illustrated exactly what I was looking for: a higher distribution of accidents in South Carolina relative to the other states. I decided to plot it the same way as I did with rain.

![](https://github.com/ddiaz164/capstone_1/blob/master/images/fog_counts.png)
South Carolina had almost as many fog accidents as the higher population states!

![](https://github.com/ddiaz164/capstone_1/blob/master/images/fog_accident_rate.png)
I normalized the data once more to account for the number of cars registered to every state and found that South Carolina had almost three times the rate as the second highest state.

### South Carolina
I did a little bit of research into South Carolina and found that they don't get very many fog days per year (10-20 fog days) but whenever they do get fog it tends to be very dense. These conditions, however, are not uncommon among some of the other states and thus I predict the fog accidents might be better explained by how the residents from South Carolina drive in the fog rather than how much fog is present.
### Future Steps

### Data Sources
[Kaggle Database](https://www.kaggle.com/sobhanmoosavi/us-accidents)

[Car Data](https://www.statista.com/statistics/196010/total-number-of-registered-automobiles-in-the-us-by-state/)

[Snow Data](http://www.usa.com/rank/us--average-snow-days--state-rank.htm)
