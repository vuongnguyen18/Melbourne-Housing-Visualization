# Melbourne Housing Market Analysis & Price Prediction

This project analyses Melbourne housing sales data using Power BI and Python machine learning. The dashboard explores market trends, regional and suburb-level price differences, property feature price drivers, and machine learning-based price recommendations.

The final report includes four interactive Power BI pages:

1. **Melbourne Housing Market Overview**
2. **Regional & Suburb Price Insights**
3. **Property Features & Price Drivers**
4. **Price Prediction & Market Recommendations**

---

## 1. Data Cleaning and Transformation

The original Melbourne housing dataset contained **27,114 property sale records** and 16 columns, including property location, property type, physical property features, sale date, and sale price.

Before building the Power BI dashboard, the dataset was cleaned and transformed to improve data quality, support reliable analysis, and prepare the data for machine learning.

### 1.1 Data Quality Issues

Several columns contained missing or invalid values, especially property feature columns.

| Column | Missing / Invalid Records | Percentage of Dataset |
|---|---:|---:|
| BuildingArea | 16,585 | 61.2% |
| YearBuilt | 15,129 | 55.8% |
| Landsize | 9,241 | 34.1% |
| Car | 6,817 | 25.1% |
| Bathroom | 6,442 | 23.8% |
| Bedroom | 6,436 | 23.7% |

The `BuildingArea` column contained invalid text values such as `"missing"` and `"inf"`, which were converted into null values.

In addition, **11 duplicate records** were removed, reducing the dataset from **27,114 rows to 27,103 rows**.

### 1.2 Why Missing Data Was Not Fully Removed

Although some columns had high missing-value rates, the rows were not removed entirely. Removing every row with missing values in fields such as `BuildingArea`, `YearBuilt`, `Landsize`, `Bedroom`, `Bathroom`, and `Car` would remove approximately **67.5% of the dataset**.

This would leave only around one-third of the original data and significantly reduce the reliability of regional, suburb-level, and price trend analysis.

Instead, missing values were retained where appropriate. Specific visuals and machine learning steps handled missing values separately through filtering, aggregation, or imputation.

### 1.3 Transformation Steps

The following cleaning and transformation steps were applied:

- Converted `Date` into a valid date format.
- Created a date dimension for quarterly, monthly, and yearly analysis.
- Removed duplicate records.
- Converted numeric columns such as `Price`, `Distance`, `Landsize`, `BuildingArea`, `Bedroom`, `Bathroom`, and `Car` into numeric data types.
- Replaced invalid `BuildingArea` values such as `"missing"` and `"inf"` with null values.
- Standardised property type labels:
  - `h` → `House`
  - `u` → `Unit`
  - `t` → `Townhouse`
- Created a unique `Sales_ID` for each property transaction.
- Built a star schema model in Power BI using a transaction-level fact table and supporting dimension tables.
- Created additional grouped fields such as distance bands and land size bands.

### 1.4 Data Model

A star schema was used to support clear relationships and accurate aggregation in Power BI.

The model includes:

- `Fact_PropertySales`
- `Dim_Date`
- `Dim_Suburb`
- `Dim_PropertyType`
- `Dim_Seller`

The fact table represents individual property sale transactions, while dimension tables support filtering and grouping by time, location, property type, and seller.

### 1.5 Use of Average and Median

Both average and median measures were used in the dashboard depending on the purpose of the analysis.

**Median price** was used as the main price comparison metric because property prices are highly skewed and affected by extreme outliers. For example, the dataset includes a highest price of approximately **$11M**, while the median sale price is approximately **$871K**. Median price provides a more stable representation of typical market value.

**Average values** were used for general numeric summaries such as average rooms, average actual price, average predicted price, and average value gap. In the machine learning page, averages help summarise overall model output across many property records.

---

## 2. Dashboard Page 1: Melbourne Housing Market Overview

The first page provides a high-level overview of the Melbourne housing market between **2016 and 2018**.

### Key Metrics

The cleaned dataset contains approximately **27K property sales**. The average sale price is around **$1.05M**, while the median sale price is approximately **$871K**. The difference between average and median price indicates that the market contains high-value outliers, which is why median price was used for most price comparisons.

<img width="97" height="52" alt="image" src="https://github.com/user-attachments/assets/63eebe88-c109-4ff0-8270-ef5acc3feb95" />


### Regional Price Insights

The regional price chart shows that **Southern Metropolitan** has the highest median property price at approximately **$1.25M**. This is followed by **Eastern Metropolitan** at approximately **$1.02M**.

In contrast, **Western Victoria** has the lowest median price at approximately **$413K**. This shows a clear price gap between premium metropolitan regions and outer regional areas.

### Property Type Distribution

Houses dominate the market, accounting for approximately **67.9%** of total sales. Units represent around **21.7%**, while townhouses account for approximately **10.4%**.

