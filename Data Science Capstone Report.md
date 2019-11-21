
# 1. Description of the Problem and Discussion of the Background 

## Finding the best area to open a Chinese restaurant in Singapore

Singapore is one of the most popular business hubs in the world.  Singapore is the only country in Asia with an AAA sovereign rating from all major rating agencies. The city-state is home to 5.6 million residents belonging to various races. The diversity in Singapore is reflected in the large variety of restaurants available in the city. Being a foodie's paradise it will be interesting to know which location will be the best to open a restaurant in Singapore.

Since roughly **75% of the population of Singapore is of Chinese descent**, it makes sense to start a Chinese restaurant so as to have the maximum footfall. Being a very expensive city the rental prices are very high here, so to make the maximum profit it is wise to open a restaurant in one of the most populous areas of Singapore.

![Singapore_Skyline.jpg](https://github.com/Anmol-Saraf10/Data-Science-Capstone-Week4_5/blob/master/Singapore_Skyline.jpg)
        Singapore Skyline (source: <https://en.wikipedia.org/wiki/Singapore>)        

#### In a nutshell, we need to find out where in Singapore shall a Chinese resturant be opened so as to maximize the profit.

### Target Audience

#### Potential restaurateurs who want to invest in or open a restaurant. This project might help them to find the optimum locations for their restuarant.



# 2. Description of the data and its gathering process

For this problem we need to find the locations in Singapore, and what types of restaurants and other venues are the most visited in those locations. Also, we would need to find the population in each of
these areas, this will help us to find out the 10 most populous areas in Singapore. Once we know that, we can find out how many Chinese restaurants are there in each of these areas, and that will help
us decide where to start a Chinese restaurant.

To create the required data frame, first we need to find out the regions/areas of Singapore. Then we will find their coordinates. Using those coordinates we can find the top venues in their proximity using FourSquare API. Then some data manipulation will be done to prepare it for clustering.

# 3. Methodology

This part dicusses the data acquisition, pre-processing and manipulation, and the analysis done on it.

## 3.1 Getting the area data of Singapore

Planning Areas are the main urban planning and census divisions of Singapore.The table available on the [Wikipedia page](https://en.wikipedia.org/wiki/Planning_Areas_of_Singapore) lists down all these planning areas along with their respective areas and populations.

BeatifulSoup and lxml were used to scape the table data from the webpage, as shown below.

![Web_Scraping.JPG](https://github.com/Anmol-Saraf10/Data-Science-Capstone-Week4_5/blob/master/Web_Scraping.JPG)

The data obtained, after some manipulation looked like this:

![Web_Scraping_Data.JPG](https://github.com/Anmol-Saraf10/Data-Science-Capstone-Week4_5/blob/master/Web_Scraping_Data.JPG)


## 3.2 Getting the coordinates of each of the areas

Geopy was used to gather all the coordinates of the areas. Some of the coordinates obtained were incorrect
hence they were replaced by the correct ones found using Google search.

After rectifying the coordinates, the data looked like this:
![Coordinates.JPG](https://github.com/Anmol-Saraf10/Data-Science-Capstone-Week4_5/blob/master/Coordinates.JPG)

These locations were plotted on the map of Singapore with the help of Folium:

![Raw_Map.JPG](https://github.com/Anmol-Saraf10/Data-Science-Capstone-Week4_5/blob/master/Raw_Map.JPG)


## 3.3 Selecting the relevant rows

To narrow down on the potential areas, the top 10 most populous areas were selected from the data frame
to create a new data frame. For this the data frame was sorted in descending order to get the top 10 most populous areas:

![Population_wise.JPG](https://github.com/Anmol-Saraf10/Data-Science-Capstone-Week4_5/blob/master/Population_wise.JPG)


## 3.4 Using FoureSquare API

Next, the venue data was gathered using the FourSquare API. In a radius of 500m 100 spots were chosen. The raw data looked like this:

![Raw_Venue.JPG](https://github.com/Anmol-Saraf10/Data-Science-Capstone-Week4_5/blob/master/Raw_Venue.JPG)

From this data, the top 10 most visited venues were determined. The bar chart below
shows that Coffee Shop is the most visited venue while Chinese Restaurant comes in at number 4.

![Top_10_most_common_venues.JPG](https://github.com/Anmol-Saraf10/Data-Science-Capstone-Week4_5/blob/master/Top_10_most_common_venues.JPG)


## 3.5 Data Manipulation

Finally, the data was prepared for analysis. In this process, one hot encoding was used to replace the
texts with categorical values. After using one-hot enoding the data looked like this:

![After_one_hot_encoding.JPG](https://github.com/Anmol-Saraf10/Data-Science-Capstone-Week4_5/blob/master/After_one_hot_encoding.JPG)

## 3.6 Exploratory Data Analysis

In order to understand these areas, it is important to see what venues are the most visited in each of these areas.
For this analysis let us see what are the top 5 most visited venues in these areas:

![Top_5_most_common_venues.JPG](https://github.com/Anmol-Saraf10/Data-Science-Capstone-Week4_5/blob/master/Top_5_most_common_venues.JPG)

Except Punggol all the areas have some sort of eatery as the most visited venue.

Now for a better understanding the similar areas need to be grouped together. So  K-Means Clustering seems to be a wise choice. However, it has to be found out what number of clusters is optimum.

Elbow Plot method comes to rescue. Several number of clusters are tried out and the one
where there seems to be a big drop in the sum of squared distances for each data point, is chosen as the optimum value.

The plot below shows that at probably 3 or 4 the elbow is obtained. For the sake of
analysis, 4 is considered to be the number of clusters.

![Elbow_plot.JPG](https://github.com/Anmol-Saraf10/Data-Science-Capstone-Week4_5/blob/master/Elbow_plot.JPG)

Then K-Means Clustering was applied on the dataset. The resultant labels were affixed to
every row of the original data frame, as shown below:

![Cluster_labels.JPG](https://github.com/Anmol-Saraf10/Data-Science-Capstone-Week4_5/blob/master/Cluster_labels.JPG)

# 4. Results and Discussion

The clustered areas were plotted on the map of Singapore as shown below:

![Final_map.PNG](https://github.com/Anmol-Saraf10/Data-Science-Capstone-Week4_5/blob/master/Final_map.PNG)

It can be seen from the above plot that most of the areas belong to **Cluster 2**.

So far from this analysis, the following could be inferred:

1. Coffee Shop is the most visited venue while Chinese Restaurant is the 4th most visited venue. 
2. Bedok and Ang Mo Kio are both Cluster 2 areas that don't have Chinese Restaurant as
   one of the top 5 most visited venues.
3. Bedok being the most populous area of all, has eateries as the most common venues, but Chinese Restaurant is not one among them.

According to this analysis, opening a Chinese restaurant at **Bedok would be highly profitable because it attracts the most number of customers, and eating joints are the most visited venues here**. If the rental is very high here, **Jurong West could also be considered** because it is the second most populous area, belongs to the same cluster as Bedok, and at the same time doesn't offer a lot of competition given that Chinese restaurant is the 5th most visited venue here.

# 5. Conclusion

The project aimed at finding out which areas in Singapore would yield the maximum profit  when one decides to open an eatery here. Given that majority of the population is of Chinese ethnicity, a Chinese restaurant then becomes the obvious choice.

A project like this requires location data, and FourSquare API usage made the analysis viable. With the powerful Pandas package a lot of manipulation was done to get a clean and meaningful dataset. The K-Means clustering gave us an insight into the similarities of the locations based on the venues visited the people.

In the end, it was inferred that Bedok was the number one choice to open a Chinese restaurant, however, given the land prices and other constraints, it is wise to have a back up option. Jurong West then becomes another option that could be considered given its similarity with Bedok.

The target audience would find this analysis as an isightful starting point in their search of finding the best location. Not only that this data could also be used to check for any othr type of eating joint or venue in Singapore.

