---
layout: post
search_exclude: true
show_reading_time: false
permalink: /datascience
title: Data Science & Machine Learning (Historical Fire Data Case Study)
categories: [Documentation]
---

# Data Science & Machine Learning (Historical Fire Data Dashboard & Machine Learning Case Study)

## Table of Contents
1. [Overview](#overview)

---

## Overview

Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nam eu quam libero. Lorem ipsum dolor sit amet, consectetur adipiscing elit. Fusce neque dolor, vestibulum quis molestie sed, eleifend in erat. Suspendisse eget justo euismod, efficitur tellus eu, viverra nibh. Aliquam eget ipsum ullamcorper, lacinia justo nec, vestibulum nisi. In maximus odio sed tempus suscipit. Sed feugiat tempor faucibus. Ut ex ligula, consequat ut volutpat tristique, feugiat sed sapien. Vestibulum ante ipsum primis in faucibus orci luctus et ultrices posuere cubilia curae; Class aptent taciti sociosqu ad litora torquent per conubia nostra, per inceptos himenaeos. Donec porta, felis nec volutpat auctor, justo lacus iaculis eros, sed consequat ex massa non dui. Maecenas iaculis volutpat placerat.

---

# Comprehensive Fire Data Science Guide: Advanced Analytics and Machine Learning

## Table of Contents

1. [Introduction](#introduction)
2. [Core Architectures and Methodologies](#core-architectures-and-methodologies)
   - 2.1 [Time Series Analysis with Prophet](#time-series-analysis-with-prophet)
   - 2.2 [Polynomial Regression Analysis](#polynomial-regression-analysis)
   - 2.3 [Advanced Clustering with HDBSCAN](#advanced-clustering-with-hdbscan)
3. [General Pipeline and Process](#general-pipeline-and-process)
   - 3.1 [Data Loading and Preprocessing](#data-loading-and-preprocessing)
   - 3.2 [Feature Engineering](#feature-engineering)
   - 3.3 [Model Training and Validation](#model-training-and-validation)
   - 3.4 [Visualization and Interpretation](#visualization-and-interpretation)
4. [Mathematical Foundations](#mathematical-foundations)
   - 4.1 [Prophet Model Components](#prophet-model-components)
   - 4.2 [Polynomial Features and Regression](#polynomial-features-and-regression)
   - 4.3 [HDBSCAN Clustering Theory](#hdbscan-clustering-theory)
5. [Implementation Details](#implementation-details)
   - 5.1 [Singleton Pattern Architecture](#singleton-pattern-architecture)
   - 5.2 [Data Pipeline Implementation](#data-pipeline-implementation)
   - 5.3 [Visualization Pipeline](#visualization-pipeline)
6. [Code Deep Dive](#code-deep-dive)
   - 6.1 [Prophet Implementation](#prophet-implementation)
   - 6.2 [Polynomial Regression Implementation](#polynomial-regression-implementation)
   - 6.3 [HDBSCAN Clustering Implementation](#hdbscan-clustering-implementation)
7. [Future Extensions](#future-extensions)
8. [Common Mistakes and Best Practices](#common-mistakes-and-best-practices)
9. [Performance Optimization](#performance-optimization)
10. [Conclusion](#conclusion)

---

## 1. Introduction

Fire data analysis represents a critical application of data science in environmental monitoring and disaster management. This comprehensive guide explores three sophisticated analytical approaches implemented in Python for analyzing wildfire patterns: **Prophet-based time series forecasting**, **polynomial regression analysis**, and **HDBSCAN clustering**. Each method addresses different aspects of fire data analysis, from temporal pattern prediction to spatial clustering and trend analysis.

The implementation follows enterprise-level design patterns, incorporating singleton architecture, comprehensive error handling, and modular design principles. This guide will walk you through the theoretical foundations, practical implementations, and real-world applications of these techniques.

---

## 2. Core Architectures and Methodologies

### 2.1 Time Series Analysis with Prophet

**Prophet** is Facebook's time series forecasting tool designed to handle seasonal patterns, holidays, and missing data robustly. In our fire data context, Prophet excels at:

- **Seasonal Pattern Detection**: Identifying yearly, monthly, and daily fire occurrence patterns
- **Trend Analysis**: Understanding long-term changes in fire frequency
- **Forecasting**: Predicting future fire occurrences based on historical patterns
- **Anomaly Detection**: Identifying unusual fire activity periods

**Key Components:**
- **Trend Component**: Captures non-periodic changes
- **Seasonal Components**: Models yearly and weekly seasonality
- **Holiday Effects**: Accounts for special events (configurable)
- **Error Term**: Handles noise and irregular fluctuations

### 2.2 Polynomial Regression Analysis

Polynomial regression extends linear regression by adding polynomial terms, allowing the model to capture non-linear relationships. For fire data analysis, this approach provides:

- **Non-linear Trend Modeling**: Capturing complex temporal patterns
- **Flexible Curve Fitting**: Adapting to various data distributions
- **Mathematical Interpretability**: Clear coefficient interpretation
- **Extrapolation Capabilities**: Extending trends into the future

**Mathematical Foundation:**
```
y = β₀ + β₁x + β₂x² + β₃x³ + ... + βₙxⁿ + ε
```

Where:
- `y` = Fire count (dependent variable)
- `x` = Time variable (independent variable)
- `βᵢ` = Polynomial coefficients
- `n` = Polynomial degree
- `ε` = Error term

### 2.3 Advanced Clustering with HDBSCAN

**HDBSCAN** (Hierarchical Density-Based Spatial Clustering of Applications with Noise) represents a sophisticated clustering algorithm that:

- **Handles Noise**: Automatically identifies outliers
- **Variable Density**: Works with clusters of different densities
- **Hierarchical Structure**: Builds cluster hierarchies
- **Parameter Robustness**: Less sensitive to parameter choices than DBSCAN

**Core Concepts:**
- **Core Distance**: Distance to kth nearest neighbor
- **Mutual Reachability Distance**: Symmetric distance measure
- **Minimum Spanning Tree**: Graph-based cluster detection
- **Cluster Stability**: Measures cluster persistence across scales

---

## 3. General Pipeline and Process

### 3.1 Data Loading and Preprocessing

The data pipeline begins with robust loading and preprocessing:

```python
def load_data(self, file_path="fire_archive.csv"):
    """Load and preprocess the fire data"""
    try:
        self.data = pd.read_csv(file_path, parse_dates=['acq_date'])
        # Filter data to reasonable range
        self.data = self.data[(self.data['acq_date'].dt.year >= 2015) & 
                             (self.data['acq_date'].dt.year <= 2022)]
        
        # Add additional time features
        self.data['year'] = self.data['acq_date'].dt.year
        self.data['month'] = self.data['acq_date'].dt.month
        self.data['day_of_year'] = self.data['acq_date'].dt.dayofyear
        self.data['year_month'] = self.data['acq_date'].dt.to_period('M')
        
        return {"status": "success", "message": "Data loaded successfully"}
    except Exception as e:
        return {"status": "error", "message": str(e)}
```

**Key Preprocessing Steps:**
1. **Date Parsing**: Converting string dates to datetime objects
2. **Data Filtering**: Removing outliers and invalid entries
3. **Feature Engineering**: Creating temporal and spatial features
4. **Missing Value Handling**: Implementing appropriate imputation strategies

### 3.2 Feature Engineering

Feature engineering transforms raw data into meaningful inputs for machine learning models:

**Temporal Features:**
- `year`, `month`, `day_of_year`: Capturing different time scales
- `year_month`: Monthly aggregation periods
- `timestamp`: Unix timestamp for regression models

**Spatial Features:**
- `latitude`, `longitude`: Geographic coordinates
- Spatial binning (optional): Grid-based location grouping

**Fire-specific Features:**
- `brightness`: Satellite-detected fire intensity
- `frp`: Fire Radiative Power
- `daynight`: Day/night occurrence indicator

### 3.3 Model Training and Validation

Each model follows a structured training approach:

**Prophet Training Pipeline:**
```python
# Prepare data for Prophet
daily_fires = filtered_data.groupby('acq_date').size().reset_index(name='fire_count')
daily_fires['ds'] = daily_fires['acq_date']  # Prophet requires 'ds' column
daily_fires['y'] = daily_fires['fire_count']   # Prophet requires 'y' column

# Train Prophet model
model = Prophet(yearly_seasonality=True, daily_seasonality=(month is not None))
model.fit(daily_fires[['ds', 'y']])

# Generate forecast
future = model.make_future_dataframe(periods=periods, freq=freq)
forecast = model.predict(future)
```

**Polynomial Regression Pipeline:**
```python
# Prepare features and target
X = fires_per_month['timestamp'].values.reshape(-1, 1)
y = fires_per_month['fire_count'].values

# Train polynomial model
poly_model = make_pipeline(PolynomialFeatures(degree=degree), LinearRegression())
poly_model.fit(X, y)

# Generate predictions
fires_per_month['poly_pred'] = poly_model.predict(X)
```

**HDBSCAN Clustering Pipeline:**
```python
# Normalize features
features = ['latitude', 'longitude', 'brightness', 'frp', 'month', 'hour', 'is_day']
X_scaled = self.scaler.fit_transform(self.clustered_data[features])

# Perform HDBSCAN clustering
self.clusterer = hdbscan.HDBSCAN(
    min_cluster_size=min_cluster_size,
    min_samples=min_samples
)
clusters = self.clusterer.fit_predict(X_scaled)
```

### 3.4 Visualization and Interpretation

Comprehensive visualization pipeline includes:

1. **Time Series Plots**: Trends, forecasts, and components
2. **Spatial Maps**: Geographic distribution and clusters
3. **Statistical Distributions**: Feature distributions by cluster
4. **Model Diagnostics**: Residuals, coefficients, and performance metrics

---

## 4. Mathematical Foundations

### 4.1 Prophet Model Components

Prophet decomposes time series data into interpretable components:

**Additive Model:**
```
y(t) = g(t) + s(t) + h(t) + εₜ
```

Where:
- `g(t)`: Trend function modeling non-periodic changes
- `s(t)`: Seasonal components (yearly, weekly, daily)
- `h(t)`: Holiday effects
- `εₜ`: Error term

**Trend Function Options:**

*Linear Trend:*
```
g(t) = (k + a(t)ᵀδ) × t + (m + a(t)ᵀγ)
```

*Logistic Growth Trend:*
```
g(t) = C(t) / (1 + exp(-(k + a(t)ᵀδ)(t - (m + a(t)ᵀγ))))
```

**Seasonal Component:**
Prophet uses Fourier series to model seasonality:
```
s(t) = Σₙ₌₁ᴺ (aₙcos(2πnt/P) + bₙsin(2πnt/P))
```

Where:
- `P`: Period of seasonality (365.25 for yearly)
- `N`: Number of Fourier terms
- `aₙ, bₙ`: Fourier coefficients

### 4.2 Polynomial Features and Regression

**Polynomial Feature Generation:**
For degree `d`, features are generated as:
```
φ(x) = [1, x, x², x³, ..., xᵈ]
```

**Extended Feature Matrix:**
For multivariate polynomial regression:
```
Φ(X) = [φ(x₁), φ(x₂), ..., φ(xₘ)]ᵀ
```

**Normal Equation Solution:**
```
β = (ΦᵀΦ)⁻¹Φᵀy
```

**Regularization (Optional):**
Ridge regression adds L2 penalty:
```
β = (ΦᵀΦ + λI)⁻¹Φᵀy
```

**Model Evaluation Metrics:**

*Mean Squared Error:*
```
MSE = (1/n)Σᵢ₌₁ⁿ(yᵢ - ŷᵢ)²
```

*R-squared:*
```
R² = 1 - (SS_res/SS_tot)
```

Where:
- `SS_res = Σ(yᵢ - ŷᵢ)²` (Residual sum of squares)
- `SS_tot = Σ(yᵢ - ȳ)²` (Total sum of squares)

### 4.3 HDBSCAN Clustering Theory

**Core Distance:**
```
core_k(x) = distance to kth nearest neighbor of x
```

**Mutual Reachability Distance:**
```
d_mreach-k(x,y) = max{core_k(x), core_k(y), d(x,y)}
```

**Minimum Spanning Tree:**
HDBSCAN builds MST using mutual reachability distances, then extracts cluster hierarchy.

**Cluster Stability:**
For each cluster C and threshold ε:
```
stability(C) = Σₓ∈C (λ(x) - λ_birth(C))
```

Where `λ(x) = 1/ε` is the density level.

---

## 5. Implementation Details

### 5.1 Singleton Pattern Architecture

All three models implement the Singleton pattern to ensure single instance management:

```python
class FireDataAnalysisAdvancedRegressionModel:
    _instance = None

    @staticmethod
    def get_instance():
        if FireDataAnalysisAdvancedRegressionModel._instance is None:
            FireDataAnalysisAdvancedRegressionModel()
        return FireDataAnalysisAdvancedRegressionModel._instance

    def __init__(self):
        if FireDataAnalysisAdvancedRegressionModel._instance is not None:
            raise Exception("This class is a singleton!")
        else:
            FireDataAnalysisAdvancedRegressionModel._instance = self
            self.data = None
            self.prophet_model = None
            self.clustering_model = None
            self.scaler = StandardScaler()
```

**Benefits:**
- **Memory Efficiency**: Single instance per model type
- **State Consistency**: Maintains model state across method calls
- **Resource Management**: Prevents multiple expensive model initializations

### 5.2 Data Pipeline Implementation

**Flexible Filtering System:**
```python
def filter_data_by_period(self, year=None, month=None):
    """Filter data by specific year and/or month"""
    if self.data is None:
        return None
        
    filtered_data = self.data.copy()
    
    if year is not None:
        filtered_data = filtered_data[filtered_data['year'] == year]
    
    if month is not None:
        filtered_data = filtered_data[filtered_data['month'] == month]
        
    return filtered_data
```

**Robust Error Handling:**
```python
try:
    # Model operations
    result = self.perform_analysis()
    return {"status": "success", "data": result}
except Exception as e:
    return {"status": "error", "message": str(e)}
```

### 5.3 Visualization Pipeline

**Base64 Image Encoding:**
```python
def _fig_to_base64(self, fig):
    """Convert matplotlib figure to base64 string"""
    buffer = io.BytesIO()
    fig.savefig(buffer, format='png', dpi=150, bbox_inches='tight', facecolor='white')
    buffer.seek(0)
    image_base64 = base64.b64encode(buffer.getvalue()).decode('utf-8')
    buffer.close()
    return f"data:image/png;base64,{image_base64}"
```

This approach enables:
- **Web Integration**: Direct embedding in HTML
- **API Compatibility**: JSON-serializable image data
- **High Quality**: Configurable DPI and formatting

---

## 6. Code Deep Dive

### 6.1 Prophet Implementation

**Time Series Data Preparation:**
```python
def generate_time_series_analysis(self, year=None, month=None):
    try:
        filtered_data = self.filter_data_by_period(year, month)
        if filtered_data is None or len(filtered_data) == 0:
            return {"error": "No data available for the specified period"}

        # Adaptive aggregation based on time scope
        if month is not None:
            # Daily aggregation for specific month
            daily_fires = filtered_data.groupby('acq_date').size().reset_index(name='fire_count')
            daily_fires['ds'] = daily_fires['acq_date']
            daily_fires['y'] = daily_fires['fire_count']
            freq = 'D'
            periods = 30  # Forecast 30 days
        else:
            # Monthly aggregation for broader analysis
            monthly_fires = filtered_data.groupby('year_month').size().reset_index(name='fire_count')
            monthly_fires['ds'] = pd.to_datetime(monthly_fires['year_month'].astype(str))
            monthly_fires['y'] = monthly_fires['fire_count']
            daily_fires = monthly_fires
            freq = 'M'
            periods = 12  # Forecast 12 months
```

**Model Configuration and Training:**
```python
        # Train Prophet model with adaptive seasonality
        model = Prophet(
            yearly_seasonality=True, 
            daily_seasonality=(month is not None),
            changepoint_prior_scale=0.05,  # Adjust trend flexibility
            seasonality_prior_scale=10.0   # Adjust seasonality strength
        )
        model.fit(daily_fires[['ds', 'y']])
        
        # Generate forecast
        future = model.make_future_dataframe(periods=periods, freq=freq)
        forecast = model.predict(future)
```

**Comprehensive Visualization:**
```python
        # Forecast plot
        fig1, ax1 = plt.subplots(figsize=(12, 6))
        model.plot(forecast, ax=ax1)
        ax1.set_title(f"Fire Count Forecast - {year or 'All Years'} {month or 'All Months'}")
        ax1.set_xlabel("Date")
        ax1.set_ylabel("Fire Count")
        plots['forecast'] = self._fig_to_base64(fig1)
        plt.close(fig1)
        
        # Components plot
        fig2 = model.plot_components(forecast)
        fig2.suptitle(f"Forecast Components - {year or 'All Years'} {month or 'All Months'}")
        plots['components'] = self._fig_to_base64(fig2)
        plt.close(fig2)
```

### 6.2 Polynomial Regression Implementation

**Feature Engineering for Regression:**
```python
def train_polynomial_model(self, filtered_data, degree=4):
    """Train polynomial regression model on filtered data"""
    try:
        from sklearn.linear_model import LinearRegression
        from sklearn.preprocessing import PolynomialFeatures
        from sklearn.pipeline import make_pipeline
        
        # Aggregate data by month
        fires_per_month = filtered_data.groupby('year_month').size().reset_index(name='fire_count')
        fires_per_month['year_month_str'] = fires_per_month['year_month'].astype(str)
        
        # Convert to timestamp for regression
        fires_per_month['timestamp'] = pd.to_datetime(fires_per_month['year_month_str']).astype(int) / 10**9
        
        # Prepare features and target
        X = fires_per_month['timestamp'].values.reshape(-1, 1)
        y = fires_per_month['fire_count'].values
        
        # Train polynomial model
        poly_model = make_pipeline(PolynomialFeatures(degree=degree), LinearRegression())
        poly_model.fit(X, y)
        
        # Generate predictions
        fires_per_month['poly_pred'] = poly_model.predict(X)
        
        return {
            "model": poly_model,
            "data": fires_per_month,
            "X": X,
            "y": y
        }
```

**Degree Comparison Analysis:**
```python
def compare_polynomial_degrees(self, year=None, month=None, degrees=[2, 3, 4, 5]):
    """Compare different polynomial degrees"""
    try:
        filtered_data = self.filter_data_by_period(year, month)
        if filtered_data is None or len(filtered_data) == 0:
            return {"error": "No data available for the specified period"}

        from sklearn.metrics import mean_squared_error, r2_score
        
        comparison_results = []
        
        for degree in degrees:
            model_result = self.train_polynomial_model(filtered_data, degree)
            if "error" not in model_result:
                fires_per_month = model_result["data"]
                y_true = model_result["y"]
                y_pred = fires_per_month['poly_pred']
                
                mse = mean_squared_error(y_true, y_pred)
                r2 = r2_score(y_true, y_pred)
                
                comparison_results.append({
                    "degree": degree,
                    "mse": float(mse),
                    "r2_score": float(r2),
                    "avg_prediction": float(y_pred.mean())
                })
```

### 6.3 HDBSCAN Clustering Implementation

**Data Preparation for Clustering:**
```python
def prepare_clustering_data(self, sample_size=10000, random_state=42):
    """Prepare and clean data for clustering"""
    try:
        if self.data is None:
            return {"status": "error", "message": "Data not loaded"}
        
        # Feature selection
        features = ['latitude', 'longitude', 'brightness', 'frp', 'month', 'hour', 'is_day']
        
        # Check if all required features exist
        missing_features = [f for f in features if f not in self.data.columns]
        if missing_features:
            return {"status": "error", "message": f"Missing features: {missing_features}"}
        
        # Clean and sample data
        df_clean = self.data[features].dropna()
        
        if len(df_clean) == 0:
            return {"status": "error", "message": "No valid data after cleaning"}
        
        # Sample data if it's larger than sample_size
        if len(df_clean) > sample_size:
            df_clean = df_clean.sample(sample_size, random_state=random_state)
        
        self.clustered_data = df_clean.copy()
```

**HDBSCAN Clustering Execution:**
```python
def perform_clustering(self, min_cluster_size=20, min_samples=None):
    """Perform HDBSCAN clustering"""
    try:
        if self.clustered_data is None:
            prep_result = self.prepare_clustering_data()
            if prep_result["status"] == "error":
                return prep_result
        
        # Normalize features
        features = ['latitude', 'longitude', 'brightness', 'frp', 'month', 'hour', 'is_day']
        X_scaled = self.scaler.fit_transform(self.clustered_data[features])
        
        # Perform HDBSCAN clustering
        self.clusterer = hdbscan.HDBSCAN(
            min_cluster_size=min_cluster_size,
            min_samples=min_samples,
            metric='euclidean',
            cluster_selection_method='eom'  # Excess of Mass
        )
        clusters = self.clusterer.fit_predict(X_scaled)
        
        # Add cluster labels to data
        self.clustered_data['cluster'] = clusters
        
        # Calculate cluster statistics
        unique_clusters = np.unique(clusters)
        n_clusters = len(unique_clusters[unique_clusters != -1])  # Exclude noise (-1)
        n_noise = np.sum(clusters == -1)
```

**t-SNE Dimensionality Reduction:**
```python
def generate_tsne_projection(self, perplexity=50, random_state=42):
    """Generate t-SNE projection of clustered data"""
    try:
        if self.clustered_data is None or 'cluster' not in self.clustered_data.columns:
            return {"status": "error", "message": "Clustering must be performed first"}
        
        # Prepare scaled data for t-SNE
        features = ['latitude', 'longitude', 'brightness', 'frp', 'month', 'hour', 'is_day']
        X_scaled = self.scaler.transform(self.clustered_data[features])
        
        # Perform t-SNE
        self.tsne_model = TSNE(
            n_components=2, 
            random_state=random_state, 
            perplexity=perplexity,
            learning_rate=200,
            n_iter=1000
        )
        tsne_results = self.tsne_model.fit_transform(X_scaled)
        
        # Add t-SNE results to data
        self.clustered_data['tsne-1'] = tsne_results[:, 0]
        self.clustered_data['tsne-2'] = tsne_results[:, 1]
```

---

