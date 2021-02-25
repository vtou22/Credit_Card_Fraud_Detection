# Credit Card Fraud Detection

## The Data 


**Credit Card Fraud** causes billions of dollars in loss every year and is continuosly rising.

This raw data comprises of credit card transactions made from a private bank for the full year of 2016.


## 1. Data Cleaning
There were 786,363 rows and 28 features with the "isFraud" as the target feature.

**Problem 1:** The data set was too large to be uploaded in one go.\
**Solution:** I split up the json file into two simimarly sized files.  Then I read them into two sepereate dataframes and then finally merged them into one.

**Problem 2:** Initial info about the data showed that there were no null entries, but visual inspection showed there were clearly some empty and useless data.\
**Solution:** I turned all empty values into null values.  There were six features with completely  null.  Then all other null values contained less than 1% of their feature, so they were all removed for simplicity.    

**Problem 3:** There were a few features that were exactly the same or over 99% the same.\
**Solution:** I found where those <1% didn't match.  I dug deeper and was quite sure that it was due to data entry error.  So I changed the value based on the most common for that customer, then deleted every other row that had only 1 different entry from the rest of that customer's transaction.

![nulls](https://user-images.githubusercontent.com/71348731/107742830-a60a1300-6cc4-11eb-9546-514bf03556f9.png)


## 2. Exploratory Data Analysis
I decided to explore this dataset by looking at each feature's variance based on the 4992 unique customerId

This was a highly imbalanced and large data set. 

![fraud_dist](https://user-images.githubusercontent.com/71348731/107742911-c1751e00-6cc4-11eb-965b-b7bf617abd7c.png)

The curse of dimensionality really came into play here.  There were many object features. Some useful numerical features were engineered from them, but the more I looked into them, the more questions and more possibities there were.
I ended up with dropping a few features and then adding some more.  The final shape of the dataset was 776,491 rows and 42 features.

*Correlation of features before EDA*

![beginning heatmap](https://user-images.githubusercontent.com/71348731/107743050-08fbaa00-6cc5-11eb-9dd0-7facdad3f6d7.png)

*Correlation of features after EDA*

![end heatmap](https://user-images.githubusercontent.com/71348731/107743088-1add4d00-6cc5-11eb-9ce9-9aba6f67f448.png)


## 3. Modeling

We use the **PyCaret** module to compare performances againts 15 different machine learning models.  This method allows for quick results compared to individually modelling 15 different models.  Although running this did slow down my computer for a bit.

The top 3 performing models were Catboost Classifier, Extreme Gradient Boosting, and Light Gradient Boosting Machine.  With an AUC score of aproximately 0.83 to 0.845

A blended model of the top 3 scored slightly better at 0.899, but not being able to tune the parameter or visualize its features made it hard to truly understand. Therefore, we continued with only the first 3 original model





## 4. Takeaway and Future Improvements

 Analyzing the models' feature importances showed a few areas we could explore further in order to refine the model and improve performance.

Through EDA, most features were almost identical with very little differences and not much can be done with a lot of the object features.  The transaction amount explained the most variance and other features were created from it.  Even so, it wasn't enough to drastically improve the model's performance.

With so many features, there is always something else that could be engineered to better improve performance, althoug at a risk of adding more complexity.

Pycarets *tune_model()* function applied gridsearchCV for us, although it returned a score worst than the orginal.  Perhaps in the future, decide on a desired model and fine tune the parameters to better fit this desired set rather than just a simple random search.

Maybe more features, especially from the six empty features such as location and recurring authorization that were droped in the beginning could explain some more of the varainces in fraudulent activty

The feature availableMoney stays the same between some transactions, but differs between other. Is there an activity, such as payment, between each transaction that is not being showed?



