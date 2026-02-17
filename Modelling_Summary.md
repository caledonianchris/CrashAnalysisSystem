# Crash Analysis System - Modelling Summary

## Project Overview

This project analyzes crash severity prediction using New Zealand Transport Agency (NZTA) Crash Analysis System (CAS) data from Kaggle. The primary objective is to predict whether a crash will result in severe outcomes (fatal or serious injury) versus minor/non-injury outcomes.

## Problem Definition

**Target Variable**: Binary classification predicting crash severity
- **Severe (1)**: Fatal Crash, Serious Crash  
- **Non-Severe (0)**: Minor Crash, Non-Injury Crash

**Business Context**: Understanding factors that contribute to severe crashes can help inform road safety policies, infrastructure improvements, and emergency response planning.

## Dataset Characteristics

- **Source**: NZTA Crash Analysis System via Kaggle
- **Data Type**: Tabular crash records with mixed categorical and numerical features
- **Target Distribution**: Imbalanced classification problem (requires class weighting)

## Feature Engineering

### Feature Categories

**Categorical Features (11 variables)**:
- Environmental: `light`, `weather_a`, `weather_b`, `road_surface`, `road_character`
- Infrastructure: `intersection_midblock`, `junction_type`, `urban`, `region`, `street_light`, `traffic_control`

**Numerical Features (6 variables)**:
- Speed: `speed_limit`, `temporary_speed_limit`
- Injuries: `fatal_count`, `serious_injury_count`, `minor_injury_count` 
- Infrastructure: `number_of_lanes`

**Vehicle Participant Features (9 binary variables)**:
- Vehicle types: `car_station_wagon`, `motorcycle`, `truck`, `bus`, `moped`, `suv`, `van_or_utility`, `bicycle`, `pedestrian`

### Preprocessing Pipeline

1. **Missing Value Treatment**: String nulls converted to actual nulls, then handled appropriately
2. **Categorical Encoding**: One-hot encoding for categorical variables
3. **Numerical Scaling**: StandardScaler for continuous variables
4. **Feature Selection**: Removed redundant columns, focused on predictive features

## Model Development

### Train/Test Split
- **Split Ratio**: 80/20 train/test split
- **Stratification**: Maintained target class distribution across splits
- **Random State**: 42 (for reproducibility)

### Models Implemented

#### 1. Logistic Regression
- **Algorithm**: Regularized logistic regression with balanced class weights
- **Hyperparameters**:
  - `class_weight="balanced"` (handles class imbalance)
  - `C=1.0` (regularization strength)
  - `solver="liblinear"` (suitable for smaller datasets)
  - `max_iter=1000`

#### 2. Support Vector Machine (Linear SVM)
- **Algorithm**: Linear SVM with probability calibration
- **Implementation**: `LinearSVC` wrapped in `CalibratedClassifierCV`
- **Hyperparameters**:
  - `class_weight="balanced"`
  - `C=1.0`
  - `max_iter=5000`
  - `cv=3` (for probability calibration)

#### 3. Naïve Bayes (Bernoulli)
- **Algorithm**: Bernoulli Naïve Bayes
- **Rationale**: Optimal for one-hot encoded binary features
- **Advantages**: 
  - Handles sparse matrices efficiently
  - Fast training and prediction
  - Natural probability calibration
  - No hyperparameter tuning required

## Model Evaluation

### Evaluation Metrics

1. **Accuracy**: Overall classification correctness
2. **Precision**: True positives / (True positives + False positives)
3. **Recall (Sensitivity)**: True positives / (True positives + False negatives)
4. **F1-Score**: Harmonic mean of precision and recall
5. **ROC-AUC**: Area under the receiver operating characteristic curve
6. **Confusion Matrix**: Detailed breakdown of predictions vs. actuals

### Model Performance Comparison

| Model | Accuracy | Precision | Recall | F1-Score | ROC-AUC |
|-------|----------|-----------|--------|----------|---------|
| Logistic Regression | [Value] | [Value] | [Value] | [Value] | [Value] |
| Linear SVM (Calibrated) | [Value] | [Value] | [Value] | [Value] | [Value] |
| Naïve Bayes (Bernoulli) | [Value] | [Value] | [Value] | [Value] | [Value] |

*Note: Actual performance values available in the notebook execution results*

### ROC Curve Analysis
- All models compared using ROC curves
- AUC scores calculated for each model
- Diagonal reference line (random classifier) included for comparison

## Key Insights

### Exploratory Data Analysis Findings
1. **Speed Limits**: Higher speed limits correlate with severe crashes
2. **Lighting Conditions**: Certain lighting conditions increase severity risk
3. **Regional Patterns**: Some regions show higher severe crash rates
4. **Vehicle Types**: Certain participant types (motorcycles, pedestrians) associated with severe outcomes
5. **Injury Distribution**: Minor injuries dominate the dataset (~[percentage]%)

### Model Insights
1. **Feature Importance**: The models identify key risk factors for severe crashes
2. **Class Imbalance**: Balanced class weights help models focus on minority class (severe crashes)
3. **Probability Calibration**: SVM required calibration to provide reliable probability estimates

## Technical Implementation

### Pipeline Architecture
```
Raw Data → Preprocessing → Feature Engineering → Model Training → Evaluation
```

### Preprocessing Pipeline Components
1. **ColumnTransformer**: Separate preprocessing for categorical and numerical features
2. **OneHotEncoder**: Categorical feature encoding with unknown value handling
3. **StandardScaler**: Numerical feature normalization
4. **Pipeline**: End-to-end workflow encapsulation

## Model Deployment Considerations

### Strengths
- **Interpretability**: All models provide explainable predictions
- **Efficiency**: Fast training and prediction times
- **Robustness**: Handles mixed data types and missing values
- **Scalability**: Suitable for real-time crash assessment

### Limitations
- **Class Imbalance**: Severe crashes are minority class (natural data characteristic)
- **Feature Engineering**: Limited to available crash report fields
- **Temporal Effects**: No time-series modeling of seasonal patterns
- **External Factors**: Weather, traffic volume not fully captured

## Recommendations

### Model Selection
1. **Primary Choice**: [Best performing model based on business requirements]
2. **Evaluation Criteria**: Balance between recall (catching severe crashes) and precision (avoiding false alarms)
3. **Deployment**: Consider ensemble methods for production use

### Business Applications
1. **Risk Assessment**: Real-time crash severity prediction for emergency response
2. **Infrastructure Planning**: Identify high-risk road segments for improvement
3. **Policy Development**: Evidence-based road safety interventions
4. **Resource Allocation**: Optimize emergency service deployment

### Future Enhancements
1. **Feature Engineering**: Add temporal, weather, and traffic volume features
2. **Advanced Models**: Experiment with ensemble methods (Random Forest, Gradient Boosting)
3. **Hyperparameter Tuning**: Grid search for optimal model parameters
4. **Cross-Validation**: More robust performance estimation
5. **Real-Time Integration**: Connect with live traffic and emergency systems

---

*Generated on February 3, 2026*  
*Project: Mini Project 2 - Crash Analysis System*  
*Data Source: NZTA Crash Analysis System (Kaggle)*