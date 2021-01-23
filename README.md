# Data Anomalies Handling
This program cleanse the dataset, which contains transactional retail data from an online electronic store located in Melbourne, Australia. The wrangling process include three major steps for targeting different types of data anomalies.

* Detect and correct syntatctic and semantic errors
* Remove outliers
* Impute missing data based on the type of missing values

## Part 1. Detect and correct syntatctic and semantic errors
Part 1 is divided into two parts - (1) correcting syntactic errors and (2) correcting semantic errors.
To clean the datasets with anomalies, first I corrected the syntactic errors in the data set. **Syntactic anomalies** include errors tackles the errors of the values or the format of the data types. This includes, lexical, domain format errors and irregularities in non-uniform use of values. **Semantic anomalies**, on the other hand, tackles the comprehensiveness and redundancy of the data set. This includes integrity constraints violations, contradictions of values based on values of other attributes, duplicates or invalid observations.

Part 1 first starts off with briefly understanding the structure of the data frame, including the numbers of rows and columns, the data types of the values in each column, as well as the column names. Based on this observations, I tried to see which columns could contain what type of errors in their values (syntatctical anomalies), and which attributes could be related to another (semantic anomalies). I also checked if there is any null values within the data frame before starting correcting any potential errors.

## Part 2. Imputing Missing Values
Part 2 detects columns with missing values and imputes the missing ones based on the client's business rules. Based on the values returned from ``isnull().sum()`` function, following columns has null values:
    
* ``nearest_warehouse``
* ``order_price``
* ``delivery_charges`
* ``order_total
* ``distance_to_nearest_warehouse``
``is_happy_customer``

The imputation was implemented under the assumption that all the values in the csv file is correct.

## Part 3. Removing Outliers

Before removing the outliers, we need to identify whether the ``delivery_charges`` attribute is independent from other attributes. If so, we can define the outliers in this attribute as univariate outliers and remove them straight away. Otherwise, we should build a model using other related attributes according to the business rule, and remove the multivariate outliers based on the distribution of the residuals. Residuals refers to the 'noise' between the values derived by the model we built and the actual values. The distribution of the residuals conveys the distribution of the attribute that we are working with in accordance to its related attributes - if the residual is exceptionally higher or lower than the norms, the data point is significantly deviating from the model and thus the relationship between the attributes. Therefore, the univariate outliers of the residual values direct us to the multivariate outliers that we are looking for.

According to the business rule of the client, ``delivery_charges`` attribute is calculated using a linear model. The model linearly depends on ``distance_to_nearest_warehouse``, ``is_happy_customer``, and ``is_expedited_delivery`` attributes, and the model differs based on each season. Since we are given that the ``delivery_charges`` is linearly dependent on three other attributes, we should build multi-linear regression model of ``delivery_charges``. Since the model differs in each season, we should build four different multi-linear regression model.

Based on the linear regression model, we calculate the 'predicted delivery charges' and obtain the residual of each data point by subtracting it to original ``delivery_charges``. After retrieving the residuals, we observe the distribution of residuals and obtain the outliers of the residual distributions. These outliers are the multivariate outliers of ``delivery_charges`` that we are looking for.

  
