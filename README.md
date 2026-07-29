# Comparative Regression Analysis: Makassar Weather Prediction

![Python](https://img.shields.io/badge/Python-3.8%2B-blue)
![Jupyter Notebook](https://img.shields.io/badge/Jupyter-Notebook-orange)
![Scikit-Learn](https://img.shields.io/badge/Scikit_Learn-Machine_Learning-success)

## Project Overview
This project focuses on predicting daily mean weather temperatures in Makassar, Indonesia, using various Machine Learning regression algorithms. The primary objective is to perform a comparative analysis of multiple models. 

## Repository Architecture
*   `dataset/`: Contains the raw and processed weather datasets sourced from OpenMeteo.
*   `notebooks/`: Jupyter Notebooks containing the data preprocessing, exploratory data analysis (EDA), unified model training pipeline, and evaluation codes.
*   `images/`: Contains all output visualizations for Actual vs. Predicted scatter plots.

## Methodology & Setup
To ensure the model performs in a same-day estimation using concurrent meteorological observations. Several methodological rules were applied:

1.  **Strict Feature Selection (Zero Leakage):** Deterministic features such as `temperature_2m_max`, `temperature_2m_min`, and `apparent_temperature_mean` were strictly dropped from the input variables. This ensures the model genuinely predicts the mean temperature rather than calculating a midpoint from already known daily extremes.
2.  **Chronological Data Splitting:** Weather data is inherently time-series. The dataset was split chronologically (`shuffle=False`) using 70:30 ratios.
3.  **Standardized Scaling via Pipeline:** Distance-based algorithms (like SVR) were encapsulated within a Scikit-Learn `Pipeline` using `StandardScaler`. This give a fair apple-to-apple comparisons against tree-based models that do not require scaling.

**Noted**:
Regarding hyperparameter tuning, I intentionally did not perform hyperparameter tuning for any model in this experiment, so that the comparison between algorithms is purely a comparison of their default performance, not how well I tuned a particular model. Hyperparameter optimization will be a separate scope for further experiments."

## Models Evaluated
We evaluated a multiple set of regression algorithms to capture the non-linear nature of meteorological data:
*   Support Vector Regression (SVR - RBF, Linear, Polynomial)
*   CatBoost Regression
*   Gradient Boosting
*   LightGBM Regression
*   XGBoost Regression
*   AdaBoost Regression
*   Linear Regression (Baseline)

## Results & Highlights

### Model Performance Comparison

| Model | MAE | RMSE | MAPE | R² | Training Duration (s) |
|-------|-----|------|------|-----|----------------------|
| **SVR RBF** | **0.6166** | **0.8082** | **0.0224** | **0.3801** | 0.2936 |
| CatBoost | 0.6321 | 0.8220 | 0.0230 | 0.3587 | 1.4037 |
| Gradient Boosting | 0.6349 | 0.8272 | 0.0230 | 0.3506 | 0.3786 |
| LightGBM | 0.6402 | 0.8354 | 0.0233 | 0.3376 | 1.9638 |
| XGBoost | 0.6531 | 0.8558 | 0.0238 | 0.3049 | 0.2340 |
| Linear Regression | 0.6508 | 0.8630 | 0.0236 | 0.2931 | **0.0130** |
| SVR Linear | 0.6505 | 0.8644 | 0.0236 | 0.2908 | 0.4279 |
| AdaBoost | 0.6743 | 0.8725 | 0.0244 | 0.2775 | 0.0776 |
| SVR Polynomial | 0.6913 | 0.9147 | 0.0251 | 0.2058 | 0.6485 |

### The Better Performing Model: SVR RBF
**Support Vector Regression (SVR - RBF)** emerged as the best performing model among those evaluated, achieving the lowest MAE (~0.61°C) and fastest training times. However, based on the R² score of 0.38 and the scatter plot visualization, the model still exhibits limitations. It shows a tendency to regress towards the mean—over-predicting at lower extreme temperatures and under-predicting at higher extreme temperatures (>29°C), indicating that meteorological data remains highly complex to model perfectly.

Tree-based models like CatBoost and Gradient Boosting performed comparably, following very closely in accuracy, though at the cost of slightly higher training durations (e.g., CatBoost at ~1.40s). Meanwhile, the baseline Linear Regression dropped to the lower ranks. However, despite its simplicity, Linear Regression still managed to outperform AdaBoost, SVR Linear, and SVR Polynomial.

### Visual Comparison: Best Model vs. Baseline

| Better Performing Model: SVR RBF | Baseline Model: Linear Regression |
| :---: | :---: |
| <img src="images\SVR_RBF.png" width="400"/> | <img src="images\Linear Regression.png" width="400"/> |
| *Achieves the best fit among evaluated models, but with significant dispersion. Crucially fails to predict extreme high temperatures, with predictions flattening out around 28°C.* | *A baseline with wider dispersion across all ranges. Shares the same limitation of failing to capture high extreme temperatures, leading to a consistent under-prediction bias.* |

## How to Run (Local / VS Code)

To reproduce this analysis locally, follow these steps:

1. **Clone the repository:**
   ```bash
   git clone [https://github.com/Payama01/comparative-regression-weather-makassar.git](https://github.com/Payama01/comparative-regression-weather-makassar.git)
   cd comparative-regression-weather-makassar
2. **Install dependencies:** \
Make sure you have Python installed, then run the following command in your terminal to install all required libraries:
   ```bash
   pip install -r requirements.txt
3. **Run the Analysis:**
*  Open the comparative-regression-weather-makassar folder in Visual Studio Code.
*  Ensure you have the official Jupyter extension installed in VS Code.
*  Navigate to the notebooks/ directory and open the .ipynb file.
*  Select your Python environment (Kernel) in the top right corner of VS Code.
*  Click Run All to execute the cells and reproduce the analysis.
## Author
**Payama (Muhammad Yusuf Erki)**
*   Information Technology Undergraduate at Universitas Tarumanagara (UNTAR)
*   [[LinkedIn]](https://www.linkedin.com/in/muhammad-yusuf-erki-78495b306/)
*   [[Github]](https://github.com/Payama01)

## License
This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
*(Feel free to use the code and datasets for your own learning or research purposes).*
