# Lending_Club

<img src="https://github.com/will-zw-wang/Lending_Club-Default_Prediction_and_Interest_Rate_Prediction_and_Loan_Grade_Prediction/blob/master/images/Lending_Club_image.jpg" width="660" height="240">

pending-report

## Project Objectives

- For Lending club, it's extremely important to know how's the loan repayment capacity for each loan applicant, and how much interest rate should be assigned to each loan application appropriately. 
    - Firstly, knowing the loan repayment capacity for each loan applicant could help it decide whether to "accept" or "deny" a loan application. 
    - Second, evaluating interest rate precisely could bring an additional incentive for those who are willing to "lend" money and also attain a balance between demand (borrowers) and supply (lenders).
- As a result, some suitable metrics must be determined by looking at the dataset.
    - **Metrics**
        - loan status: To evaluate the loan repayment capacity for each loan applicant.
        - grade: A good categorical index to know the loan repayment capacity.
        - interest rate: A numerical feature playing a role of balancing demand and supply. 
- In our work, we bulid models to predict **Default**, **Interest Rate** and **Loan Grade**.

## Dataset description
Dataset is downloaded from https://www.lendingclub.com/info/download-data.action.
- These files contain complete loan data for all loans issued in 2018Q1, including the current loan status (Current, Late, Fully Paid, etc.) and latest payment information. 
- The file containing loan data through the "present" contains complete loan data for all loans issued through the previous completed calendar quarter. 
- 'LoanStats_2018Q1.csv' contains loan application records from January 2018 to March 2018.
    - 107866 loan applications.
    - 145 features.
