
Technical Report Draft 1-
(This will be expanded upon and edited as I am still working on the models)

Problem: 

Can I predict if an arrest will be made when sexual assault is reported in Chicago? 

Data Acquisition:  

Criminal Sexual Assault data from 2001 to present and police station data was retrieved from the City of Chicago Data Portal. Additional information about the location of college/university residence halls was acquired manually. Criteria for inclusion included residence halls and a population of over 3500 students.  A location for each college/university was chosen based on how central it was to the surrounding residence halls; all fell within an approximate distance of 0.3 miles or less. The latitudes and longitudes of these central locations were calculated using Google maps.

Data Transformation: 
Criminal Sexual Assault data from 2017 should be excluded from the dataset, as this can be used as a test set for the model(s). 
Unnecessary columns were dropped, including ‘FBI Code’, ‘Updated On’, ‘Primary Type’, ‘IUCR’, ‘X Coordinate’, ‘Y Coordinate’, ‘Location’ and ‘Block’ Nulls were dropped from “Latitude’, ‘Longitude’, ‘Ward’, ‘Community Area’ and ‘District’ columns. Columns were converted to appropriate data types. There were a small number of null values in the ‘Location Description’ column. Rather than drop the data entirely, they were filled with ‘Residence’ since it was by far the most common description. 
Using the latitude and longitude data, the distance between the crime scenes and their nearest police stations were calculated, yielding a new column ‘nearest_police_station’. The distance between crime scene and the nearest college/university campus was calculated in the same way, yielding another new column, ‘nearest_campus’
The ‘Date’ column was converted to DateTime format and separated out into the hour of the day (in military time), day of the month, month, and day of the week, each in their own column.
The ‘Description’ column was binned into 5 main types: ‘Attempt Non-Aggravated’, ‘Non-Aggravated’, ‘Aggravated’, ‘Attempt-Aggravated’, ‘Predatory’.

The ‘Location Description’ column was binned into 10 main types:  ‘Residential Property’, ‘Public Housing’, ‘Business’, ‘Public’, ‘School’, ‘Care Facility’, ‘Public Transportation’, ‘Private Vehicle’, ‘Vacant Property’, ‘Other’.

Using pd.get_dummies, dummy variables were made for ‘Location Description’, ‘Description’, ‘Ward’, ‘District’, ‘Beat’, ‘Community Area’, ‘Day of Week’.

Operationalizing Variables: 
The original dataframe contained a column that indicated if an arrest had been made for each incident. This column was chosen as my target (y-variable). The x variables were chosen by determining the feature importance of each feature and selecting the top 20. *#This is something that needs to be fleshed out more – ran into some road blocks and am still working on this*

Model Selection, Hyperparameters and Metrics of Success
*#Also need to flesh this out more/explain more clearly*:
Two model types were chosen. A bagged Random Forest Classifier was chosen because it yielded high mean accuracy, precision, and recall scores, and a Logistic Regression because it yielded the highest True Positive value and is more interpretable. Since I am trying to predict arrest, it seemed valuable to choose a model that could optimize the number of True Positives, even at the expense of a higher mean accuracy score.
ROC/AUC scores were taken into consideration  - explain
Future Steps:
-	Using Stratified K Fold instead of Train, Test, Split since the data is skewed (particularly the y variable)
-	Trying a different (less) number of features to see how this effects the scores.
-	More plotting
-	Looking at rows that were dropped based on null values in certain columns – if those columns weren’t used in the final model(s), re-include them in the data set to see if there is any effect.
-	Run the 2017 data through to see how it predicts arrest
-	Major problem with the availability of data – there are a number of variables that may have an affect on arrest rates that are not accounted for here. (demographics, officer salaries, underreporting, time of report vs time of incident, etc)
