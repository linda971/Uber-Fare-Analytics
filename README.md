
# Uber Fares Dataset Analysis: Comprehensive Analytical Report

**Course:** Introduction to Big Data Analytics INSY 8413  
**Instructor:** Eric Maniraguha  
**Student:**  Linda Cynthia Mutembe

**Assignment:** Assignment I - Uber Fares Dataset Analysis using Power BI  

---

## 🧾 Executive Summary

TThis project presents a detailed analysis of Uber trip data using Power BI, uncovering significant trends in rider behavior, temporal patterns, and geographic activity. The insiA
### Key Highlights:
Peak Ride Periods: Demand is highest during weekday evenings (5 PM to 9 PM), with Fridays recording the most rides.

Top Performing Areas: The base B02512 leads in trip volume, with urban pickup zones showing dense activity concentrations.

Seasonal Variations: Trip frequency increases during warmer months, highlighting seasonal shifts in rider activity.

Performance Optimization Opportunities: Analysis indicates room for improvement in base-level performance and resource distribution.

##Strategic Recommendations:
Align driver deployment strategies with peak demand periods to minimize wait times and improve service quality.

Expand services and marketing efforts in high-demand regions to boost engagement and ridership.

Review and adjust pricing strategies during high-traffic hours and locations to enhance profitability.

Reassess underperforming bases for potential optimization or reallocation of resources.

---

##  Introduction

### 1.1 Project Overview
The goal of this project is to perform an in-depth analysis of Uber ride data to extract insights that can help inform operational and strategic decisions. Using Power BI as the primary visualization and analytics tool, this analysis provides comprehensive insights into ride patterns, customer behavior, and performance metrics over time.

## Objectives:
Understand trip distribution over time (daily, weekly, monthly)

Identify peak hours and days for Uber usage

Analyze geographic ride patterns and demand distribution

Examine performance indicators such as total trips, active drivers, and ride duration

Recommend actionable strategies based on insights

#### Secondary
- Detect anomalies and outliers
- Correlate trip distance, fare, and time
- Communicate insights via interactive dashboards

### 1.3 Business Context
Understanding Uber ride data improves:
- **Operational Efficiency**
- **Customer Experience**
- **Pricing Models**
- **Strategic Expansion**

---

## 🔬 2. Methodology

### 2.1 Data Collection

- **Source:** `yasserh/uber-fares-dataset` (Kaggle)
- **Format:** CSV  
- **Download:** KaggleHub API

```python
import kagglehub
import pandas as pd

path = kagglehub.dataset_download("yasserh/uber-fares-dataset")
df = pd.read_csv(path + "/uber.csv")
```
<img width="1486" height="213" alt="image" src="https://github.com/user-attachments/assets/f7754185-c00f-49d3-a044-6c2e68b25169" />

### 2.2 Data Processing Pipeline
📊 Understanding Phase
Column types, nulls, duplicates, outliers, value ranges.

## Cleaning

```python
# Missing values
print("Missing values per column:\n", df.isnull().sum())

# Check duplicates
print("\nNumber of duplicate rows:", df.duplicated().sum())

# Summary statistics
df.describe()
```
<img width="1485" height="409" alt="image" src="https://github.com/user-attachments/assets/3b194a12-aa99-4dda-aab4-7b17e1cf87d9" />


Type optimization and datetime conversion

### Feature Engineering
a. Create Hour, Day, Month, Weekday 
 Encode Peak vs Off-Peak

```python
# Convert pickup_datetime to datetime if not already
df_cleaned['pickup_datetime'] = pd.to_datetime(df_cleaned['pickup_datetime'])

# Extract hour
df_cleaned['hour'] = df_cleaned['pickup_datetime'].dt.hour

# Average fare by hour of day
hourly_fare = df_cleaned.groupby('hour')['fare_amount'].mean()

# Line plot
plt.figure(figsize=(10, 5))
sns.lineplot(x=hourly_fare.index, y=hourly_fare.values)
plt.title('Average Fare by Hour of Day')
plt.xlabel('Hour of Day')
plt.ylabel('Average Fare Amount ($)')
plt.xticks(range(0, 24))
plt.grid(True)
plt.show()

```
<img width="842" height="470" alt="image" src="https://github.com/user-attachments/assets/42d4ff89-ed2e-4bd2-be99-9eec0f5bb5fb" />

