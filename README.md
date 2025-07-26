# Supply Chain Emissions Prediction Model

This project develops and evaluates a series of regression models to predict **Supply Chain Emission Factors with Margins** for various U.S. industries and commodities from 2010 to 2016. The model leverages descriptive features and data quality metrics to achieve high prediction accuracy.

## 📝 Table of Contents
- [Problem Statement](#problem-statement)
- [Project Workflow](#project-workflow)
- [Data Analysis and Preprocessing](#data-analysis-and-preprocessing)
- [Model Training and Comparative Analysis](#model-training-and-comparative-analysis)
- [Dimensionality Reduction and Visualization](#dimensionality-reduction-and-visualization)
- [Key Findings and Insights](#key-findings-and-insights)
- [How to Use This Project](#how-to-use-this-project)
- [Dependencies](#dependencies)

## 🎯 Problem Statement

Given annual supply chain emission data from 2010–2016 categorized by industries and commodities, the goal is to develop a regression model that can predict the **Supply Chain Emission Factors with Margins**. The prediction is based on descriptive and quality metrics such as the substance (e.g., carbon dioxide, methane), unit, and data quality scores (reliability, temporal, geographical, and technological correlations).

## 🚀 Project Workflow

The project follows a structured machine learning pipeline:
1.  **Data Loading**: Data is loaded from multiple sheets in an Excel file, representing different years and sources (Industry vs. Commodity), and consolidated into a single DataFrame.
2.  **Preprocessing**: The data is cleaned, categorical features are numerically encoded, and all features are standardized for optimal model performance.
3.  **Model Training**: Several regression models are trained, including Linear Regression, Random Forest, KNN, SVR, and Gradient Boosting.
4.  **Hyperparameter Tuning**: `GridSearchCV` is used to find the optimal parameters for the best-performing model (Random Forest).
5.  **Evaluation**: Models are rigorously evaluated using **R-squared (R²)** and **Root Mean Squared Error (RMSE)** metrics.
6.  **Visualization**: Principal Component Analysis (PCA) is employed to reduce the feature space to two dimensions, allowing for the visualization of each model's decision regions.
7.  **Model Persistence**: The final tuned model and the data scaler are saved using `joblib` for future predictions.

## 📊 Data Analysis and Preprocessing

The initial dataset consists of 22,092 entries after consolidating data from 2010 to 2016.

**Key Preprocessing Steps:**
* **Data Consolidation**: Data for each year from both 'Industry' and 'Commodity' sheets were combined, with new 'Year' and 'Source' columns to maintain context.
* **Categorical Encoding**: Text-based categorical features (`Substance`, `Unit`, `Source`) were converted to numerical values using dictionary mapping, making them suitable for machine learning algorithms.
    - **Substance**: `{'carbon dioxide': 0, 'methane': 1, 'nitrous oxide': 2, 'other GHGs': 3}`
    - **Unit**: `{'kg/2018 USD, purchaser price': 0, 'kg CO2e/2018 USD, purchaser price': 1}`
    - **Source**: `{'Commodity': 0, 'Industry': 1}`
* **Feature Selection**: High-cardinality and identifier columns like `Name`, `Code`, and `Year` were dropped to create a focused feature set.
* **Feature Scaling**: `StandardScaler` was applied to normalize the features, ensuring that models sensitive to feature magnitude (like SVR and KNN) perform effectively.

## 🤖 Model Training and Comparative Analysis

Five different regression models were trained and evaluated on the preprocessed data. The results were exceptionally strong across the board, with Linear Regression surprisingly emerging as the top performer.

| Model | RMSE | R² Score |
| :--- | :--- | :--- |
| **Linear Regression** | **0.00028** | **0.999998** |
| Random Forest (Tuned) | 0.00595 | 0.999370 |
| Gradient Boosting Regressor | 0.00626 | 0.999301 |
| K-Nearest Neighbors (KNN) | 0.01606 | 0.995410 |
| Support Vector Regressor (SVR) | 0.06683 | 0.920483 |

The outstanding performance, especially of the Linear Regression model, is attributed to a strong linear relationship within the features. The target variable, **`Supply Chain Emission Factors with Margins`**, is a direct sum of two features in the dataset: **`Supply Chain Emission Factors without Margins`** and **`Margins of Supply Chain Emission Factors`**. The model successfully learned this fundamental additive relationship.

## 🔬 Dimensionality Reduction and Visualization

To better understand how each model makes predictions, Principal Component Analysis (PCA) was used to reduce the 10-dimensional feature space to 2 dimensions. The models were then retrained on this reduced data and their decision regions were plotted.

**Performance on 2D PCA Data:**

| Model (on PCA Data) | RMSE | R² Score |
| :--- | :--- | :--- |
| Random Forest | 0.0394 | 0.9723 |
| Gradient Boosting | 0.0424 | 0.9680 |
| K-Nearest Neighbors (KNN) | 0.0455 | 0.9631 |
| Support Vector Regressor (SVR) | 0.0675 | 0.9188 |
| Linear Regression | 0.1134 | 0.7711 |

**Decision Region Plots:**

The plots visualize how each model partitions the feature space.
* **Linear Regression** creates a smooth, linear gradient.
* **Random Forest** and **Gradient Boosting** show more complex, step-like boundaries, capturing non-linear interactions.
* **KNN** results in a patchy, instance-based map.
* **SVR** generates smooth, non-linear curves.


## 💡 Key Findings and Insights

1.  **Dominant Linear Relationship**: The primary driver of the target variable is the sum of the "without margins" and "margins" features. This explains the near-perfect score of the Linear Regression model.
2.  **Model Robustness**: While Linear Regression was superior on the full dataset, non-linear models like Random Forest and Gradient Boosting proved more robust on the dimensionality-reduced data, indicating their ability to capture complex patterns even with less information.
3.  **Impact of PCA**: As expected, reducing dimensions from 10 to 2 led to a drop in performance for all models, as significant variance was lost. However, the R² scores remained high for tree-based models, highlighting their effectiveness.
4.  **Practical Application**: For this specific dataset, a simple Linear Regression model is sufficient and highly accurate. For datasets where the target relationship is less direct, the tuned Random Forest model would be a powerful and reliable choice.

## ⚙️ How to Use This Project

### Prerequisites
Ensure you have Python 3 installed along with the necessary libraries.

### Installation
Clone the repository and install the required packages:
```bash
git clone https://github.com/An-Ano-nymus/IBM-SKILL_BUILD.git
cd <repository-directory>
pip install -r requirements.txt
```

### Running the Notebook
1.  Place the dataset `SupplyChainEmissionFactorsforUSIndustriesCommodities (1).xlsx` in the `/content/` directory or update the file path in the notebook.
2.  Launch Jupyter Notebook:
    ```bash
    jupyter notebook IBM_Shell_internship_GHG_model_making.ipynb
    ```
3.  Run the cells sequentially to reproduce the analysis, model training, and visualizations.

### Making Predictions with the Saved Model
The trained model (`final_model.pkl`) and scaler (`scaler.pkl`) are saved in the `models/` directory. The notebook includes a section demonstrating how to load these files and make predictions on new data.

## 📚 Dependencies
The project requires the following Python libraries, which can be found in `requirements.txt`:

```
pandas
numpy
seaborn
matplotlib
scikit-learn
joblib
