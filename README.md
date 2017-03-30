# coursera_dsci_blight
Capstone of WashU Coursera: Data Science at Scale
The detailed code (in R) can be found [in RPubs in this notebook](http://rpubs.com/aabellav/254303)

## Summary
This project aimed at exploring data related to Blight in Detroit, and building a predictive model of blight. We generated 101 features from the provided data, and explored multiple stochastic models. Finally, the Gradient Boosting Model seemed to perform best, with a 78.7% AUC in cross-validation, and 75.0% AUC in the validation set.  
## The Data
The data used in the project was the following
* Demolition permits in Detroit, used to define whether a particular building was blighted

The rest of data was used for feature extraction
* 311 call records in Detroit
* Crime records in Detroit
* Blight violation records in Detroit

## The Steps
The following steps were 
* Data exploration
* Generating a list of buildings
* Feature engineering
* Data modeling
* Evaluation of best model

### Data exploration & cleanse
* Sliced and diced the data to calculate multiple aggregates and understand the information
* Cleaned data, e.g., plotted top bligth violations and found 21k records that had the same lat/long coordinate, seeming a data entry error
* Built interactive exploratory map to visualize demolitions, 311 calls, blight violations, and crime on the map

### Generating a list of building
* Built a fast algorithm to scan through all the available lat/long coordinates from 311 call, blight violations, and crime. The algorthim groups lat/long coordinates that belong to a 74x74 square feet rectangle around each particular coordinate in all records. Grouped lat/longs are assigned the to the same building ID, which will later be used to mash up the different sources of data. 
* Built a lambda function to determine if a building lat/long is within a blight parcel. Blight parcels are defined by a circumference centered at their lat/long and with a radious such as the resulting area is equal to parcel size.

### Feature engineering
varcrime <- "num_crime"
var311 <- paste("min_rating", "max_rating", "diff_rating","num_311",sep = "+")
varviols <- paste("max_amt","num_viols","num_responsible",sep = "+")
varneigh <- paste("num_crime_neigh", "num_311_neigh","avg_max_rating_neigh", 
                  "avg_min_rating_neigh", "num_viols_neigh", "num_respons_neigh", 
                  "avg_max_amt_neigh", "total_max_amt_neigh",sep="+")

