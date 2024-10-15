# Ensemble-model
# Flight Delay Prediction

This project predicts flight delays using machine learning models implemented from scratch. The goal is to classify flight delays into different categories based on various features derived from the data. The project includes data preprocessing, feature extraction, model development, and evaluation using three machine learning models: Support Vector Machine (SVM), Decision Tree, and Logistic Regression. An ensemble learning approach is applied to improve the model's performance.

## Table of Contents
1. [Data Preprocessing](#data-preprocessing)
2. [Feature Extraction](#feature-extraction)
3. [Handling Missing Values](#handling-missing-values)
4. [Time Calculations](#time-calculations)
5. [Binning of Target Variable](#binning-of-target-variable)
6. [Feature Creation](#feature-creation)
7. [Balancing the Dataset](#balancing-the-dataset)
8. [Feature Encoding and Transformation](#feature-encoding-and-transformation)
9. [Model Development](#model-development)
10. [Ensemble Learning](#ensemble-learning)
11. [Model Evaluation](#model-evaluation)

## Data Preprocessing

The dataset contains nested fields such as `departure`, `arrival`, `airline`, `flight`, and `codeshared`, which were flattened for easier analysis and model development.

## Feature Extraction

- From the `departure` field, features like IATA code, ICAO code, terminal, delay, and scheduled/estimated times were extracted.
- From the `arrival` field, IATA, ICAO, and scheduled times were captured.
- The `airline` and `flight` fields provided details such as airline name, IATA and ICAO codes, and flight numbers.
- Codeshared flights (if available) were handled by checking for their presence and extracting the corresponding information.

## Handling Missing Values

- Missing values in `departure_delay` were imputed with the mean value of the delays.
- For time-based features like `departure_scheduledTime` and `arrival_scheduledTime`, missing values were checked and appropriate imputation techniques were applied.

## Time Calculations

- The delay time (in minutes) was calculated as the difference between `departure_estimatedTime` and `departure_scheduledTime`. This new feature, `delay_time`, was crucial for predicting delay categories.

## Binning of Target Variable

- The `delay_time` was categorized into 8 bins using the following breakpoints (in minutes): `[-inf, 30, 60, 120, 240, 480, 720, 1440, inf]`. These bins represent different delay time ranges (e.g., 0-30 minutes, 30-60 minutes, etc.).
- The bins served as the target variable (`delay_time_bin`) for classification, allowing the problem to be tackled as a multi-class classification task.

## Feature Creation

- Time features were extracted from `departure_scheduledTime` and `arrival_scheduledTime`, including `hour`, `day`, `month`, and `year`.
- Additional features like `departure_hour`, `departure_day`, `arrival_hour`, and `arrival_day` were created to capture temporal patterns in flight delays.

## Balancing the Dataset

Flight delays are inherently skewed, with a large proportion of flights experiencing minimal delays. To address this imbalance:
- **Upsampling Minority Class**: The minority class (flights with delays) was upsampled to match the number of samples in the majority class (on-time flights), creating a balanced dataset for training.

## Feature Encoding and Transformation

- **One-Hot Encoding**: Categorical variables such as `departure_iata`, `departure_terminal`, and `airline_name` were one-hot encoded to treat each category as a unique feature.
- **Numerical Features**: Features like `departure_delay`, `departure_hour`, `departure_day`, and various time-based features were scaled and normalized before being used in the models.

## Model Development

### 1. Support Vector Machine (SVM)
- **Kernel Options**: Linear, Polynomial, and Radial Basis Function (RBF) kernels were explored.
- **Optimization**: Alpha values were optimized using a simplified Sequential Minimal Optimization (SMO) algorithm.
- **Output**: The SVM model produced decision values for classification.

### 2. Decision Tree
- **Tree Construction**: A recursive binary split was applied to find the best feature and threshold.
- **Stopping Criteria**: The tree's growth was controlled using a maximum depth parameter to prevent overfitting.
- **Entropy and Information Gain**: These metrics were used to evaluate splits.

### 3. Logistic Regression
- **Gradient Descent**: Used to optimize model weights by minimizing the cross-entropy loss function.
- **Sigmoid Activation**: A sigmoid function was used for probabilistic predictions.
- **Learning Rate**: A small learning rate ensured gradual convergence during training.

## Ensemble Learning

After implementing individual models, they were combined using the Bagging technique:
- **Bagging Approach**: Multiple instances of each model (SVM, Decision Tree, and Logistic Regression) were trained on different bootstrapped subsets of the data. Their predictions were aggregated to form a final prediction, reducing variance and improving generalization.

## Model Evaluation

The models were evaluated using 5-fold cross-validation with the following results:

- **Logistic Regression Cross-Validation Scores**: [0.95, 0.95, 1.0, 1.0, 0.95]
- **SVM Average Accuracy**: 0.46
- **Decision Tree Average Accuracy**: 0.96
- **Logistic Regression Average Accuracy**: 0.97

## Conclusion

The flight delay prediction project successfully implemented and evaluated three machine learning models from scratch. The ensemble approach improved performance, and the results demonstrated the models' capabilities in predicting flight delay categories.
