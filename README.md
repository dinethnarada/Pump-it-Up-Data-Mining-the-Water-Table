
# Pump-it-Up-Data-Mining-the-Water-Table
Ipython notebook for Pump it Up: Data Mining the Water Table Challange held by drivendata.org

## Tasks
1. Load the data
2. Recongize the Data
3. Data Preprocessing
4. Model Selection
5. Evaluation

## Requirnments
* pandas
* numpy
* matplotlib
* seaborn
* sklearn

## Load the data

* Load the train data set, train data label set and test data label set to the variables. Then I plot the counts of each status_group column.
* Then check the null values and duplicates exists in the both train and test datasets. Following are the results I got,

## Recognize the Data

```
amount_tsh               float64
date_recorded             object
funder                    object
gps_height                 int64
installer                 object
longitude                float64
latitude                 float64
wpt_name                  object
num_private                int64
basin                     object
subvillage                object
region                    object
region_code                int64
district_code              int64
lga                       object
ward                      object
population                 int64
public_meeting            object
recorded_by               object
scheme_management         object
scheme_name               object
permit                    object
construction_year          int64
extraction_type           object
extraction_type_group     object
extraction_type_class     object
management                object
management_group          object
payment                   object
payment_type              object
water_quality             object
quality_group             object
quantity                  object
quantity_group            object
source                    object
source_type               object
source_class              object
waterpoint_type           object
waterpoint_type_group     object
dtype: object
```


## Data Prepocessing

* Drop the duplicates from training data set.
* Then Merge the training and test data set for imputation process

1. Nan value imputation
*   First get all count of zero values and the NAN values for columns excluding label column.
```
amount_tsh has 52013 zero values
amount_tsh has 0 null values


date_recorded has 0 zero values
date_recorded has 0 null values


funder has 0 zero values
funder has 4504 null values


gps_height has 25613 zero values
gps_height has 0 null values


installer has 0 zero values
installer has 4532 null values


longitude has 2234 zero values
longitude has 0 null values


latitude has 0 zero values
latitude has 0 null values


wpt_name has 0 zero values
wpt_name has 0 null values


num_private has 73263 zero values
num_private has 0 null values


basin has 0 zero values
basin has 0 null values


subvillage has 0 zero values
subvillage has 470 null values


region has 0 zero values
region has 0 null values


region_code has 0 zero values
region_code has 0 null values


district_code has 27 zero values
district_code has 0 null values


lga has 0 zero values
lga has 0 null values


ward has 0 zero values
ward has 0 null values


population has 26798 zero values
population has 0 null values


public_meeting has 6345 zero values
public_meeting has 4135 null values


recorded_by has 0 zero values
recorded_by has 0 null values


scheme_management has 0 zero values
scheme_management has 4846 null values


scheme_name has 0 zero values
scheme_name has 35231 null values


permit has 21829 zero values
permit has 3793 null values


construction_year has 25933 zero values
construction_year has 0 null values


extraction_type has 0 zero values
extraction_type has 0 null values


extraction_type_group has 0 zero values
extraction_type_group has 0 null values


extraction_type_class has 0 zero values
extraction_type_class has 0 null values


management has 0 zero values
management has 0 null values


management_group has 0 zero values
management_group has 0 null values


payment has 0 zero values
payment has 0 null values


payment_type has 0 zero values
payment_type has 0 null values


water_quality has 0 zero values
water_quality has 0 null values


quality_group has 0 zero values
quality_group has 0 null values


quantity has 0 zero values
quantity has 0 null values


quantity_group has 0 zero values
quantity_group has 0 null values


source has 0 zero values
source has 0 null values


source_type has 0 zero values
source_type has 0 null values


source_class has 0 zero values
source_class has 0 null values


waterpoint_type has 0 zero values
waterpoint_type has 0 null values


waterpoint_type_group has 0 zero values
waterpoint_type_group has 0 null values


```
* Note that I does not consider 'Permit' and 'Public_meeting' columns for the imputation.(Their zero count means number of False count)
* `gps_height`,`population`,`longitude`,`latitude` and `amount_tsh` features grouped by `region` and `district_code` and imputed with mean value.
* `contruction_year` grouped by `region` and `district_code` and imputed with median value. I found some NAN values are still existing. To them grouped by `region` and `construction_year` and imputed with median value. 
* `Funder` and `installer` have ‘0’, ‘-‘ and some other irrelevant character except to the nan values. So impute those with 'other' category
* `subvillage` and `scheme_management` are grouped by `region_code` and imputed the nan values with mode.
* `scheme_name` grouped by `region` and imputes the nan values with the mode.
* `Public_meeting` and `permit` features are simply imputed with the median(In this case equal to the mode).

2. Create New Features
* Add `cluster` feature to dataset by clustering dataset using KMeans algorithm.`population`,`latitude` and `longitude` features used for clustering.
* Add `year` and `month` features to dataset by extracting `data_recorded` feature.
* Add `operational_year` feature to the dataset by substracting  `construction_year` year from `date_record` year.

3. Normalize the Data
* I used log-normalize technique for the `population` and `amount_tsh` features.

## Model Selection
* I tried Random-forest model, XGBoost model and CatBoost model. I got best results in Catboost Model.(Becuase Catboost algorithm optimised for the categorical features)
* In here I used Catboost Model as the Model

## Evaluation

*	For the evaluation I split the train dataset `train : Val as 90% : 10%`.
*	Then I evaluated `CatBoost` Model
*	I checked accuracy and plotted the confusion matrix for look more details.

## Testing

* For the testing I re-train the Catboost model with all the training data given. Then I got the prediction results to the Test dataset. Then submit to the drivendata website.
* Result - `0.8198`
