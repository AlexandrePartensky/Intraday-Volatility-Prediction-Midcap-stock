# Intraday Volatility Prediction

This repository contains the code for a project on forecasting short-horizon intraday volatility for U.S. mid-cap stocks. The goal is to predict realized volatility 30 minutes ahead using limit-order book features and lagged volatility signals.

The work was done as part of an industry project with J.P. Morgan and is documented in the accompanying report/

## Project Overview

Volatility forecasting is important for risk management, derivatives pricing, and trading strategies. Traditional daily volatility models (e.g., GARCH) often fail to capture dynamics at intraday horizons where market microstructure effects dominate.

This project builds a forecasting system using:

- limit-order book features (spread, depth, imbalance)
- interaction terms between liquidity and order flow
- lagged realized volatility components

The target variable is the **30-minute ahead realized variance**, computed from mid-price log returns.

## Data

The dataset consists of limit-order book snapshots sampled every 30 minutes for **25 U.S. mid-cap stocks** between **May and September 2025**. 

From these snapshots we construct microstructure features such as:

- bid-ask spread
- order book depth and thinness
- order imbalance
- interaction terms (e.g., imbalance × spread)

Lagged realized volatility features (daily, weekly, monthly horizons) are also included as predictors.

## Methodology

The workflow of the project is:

1. **Data processing**
   - construct mid-price and realized variance
   - compute microstructure features from LOB snapshots

2. **Exploratory analysis**
   - study distribution and autocorrelation of realized volatility
   - examine cross-ticker correlations

3. **Feature selection**
   - combine Lasso and XGBoost importance scores
   - apply backward elimination to obtain a compact feature set

4. **Modeling**
   - HAR (heterogeneous autoregressive) baseline
   - Random Forest
   - XGBoost
   - stacked models combining linear regression and tree predictions

## Results

A small number of features capture most of the predictive signal, notably:

- order book thinness  
- imbalance × spread interaction  
- imbalance × thinness interaction  
- rolling imbalance magnitude  

Lagged realized volatility remains the strongest predictor. Combining a linear model with Random Forest predictions produced the best balance between prediction accuracy and ranking performance.

## Repository Structure

data.ipynb
Loads and prepares the dataset.

eda.ipynb
Exploratory analysis of volatility and microstructure features.

comparaison.ipynb
Feature engineering and dataset preparation.

benchmark.ipynb
Model comparison and evaluation.

main.ipynb
Main modeling workflow.
