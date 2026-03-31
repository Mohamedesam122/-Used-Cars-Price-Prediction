# 🚗 Used Cars Price Prediction

> A machine learning regression project that predicts the price of used cars based on vehicle features such as brand, mileage, engine specifications, age, and more.

---

## 📌 Table of Contents

- [Overview](#overview)
- [Dataset](#dataset)
- [Project Structure](#project-structure)
- [Workflow](#workflow)
- [Feature Engineering](#feature-engineering)
- [Models & Results](#models--results)
- [Installation & Usage](#installation--usage)
- [Key Learnings](#key-learnings)
- [Limitations](#limitations)

---

## Overview

Used car prices are influenced by many factors — brand reputation, mileage, engine power, age, accident history, and more. This project builds a regression pipeline to predict used car prices from a real-world dataset of ~188,000 listings.

The target variable (`price`) is log-transformed to handle skewness, and multiple regression models are trained and compared to find the best performer.

---

## Dataset

| Property | Details |
|----------|---------|
| Source | [Kaggle — Used Cars Dataset](https://www.kaggle.com/) |
| Rows | ~188,000 listings |
| Target | `price` (USD) |
| Features | brand, model, mileage, fuel type, transmission, engine, exterior/interior color, accident history |

---

## Project Structure

```
used-cars-price-prediction/
│
├── used_cars.csv                        # Raw dataset
├── used_cars_price_prediction.ipynb     # Main notebook
└── README.md                            # This file
```

---

## Workflow

```
1. Data Loading & Exploration
        ↓
2. Data Cleaning
   - Remove duplicates
   - Fill missing values
   - Fix invalid entries
        ↓
3. Feature Engineering
   - Extract engine specs from text (Regex)
   - Create car age & era
   - Mileage per year
   - Group rare colors
        ↓
4. Exploratory Data Analysis (EDA)
   - Correlation heatmap
   - Price distributions
   - Feature vs price plots
        ↓
5. Preprocessing & Encoding
   - Ordinal encoding (car era)
   - One-hot encoding (fuel, transmission, colors)
   - Target encoding (brand, model)
   - RobustScaler (linear models only)
        ↓
6. Model Training & Comparison
        ↓
7. Evaluation & Visualization
```

---

## Feature Engineering

One of the most important parts of this project was extracting meaningful features from the raw `engine` column using Regex.

**From this:**
```
"355.0HP 5.3L 8 Cylinder Engine Gasoline Fuel"
```

**We extracted:**

| Feature | Value |
|---------|-------|
| `horsepower` | 355.0 |
| `engine_size` | 5.3 |
| `num_cylinders` | 8 |
| `valve_count` | 16 |
| `is_turbo` | 0 |
| `is_electric` | 0 |
| `hp_per_liter` | 66.98 |
| `hp_per_cylinder` | 44.37 |

**Other engineered features:**

| Feature | Description |
|---------|-------------|
| `car_age` | `2025 - model_year` |
| `car_era` | Classic / Old / Mid / Recent / Modern |
| `milage_per_year` | `mileage / car_age` |
| `is_twin_turbo` | Boolean flag |
| `is_supercharged` | Boolean flag |
| `is_diesel` | Boolean flag |
| `is_dohc` | Boolean flag |

---

## Models & Results

All tree-based models were trained on **unscaled** data. Linear models were trained on **RobustScaler** transformed data. The target is `log(price)` to handle skewness.

| Model | R² Score | MAE | MSE |
|-------|----------|-----|-----|
| **XGBoost** | **0.6544** | **0.3488** | **0.2486** |
| Gradient Boosting | 0.6470 | 0.3537 | 0.2540 |
| Random Forest | 0.6346 | 0.3611 | 0.2629 |
| Ridge | 0.6171 | 0.3720 | 0.2755 |
| Linear Regression | 0.6171 | 0.3720 | 0.2755 |
| Lasso | 0.6058 | 0.3782 | 0.2836 |

> ✅ **Best Model: XGBoost** with R² = 0.6544

### Why R² ~ 0.6544 is a Good Score for This Dataset

The dataset is missing key real-world pricing factors:
- Geographic location (a car in NYC costs more than in Ohio)
- Seller type (dealer vs. private)
- Physical condition (excellent / good / fair)
- Actual negotiated price vs. listed price

These missing factors account for the unexplained ~33% variance. A score of 0.65–0.70 is well within the typical benchmark for used car pricing datasets.


---

## 🛠️ Tech Stack

![Python](https://img.shields.io/badge/Python-3.11-blue)
![Pandas](https://img.shields.io/badge/Pandas-2.0-green)
![Scikit--learn](https://img.shields.io/badge/Scikit--learn-1.3-orange)
![XGBoost](https://img.shields.io/badge/XGBoost-1.7-red)
