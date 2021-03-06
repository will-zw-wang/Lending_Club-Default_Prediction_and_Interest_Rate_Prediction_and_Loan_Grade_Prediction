# Lending_Club

<img src="https://github.com/will-zw-wang/Lending_Club-Default_Prediction_and_Interest_Rate_Prediction_and_Loan_Grade_Prediction/blob/master/images/Lending_Club_image.jpg" width="660" height="200">

[**Code**](https://github.com/will-zw-wang/Lending_Club-Default_Prediction_and_Interest_Rate_Prediction_and_Loan_Grade_Prediction/tree/master/code)

## Project Objectives

- For Lending club, it's extremely important to know how's the loan repayment capacity for each loan applicant, and how much interest rate should be assigned to each loan application appropriately. 
    - Firstly, knowing the loan repayment capacity for each loan applicant could help it decide whether to "accept" or "deny" a loan application. 
    - Second, evaluating interest rate precisely could bring an additional incentive for those who are willing to "lend" money and also attain a balance between demand (borrowers) and supply (lenders).
- As a result, some suitable metrics must be determined by looking at the dataset.
    - **Metrics**
        - **loan status**: To evaluate the loan repayment capacity for each loan applicant.
        - **interest rate**: A numerical feature playing a role of balancing demand and supply. 
        - **grade**: A good categorical index to know the loan repayment capacity.       
- In our work, we built models to predict **Default(loan status)**, **Interest Rate** and **Loan Grade**.

## Dataset description
- Dataset was downloaded from [**Lending Club Statistics**](https://www.lendingclub.com/info/download-data.action).
    - The file contains complete loan data for all loans issued in 2018Q1, including the current loan status (Current, Late, Fully Paid, etc.) and latest payment information. 
    - The file contains loan data through the "present" contains complete loan data for all loans issued through the previous completed calendar quarter. 
    - 'LoanStats_2018Q1.csv' contains loan application records from January 2018 to March 2018.
        - 107866 loan applications.
        - 145 features.
    - [**Data is available here**](https://github.com/will-zw-wang/Lending_Club-Default_Prediction_and_Interest_Rate_Prediction_and_Loan_Grade_Prediction/blob/master/data/LoanStats_2018Q1.csv.zip)
- I classified all 145 features into eight types:
    - User feature (general)
    - User feature (financial specific)
        - Income
        - Credit scores
        - Credit lines
    - Loan general feature
    - Current loan payment feature
    - Potential response variables
    - Secondary applicant info
    - Hardship 
    - Settlement
    - [**Grouped features dictionary is available here**](https://github.com/will-zw-wang/Lending_Club-Default_Prediction_and_Interest_Rate_Prediction_and_Loan_Grade_Prediction/blob/master/data/LC_DataDictionary.xlsx)

## Analysis Structure
1. Data_Exploration
2. Missing values imputation
3. Feature transformation
4. Outliers imputation
5. Feature selection by feature significance
6. Default_Prediction
7. Interest_Rate_Prediction
8. Loar_Grade_Prediction


## Analysis Details

### 1. Data_Exploration
- Read in the data
    - In total, 145 features, 107866 loan applications in this dataset.
    - We demonstrated more meaningful visualizations in later 'Data_Preprocessing_and_Feature_Engineering' section.
- Exploratory Data Analysis (EDA)
    - Explored user related features (basic information)
    - Explored user related feature (financial specific - income)
    - Explored user feature (financial specific - credit scores)
    - Explored user feature (financial specific - credit lines)
- [**Detailed Code and Plotting**](https://github.com/will-zw-wang/Lending_Club-Default_Prediction_and_Interest_Rate_Prediction_and_Loan_Grade_Prediction/blob/master/code/Lending_Club_Data_Exploration.ipynb)

### 2. Missing values imputation
- Goal
    - Three interesting problems (demonstrated as columns below) can be solved given this dataset.
        - grade
        - int_rate
        - loan_status
    - For this section, I mainly focused in 'loan_status'. 
        - My goal was trying to predict the loan repayment capacity for each loan applicant of each loan application. Furthermore, I started by creating a boolean type of 'loan_status'.
        - Created 'loan_status_binary', my target response variable 
        - 0 means a loan_status is either 'Fully Paid' or 'Current'
        - 1 means the rest values of loan_status 
- Numerical features
    - Dealt with numerical features containing at least 80% NA values.
        - Irrelevant features
        - Joint-type features
        - Secondary applicant related features
        - Hardship related features
        - Settlement related features
        - Other numerical features
    - Dealt with numerical features containing at least 1 NA vlaue.
        - Month type features
        - The rest of features 
- Categorical features
    - Dealt with categorical features containing at least 80% NA values.
        - Irrelevant features
        - Hardship related features
        - Settlement related features
        - Other features
    - Dealt with other categorical features containing at least one NA value.
        - Month type features
        - The rest of features
- [**Detailed Code and Plotting**](https://github.com/will-zw-wang/Lending_Club-Default_Prediction_and_Interest_Rate_Prediction_and_Loan_Grade_Prediction/blob/master/code/Lending_Club_Data_Preprocessing_and_Feature_Engineering.ipynb)

### 3. Feature transformation
- Datetime type features transformation
- Features with a wrong data type
- Categorical features with too many levels
- Log transformation for skewed features
- Final check
    - Final check I: NA values
            - If these were variables forgot to drop after complete the transformation
    - Final check II: If these were features with only one unique value among all records (no prediction power)
    - Final check III: Manually scanned the data type & columns forgot to drop
- [**Detailed Code and Plotting**](https://github.com/will-zw-wang/Lending_Club-Default_Prediction_and_Interest_Rate_Prediction_and_Loan_Grade_Prediction/blob/master/code/Lending_Club_Data_Preprocessing_and_Feature_Engineering.ipynb)

### 4. Outliers imputation
- Manipulated outliers with **IQR method**
- [**Detailed Code and Plotting**](https://github.com/will-zw-wang/Lending_Club-Default_Prediction_and_Interest_Rate_Prediction_and_Loan_Grade_Prediction/blob/master/code/Lending_Club_Data_Preprocessing_and_Feature_Engineering.ipynb)

### 5. Feature selection by feature significance
- Numerical type of features: using **t-test**
- Categorical type of features: using **chi-square test**
- [**Detailed Code and Plotting**](https://github.com/will-zw-wang/Lending_Club-Default_Prediction_and_Interest_Rate_Prediction_and_Loan_Grade_Prediction/blob/master/code/Lending_Club_Data_Preprocessing_and_Feature_Engineering.ipynb)

### 6. Default_Prediction
- Modeling
    - **Logistic Regression**
        - Model Performance
            - <img src="https://github.com/will-zw-wang/Lending_Club-Default_Prediction_and_Interest_Rate_Prediction_and_Loan_Grade_Prediction/blob/master/images/Default_Prediction/Model_Performance_LR.png" width="360" height="360">
        - Confusion Matrix
            - <img src="https://github.com/will-zw-wang/Lending_Club-Default_Prediction_and_Interest_Rate_Prediction_and_Loan_Grade_Prediction/blob/master/images/Default_Prediction/Confusion_Matrix_LR.png" width="360" height="100">
    - **Random Forest** 
        - Model Performance
            - <img src="https://github.com/will-zw-wang/Lending_Club-Default_Prediction_and_Interest_Rate_Prediction_and_Loan_Grade_Prediction/blob/master/images/Default_Prediction/Model_Performance_RF.png" width="360" height="360">
        - Confusion Matrix
            - <img src="https://github.com/will-zw-wang/Lending_Club-Default_Prediction_and_Interest_Rate_Prediction_and_Loan_Grade_Prediction/blob/master/images/Default_Prediction/Confusion_Matrix_RF.png" width="360" height="100">
    - **Gradient Boosting Decision Tree**
        - Model Performance
            - <img src="https://github.com/will-zw-wang/Lending_Club-Default_Prediction_and_Interest_Rate_Prediction_and_Loan_Grade_Prediction/blob/master/images/Default_Prediction/Model_Performance_GDBT.png" width="360" height="360">
        - Confusion Matrix
            - <img src="https://github.com/will-zw-wang/Lending_Club-Default_Prediction_and_Interest_Rate_Prediction_and_Loan_Grade_Prediction/blob/master/images/Default_Prediction/Confusion_Matrix_GDBT.png" width="360" height="100">
- Summary
    - **Feature Conclusion**
        - Features' coefficients estimated by **Logistic Regression** - **Positive Direction**: 
            - Once the **difference between issue and next payment day**, the **monthly payment owed by the borrower if the loan originates**, and **levels of day difference between issue and last payment day is less than 6 months** became larger, the higher probability that a loan applicant might be default.
            - <img src="https://github.com/will-zw-wang/Lending_Club-Default_Prediction_and_Interest_Rate_Prediction_and_Loan_Grade_Prediction/blob/master/images/Default_Prediction/Features_positive_coefficients_LR.png" width="360" height="400">           
        - Features' coefficients estimated by **Logistic Regression** - **Negative Direction**:
            - As long as the **difference between issue and last payment day  is more than 6 months**, **payment received to date for portion of total amount funded by investors**, and **principal received to date** became larger, the lower probability that a loan applicant might be default.
            - <img src="https://github.com/will-zw-wang/Lending_Club-Default_Prediction_and_Interest_Rate_Prediction_and_Loan_Grade_Prediction/blob/master/images/Default_Prediction/Features_negative_coefficients_LR.png" width="360" height="400">
        - Features' importances by **Random Forest**: 
            - Six of the feature importances provided by Random Forest were overlapped with the results of feature importance by Logistic Regression. These top 10 features had a better prediction power compared with the rest of the features in Random Forest model.
            - <img src="https://github.com/will-zw-wang/Lending_Club-Default_Prediction_and_Interest_Rate_Prediction_and_Loan_Grade_Prediction/blob/master/images/Default_Prediction/Features_importance_RF.png" width="360" height="400">
    - **Model Deployment**
        - For the Lending Club, it can decide a model deployment by a predefined objective. It might care a lot on predicting the correct **Default** applications, so a higher **recall** score becomes extremely important.
        - <img src="https://github.com/will-zw-wang/Lending_Club-Default_Prediction_and_Interest_Rate_Prediction_and_Loan_Grade_Prediction/blob/master/images/Default_Prediction/Model_Deployment.png" width="360" height="120">
- [**Detailed Code and Plotting**](https://github.com/will-zw-wang/Lending_Club-Default_Prediction_and_Interest_Rate_Prediction_and_Loan_Grade_Prediction/blob/master/code/Lending_Club_Default_Prediction.ipynb)

### 7. Interest_Rate_Prediction
- Modeling
    - Linear Regression
    - Bagged Decision Tree Regression
    - Random Forest Regression
    - Gradient Boosting Regression
- Summary
    - **Feature Conclusion (by Random Forest Regression)**
        - Top 10 most important features provided by the Random Forest. 
            - <img src="https://github.com/will-zw-wang/Lending_Club-Default_Prediction_and_Interest_Rate_Prediction_and_Loan_Grade_Prediction/blob/master/images/Interest_Rate_Prediction/Features_importance_RF.png" width="300" height="320">       
        - Even though the model explained variance was low in our work, we noticed that the top three features explaind much more model total variances than the others. We want to explain the relationship between these features and the response feature (interest rate on the loan). 
            - The first feature, **term** (The number of payments on the loan. Values are in months and can be either 36 or 60) was important to int_rate, because higher term indicated higher int_rate. 
            - The second & thrid feature, if a loan applicant had more **bc_open_to_buy** (Total open to buy on revolving bankcards) and **percent_bc_gt_75** (Percentage of all bankcard accounts > 75% of limit), the int_rate might be higher since a loan applicant might under greater financial pressure in the past.
    - **Model Comparison (Metric evaulations)**
        - At here, I had two defined objectives, mean squared error and R-squared, to evaluate a continuous regression problem. For the Lending Club, it can decide a model deployment by a predefined objective. It might care a lot on the **MSE** as long as the variance is not too high. Thus, **GB Regression** is preferred here.
        - <img src="https://github.com/will-zw-wang/Lending_Club-Default_Prediction_and_Interest_Rate_Prediction_and_Loan_Grade_Prediction/blob/master/images/Interest_Rate_Prediction/Model_Deployment.png" width="560" height="140">
    - **Next Step**
        - The predictive power of our models were week here(MSEs were high), this might because that Lending Club decide the interest rate based more on set of qualitative norms or quantitative data that are not in our dataset. 
        - To build better models, we should spend more time to understand Lending Club's business model and interest rate decision making process to get more related and complete dataset.
- [**Detailed Code and Plotting**](https://github.com/will-zw-wang/Lending_Club-Default_Prediction_and_Interest_Rate_Prediction_and_Loan_Grade_Prediction/blob/master/code/Lending_Club_Interest_Rate_Prediction.ipynb)

### 8. Loar_Grade_Prediction
- Modeling
    - **Logistic Regression**
        - Model Performance
            - accuracy_train: 0.5418
            - accuracy_test: 0.5342
        - Confusion Matrix
            - <img src="https://github.com/will-zw-wang/Lending_Club-Default_Prediction_and_Interest_Rate_Prediction_and_Loan_Grade_Prediction/blob/master/images/Loan_Grade_Prediction/Confusion_Matrix_LR.png" width="600" height="240"> 
    - **Random Forest**
        - Model Performance
            - accuracy_train: 0.5912
            - accuracy_test: 0.5167
        - Confusion Matrix
            - <img src="https://github.com/will-zw-wang/Lending_Club-Default_Prediction_and_Interest_Rate_Prediction_and_Loan_Grade_Prediction/blob/master/images/Loan_Grade_Prediction/Confusion_Matrix_RF.png" width="600" height="240">         
    - **Gradient Boosting Decision Tree**
        - Model Performance
            - accuracy_train: 0.6686
            - accuracy_test: 0.5479
        - Confusion Matrix
            - <img src="https://github.com/will-zw-wang/Lending_Club-Default_Prediction_and_Interest_Rate_Prediction_and_Loan_Grade_Prediction/blob/master/images/Loan_Grade_Prediction/Confusion_Matrix_GDBT.png" width="600" height="240">    
- Summary
    - **Feature Conclusion**
        - Features' coefficients estimated by **Logistic Regression** - **Positive** Direction: 
            - Positive Direction: As long as the **funded_amnt_inv** (total amount committed by investors for that loan at that point in time), **loan_amnt** (The listed amount of the loan applied for by the borrower), and **out_prncp_inv** (Remaining outstanding principal for portion of total amount funded by investors) became larger, the higher probability that a loan applicantion might be graded with a lower grade.
            - <img src="https://github.com/will-zw-wang/Lending_Club-Default_Prediction_and_Interest_Rate_Prediction_and_Loan_Grade_Prediction/blob/master/images/Loan_Grade_Prediction/Features_positive_coefficients_LR.png" width="300" height="400">           
        - Features' coefficients estimated by **Logistic Regression** - **Negative** Direction:
            -  When the **total_rec_int** (interest received to date), **installment**(The monthly payment owed by the borrower if the loan originates), and **term**(The number of payments on the loan) became smaller, the higher probability that a loan applicantion might be graded with a lower grade.
            - <img src="https://github.com/will-zw-wang/Lending_Club-Default_Prediction_and_Interest_Rate_Prediction_and_Loan_Grade_Prediction/blob/master/images/Loan_Grade_Prediction/Features_negative_coefficients_LR.png" width="360" height="400">
        - Features' importances by **Random Forest**: 
            - Nine of the feature importances provided by Random Forest were overlapped with the results by Logistic Regression. These top 10 features had a better prediction power compared with the rest of the features in Random Forest model.
            - <img src="https://github.com/will-zw-wang/Lending_Club-Default_Prediction_and_Interest_Rate_Prediction_and_Loan_Grade_Prediction/blob/master/images/Loan_Grade_Prediction/Features_importance_RF.png" width="300" height="400">
    - **Model Deployment**
        - For the Lending Club, it can decide a model deployment by a predefined objective. At here, my defined objective is the highest 'accuracy' score across all lables (grade A to G). However, it might be better (or more intuitive) for the Lending Club to target at the accuracy score of 'lower grades' (such as from grade D to G). Thus, **GB Regression** is preferred here.
        - <img src="https://github.com/will-zw-wang/Lending_Club-Default_Prediction_and_Interest_Rate_Prediction_and_Loan_Grade_Prediction/blob/master/images/Loan_Grade_Prediction/Model_Deployment.png" width="560" height="140"> 
    - **Next Step**
        - The predictive power of our models were week here(Accuracies were low, especially in lower grade prediction), this might because that Lending Club decide the loan grade based more on set of qualitative norms or quantitative data that are not in our dataset. 
        - To build better models, we should spend more time to understand Lending Club's business model and loan grade decision making process to get more related and complete dataset.
- [**Detailed Code and Plotting**](https://github.com/will-zw-wang/Lending_Club-Default_Prediction_and_Interest_Rate_Prediction_and_Loan_Grade_Prediction/blob/master/code/Lending_Club_Loan_Grade_Prediction.ipynb)







