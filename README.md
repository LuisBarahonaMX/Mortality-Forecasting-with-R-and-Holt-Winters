# Mortality Time Series Forecasting with R and Holt-Winters

### Disease-Related Mortality Analysis, Exponential Smoothing, and Public Health Data Exploration

![Time Series Analysis in R — Grow Up Data Analytics](https://www.ides.org.ar/formacion/curso-virtual/analisis-visualizacion-datos-r)

*Image source: [Grow Up Data Analytics — Time Series Analysis in R](https://www.ides.org.ar/formacion/curso-virtual/analisis-visualizacion-datos-r). External educational image, credited to its original publisher.*

**Project Type:** Academic Data Science and Public Health Analytics  
**Field:** Data Science | Time Series Analysis | Statistical Forecasting | Health Data Analytics  
**Programming Languages:** Python and R  
**Environment:** Google Colab | Jupyter Notebook | R via rpy2  
**Author:** Data Engineer Luis Barahona

---

## 1. Project Overview

This project explores the application of time series analysis to disease-related mortality data using Python and R.

The main objective is to analyze historical records from 2013 to 2023 and investigate how exponential smoothing techniques can generate projections of future disease-related patient counts.

The notebook follows a CRISP-DM-inspired analytical workflow, covering data understanding, preprocessing, statistical modeling, evaluation, and visualization.

The experiments include:

- Historical mortality data analysis.
- Annual aggregation by disease category.
- Data cleaning and missing-value treatment.
- Time series construction.
- Exponential smoothing with R.
- Individual disease forecasting for 2024.
- Ranking the ten disease categories with the highest projected counts.
- Exploring cumulative projections for 2024–2034.
- Visualizing historical observations and model outputs.

This project is an educational forecasting experiment and should not be interpreted as a clinically validated public health prediction system.

---

## 2. Research Objectives

The project aims to:

1. Explore historical mortality records by year and disease category.
2. Prepare structured data for time series analysis.
3. Identify and handle missing observations.
4. Aggregate patient counts by year.
5. Examine historical patterns in individual disease categories.
6. Apply exponential smoothing models in R.
7. Generate experimental forecasts for 2024.
8. Rank disease categories by predicted values.
9. Explore cumulative projections for 2024–2034.
10. Investigate how statistical forecasting may support exploratory health data analysis.

---

## 3. Dataset Description

**Input file:** `Mortalidad.xls`

**Historical period:** 2013–2023

The notebook describes a dataset containing approximately 1,122 records.

The Excel file is loaded from its second worksheet.

### Selected Variables

| Variable | Description |
|:---|:---|
| AÑO | Year of observation |
| C.I.E.10 | Disease classification code |
| MORTALIDAD | Disease or mortality category |
| NUMERO DE PACIENTES | Recorded patient count |

### Data Preparation

The notebook selects the relevant columns, checks for missing values, removes duplicate rows, and aggregates recorded counts by disease category and year.

A derived variable, `Total_Pacientes`, represents the aggregated count for each disease category in a given year.

The workflow also calculates annual totals and category proportions.

### Data Interpretation

The exact definition and population coverage of the original Excel dataset must be established before using the results for epidemiological interpretation.

The presence of a mortality-related category does not independently establish that every recorded count represents a verified death.

---

## 4. CRISP-DM Methodology

The notebook follows the main stages of the Cross-Industry Standard Process for Data Mining (CRISP-DM).

### 4.1 Data Understanding

The initial stage examines:

- Available variables.
- Historical time coverage.
- Dataset dimensions.
- Disease classifications.
- Numerical patient counts.
- Missing observations.

### 4.2 Data Preparation

Python and Pandas are used to clean and restructure the data.

Activities include:

- Column selection.
- Duplicate removal.
- Missing-value inspection.
- Median imputation for missing patient counts.
- Annual aggregation by disease category.
- Construction of yearly proportions.
- Conversion of numerical fields.

An additional illustrative table containing placeholder disease descriptions is merged into the analytical dataset.

These placeholder records are demonstrations of data integration, not verified clinical reference data.

### 4.3 Modeling

The forecasting stage uses R through the `rpy2` interface in Google Colab.

Historical patient counts are converted into annual time series.

The notebook applies:

```r
model <- HoltWinters(
  ts_d,
  beta = FALSE,
  gamma = FALSE
)
```

This configuration disables trend and seasonality.

Therefore, the implemented model is equivalent to simple exponential smoothing.

### 4.4 Evaluation

The notebook inspects historical series, smoothing parameters, and generated forecasts.

Graphical outputs support exploratory interpretation.

However, no independent test-set forecasting accuracy metrics are reported.

### 4.5 Results and Visualization

The final workflow generates:

- Disease-specific plots.
- A 2024 predicted-count ranking.
- A top-ten disease visualization.
- Cumulative projections for 2024–2034.
- Multi-panel forecast charts.
- A percentage-distribution visualization.

---

## 5. Python and R Integration

The notebook combines Python data processing with R statistical modeling.

### Python

Python is used for:

- Loading the Excel dataset.
- Cleaning records.
- Aggregating observations.
- Creating derived variables.
- Preparing analytical tables.
- Supporting visualization.

### R

R is used for:

- Constructing annual time series.
- Applying exponential smoothing.
- Generating forecasts.
- Producing statistical graphics.
- Creating ranked prediction tables.

### rpy2

The `rpy2` package allows R code to execute inside a Python-based Google Colab environment.

Example setup:

```python
!pip install rpy2
%load_ext rpy2.ipython
```

An R cell is executed using:

```r
%%R
library(forecast)
library(dplyr)
library(ggplot2)
```

---

## 6. Time Series Modeling

### 6.1 Annual Aggregation

The historical observations are grouped by year and disease category.

Example:

```r
df_agg <- df_sel %>%
  group_by(`AÑO`, MORTALIDAD) %>%
  summarise(
    Total_Pacientes = sum(
      `NUMERO DE PACIENTES`,
      na.rm = TRUE
    )
  ) %>%
  ungroup()
```

The aggregated data provides the basis for constructing annual time series.

### 6.2 Missing-Year Treatment

For each disease category, the notebook constructs a complete annual sequence:

```r
2013:2023
```

When a category has no observation for a particular year, the missing value is replaced with zero.

This produces regularly spaced annual series.

However, a missing record does not necessarily mean that zero cases occurred.

The zero-filling strategy is a modeling assumption that should be validated against the source data.

### 6.3 Exponential Smoothing

The notebook applies:

```r
HoltWinters(
  ts_d,
  beta = FALSE,
  gamma = FALSE
)
```

The model estimates the underlying level of each annual series.

Because trend and seasonality are disabled, future point forecasts remain constant over the prediction horizon.

This implementation is more appropriately described as simple exponential smoothing than as a complete Holt-Winters seasonal model.

---

## 7. Individual Disease Forecasting

The notebook allows a disease category to be selected for closer examination.

Its workflow includes:

1. Selecting a disease category.
2. Filtering historical observations.
3. Constructing an annual series.
4. Filling missing years.
5. Visualizing the observations.
6. Fitting an exponential smoothing model.
7. Generating a forecast for 2024.

### Saved Example

The notebook contains a saved experiment for:

**Neumonía debida a otros virus**

The historical filtered dataset includes observations for 2013 and 2022, with one recorded patient in each observed year.

After filling missing years with zero, the fitted smoothing model produces an experimental 2024 forecast of approximately:

**0.2425 patients**

This is a continuous statistical estimate, not an observed or clinically meaningful fractional patient.

The example is highly sensitive to the small number of recorded observations and the assumption that missing years represent zero counts.

---

## 8. Disease Ranking for 2024

The notebook applies exponential smoothing separately to the available disease categories.

It then ranks them by their estimated counts for 2024.

### Selected Saved Results

| Disease Category | Predicted Count for 2024 |
|:---|---:|
| Unspecified pneumonia | 55.00 |
| COVID-19, virus identified | 54.00 |

These figures are rounded values from the notebook's saved outputs.

They should be interpreted as model-generated historical forecasts rather than observed 2024 outcomes.

The complete top-ten table can be reproduced by executing the notebook with the original dataset.

### Important Consideration

The experiment does not contain a documented out-of-sample evaluation against actual 2024 observations.

Therefore, the ranking demonstrates the modeling procedure but does not establish forecasting accuracy.

---

## 9. Forecasting for 2024–2034

The notebook also generates forecasts for an eleven-year horizon, from 2024 through 2034.

For each disease category, the procedure:

- Constructs an annual series.
- Fills missing years with zero.
- Fits a simple exponential smoothing model.
- Generates eleven future values.
- Calculates the sum of the forecast values.
- Ranks categories by cumulative predicted counts.

The cumulative ranking is then visualized.

Because the fitted model does not include trend or seasonality, its point forecasts remain constant for each disease category.

Accordingly, the cumulative totals should be understood as exploratory statistical projections rather than estimates of future disease evolution under changing population or healthcare conditions.

---

## 10. Data Visualization

The notebook includes several visualization techniques.

### Historical Time Series

Line charts display annual observations for selected disease categories.

### Smoothed Trend Visualization

LOESS smoothing is used to explore historical patterns.

### Forecast Visualization

Forecast plots display the historical series and future estimates.

### Three-Dimensional Scatter Plot

A 3D scatter plot represents:

- Year.
- Patient count.
- Numerical disease category identifier.

### Top-Ten Ranking

A point chart displays disease categories with the highest estimated values for 2024.

### Long-Term Forecast Panels

Faceted line charts display projected values for selected disease categories from 2024 through 2034.

### Percentage Distribution

A donut chart represents each selected category's share of the cumulative forecast total among the displayed categories.

---

## 11. Technologies Used

| Technology | Purpose |
|:---|:---|
| Python | Data preparation and processing |
| R | Statistical modeling and forecasting |
| Pandas | Data cleaning and aggregation |
| NumPy | Numerical operations |
| Matplotlib | Python visualization |
| Seaborn | Statistical visualization |
| rpy2 | Python–R interoperability |
| readxl | Reading Excel files in R |
| dplyr | Data manipulation in R |
| tidyr / tidyverse | Data preparation |
| forecast | Exponential smoothing and forecasting |
| ggplot2 | Statistical visualization |
| scatterplot3d | Three-dimensional charts |
| Google Colab | Interactive execution environment |

---

## 12. How to Run

### Google Colab

[Open the Original Notebook](https://colab.research.google.com/drive/1u5m5WdXKjn7sgrD7QAITtuCeFKfvpOBN)

### Requirements

- Google Colab or a compatible Jupyter environment.
- Python.
- R.
- rpy2.
- The required R packages.
- The original `Mortalidad.xls` dataset.

### Execution Steps

1. Open the notebook in Google Colab.
2. Upload `Mortalidad.xls` to the Colab working directory.
3. Install and activate rpy2.
4. Install the required R packages.
5. Execute the data cleaning cells.
6. Generate the annual disease aggregates.
7. Execute the R modeling cells in sequence.
8. Select a disease category when prompted.
9. Examine the historical series and forecasts.
10. Generate the 2024 ranking and 2024–2034 projections.

### Important Notes

The notebook references the Excel file at:

```text
/content/Mortalidad.xls
```

The dataset must be available at that path unless the code is modified.

Some cells depend on variables created by earlier cells.

The saved notebook also contains an R cell execution error, so sequential execution and environment initialization should be checked before publication.

---

## 13. Limitations

This research notebook has several important limitations.

### Data Availability

The original Excel dataset must be supplied separately.

### Missing Observations

Missing years are filled with zero, which may not accurately represent the underlying records.

### Short Historical Series

Each disease-specific series contains at most eleven annual periods.

Some disease categories have substantially fewer observed records.

### No Seasonal Component

The implemented exponential smoothing model does not estimate seasonality.

### No Trend Component

Trend estimation is disabled in the fitted HoltWinters models.

### No Neural Network Implementation

The notebook references neural networks conceptually but does not include an implemented or evaluated neural forecasting model.

### No Independent Forecast Accuracy Test

The notebook does not report MAE, RMSE, MAPE, or another out-of-sample accuracy metric for its disease forecasts.

### Public Health Interpretation

The projections do not incorporate population changes, policy changes, disease transmission, healthcare access, or other explanatory factors.

Consequently, the results should not be used directly for medical or public health decisions.

---

## 14. Future Improvements

Potential developments include:

- Validating the original data definitions and source.
- Distinguishing missing observations from genuine zero counts.
- Expanding historical coverage.
- Implementing rolling-origin validation.
- Comparing simple exponential smoothing with Holt and ETS models.
- Evaluating ARIMA and SARIMA.
- Implementing neural-network forecasting with nnetar.
- Comparing Prophet and other forecasting approaches.
- Reporting MAE, RMSE, and other accuracy metrics.
- Including prediction intervals.
- Establishing reproducible evaluation procedures.
- Developing an interactive public health analytics dashboard.

---

## 15. Practical Applications

The workflow provides an educational foundation for:

- Health data analytics.
- Historical disease count exploration.
- Time series preprocessing.
- Statistical forecasting education.
- Data mining workflows.
- Public health research prototyping.
- Python and R interoperability.
- Visualization of longitudinal datasets.

Additional validation and domain-specific review would be required for operational applications.

---

## 16. Project Resources

### Google Colab Notebook

[Open Mortality Time Series Forecasting](https://colab.research.google.com/drive/1u5m5WdXKjn7sgrD7QAITtuCeFKfvpOBN)

### Grow Up Data Analytics — Time Series Analysis in R

[Visit the Time Series Analysis in R Course](https://escuela.growupcr.com/p/series-tiempo-r)

This external educational resource discusses time series preprocessing, statistical models, forecasting evaluation, and practical analysis in R.

The course is referenced as a related learning resource. This independent notebook is not presented as an official Grow Up Data Analytics project.

### R Forecast Package

[Forecast Package Documentation](https://pkg.robjhyndman.com/forecast/)

### CRISP-DM

[CRISP-DM Overview — IBM](https://www.ibm.com/think/topics/crisp-dm)

---

## 17. Conclusion

This project demonstrates how Python and R can be combined to explore disease-related mortality records and generate experimental forecasts.

The analysis follows a CRISP-DM-inspired process, including data preparation, annual aggregation, statistical modeling, and visualization.

The notebook uses simple exponential smoothing to produce an experimental disease ranking for 2024 and cumulative projections through 2034.

The research also identifies important opportunities for improving data quality, forecasting validation, and model comparison.

Overall, the notebook represents an educational foundation for further work in statistical forecasting and health data analytics rather than a validated predictive system.

---

## Author

**Data Engineer Luis Barahona**

Mexican Data Engineer and 2026 graduate of Universidad del Caribe, interested in Data Engineering, Data Science, Artificial Intelligence, Statistical Modeling, and technology applied to real-world problems.

---

## Contact

| Platform | Contact |
|:---|:---|
| Email | [190300395@ucaribe.edu.mx](mailto:190300395@ucaribe.edu.mx) |
| GitHub | [github.com/LuisBarahonaMX](https://github.com/LuisBarahonaMX) |
| LinkedIn | [linkedin.com/in/luis-barahona-mx](https://www.linkedin.com/in/luis-barahona-mx/) |
| Instagram | [@luisbarahonamx](https://www.instagram.com/luisbarahonamx/) |
| X | [@luisbarahonaMX](https://x.com/luisbarahonaMX) |
| Website | [SIKO AI Platform](https://sikoai.web.app/) |
