# B1. Problem Formulation


## B1(a) ML Problem

The objective is to predict the number of items sold for each store under different promotional strategies.  
The target variable is `items_sold`, which represents the sales volume for a given store and time period.  

The input features include store characteristics (such as store size and location type), promotion type, competition density, and temporal variables such as month, day of the week, and whether the date falls near the end of the month.  

This is a supervised machine learning problem, specifically a regression task, as the goal is to predict a continuous numerical outcome (`items_sold`) based on input features.

## B1(b) Why `items_sold` Instead of Revenue?

Using `items_sold` as the target variable is more reliable than total sales revenue because revenue can be influenced by price variations, discounts, and product mix, which may distort the true impact of promotions.  

Items sold directly reflects customer demand and purchasing behaviour, making it a more stable and consistent measure of promotion effectiveness.  

This illustrates the broader principle that the target variable in machine learning should closely align with the underlying objective being analysed, rather than being influenced by external or confounding factors.

## B1(c) Why Not One Global Model?

Using a single global model across all 50 stores may fail to capture the heterogeneity in customer behaviour across different locations.  

An alternative approach would be to use either segmented models (e.g., separate models for urban, semi-urban, and rural stores) or include interaction terms that allow the model to account for differences in how promotions perform across store types.  

This approach ensures that the model captures location-specific patterns and improves prediction accuracy.

---

# B2. Data and EDA Strategy


## B2(a) Joining Tables

The data from transactions, store attributes, promotion details, and the calendar table would be joined using common keys such as `store_id` and `transaction_date`.  

The final modelling dataset would have one row per store per day (or per transaction period), representing aggregated sales activity.  

Before modelling, transactions would be aggregated to compute total items sold per store per time period, along with summary statistics such as average basket size or total transactions. Store attributes and promotion details would then be merged to enrich each record.

## B2(b) EDA Strategy

Several exploratory data analysis techniques would be applied before modelling:

1. A distribution plot of `items_sold` to understand skewness and detect outliers.  
2. A boxplot of `items_sold` across different promotion types to compare effectiveness.  
3. A time-series plot of sales over months to identify seasonal trends.  
4. A correlation heatmap between numerical variables to detect relationships and multicollinearity.  

These analyses would guide feature engineering decisions and model selection.

## B2(c) Imbalance Problem

If 80% of transactions occur without any promotion, the dataset is imbalanced with respect to promotion exposure.  

This could cause the model to learn patterns dominated by non-promotion periods, reducing its ability to accurately estimate the effect of promotions.  

To address this, techniques such as stratified sampling, weighting observations, or building separate models for promotion vs non-promotion periods could be used.

---

# B3. Evaluation and Deployment


## B3(a) Train-Test Split

The dataset should be split based on time, using earlier periods for training and more recent periods for testing.  

A random split is inappropriate because it can introduce data leakage, where future information influences the model during training.  

Evaluation metrics such as RMSE and MAE should be used. RMSE penalises larger errors more heavily, while MAE provides an average magnitude of prediction error.

## B3(b) Explaining Model Decisions

Feature importance can be used to understand why the model recommends different promotions for the same store in different months.  

For example, temporal features such as month and seasonal indicators may have high importance, indicating that customer behaviour changes over time.  

Additionally, features such as competition density and store characteristics may influence promotion effectiveness.  

This demonstrates that promotion performance is context-dependent.

## B3(c) Deployment

The trained model can be saved using tools such as `joblib` or `pickle` for reuse in production.  

At the start of each month, new data would be collected, preprocessed using the same pipeline, and fed into the model to generate predictions for each store.  

To monitor performance, prediction errors should be tracked over time using RMSE and MAE. If performance degrades, the model should be retrained using more recent data.

This ensures the model remains accurate and relevant in a changing business environment.