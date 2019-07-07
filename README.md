# Neighborhood-Walkability-Analysis

Walkability score is a quantitative measure of the walking friendliness of an area, which has benefits in many aspects. In this project, we are interested in assessing the walkability scores for various Sydney neighborhoods and how these scores are correlated with other measures. 

## Dataset Description
* the statistical area 2(SA2) data from the Australian Bureau of Statistics (ABS)
* extension data (the density of train stations)

## Data Cleaning and Integration
* performed data cleaning for each csv files 
* spatial join using Google API. Scrapped and stored the corresponding latitude and longitude of each neighbourhood in the neighbourhood dictionary. Set a sleep time which makes sure every failed scrapping will be conducted repeatedly until it is successful.

## Database Description
### Schema design
<p align="center">
<img width="500" alt="skema" src="https://user-images.githubusercontent.com/46860162/60764275-0fc64200-a0ca-11e9-8784-d94c97d5dbd7.PNG">
</p>

### Index creation
- Index was considered to be created on “area_id” column as it is used most often. 
- As we may be interested in the walkability score in different regions that cover a range of neighbourhoods, clustered index was chosen. It is the most appropriate index for range searches which minimizes the access costs of a range scan.
- Spatial index was also created. For each neighbourhood, a geometry-type box object was created which contains the pair of geolocations for each train station and Generalized Searched Tree- based Index(GiST) was created on the box object. As the neighbourhood geolocations are frequently searched during the spatial join process, a spatial index created in this way tends to highly boost the searching efficiency.

## Walkability Analysis

Walkability formula:

𝑤𝑎𝑙𝑘𝑎𝑏𝑖𝑙𝑖𝑡𝑦 = 𝑧(𝑝𝑜𝑝 𝑑𝑒𝑛𝑠𝑖𝑡𝑦) + 𝑧(𝑑𝑤𝑒𝑙𝑙𝑖𝑛𝑔 𝑑𝑒𝑛𝑠𝑖𝑡𝑦) + 𝑧(𝑠𝑒𝑟𝑣𝑖𝑐𝑒 𝑏𝑎𝑙. ) + 𝑧(𝑡𝑟𝑎𝑛𝑠𝑝𝑜𝑟𝑡 𝑑𝑒𝑛𝑠𝑖𝑡𝑦) + 𝑧(𝑡𝑟𝑎𝑖𝑛 𝑠𝑡. 𝑑𝑒𝑛𝑠𝑖𝑡𝑦)

Where z is the standardized z score for each measure. Each measure is assumed to follow a normal distribution.

## Walkability Results
<p align="center">
<img width="937" alt="Screen Shot 2019-07-07 at 9 04 51 pm" src="https://user-images.githubusercontent.com/46860162/60767409-0903f300-a0fb-11e9-8507-2ad88990165f.png">
</p>

## Correlation Analysis
<br/>

<p align="center">
  <img width="700" height="400" src="https://user-images.githubusercontent.com/46860162/60764092-b3f9ba00-a0c5-11e9-9628-aa43a2f3bec0.png">
</p>