This indicates that houses are the main property type in the dataset and strongly influence overall market trends.

### Quarterly Sales and Price Trends

Quarterly sales volume increased strongly through 2017, reaching its highest level in **Q3 2017** and **Q4 2017**, with more than **5K sales** in each quarter.

Median property price peaked around **Q1 2017** at approximately **$942K**, before declining in later quarters. This shows that sales volume and median price did not always move in the same direction, highlighting the importance of analysing both transaction volume and pricing trends.

---

## 3. Dashboard Page 2: Regional & Suburb Price Insights

The second page focuses on regional, suburb-level, and distance-based price patterns.

### Regional Price Differences

The regional analysis confirms that property prices vary significantly across Melbourne. Southern Metropolitan has the highest median price, while Western Victoria has the lowest.

Sales volume is concentrated mainly in major metropolitan regions, especially:

- Southern Metropolitan
- Northern Metropolitan
- Western Metropolitan

These regions account for a large share of total transactions.

### Dynamic Suburb Ranking

A dynamic suburb ranking visual was created to allow users to switch between:

- Top suburbs by sales volume
- Bottom suburbs by sales volume
- Top suburbs by median price
- Bottom suburbs by median price

For median price rankings, only suburbs with at least 50 sales were included to avoid misleading results from suburbs with very small sample sizes.

High-volume suburbs such as **Reservoir**, **Bentleigh East**, **Richmond**, and **Preston** indicate strong buyer activity. Premium suburbs such as **Canterbury**, **Malvern**, and **Middle Park** show much higher median prices.

### Distance from CBD

The distance analysis shows a clear relationship between distance from the CBD and property price.

Properties within **0–5 km** and **5–10 km** from the CBD have median prices close to **$980K**, while properties more than **30 km** away have a median price of approximately **$592K**.

This suggests that proximity to the CBD remains an important location-based price driver in the Melbourne housing market.

---

## 4. Dashboard Page 3: Property Features & Price Drivers

The third page explores how property characteristics influence median property prices.

### Property Type and Price

The property type analysis shows that houses have the highest median property price at approximately **$1.02M**. Townhouses follow at around **$850K**, while units have a lower median price of approximately **$580K**.

This indicates that property type is a major price driver, with houses generally commanding higher prices due to larger land size, more rooms, and stronger buyer demand.

### Feature-Based Price Analysis

A dynamic feature selector was added to compare median price by:

- Bathrooms
- Bedrooms
- Car spaces

This allows users to interactively analyse how different property features affect median price.

In general, properties with more rooms, bedrooms, bathrooms, and car spaces tend to show higher median prices. However, some categories contain fewer records or unusual values, so the results should be interpreted alongside sales volume.

### Land Size Analysis

The land size visual combines sales volume and median price by land size band.

This helps show both:

- How many properties were sold in each land size group
- How median price changes as land size increases

Median price generally increases across larger land size bands. However, very large land size groups may be affected by lower transaction volume and outliers.

This is another reason median price was preferred over average price for price comparison visuals.

### Time-Based Filtering

Year and month slicers were added to allow users to explore how property features and prices vary across different time periods.

---

## 5. Dashboard Page 4: Price Prediction & Market Recommendations

The final page adds a machine learning layer to the Power BI dashboard.

The goal of this page is to estimate property prices and compare predicted prices against actual sale prices to identify potential pricing opportunities or risks.

### 5.1 Machine Learning Objective

The machine learning model was designed to answer the following question:

> Can property and location features be used to estimate expected property prices and identify potential underpricing or overpricing?

The predicted price is compared with the actual sale price to classify each property into recommendation categories.

### 5.2 Model Development

A baseline linear regression model was first considered. However, linear regression can produce unrealistic predictions when extreme outliers are present. For example, properties with unusually large land sizes can cause the model to extrapolate too strongly and predict unrealistic prices.

To improve prediction reliability, the model was upgraded to a **Random Forest Regressor**.

Random Forest was selected because it can better capture non-linear relationships between property features and sale price.

### 5.3 Features Used in the Model

The model used both property and location features:

- Rooms
- Bedrooms
- Bathrooms
- Car spaces
- Land size
- Building area
- Year built
- Distance from CBD
- Property count
- Property type
- Region

### 5.4 Outlier and Model Scope Handling

Extreme records were handled using a model scope flag. Properties outside the reasonable modelling range were marked as **Out of Model Scope**.

This helped reduce unreliable predictions and prevented extreme outliers from being used as recommendation outputs.

This step was important because a machine learning model should not only generate predictions but also identify when a prediction may be unreliable.

### 5.5 Prediction Logic

The model calculates a value gap between predicted price and actual price.

```text
Value Gap % = (Predicted Price - Actual Price) / Actual Price