- [**Data is available here**](https://www.lendingclub.com/info/download-data.action)

## Analysis Structure
1. Data_Exploration
2. Data_Preprocessing_and_Feature_Engineering


## Analysis Details

### 1. Data_Exploration
- Read in the data
    - In total, 145 features, 107866 loan applications in this dataset.
    - Will demonstrate more meaningful visualizations in later 'Data_Preprocessing_and_Feature_Engineering' section.
- Exploratory Data Analysis (EDA)
    - User related features (basic information)
        - addr_state
            - Description: The state provided by the borrower in the loan application.
            - Summary: 
                - Most loan applications came from California. 
                - Originally 50 levels under addr_state. 
                - **Note** : Too many levels and need to collapse into 5 levels by assigning each state to its corresponding region (by west or east coast).
        - zip_code
            - Summary: Too many levels under zip_code. Instead, use addr_state to express the similar information.        
        - emp_length
            - Description: 
                - Employment length in years. 
                - Possible values are between 0 and 10, where 0 means less than one year and 10 means ten or more years. 
            - Summary: Most loan applicants have employment length more than 10 years.
        - emp_title
            - Description: The job title supplied by the Borrower when applying for the loan.
            - Summary: 
                Too many levels under emp_title. 
                Use a basic string preprocessing on it and select top K levels to express more than half of total information.
        - home_ownership
            - Description: 
                - The home ownership status provided by the borrower during registration. P
                - ossible values are: RENT, OWN, MORTGAGE, and OTHER.
            - Summary: Most loan applicants have a mortgage on their houses.           
    - User related feature (financial specific - income)
        - annual_inc
            - Description: The self-reported annual income provided by the borrower during registration.
            - Summary: 
                - Many outliers and a very skewed distribution.
                - Perform log transformation in later section            
        - annual_inc_joint
            - Description: The combined self-reported annual income provided by the co-borrowers during registration.
            - Summary: Same issue as in annual_inc. Many outliers and a very skewed distribution.
        - verification_status
            - Description: Indicates if income was verified by LC, not verified, or if the income source was verified. 
            - Summary: Most loan applicants' income was either Income 'Source Verified' or 'Not Verified'.
        - verification_status_joint
            - Description: Indicates if the co-borrowers' joint income was verified by LC, not verified, or if the income source was verified
            - Summary: Most co-borrowers' joint income were 'not verified' by LC.
        - dti
            - Description: (debt to income ratio) A ratio calculated using the borrower’s total monthly debt payments on the total debt obligations, excluding mortgage and the requested LC loan, divided by the borrower’s self-reported monthly income.
            - Summary: 
                - Many outliers and a very skew distribution under dti .
                - Same issue as in annual_inc, because dti is a ratio of debt to income. (might due to the issue already found in annual_inc).
        - dti_joint
            - Description: A ratio calculated using the co-borrowers' total monthly payments on the total debt obligations, excluding mortgages and the requested LC loan, divided by the co-borrowers' combined self-reported monthly income. 
            - Summary: No outlier issue and a distribution close to a normal distribution.
    - User feature (financial specific - credit scores)
        - earliest_cr_line
            - Description: The month the borrower's earliest reported credit line was opened. 
            - Summary: 
                - It's a datetime type variable ranged from April 1963 to September 2014.
                - **Note** : Need a further transformation on it such as compare with other datetime type variables to attain day difference, which might explain more information rather than using itself as a variable.   
        - inq_fi
            - Description: Number of personal finance inquiries.
            - Summary: Most loan applicants inquired 0 time on their personal finance.
        - inq_last_12m
            - Description: Number of credit inquiries in past 12 months
            - Summary: Most loan applicants inquired 0 time on their credit information during past 12 months.
        - inq_last_6mths
            - Description: The number of inquiries in past 6 months (excluding auto and mortgage inquiries).
            - Summary: Most loan applicants inquired 0 time during past 6 months.
        - mths_since_recent_inq
            - Description: Months since most recent inquiry.
            - Summary: Months since most recent inquiry for most loan applicants are close to recent, from 0 to 5 months.
        - last_credit_pull_d
            - Description: The most recent month LC pulled credit for this loan.
            - Summary: LC pulled credit for loans on July 2018 mostly.
    - User feature (financial specific - credit lines)
        - total_acc
            - Description: The total number of credit lines currently in the borrower's credit file.
            - Summary: Many outliers under total_acc, but it has a distribution close to a normal distribution.
        - avg_cur_bal
            - Description: Average current balance of all accounts.
            - Summary: Many outliers and a very skewed distribution.
        - tot_cur_bal
            - Description: Total current balance of all accounts. 
            - Summary: Many outliers and a very skewed distribution.
        - mo_sin_rcnt_tl
            - Description: Months since most recent account opened.
            - Summary: Many outliers and a very skewed distribution.
        - mort_acc
            - Description: Number of mortgage accounts.
            - Summary: 
                - Many outliers and a very skewed distribution. 
                - Most loan applicants have 0 mortgage accounts.
        - bc_util
            - Description: Ratio of total current balance to high credit/credit limit for all bankcard accounts.
            - Summary: Few outilers under bc_util.
        - all_util
            - Description: Balance to credit limit on all trades.
            - Summary: Few outilers under all_util.
        - open_acc
            - Description: The number of open credit lines in the borrower's credit file.
            - Summary: Most loan applicants have 7 to 11 opened credit lines.
        - open_acc_6m
            - Description: Number of open trades in last 6 months. 
            - Summary: Few outliers under open_acc_6m. Most loan applicants have 0 open trade in last 6 months.
        - acc_open_past_24mths
            - Description: Number of trades opened in past 24 months.
            - Summary: Few outliers under acc_open_past_24mths. Most loan applicants have 1 to 5 open trades in past 24 months.
        - total_cu_tl
            - Description: Number of finance trades.
            - Summary: Many outliers under total_cu_tl. Most loan applicants have 0 finance trade.
        - mths_since_recent_bc
            - Description: Months since most recent bankcard account opened.
            - Summary: Many outliers and a very skewed distribution.
        - mths_since_recent_bc_dlq
            - Description: Months since most recent bankcard delinquency.
            - Summary: Many outliers under mths_since_recent_bc_dlq.
        - num_accts_ever_120_pd
            - Description: Number of accounts ever 120 or more days past due.
            - Summary: Many outliers under num_accts_ever_120_pd. Most loan applicants have no account ever 120 or more days past due.
        - num_actv_bc_tl
            - Description: Number of currently active bankcard accounts.
            - Summary: Few outliers under num_actv_bc_tl. Most loan applicants have 1 to 5 active bankcard accounts.
        - num_bc_sats
            - Description: Number of satisfactory bankcard accounts.
            - Summary: Few outliers under num_actv_bc_tl. Most loan applicants have 2 to 6 satisfactory bankcard accounts.
        - num_bc_tl
            - Description: Number of bankcard accounts.
            - Summary: Many outliers under num_actv_bc_tl. Most loan applicants have 3 to 7 bankcard accounts.
        - num_sats
            - Description: Number of satisfactory accounts.
            - Summary: Many outliers under num_sats.
        - num_tl_120dpd_2m
            - Description: Number of accounts currently 120 days past due (updated in past 2 months).
            - Summary: Nearly all loan applicatns have 0 account currently 120 days past due.
        - num_tl_30dpd
            - Description: Number of accounts currently 30 days past due (updated in past 2 months).
            - Summary: Nearly all loan applicatns have 0 account currently 30 days past due.
        - num_tl_90g_dpd_24m
            - Description: Number of accounts 90 or more days past due in last 24 months.
            - Summary: Nearly all loan applicatns have 0 account 90 or more days past due in last 24 months.
        - num_tl_op_past_12m
            - Description: Number of accounts opened in past 12 months.
            - Summary: A fourth of all loan applicants have one account opened in past 12 months.
        - percent_bc_gt_75
            - Description: Percentage of all bankcard accounts > 75% of limit.
            - Summary: Most loan applicants have 0% of all bankcard accounts > 75% of limit.
        - tax_liens
            - Description: Number of tax liens.
            - Summary: Most loan applicants have 0 tax lien.
        - tot_hi_cred_lim
            - Description: Total high credit/credit limit.
            - Summary: Many outliers and a very skewed distribution under tot_hi_cred_lim.
        - total_bal_ex_mort
            - Description: Total credit balance excluding mortgage.
            - Summary: Many outliers and a very skewed distribution under total_bal_ex_mort.
        - total_bc_limit
            - Description: Total bankcard high credit/credit limit.
            - Summary: Many outliers and a very skewed distribution under total_bc_limit.
- [**Detailed Code and Plotting**](pending)

### 2. Data_Preprocessing_and_Feature_Engineering
- Goal
    - Three interesting problems (demonstrated as columns below) can be solved given this dataset.
        - grade
        - int_rate
        - loan_status
    - For this section, I mainly focus in 'loan_status'. 
        - My goal is trying to predict the loan repayment capacity for each loan applicant of each loan application. Furthermore, I started by creating a boolean type of 'loan_status'.
        - Create 'loan_status_binary', my target response variable 
        - 0 means a loan_status is either 'Fully Paid' or 'Current'
        - 1 means the rest values of loan_status 
- Data preprocessing - missing values imputation
    - Numerical features
        - Deal with numerical features containing at least 80% NA values.
            - 27 numerical type of features contain 80% of NA values.
            - Irrelevant features
                - Summary: Three features contain no information in the dataset.
                - Features
                    - member_id: A unique LC assigned Id for the borrower member. (107864 NA values)
                    - url: URL for the LC page with listing data. (107864 NA values)
                    - desc: Loan description provided by the borrower. (107864 NA values)
                - NA imputation: drop off irrelevant features
            - Joint-type features
                - Summary: Four joint-type features have their corresponding non-joint type features.
                - Features
                    - annual_inc_joint: The combined self-reported annual income provided by the co-borrowers during registration. (91533 NA values)
                    - dti_joint: A ratio calculated using the co-borrowers' total monthly payments on the total debt obligations, excluding mortgages and the requested LC loan, divided by the co-borrowers' combined self-reported monthly income. (91533 NA values)
                    - verification_status_joint: Indicates if the co-borrowers' joint income was verified by LC, not verified, or if the income source was verified. (91847 NA values)
                    - revol_bal_joint: Sum of revolving credit balance of the co-borrowers, net of duplicate balances. (91533 NA values)
                - NA imputation: NA imputation with the corresponding non-joint type features replacement.
            - Secondary applicant related features
                - Summary: 
                    - All secondary applicant related features have the same and no significantly different trend in loan_status between NA and non-NA values. 
                    - However, there is a strong relationship between each pair of secondary applicant related features.
                - Features
                    - sec_app_inq_last_6mths: Credit inquiries in the last 6 months at time of application for the secondary applicant. (91533 NA values)
                    - sec_app_mort_acc: Number of mortgage accounts at time of application for the secondary applicant. (91533 NA values)
                    - sec_app_open_acc: Number of open trades at time of application for the secondary applicant. (91533 NA values)
                    - sec_app_revol_util: Ratio of total current balance to high credit/credit limit for all revolving accounts. (91843 NA values)
                    - sec_app_open_act_il: Number of currently active installment trades at time of application for the secondary applicant. (91533 NA values)
                    - sec_app_num_rev_accts: Number of revolving accounts at time of application for the secondary applicant. (91533 NA values)
                    - sec_app_chargeoff_within_12_mths: Number of charge-offs within last 12 months at time of application for the secondary applicant. (91533 NA values)
                    - sec_app_collections_12_mths_ex_med: Number of collections within last 12 months excluding medical collections at time of application for the secondary applicant. (91533 NA values)
                    - sec_app_mths_since_last_major_derog: Months since most recent 90-day or worse rating at time of application for the secondary applicant. (102437 NA values)
                - NA imputation with the median value replacement.
            - Hardship related features
                - Summary: Only one record available in hardship related features. Thus, these features might have lower feature importance on the response variable.
                - Features
                    - deferral_term: Amount of months that the borrower is expected to pay less than the contractual monthly payment amount due to a hardship plan.
                    - hardship_amount: The interest payment that the borrower has committed to make each month while they are on a hardship plan. 
                    - hardship_length: The number of months the borrower will make smaller payments than normally obligated due to a hardship plan.
                    - hardship_dpd: Account days past due as of the hardship plan start date.
                    - orig_projected_additional_accrued_interest: The original projected additional interest amount that will accrue for the given hardship payment plan as of the Hardship Start Date. This field will be null if the borrower has broken their hardship payment plan.
                    - hardship_payoff_balance_amount: The payoff balance amount as of the hardship plan start date.
                    - hardship_last_payment_amount: The last payment amount as of the hardship plan start date.
                - NA imputation: Replace all NA values with 0 to signify not using the hardship plan. (or not using other hardship related loan services)
            - Settlement related features
                - Summary: Only nine records available in settlement related features. Thus, these features might have lower feature importance on the response variable.
                - Features
                    - settlement_amount: The loan amount that the borrower has agreed to settle for.
                    - settlement_percentage: The settlement amount as a percentage of the payoff balance amount on the loan.
                    - settlement_term: The number of months that the borrower will be on the settlement plan.
                - NA imputation: Replace all NA values with 0 to signify borrowers are not working with a debt-settlement company.
            - Other numerical features
                - Features
                    - mths_since_last_record: The number of months since the last public record. (92595 NA values)
                    - mths_since_recent_bc_dlq: Months since most recent bankcard delinquency. (86566 NA values)
                - NA imputation: Replace NA values with the median value.
        - Deal with numerical features containing at least 1 NA vlaue.
            - 15 numerical type variable contains NA values.
            - Month type features
                - Features
                    - mths_since_last_delinq: The number of months since the borrower's last delinquency.
                    - mths_since_last_major_derog: Months since most recent 90-day or worse rating
                    - mths_since_rcnt_il: Months since most recent installment accounts opened
                    - mths_since_recent_bc: Months since most recent bankcard account opened.
                    - mths_since_recent_inq: Months since most recent inquiry.
                    - mths_since_recent_revol_delinq: Months since most recent revolving delinquency.
                    - mo_sin_old_il_acct: Months since oldest bank installment account opened.
                - NA imputation: Replace NA values with the median value.
            - The rest of features
                - il_util: 
                    - Description: Ratio of total current balance to high credit/credit limit on all installment account.
                    - NA imputation: 
                        - First, use total_bal_il / total_il_high_credit_limit to replace NA values. 
                        - Second, replace NA values with 0.
                - avg_cur_bal
                    - Description: Average current balance of all accounts.
                    - NA imputation: 
                        - Use tot_cur_bal / total_acc to replace NA values. But, I found when avg_cur_bal is NA, the corresponding values of tot_cur_bal & total_acc are all 0 too (except one record total_acc is 1). 
                        - Therefore, I replace NA values with 0.
                - num_tl_120dpd_2m
                    - Description: Number of accounts currently 120 days past due (updated in past 2 months).
                    - NA imputation: Replace NA values with 0.                
            - Remaining numerical type variables contain NA value
                - Features
                    - all_util: Balance to credit limit on all trades.
                    - bc_open_to_buy: Total open to buy on revolving bankcards.
                    - bc_util: Ratio of total current balance to high credit/credit limit for all bankcard accounts.
                    - pct_tl_nvr_dlq: Percent of trades never delinquent.
                    - percent_bc_gt_75: Percentage of all bankcard accounts > 75% of limit.
                - NA imputation: Replace NA values with median.
    - Categorical features
        - Deal with categorical features containing at least 80% NA values.
            - 12 categorical type features contain 80% of NA values.
            - Irrelevant features
                - Summary: One feature contain no information in the dataset.
                - Features
                    - id: A unique LC assigned ID for the loan listing. (107864 NA values)
                - NA imputation: drop off irrelevant features.
            - Hardship related features
                - Summary: Only one record available in hardship related features. Thus, these features might have lower feature importance on the response variable.
                - Features
                    - Non datetime type
                        - hardship_type: Describes the hardship plan offering.
                        - hardship_reason: Describes the reason the hardship plan was offered.
                        - hardship_status: Describes if the hardship plan is active, pending, canceled, completed, or broken.
                        - hardship_loan_status: Loan Status as of the hardship plan start date.
                    - Datetime type
                        - hardship_start_date: The start date of the hardship plan period.
                        - hardship_end_date: The end date of the hardship plan period.
                        - payment_plan_start_date: The day the first hardship plan payment is due. 
                            - For example, if a borrower has a hardship plan period of 3 months, the start date is the start of the three-month period in which the borrower is allowed to make interest-only payments.
                - NA imputation: 
                    - Non datetime type: Replace NA values with a new level signifying NA values.
                    - Datetime type: Compared itself with the issue_d to create a new set of day difference features and then replace NA values with a NA level.
            - Settlement related features
                - Summary: Only nine records available in settlement related features. Thus, these features might have lower feature importance on the response variable.
                - Features
                    - Non datetime type
                        - settlement_status: The status of the borrower’s settlement plan. Possible values are: COMPLETE, ACTIVE, BROKEN, CANCELLED, DENIED, DRAFT.
                    - Datetime type
                        - debt_settlement_flag_date: The most recent date that the Debt_Settlement_Flag has been set.
                        - settlement_date: The date that the borrower agrees to the settlement plan.
                - NA imputation: 
                    - Non datetime type: Replace NA values with a new level signifying NA values.
                    - Datetime type: Compared itself with the issue_d to create a new set of day difference features and then replace NA values with a NA level.
            - Other features
                - Features
                    - sec_app_earliest_cr_line: Earliest credit line at time of application for the secondary applicant.
                - NA imputation: 
                    - Datetime type: Compared itself with the maximum issue_d to create a day difference feature and then replace NA values with the median value.    
        - Other categorical features containing at least one NA value.
            - 6 categorical type of features contain NA values.
            - Month type features
                - Summary
                    - Three datetime type features found in this step. 
                    - It's more reasonable to transform these features into the day difference between itself and issue day or into the day difference between itself and a benchmark date. 
                    - We are not sure whether the day difference is a great feature to explain a linear relationship with the response variable (requires a linear assumption). 
                    - We can also further do grouping on day difference features by creating bins (create a categorical type of feature).
                - Features
                    - last_pymnt_d: Last month payment was received
                    - next_pymnt_d: Next scheduled payment date
                    - last_credit_pull_d: The most recent month LC pulled credit for this loan.
                - NA imputation:
                    - last_pymnt_d: compare itself with issue_d
                        - The minimum day difference between last payment day and issue day is 0
                        - The maximum day difference between last payment day and issue day is 212
                        - The difference between each level is around 3 months, and we will have 8 levels (categorical feature)
                    - next_pymnt_d: compare itself with issue_d
                        - The minimum day difference between next payment day and issue day is 184
                        - The maximum day difference between next payment day and issue day is 273
                        - The difference between each level is around 3 months, and we will have 6 levels (categorical feature)
                    - last_credit_pull_d: compare itself with the maximum value in issue_d
                        - The difference between each level is around 1 months, and we will have 10 levels (categorical feature)
            - The rest of features
                - emp_title
                    - Description: The job title supplied by the Borrower when applying for the loan.    
                    - NA imputation:
                        - Originally, emp_title has 37289 levels. 
                        - Use a basic string preprocessing to lower down levels. 
                        - For each title, extract the last element splitting by the space and lowercase it. 
                        - Then, find top k levels to include as much information as we can and specify all the rest of levels as the NA level.
                            - Use top 20 levels to include 56% information and specify the rest levels as 'Other titles'
                - emp_length
                    - Description: Employment length in years. Possible values are between 0 and 10 where 0 means less than one year and 10 means ten or more years.
                    - NA imputation: Replace NA values with a new NA level.
                - revol_util
                    - Description: Revolving line utilization rate, or the amount of credit the borrower is using relative to all available revolving credit.      
                    - NA imputation: Replace NA values with the median value.                   
- Feature transformation
    - Datetime type features transformation
        - Summary: Still, there are other datetime type features needed to be deal with. They could be important features after the transformation.
        - issue_d
            - Description: The month which the loan was funded.
            - Transformation: compare itself with the maximum value (2018-03-01) in itself. (Using the same benchmark day to avoid a biased outcome.)
        - earliest_cr_line
            - Description: The month the borrower's earliest reported credit line was opened. 
            - Transformation: compare itself with the maximum value (2018-03-01) in issue_d. (Using the maximum value in issue_d as a benchmark to compare with earliest_cr_line is reasonable because if the day difference is larger, it might indicate that a loan applicant had a long credit history before, which might further indicate that a loan applicant could fully paid a loan application with a higher probability.)    
    - Features with a wrong data type
        - int_rate:
            - Description: Iterest rate on the loan.
            - Transformation: Change data type from 'string' to 'float'.
    - Categorical features with too many levels
        - addr_state:
            - Description: The state provided by the borrower in the loan application.
            - Transformation: Assign each state by west or east coast and drop zip_code column.
    - Log transformation for skewed features
        - Features:
            - annual_inc: The self-reported annual income provided by the borrower during registration. 
            - avg_cur_bal: Average current balance of all accounts.
            - tot_cur_bal: Total current balance of all accounts.
            - mo_sin_rcnt_tl: Months since most recent account opened.
            - mort_acc: Number of mortgage accounts.
            - mths_since_recent_bc: Months since most recent bankcard account opened.
            - tot_hi_cred_lim: Total high credit/credit limit.
            - total_bal_ex_mort: Total credit balance excluding mortgage.
            - total_bc_limit: Total bankcard high credit/credit limit.
        - Transformation: In the previoous section 'Data_Exploration', we find these features with a very skewed distribution, we perform log transformation for these feature here.   
    - Final check
        - Final check I: NA values
            - These are variables forgot to drop after complete the transformation
        - Final check II: features with only one unique value among all records (no prediction power)
        - Final check III: manually scan the data type & columns forgot to drop
- Outliers imputation
- Feature selection by feature significance
    - 6.1 Numerical type of features: using t-test
    - 6.2 Categorical type of features: using chi-square test
7. Save the final dataset into a csv file for a later model fitting operation








