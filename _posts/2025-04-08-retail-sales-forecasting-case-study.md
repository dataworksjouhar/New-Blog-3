---
layout: post
title: "Case Study: Retail Sales Forecasting with Machine Learning"
date: 2025-04-08 15:45:00 +0000
categories: [Projects & Case Studies]
tags: [machine learning, forecasting, retail, python, data science]
---

In this case study, I'll walk through a recent project where I implemented a machine learning solution to improve sales forecasting for a mid-sized retail chain. This project demonstrates how data science can deliver tangible business value when applied to real-world problems.

## Project Background

### The Client Challenge

A retail chain with 50+ locations was struggling with inventory management. Their existing forecasting system, based on simple historical averages, resulted in:

- Frequent stockouts of popular items
- Excess inventory of slow-moving products
- Significant manual adjustment by store managers
- Inability to effectively plan for seasonal variations

The company estimated these inefficiencies were costing them approximately $2.5 million annually in lost sales, markdowns, and operational inefficiency.

### Project Objectives

The primary goals for this project were to:

1. Reduce forecast error by at least 20% compared to the existing system
2. Create store-level and product-level forecasts for 8 weeks ahead
3. Account for seasonality, promotions, and external factors
4. Develop an automated system requiring minimal manual intervention
5. Provide interpretable results to build trust with business users

## Data Exploration and Preparation

### Available Data

We had access to three years of historical data, including:

- Daily sales by product and store
- Pricing and promotion history
- Store characteristics (size, location, opening hours)
- Product attributes (category, brand, price point)
- Local weather data
- Local events calendar

### Initial Exploration

Our exploratory analysis revealed several interesting patterns:

```python
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# Load the data
sales_data = pd.read_csv('retail_sales.csv')
store_data = pd.read_csv('store_info.csv')
product_data = pd.read_csv('product_info.csv')

# Merge datasets
full_data = sales_data.merge(store_data, on='store_id').merge(product_data, on='product_id')

# Examine weekly patterns
weekly_sales = full_data.groupby(['store_id', pd.Grouper(key='date', freq='W')])['sales'].sum().reset_index()
plt.figure(figsize=(12, 6))
for store in weekly_sales['store_id'].unique()[:5]:  # Plot first 5 stores
    store_sales = weekly_sales[weekly_sales['store_id'] == store]
    plt.plot(store_sales['date'], store_sales['sales'], label=f'Store {store}')
plt.legend()
plt.title('Weekly Sales Patterns by Store')
plt.xlabel('Date')
plt.ylabel('Weekly Sales ($)')
plt.show()
```

Key findings from our exploration:

1. **Strong weekly seasonality**: Sales consistently peaked on weekends
2. **Annual seasonality**: Clear holiday patterns with peaks in December and summer
3. **Promotion impact**: 2.7x average sales lift during promotions
4. **Weather sensitivity**: Significant correlation between weather and certain product categories
5. **Store variations**: Urban stores showed different patterns than suburban locations

### Data Preparation Challenges

We encountered several data quality issues:

- **Missing values**: 3% of daily sales records were missing
- **Outliers**: Promotional periods created extreme values
- **Inconsistent categorization**: Product categories had changed over time
- **Granularity mismatches**: Weather data was hourly while sales were daily

To address these challenges, we:

1. Imputed missing values using store-product averages for similar days
2. Created a robust outlier detection system using IQR method
3. Standardized product categories through mapping tables
4. Aggregated weather data to daily summaries with min, max, and average values

## Modeling Approach

### Feature Engineering

Feature engineering was critical to the project's success. We created:

| Feature Type | Examples | Rationale |
|--------------|----------|-----------|
| Temporal | Day of week, month, holiday flags | Capture seasonality patterns |
| Lagged Variables | Sales from 1, 7, 14, 28 days prior | Incorporate recent trends |
| Rolling Statistics | 7-day, 28-day moving averages | Smooth out noise |
| Promotion Features | Current promotions, days since last promotion | Capture promotion effects |
| Store Attributes | Location type, size, years open | Account for store differences |
| Product Features | Category, price tier, lifecycle stage | Product-specific patterns |
| External Factors | Temperature, precipitation, local events | External influences |

### Model Selection

We evaluated several forecasting approaches:

1. **Statistical methods**:
   - ARIMA
   - Exponential Smoothing
   - Prophet

2. **Machine learning approaches**:
   - Random Forest
   - Gradient Boosting (XGBoost)
   - Deep Learning (LSTM networks)

3. **Hybrid approaches**:
   - Combined statistical and ML models

