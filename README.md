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
Generated a total of 101 features

Basic features at the building level
* number of blight violations
* number of 311 calls
* minimum, maximum of 311 call rating, and their difference
* number of crime reports
* number of crime reports with determination being "responsible"
* max fine amount for all crime records at a particular building location (from JudgmentAmt)


Surrounding features, around 111m of each building:
* number of blight violations
* number of 311 calls
* average of max rating of surrounding 311 calls
* average of min rating of surrounding 311 calls
* number of crime reports
* average of max fine amounts of surrounding crime reports
* total sum of max fine amounts of surrounding crime reports

Counts of type of crime, violations and 311 calls
* 
* 23 types of 311 issues
* For violations, reduced from 313 violation codes to 12 groups


### Data modelling
Explored different models and sets of features

With number of violations only
* Linear model
* Decision Tree
* Random forest

With combination of features
* Decision tree
* Random forest

With all features and subsets based on importance
* Random forest
* Gradient boosting

### Evaluation of the best model
The best model is a gradient boosting model with all the features, with only the top 20 features yields similar performance of 78% AUC in cross-validation and 74-75% AUC in the validation set. 

||Ref Not Blighted|Ref Blighted|
|-|:-:|:-:|
|Pred Not Blighted|913|258|
|Pred Blighted|476|636|

