# Blood-Glucose-Prediction

This is a [Kaggle competition](https://www.kaggle.com/competitions/brist1d), aiming to predict blood glucose levels in patients with type 1 diabetes.

The model used for prediction was `CatBoostRegressor`.

Predicting blood glucose fluctuations is crucial for managing type 1 diabetes. Developing effective algorithms for this can alleviate some of the challenges faced by individuals with the condition.

**Goal**: Forecast blood glucose levels one hour ahead using the previous six hours of participant data.

## About the Competition

The dataset is from a study that collected data from young adults in the UK with type 1 diabetes, who used a continuous glucose monitor (CGM), an insulin pump and a smartwatch. These devices collected blood glucose readings, insulin dosage, carbohydrate intake, and activity data. The data collected was aggregated to five-minute intervals and formatted into samples. Each sample represents a point in time and includes the aggregated five-minute intervals from the previous six hours. The aim is to predict the blood glucose reading an hour into the future, for each of these samples.

The training set takes samples from the first three months of study data from nine of the participants and includes the future blood glucose value. These training samples appear in chronological order and overlap. The testing set takes samples from the remainder of the study period from fifteen of the participants (so unseen participants appear in the testing set). These testing samples do not overlap and are in a random order to avoid data leakage.

Complexities to be aware of:

* this is medical data so there are missing values and noise in the data
* the participants did not all use the same device models (CGM, insulin pump and smartwatch) so there may be differences in the collection method of the data
* some participants in the test set do not appear in the training set

## Files

* **activities.txt** - a list of activity names that appear in the activity-X:XX columns
* **sample_submission.csv** - a sample submission file in the correct format
* **test.csv** - the test set
* **train.csv** - the training set

## Columns
### train.csv
* `id` - row id consisting of participant number and a count for that participant
* `p_num` - participant number
* `time` - time of day in the format HH:MM:SS
* `bg-X:XX` - blood glucose reading in mmol/L, X:XX(H:MM) time in the past (e.g. bg-2:35, would be the blood glucose reading from 2 hours and 35 minutes before the time value for that row), recorded by the continuous glucose monitor
* `insulin-X:XX` - total insulin dose received in units in the last 5 minutes, X:XX(H:MM) time in the past (e.g. insulin-2:35, would be the total insulin dose received between 2 hours and 40 minutes and 2 hours and 35 minutes before the time value for that row), recorded by the insulin pump
* `carbs-X:XX` - total carbohydrate value consumed in grammes in the last 5 minutes, X:XX(H:MM) time in the past (e.g. carbs-2:35, would be the total carbohydrate value consumed between 2 hours and 40 minutes and 2 hours and 35 minutes before the time value for that row), recorded by the participant
* `hr-X:XX` - mean heart rate in beats per minute in the last 5 minutes, X:XX(H:MM) time in the past (e.g. hr-2:35, would be the mean heart rate between 2 hours and 40 minutes and 2 hours and 35 minutes before the time value for that row), recorded by the smartwatch
* `steps-X:XX` - total steps walked in the last 5 minutes, X:XX(H:MM) time in the past (e.g. steps-2:35, would be the total steps walked between 2 hours and 40 minutes and 2 hours and 35 minutes before the time value for that row), recorded by the smartwatch
* `cals-X:XX` - total calories burnt in the last 5 minutes, X:XX(H:MM) time in the past (e.g. cals-2:35, would be the total calories burned between 2 hours and 40 minutes and 2 hours and 35 minutes before the time value for that row), calculated by the smartwatch
* `activity-X:XX` - self-declared activity performed in the last 5 minutes, X:XX(H:MM) time in the past (e.g. activity-2:35, would show a string name of the activity performed between 2 hours and 40 minutes and 2 hours and 35 minutes before the time value for that row), set on the smartwatch
* `bg+1:00` - blood glucose reading in mmol/L an hour in the future, this is the value you will be predicting (not provided in test.csv)

## Evaluation Metric

Submissions are evaluated on Root Mean Square Error (RMSE) between the predicted blood glucose levels an hour into the future and the actual values that were then collected.

RMSE is defined as: 

$RMSE = \sqrt{\sum^{n}_{i=1}\frac{(\hat{y}_i-y_i)^2}{n}},$

where $`\hat{y}_i`$ is the ith predicted value, $`y_i`$ is the ith true value and $`n`$ is the number of samples.

The RMSE value is calculated from the bg+1:00(future blood glucose) prediction values in the submission file against the true future blood glucose values. The RMSE values for the public and private leaderboards are calculated from unknown and non-overlapping samples from the submission file across all of the participants.
