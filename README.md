# E-Commerce Data Analysis

## Objective
The objective of this project is to predict customer churn for an online retail business using historical transaction data, and to identify at-risk customers for targeted retention efforts.

## Data Description 
- Dataset: Online Retail II from UCI: https://archive.ics.uci.edu/dataset/502/online+retail+ii
- Training Period: 2009-2010
- Testing Period: 2010-2011
- Unit of Analysis: Customer-level
  
(Transactions were aggregated to the customer level to create behavioral features based on purchase history.)

## Data Frames
```
df_2009 = pd.read_excel(file_path, sheet_name='Year 2009-2010')
df_2010 = pd.read_excel(file_path, sheet_name='Year 2010-2011')
```
## Data Sanity Checks
- df_2009.info(): used to get a structural look at the dataset, look for missing or wrong data types.
- df_2009.describe(): used to get a feel of the data. Are the numbers reasonable? Are there outliers and strange values?
- df_2009.isna().sum(): used to get the total number of null items in each column of the dataset.

## Data Cleaning & Preparation
- Removed transactions with missing customer IDs or descriptions
- Removed canceled invoices and non-positive quantities or prices
- Aggregated transactions at the invoice level
- Removed duplicate records
- Aggregated transactions by customer IDs

## Feature Engineering
Two time-based behavioral features were engineered per customer:
  - Tenure (days): time between first purchase and the reference date, where reference date is the date we consider to have enough information to determine whether a customer has churned or not.
  - Recency (days): time since the most recent purchase.

These features capture customer engagement and purchasing inactivity.

## Model
- Algorithm: Logistic Regression
- Features Used: tenure_days, recency
- Target Variable: Customer churn (binary)

Additionally, a Random Forest Model was trained on the same data and got similar results. However, Logistic regression was selected for its interpretability and sustainability as a baseline churn model.

## Model Performance (Validation on 2009-2010 Data)
Using a probability threshold of 0.3, selected to favor recall:
- Accuracy: 66.31%
- Precision: 45.21%
- Recall: 71.95%
- ROC AUC: 0.714

Interpretation:
- The model correctly identifies approximately 72% of churned customers, which is appropriate for churn prevention use cases.
- Precision is moderate, indicating some false positive, but this trade-off is done when retention outradch is less costly than customer loss.
- ROC AUC of 0.714 suggests good discriminative power.

## Feature Importance & Interpretation
- tenure_days (weight: -0.789): longer-tenured customers are less likely to churn.
- recency (weight: 0.894): customers inactive for longer periods are more likely to churn
These results align with business intuition: loyal customers churn less, while disengaged customers are at a higher risk.

## Customer Risk Scoring (2010-2011 Data)
The trained model was applied to the 2010-2011 dataset to generate predicted churn probabilities and risk segments.
### Risk Segmentation Assigned Thresholds:
- Low Risk: probability < 0.3
- Medium Risk: 0.3 <= probability <= 0.7
- High Risk: probability > 0.7
### Customer Distribution:
```
              Count (customers) |	Percentage (%) | Average Tenure (days)	| Average Recency (days)
risk_segment				
Low    	           3000	        |     69.16	       |        234.76	        |         46.50
Medium	           1338	        |     30.84	       |        194.38          |        192.51
```
Medium-risk customers exhibit significantly higher recency vlaues, indicating prolonged inactivity despite moderate tenure.

## Key Insights
- Customer churn is strongly driven by recency of engagement.
- A sizable portion of the customer base (~31%) is at moderate churn risk.
- No customers fall into an extreme high-risk category, suggesting a generally stable customer base or conservative probability calibration.

## Actionable Recommendations
- Prioritize medium-risk customers for re-engagement campaigns.
- Use targeted incentives, reminders, or personalized offers to reduce inactivity.
- Monitor churn outsomes in the 2010-2011 period to validate model performance over time.
