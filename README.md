# Predictive Analytics of Soft Drinks Production

---
## Abstract - This report aims to represents my analysis for the soft drink production to predict the quality if product in several stages of the production process.
---
### I. DATA INSIGHTS AND VISUALIZATION

Given the historical data from Jan 2021 to June 2021, we see the predict variable “g4_var2” is dependent on the values generated from its preceding variables. This is a complete time series forecasting and the variable “datetime” should be considered”. The optimal value is -0.8574 and hence a threshold of +0.5 can be considered to predict the variable being optimal. A quick overview of the data spread is given in fig1.

![image](https://user-images.githubusercontent.com/38722860/131846271-b0527153-b27f-4449-9b88-6e80022e45f6.png)

Despite the data being normalized to mean=0 and var=1, target fails to show to any relationship with other variable except g2_var_4 (Positively correlated).

![image](https://user-images.githubusercontent.com/38722860/131846335-18dd6222-c01c-49eb-8d6e-3df6b0a1fe9f.png)

The outliers seen are very informative about the subject-area and data collection process. It’s essential to understand how these outliers occur and it’s best to keep them, they can capture valuable information that is part of our study area. Though the data looks noisy and It is obvious, that we have peaks in every month. Smoothing is forbidden because it neglects the ups and downs associated with random variation and underlying phenomenon.

![image](https://user-images.githubusercontent.com/38722860/131846361-7a3635c1-f906-4d12-8bdc-eb082d8bab32.png)

---
### II. FEATURE ENGINEERING

Around 0.1% of predict variables are missing, this may be due to failure to load information. The right decision is deleting missing data rows and there is no need of imputation keeping in mind the missing percentage is small. However Variable “g3_var_3” and “g6_var_9” has 0 variance and this will not improve the performance of the model. In that case, it is removed.

Using just the variables alone to predict optimal value is not sophisticated and will likely result in a poor model. Nevertheless, this information coupled with additional engineered features may ultimately result in a better model. This is achieved by including the values at previous time steps by lagging and rolling mean.

![image](https://user-images.githubusercontent.com/38722860/131846510-c20ddec4-0a48-45df-9381-cfe59f2ce3ab.png)  ![image](https://user-images.githubusercontent.com/38722860/131846525-baa81313-2742-486c-921d-c4d7badc6433.png)

---
### III. MODEL BUILDING

The entire data is divided into three separate data sets, i.e. train, validation and test, each of which will be used for only one phase of the project. When creating each data set, we have ensured to keep mixture of data points at the high/low extremes, this ensures that the model can and must be accurate at all ranges of the spectrum.

![image](https://user-images.githubusercontent.com/38722860/131846681-8646bf54-ffb5-4ecb-8449-44fb83dc99ce.png)

Because all of the input variables are numeric and the problem is a simple supervised binary classification, Ensemble method seeks to create a strong classifier (model) based on “weak” classifiers, hence have used “XGBoost” to train the data. We are going to use Root mean square error (RMSE), Accuracy and Sensitivity to evaluate the quality of our predictions. Grid search 5-fold validation model along with all the hypertuning parameters are used to find an optimal solution. Here we will tune 5 of the hyperparameters that are usually having a big impact on performance like eta, max_depth, subsample, colsample_bytree, gamma. The best module generated is used to train the model and achieve RMSE as below.

![image](https://user-images.githubusercontent.com/38722860/131846898-f1e42226-194e-4e82-a300-9a1150d57aea.png)

Feature importance is used to estimate the relative importance of input features. The plot shows the high relative importance of lag and rolling mean and a lesser degree of importance to the actual provided variables.

![image](https://user-images.githubusercontent.com/38722860/131846954-44fafd1e-b9ca-42b7-9d7f-19983fea8455.png)

For the data trained in the model we have obtained the classification accuracy of about 98% on the validation set and with RMSE of 0.1. This occurs to be a good
model. The trained model was used to predict the test data and a confusion matrix provides a more detailed breakdown of correct and incorrect classifications. In our case, the classifier predicted all the 929 non optimal and 1565 optimal. However, it incorrectly classified 95 instances of non-optimal data as optimal and another 96 instances of optimal data as non- optimal. Looking at our problem statement non optimal category getting predicted to be optimal is huge risk for our client and hence sensitivity plays an important role.

![image](https://user-images.githubusercontent.com/38722860/131847077-67cb2879-b673-4f35-a158-308adae8b586.png)

---
### IV. MODEL DEPLOYMENT

Incoming training Data can be stored in on-premise, in cloud storage, or in a hybrid of the two. It makes sense to store your data where the model training will occur
and the results will be served. Build a web app using Flask framework. It will use the trained ML pipeline to generate predictions on new data points in real-time. Create a docker image and container. Publish the container onto Cloud Container Registry. Deploy the web app in the container by publishing onto Registry. Once deployed, it will become publicly available and can be accessed via a Web URL.

![image](https://user-images.githubusercontent.com/38722860/131847188-eefd157f-d1ec-4d9f-9d94-0905a7e3f2f6.png)

---
### V. REFERENCES
- https://arxiv.org/pdf/1603.02754.pdf
- https://kth.diva-portal.org/smash/get/diva2:1089425/FULLTEXT01.pdf
- https://christophm.github.io/interpretable-ml-book/rules.html
- https://cran.rproject.org/web/packages/datarobot/vignettes/TimeSeries.html
- https://r4ds.had.co.nz/