After testing on a validation dataset, we selected a gradient boosting approach using XGBoost as our primary model, with the following advantages:

- Handled non-linear relationships effectively
- Captured complex interactions between features
- Provided feature importance for interpretability
- Performed well with relatively small training data
- Required less computational resources than deep learning approaches

### Implementation Details

The final model architecture:

```python
import xgboost as xgb
from sklearn.model_selection import TimeSeriesSplit
from sklearn.metrics import mean_absolute_percentage_error

# Define features and target
X = prepared_data.drop(['sales', 'date'], axis=1)
y = prepared_data['sales']

# Time series cross-validation
tscv = TimeSeriesSplit(n_splits=5)

# XGBoost parameters
params = {
    'objective': 'reg:squarederror',
    'max_depth': 8,
    'learning_rate': 0.05,
    'subsample': 0.8,
    'colsample_bytree': 0.8,
    'n_estimators': 500,
    'early_stopping_rounds': 50
}

# Train model with cross-validation
model = xgb.XGBRegressor(**params)
for train_idx, val_idx in tscv.split(X):
    X_train, X_val = X.iloc[train_idx], X.iloc[val_idx]
    y_train, y_val = y.iloc[train_idx], y.iloc[val_idx]
    
    model.fit(
        X_train, y_train,
        eval_set=[(X_val, y_val)],
        verbose=100
    )
    
    # Evaluate performance
    val_preds = model.predict(X_val)
    mape = mean_absolute_percentage_error(y_val, val_preds)
    print(f"Validation MAPE: {mape:.2%}")
```

## Results and Business Impact

### Forecast Accuracy

The new forecasting system achieved:

- 32% reduction in overall forecast error (MAPE)
- 41% improvement for promotional periods
- 27% improvement for seasonal peaks
- Consistent performance across all store types

### Business Outcomes

After six months of implementation, the client reported:

- 18% reduction in stockouts
- 22% decrease in excess inventory
- $1.8 million in annualized savings
- 15 hours per week saved in manual forecast adjustments per store

### Visualization of Results

We created dashboards to help business users understand and trust the forecasts:

> "The ability to see not just what the system predicts, but why it's making those predictions, has been transformative for our planning process." — Client Merchandise Director

## Challenges and Lessons Learned

### Technical Challenges

1. **Computational efficiency**: Generating forecasts for 50,000+ store-product combinations required optimization
   - Solution: Implemented parallel processing and hierarchical forecasting

2. **Model drift**: Forecast accuracy degraded over time
   - Solution: Implemented automated retraining and monitoring

3. **Rare events**: Difficulty predicting impact of unusual events (e.g., store remodels)
   - Solution: Created a separate model for special events with transfer learning

### Process Challenges

1. **Data access**: Initial difficulty obtaining all relevant data sources
   - Solution: Created a data requirements document and staged implementation

2. **Stakeholder alignment**: Different departments had different forecasting needs
   - Solution: Conducted workshops to align on priorities and metrics

3. **Change management**: Store managers initially skeptical of automated forecasts
   - Solution: Developed a phased rollout with side-by-side comparison period

## Implementation Architecture

The final system architecture included:

1. **Data pipeline**: Automated ETL process for daily data updates
2. **Model training subsystem**: Weekly retraining with performance monitoring
3. **Forecast generation engine**: Daily forecasts with 8-week horizon
4. **API layer**: Integration with inventory management system
5. **Visualization dashboard**: Business-friendly interface for forecast review

## Future Enhancements

Based on initial success, we identified several opportunities for future enhancement:

1. **Granular forecasting**: Moving from daily to hourly forecasts for labor planning
2. **External data expansion**: Incorporating social media trends and competitor pricing
3. **Demand planning integration**: Connecting forecasts to automated purchasing
4. **Prescriptive analytics**: Adding recommendation engine for optimal inventory levels

## Conclusion

This project demonstrates how modern machine learning approaches can solve traditional business problems with significant ROI. The keys to success were:

1. **Business-first approach**: Starting with clear business objectives
2. **Data foundation**: Investing in data preparation and feature engineering
3. **Model selection**: Choosing the right approach for the specific problem
4. **Interpretability**: Ensuring business users understand and trust the results
5. **Continuous improvement**: Building a system that learns and adapts over time

For organizations considering similar projects, I recommend starting with a focused pilot in one business area, demonstrating value, and then expanding. The technical complexity is often less challenging than the organizational change management required for successful adoption.

In future posts, I'll dive deeper into specific aspects of this project, including our approach to feature engineering and the monitoring system we developed to ensure ongoing forecast accuracy.
