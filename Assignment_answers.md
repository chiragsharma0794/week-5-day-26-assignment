# Assignment Answers — Pharma link Churn Prediction

1. Problem Identification

### Is this supervised or unsupervised learning?
This is supervised learning because the dataset already includes a target variable called `will_churn_next_30d`. Since the model will learn from past examples where the outcome is known, it fits the definition of supervised learning.

### Is this classification or regression?
This is a classification problem because the output has only two classes: 1 for churn and 0 for active. The model is predicting a category, not a continuous value.

### Which specific ML category from this week’s curriculum does this belong to?
This belongs to supervised machine learning, specifically binary classification. More specifically, it is a churn prediction use case where the goal is to identify pharmacies that may stop ordering soon.

--------------------------------------------------------------------------------------------------------------------------------------

2. Map to the 5-Stage ML Pipeline

### Data Collection
Pharma link should collect the last 15 months of pharmacy-level data using columns such as last_order_date, order_frequency_60d, avg_order_value, app_sessions_30d, credit_utilization_ratio, payment_delay_days, city_tier, and the target will_churn_next_30d.

### Data Preprocessing
They should convert last_order_date into a useful numeric feature like days_since_last_order, handle missing values in fields such as complaints_logged and payment_delay_days, and encode categorical columns like city_tier and store_type.

### Model Training
Pharmalink should train a binary classification model using the prepared features and the target column `will_churn_next_30d`. A simple starting point could be logistic regression, and then they can compare it with a tree-based model.

### Model Evaluation
The company should evaluate the model using metrics such as precision, recall, F1-score, and confusion matrix. They should also check model performance separately across different city_tier groups because pharmacy ordering behavior is not the same in every tier.

### Deployment
Once the model is finalized, Pharmalink can deploy it to score active pharmacies regularly and generate a churn-risk list for the retention team. This will help the business team contact high-risk pharmacies before they stop ordering.

---------------------------------------------------------------------------------------------------------------------------------------

3. Evaluation Strategy

### Should the company prioritize Precision or Recall?
The company should prioritize recall.

### Why?
Because the CRO clearly says that missing a high-revenue hospital account is unacceptable, even if the company ends up contacting many pharmacies that were never actually going to churn. This means catching as many real churn cases as possible is more important than avoiding extra outreach.

### What business trade-off does this represent?
This represents a trade-off where the company accepts more false positives in order to reduce false negatives. In simple terms, Pharmalink is okay with making some unnecessary calls if that helps prevent losing valuable pharmacy accounts.

----------------------------------------------------------------------------------------------------------------------------------------

4. Advanced Thinking

Tier-3 stores do not behave the same way as Tier-1 stores, so using one simple rule for everyone may not work well. Since Tier-2 and Tier-3 pharmacies place larger but less frequent bulk orders, a 30-day gap may not always mean true churn for them.

My first approach would be to train one global model but include city_tier as an important feature and also add interaction features such as:
-  city_tier × order_frequency_60d
-  city_tier × avg_order_value
-  city_tier × days_since_last_order

This helps the model learn that the same inactivity period can mean different things in different city tiers.

After that, I would evaluate the model separately for Tier-1, Tier-2, and Tier-3 pharmacies. If the performance is weak for Tier-3 stores, then I would consider training separate models by city tier. I think this is a practical approach because it starts simple, but still leaves room for a more specialized model if needed.

---------------------------------------------------------------------------------------------------------------------------------------

## Final Note
This is a supervised binary classification problem focused on churn prediction. The most important business point is that Pharmalink should optimize for recall, because losing a major account is costlier than spending extra effort on false alarms.
