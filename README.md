# EnergyForecastFinal

# Energy Consumption Forecasting ⚡📈

This project implements a machine learning pipeline to forecast hourly electricity consumption for various consumer groups 48 hours into the future. It leverages **LightGBM**, robust time-series feature engineering, and astronomical data to achieve accurate predictions.

## 🚀 Key Features

* **Model:** LightGBM Regressor (Gradient Boosting Machine) optimized for time-series forecasting.
* **Metric:** Mean Absolute Percentage Error (MAPE).
* **Feature Engineering:**
    * **Temporal:** Hour, Day of Week, Month, Season, Year.
    * **Lags & Rolling Windows:** 2-day, 3-day, 1-week, and 1-year lags; Rolling maximums to capture trends.
    * **Solar Data:** Dynamic calculation of Sun Elevation and "Is Sun Up" flags using the `astral` library, based on geographic coordinates of Finnish cities.
    * **Weather:** Temperature, Humidity, Rain, and Wind Speed integration.
    * **Pricing:** 24-hour lagged electricity spot prices.
    * **Group specific:** private/enterprise, type of electricity contract.
* **Validation:** 8-fold Time Series Cross-Validation to prevent data leakage and ensure robustness across different seasons.
* **Forecast Generation:** A complete pipeline to generate a wide-format 48-hour forecast CSV from raw data.

## ⚙️ Installation

1.  Clone the repository:
    ```bash
    git clone [https://github.com/anton-saari/EnergyForecastFinal.git](https://github.com/anton-saari/EnergyForecastFinal.git)
    cd EnergyForecastFinal
    ```

2.  Install the required packages:
    ```bash
    pip install pandas numpy lightgbm astral scikit-learn matplotlib
    ```

## 🏃‍♂️ Usage

1.  Ensure all data files are placed in the root directory.
2.  Open the Jupyter Notebook:
    ```bash
    jupyter notebook code.ipynb
    ```
3.  Run all cells to execute the training pipeline and generate the forecast.
4.  **Output:** The script generates `48_hour_forecast.csv`, a wide-format CSV where:
    * **Index:** `measured_at` (Timestamp)
    * **Columns:** `group_id` (Consumer Group IDs)
    * **Values:** Predicted consumption (kWh)

## 🧠 Methodology

### 1. Data Preprocessing
The pipeline cleans raw data, handles missing values, and merges disparate data sources (weather, price, client info) into a unified time-series dataframe.

### 2. Advanced Feature Engineering
To capture the cyclic nature of energy usage, the model uses:
* **Cyclical Encoding:** Months and seasons are treated mathematically to preserve their cyclic nature (e.g., December is close to January).
* **Astronomical Features:** Instead of just using "hour of day", the model calculates the exact sun elevation angle for each location. This helps the model distinguish between a dark winter afternoon and a sunny summer evening.

### 3. Training & Validation
The model is trained using an **8-fold Time Series Split** scheme. This ensures the model is tested on "future" data it hasn't seen during training, simulating real-world forecasting conditions.

* **CV Score (MAPE):** ~5.8% (Average across folds)

## 📊 Results

The final model produces a robust 48-hour forecast that accounts for weather changes, solar activity, and historical consumption patterns.
* **Final forecasted average MAPE:** 6.55%

<img width="1242" height="552" alt="image" src="https://github.com/user-attachments/assets/9c82694f-a637-4859-bbba-e5fa0ccc4cd0" />

