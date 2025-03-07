# Oil Price Prediction using GAMLSS

### Year : 2025

## Project Overview
This project aims to predict the next day's oil price using the **gamlss** package in R. The dataset used is the **Oil dataset** in RStudio. The analysis involves data preprocessing, exploratory data analysis (EDA), stationarity testing, and applying necessary transformations to prepare the data for predictive modeling.

## Table of Contents
- [Project Overview](#project-overview)
- [Installation & Setup](#installation--setup)
- [Dataset](#dataset)
- [Exploratory Data Analysis (EDA)](#exploratory-data-analysis-eda)
- [Normality Test](#normality-test)
- [Stationarity Test](#stationarity-test)
- [Model Evaluation](#model-evaluation)
- [Usage](#usage)
- [Results](#results)
- [License](#license)

---

## Installation & Setup
To run this project, ensure you have **R** and the necessary libraries installed.

### **Install Required Libraries**
```r
install.packages(c("gamlss", "dplyr", "ggplot2", "tseries", "forecast", "zoo", "plotly", "shinythemes", "corrplot", "Hmisc", "gridExtra"))
```

### **Load the Libraries**
```r
library(gamlss)
library(dplyr)
library(ggplot2)
library(tseries)
library(forecast)
library(zoo)
library(plotly)
library(shinythemes)
library(corrplot)
library(Hmisc)
library(gridExtra)
```

---

## Dataset
The **Oil dataset** in R contains multiple variables affecting oil prices. We extract the relevant columns and preprocess them.

```r
df = oil[,-2:-15]
head(df,3)
```

---

## Exploratory Data Analysis (EDA)
We analyze trends in the dataset and visualize the oil price over time:

```r
# Create an index column
df$index <- 1:nrow(df)

# Plot Oil Price over Time
ggplot(df) +
  aes(x = index, y = OILPRICE) +
  geom_line(size = 0.7, colour = "#EF562D") +
  geom_smooth(method = "loess", colour = "#9BBEDB", se = FALSE) +
  labs(title = 'Oil Price Over Time', x = 'Days', y = 'Oil Price') +
  theme(plot.title = element_text(size = 15L, face = "bold", hjust = 0.5))
```

---

## Normality Test
We test whether the variables follow a normal distribution using the **Shapiro-Wilk test**:

```r
vars <- c("BDIY_log", "SPX_log", "DX1_log", "GC1_log", "HO1_log", "USCI_log", "GNR_log", "SHCOMP_log", "FTSE_log", "OILPRICE", "respLAG")

normality_results <- sapply(vars, function(var) {
  shapiro.test(oil[[var]])$p.value
})

print(normality_results)
```

---

## Stationarity Test
Since time series models require stationarity, we apply the **Augmented Dickey-Fuller (ADF) Test**:

```r
adf_results <- lapply(df, adf.test)
adf_results
```

---

## Model Evaluation
After preparing the dataset, we train a predictive model using the **gamlss** package. The model is evaluated using various statistical metrics, including:

- **AIC (Akaike Information Criterion)**: Measures the model's quality, penalizing complexity.
- **BIC (Bayesian Information Criterion)**: Similar to AIC but penalizes complexity more heavily.
- **Residual Analysis**: Examines model performance by analyzing residuals.
- **Prediction Accuracy**: Compares predicted vs. actual oil prices.

Example of model training and evaluation:

```r
# Fit a GAMLSS model
model<- gamlss(OILPRICE ~ ., family = SHASH, data = train_data)

# Model summary
summary(model)

# Evaluate model performance
AIC(model)
BIC(model)
```

---

## Usage
Run the R script to process the dataset, visualize trends, and perform statistical tests before modeling.

1. Load the dataset.
2. Perform EDA and visualize trends.
3. Conduct normality and stationarity tests.
4. Train and evaluate the model.
5. Use the model for predictive analysis.

---

## Results
- The exploratory analysis showed a downward trend in oil prices over time.
- Normality tests confirmed that most variables do not follow a normal distribution.
- Stationarity testing was conducted to ensure valid time series modeling.
- The **gamlss** model was trained and evaluated using AIC, BIC, and residual analysis.
- The predictive model demonstrated reasonable accuracy in forecasting oil prices, with some deviations due to market volatility.

---

## License
This project is open-source and available under the [MIT License](LICENSE).

---

**Author:** Sarvesh Sairam Naik  
**GitHub Repository:** [GitHub Link](https://github.com/sarveshsn)
