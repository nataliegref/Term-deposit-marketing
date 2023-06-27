
# Term Deposit Marketing

## Background:

We are a small startup focusing mainly on providing machine learning solutions in the European banking market. We work on a variety of problems including fraud detection, sentiment classification and customer intention prediction and classification.

We are interested in developing a robust machine learning system that leverages information coming from call center data.

Ultimately, we are looking for ways to improve the success rate for calls made to customers for any product that our clients offer. Towards this goal we are working on designing an ever evolving machine learning product that offers high success outcomes while offering interpretability for our clients to make informed decisions.

## Data Description:

The data comes from direct marketing efforts of a European banking institution. The marketing campaign involves making a phone call to a customer, often multiple times to ensure a product subscription, in this case a term deposit. Term deposits are usually short-term deposits with maturities ranging from one month to a few years. The customer must understand when buying a term deposit that they can withdraw their funds only after the term ends. All customer information that might reveal personal information is removed due to privacy concerns.

## Attributes:

age : age of customer (numeric)

job : type of job (categorical)

marital : marital status (categorical)

education (categorical)

default: has credit in default? (binary)

balance: average yearly balance, in euros (numeric)

housing: has a housing loan? (binary)

loan: has personal loan? (binary)

contact: contact communication type (categorical)

day: last contact day of the month (numeric)

month: last contact month of year (categorical)

duration: last contact duration, in seconds (numeric)

campaign: number of contacts performed during this campaign and for this client (numeric, includes last contact)

Output (desired target):

y - has the client subscribed to a term deposit? (binary)

## Download Data:

https://drive.google.com/file/d/1EW-XMnGfxn-qzGtGPa3v_C63Yqj2aGf7

## Goal(s):

Predict if the customer will subscribe (yes/no) to a term deposit (variable y)

## Success Metric(s):

Hit %81 or above accuracy by evaluating with 5-fold cross validation and reporting the average performance score.



# Method and Results
## Exploratory data analysis
I first explored the data to gain insights. This included assessing the balance of the data (7% of the data is Y=1) and how many entries we have (40000) and how many features (13).
I will have to deal with the imbalance in the dataset.

Data exploration showed there was no missing data.
I then looked at histograms of the distribution of each of the features to better understand the dataset.
I also looked at the feature importance.

## Cleaning data 
I next had to make all the data numerical in order for it to be used by models, and make sure none of the numbers (from the balance column) were in the negative (I did so by temporarily adding the lowest deficit to all balance, thereby shifting the data).

## Class imbalance
In order to deal with the class imbalance identified above, I resampled data (upsampled the minority class by a factor of 3 or 4; downsampled the majority class by half)

## Building models
I then built several models: Logistic regression (LR), KNN, Decision tree, Random forest, Gradient boosting, and XGboost.
I trained these models using using training data (randomly selected from the whole dataset) at both a 80% split.
I compared these models along accuracy and F1 scores and elected to focus on LR models based on these results.

## Improving the LR model
### Resampling dataset
I then used the resampled datasets and compared then using a LR model and accuracy and F1 scores.
The resampled data with 3x upsampling and downsampling by half worked best for F1 (0.45) and second best accuracy (89.7%).

### Changing probability threshold
I then used the original dataset and tried to improve the results by changing the threshold in probability score in the LR model.
Using threshold 0.2 gave best the F1 score (0.45) and second best accuracy results (91.1%).

### Changing class weights
I also tried to improve the results by using the original dataset and changing the class weights in the LR model.
Using class weights of 0.2 for Y=0 and 0.8 for Y=1 led to the best F1 score (0.47) and accuracy results (91%)

### Results of LR models
Using the class weight technique on the data yielded the best F1 score with great accuracy.

## Improving the XGBoost model
I also tried the above on the XGBoost model

### Resampling dataset
I  used the resampled datasets and compared then using a XGBoost model and accuracy and F1 scores.
The resampled data with 3x upsampling and downsampling by half worked best for F1 (0.55) and second best accuracy (91.9%).

### Changing probability threshold
I then used the original dataset and tried to improve the results by changing the threshold in probability score in the XGBoost model.
Using threshold 0.3 gave best the F1 score (0.55) and second best accuracy results (92.4%).

### Changing class weights
I also tried to improve the results by using the original dataset and changing the class weights in the XGBoost model.
Using class weights of 8 led to the best F1 score (0.56) and accuracy results (91.4%)

### Results of XGBoost models
Using the class weight technique on the data yielded the best F1 score with great accuracy.

## Testing models with resampled data
As a second pass I decided to test all the models I ahd built using the resampled data identified above.
None of the other models performed better than XGBoost with class weights.

## Conclusion
XGBoost with changed class weights led to best F1 score (0.56) with good accuracy (91.4%).
5-fold cross validation on the best algorithm from the above (XGBoost with class weight=8) gives an average score of 87.3%


## Bonus questions

We are also interested in finding customers who are more likely to buy the investment product. Determine the segment(s) of customers our client should prioritize.

What makes the customers buy? Tell us which feature we should be focusing more on.

### Results
Using the XGBoost model identifiedabove, we looked at the feature importance within the model.
This showed that the duration of the call influences whether customers buy or not, and there should be more focus on this feature.
Futhermore, by grouping the data along the balance and age features, and looking at high probabilities from the model above, we can identify segments of high priority age group              
78-88 and balances 10-20k; age group
88+ and balances 0-10k.
These segments have a high probability (>0.7) of buying the product so the client should focus on them. Another suggestion is to look at the class of clients in the model results that were predicted to buy the product but haven't (false positives). These might also be good candidates for buying product.