### 2.3 Analytical Techniques
Descriptive Stats: Mean, median, IQR, skewness

EDA: Uni/bivariate/multivariate analysis

Visualization: Box plots, histograms, time series, spatial heatmaps

### 2.4 Tools Used
Data: Python (Pandas, NumPy), Seaborn/Matplotlib
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/3cf2d801-39e1-4c2f-941e-66395c87a3da" />

BI: Power BI, DAX, Power Query
<img width="876" height="470" alt="download (1)" src="https://github.com/user-attachments/assets/ced62707-699c-4baf-b796-7f903b69ee04" />

<img width="799" height="239" alt="image" src="https://github.com/user-attachments/assets/16be0e64-a018-43c0-b6db-9340a864fe48" />

📍 Screenshot of Box plot of fares
### 3.4 Correlation Matrix

```python
# Correlation matrix
corr_matrix = df_cleaned[['fare_amount', 'distance_km', 'hour']].corr()

# Heatmap
plt.figure(figsize=(6, 4))
sns.heatmap(corr_matrix, annot=True, cmap='Blues')
plt.title('Correlation Matrix')
plt.show()
```
<img width="486" height="374" alt="image" src="https://github.com/user-attachments/assets/8a7b3d76-a1b1-42ee-bf56-eafdc2830078" />

```python
import numpy as np

# Function to compute distance between lat/lon points
def haversine(lat1, lon1, lat2, lon2):
    R = 6371  # Earth radius in km
    phi1 = np.radians(lat1)
    phi2 = np.radians(lat2)
    delta_phi = np.radians(lat2 - lat1)
    delta_lambda = np.radians(lon2 - lon1)
    
    a = np.sin(delta_phi / 2.0)**2 + \
        np.cos(phi1) * np.cos(phi2) * np.sin(delta_lambda / 2.0)**2
    c = 2 * np.arcsin(np.sqrt(a))
    return R * c

# Add a distance column
df_cleaned['distance_km'] = haversine(
    df_cleaned['pickup_latitude'],
    df_cleaned['pickup_longitude'],
    df_cleaned['dropoff_latitude'],
    df_cleaned['dropoff_longitude']
)

# Scatter plot: Fare vs Distance
plt.figure(figsize=(10, 5))
sns.scatterplot(data=df_cleaned, x='distance_km', y='fare_amount', alpha=0.5)
plt.title('Fare Amount vs. Distance')
plt.xlabel('Distance (km)')
plt.ylabel('Fare Amount ($)')
plt.xlim(0, 50)  # Optional: remove extreme outliers
plt.ylim(0, 100)
plt.show() 
```
<img width="859" height="470" alt="image" src="https://github.com/user-attachments/assets/1f2dcde8-3b03-4c87-aba2-46f6932f0e57" />



### 3.5 Temporal Patterns
Morning Rush: 7-9 AM, contributes % of total trips

Late Night: Average fare increases 

Day-of-Week Patterns: Weekends show % higher demand

<img width="842" height="470" alt="image" src="https://github.com/user-attachments/assets/bf2f94a8-36c7-4676-b436-e925c9f3cf93" />

📍 Screenshot of  Time series chart


## 📌 4. Results
4.1 Discoveries
🧾 Fare Insights
Dynamic pricing opportunities during rush hours

Fare variation linked to location and time

🚗 Operational Opportunities
Increase driver density during [7-9 AM, 5-7 PM]

Optimize dispatch around business zones

### 4.2 Customer Patterns
Price-sensitive users: % show avoidance during peak fares

Distance preference:  miles average ride

### 4.3 Hypothesis Testing
Time-based Fare Differences

Geo-Fare Variability: Differences significant across districts

🧠 5. Conclusion
### 5.1 Summary of Key Insights
Fare and distance highly correlated

Peak demand times align with commuter schedules

Urban cores dominate high-fare activity

### 5.2 Objectives Achieved 
Objective	Status
Fare Distribution Analysis	
Temporal Pattern Identification	
Geographic Distribution Insights	
Predictive Recommendations	

### 5.3 Limitations
Single-city dataset

Missing weather/event contextual data

Assumed constant pricing logic

## 📈 6. Recommendations
6.1 Pricing Strategy
Increase fares by % during 7–9 AM, 5–7 PM

Introduce location-based pricing premiums

### 6.2 Operational Improvements
Add drivers during morning/evening rush

Implement predictive dispatch based on demand heatmaps


