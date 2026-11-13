
# Electrical Transformer Anomaly Detection

## Overview

This project explores statistical and unsupervised Machine Learning techniques for identifying unusual patterns in electrical transformer sensor measurements.

Developed as a group laboratory during my Master's in Artificial Intelligence at Universidad Internacional de La Rioja (UNIR), the project compares four anomaly detection approaches:

- Rolling Mean and Standard Deviation
- Z-Score
- Isolation Forest
- Local Outlier Factor (LOF)

The experiments focus on neutral current measurements (`INUT`) from an electrical transformer monitoring dataset.

The objective is to explore how different detection methods identify unusual observations in sensor data and how their assumptions affect the results.

## Project Objectives

- Explore electrical transformer sensor measurements.
- Analyze data quality, distributions, and correlations.
- Identify unusual current measurements.
- Apply statistical anomaly detection techniques.
- Train unsupervised anomaly detection models.
- Compare anomaly counts and detection behavior.
- Visualize unusual observations.
- Discuss methodological limitations and practical applications.

## Technologies

### Programming and Data Analysis

- Python
- pandas
- NumPy
- SciPy

### Machine Learning

- scikit-learn
- IsolationForest
- LocalOutlierFactor

### Visualization

- Matplotlib
- Seaborn

### Development Environment

- Jupyter Notebook

## Dataset

The academic laboratory uses:

`muinar07_act3_CurrentVoltage.csv`

The dataset contains **19,352 observations and 11 columns**.

It includes electrical voltage and current measurements collected through IoT devices.

The laboratory documentation describes observations from June 2019 to April 2020.

### Variables

| Variable | Description |
|---|---|
| DeviceTimeStamp | Measurement timestamp |
| VL1 | Phase voltage, line 1 |
| VL2 | Phase voltage, line 2 |
| VL3 | Phase voltage, line 3 |
| IL1 | Phase current, line 1 |
| IL2 | Phase current, line 2 |
| IL3 | Phase current, line 3 |
| VL12 | Line-to-line voltage, phases 1 and 2 |
| VL23 | Line-to-line voltage, phases 2 and 3 |
| VL31 | Line-to-line voltage, phases 3 and 1 |
| INUT | Neutral current measurement |

The original notebook focuses its anomaly detection experiments on `INUT`.

The dataset is kept outside version control until its provenance and redistribution permissions are verified.

## Project Structure

```text
electrical-transformer-anomaly-detection/
├── README.md
├── requirements.txt
├── .gitignore
├── notebooks/
│   └── 01_transformer_anomaly_detection.ipynb
├── data/
│   └── README.md
└── results/
    └── .gitkeep
```

## Methodology

### 1. Exploratory Data Analysis

The notebook begins by examining:

- Dataset dimensions.
- Data types.
- Missing values.
- Descriptive statistics.
- Correlations between numerical measurements.
- Distribution of the neutral current variable (`INUT`).

The original analysis found no null values in the loaded dataset.

A correlation heatmap is used to inspect relationships between voltage and current measurements.

### 2. Rolling Mean and Standard Deviation

The first anomaly detection approach uses rolling statistics.

Configuration:

- Window size: 20 observations.
- Threshold: 2 rolling standard deviations.

An observation is marked as unusual when its absolute deviation from the rolling mean exceeds the defined threshold.

This method detects local changes relative to the surrounding measurements.

### 3. Z-Score

The second method applies a global Z-Score to `INUT`.

Configuration:

- Threshold: absolute Z-Score greater than 3.

The method identifies observations that are statistically distant from the global mean.

Unlike the rolling method, this approach does not explicitly consider local temporal behavior.

### 4. Isolation Forest

Isolation Forest is used to identify unusual observations through random partitioning.

Two configurations are evaluated:

```python
IsolationForest(
    contamination=0.05,
    random_state=42
)
```

and:

```python
IsolationForest(
    contamination=0.01,
    random_state=42
)
```

The model is trained using the `INUT` variable.

Predictions use:

- `-1`: Anomalous observation.
- `1`: Inlier.

Anomaly scores are calculated using `decision_function()`.

The experiments examine how the contamination parameter affects the number of observations flagged as unusual.

### 5. Local Outlier Factor

Local Outlier Factor is applied to identify observations with unusual local density.

Configuration:

```python
LocalOutlierFactor(
    n_neighbors=20,
    contamination=0.05,
    metric="euclidean",
    novelty=False
)
```

The experiment uses the neutral current variable (`INUT`).

LOF compares the density of each observation with the density of nearby observations.

Unlike a simple global threshold, it can identify observations that are locally unusual.

## Experimental Results

The original academic notebook reports the following anomaly counts:

| Method | Flagged Observations |
|---|---:|
| Rolling Mean and Standard Deviation | 1,126 |
| Z-Score | 72 |
| Isolation Forest — 5% contamination | 963 |
| Isolation Forest — 1% contamination | 189 |

The notebook also implements LOF with 5% contamination and visualizes its results.

The counts were obtained from the saved notebook outputs and have not yet been independently reproduced in a clean environment.

### Interpretation

The methods identify different patterns:

**Rolling Statistics**

Detects local deviations and produced the largest number of flagged observations in the reported comparison.

**Z-Score**

Identifies more extreme global deviations and produced fewer flagged observations.

**Isolation Forest**

Flags observations according to the model's isolation scores and the selected contamination threshold.

**Local Outlier Factor**

Examines unusual local density patterns, which do not necessarily correspond to the largest signal peaks.

### Important Distinction

Flagged observations are **not confirmed electrical failures**.

The dataset used in the experiment does not provide validated incident labels for calculating detection precision, recall, or false-positive rates against real equipment failures.

Therefore, the results demonstrate anomaly detection behavior, not proven fault diagnosis accuracy.

## Installation

### 1. Clone the Repository

```bash
git clone https://github.com/ngualteros/Electrical-Transformer-Anomaly-Detection.git
```

### 2. Create a Virtual Environment

```bash
python -m venv .venv
```

Activate it on Windows PowerShell:

```powershell
.\.venv\Scripts\Activate.ps1
```

### 3. Install Dependencies

```bash
python -m pip install -r requirements.txt
```

### 4. Prepare the Dataset

Place the original CSV file at:

`data/muinar07_act3_CurrentVoltage.csv`

The original academic notebook uses a relative filename and requires a path adjustment to match the repository structure.

### 5. Run the Notebook

```bash
python -m jupyter notebook
```

Open:

`notebooks/01_transformer_anomaly_detection.ipynb`

Execute the notebook cells sequentially.

## Limitations

- The experiments focus on a single variable (`INUT`).
- The methods are used primarily for exploratory anomaly detection.
- No verified electrical fault labels are available.
- Contamination parameters are assumptions, not measured fault frequencies.
- Results depend on the selected detection thresholds.
- The original timestamp column is not converted to a datetime index.
- Several plots use observation indices rather than real timestamps.
- LOF produces a warning related to duplicate values.
- The original notebook contains some deprecated Seaborn visualization calls.
- The experiments do not demonstrate a deployed monitoring or alerting system.

## Future Improvements

- Parse and validate measurement timestamps.
- Verify chronological ordering and sampling intervals.
- Extend anomaly detection to multiple voltage and current features.
- Compare univariate and multivariate approaches.
- Investigate how duplicate values affect LOF.
- Develop a common anomaly comparison table.
- Analyze disagreement between detection methods.
- Introduce time-aware validation strategies.
- Build a monitoring dashboard prototype.
- Explore integration with an incident triage workflow.

These are potential improvements, not completed functionality.

## Academic Context

**Course:** Unsupervised Machine Learning

**Degree:** Master's in Artificial Intelligence

**University:** Universidad Internacional de La Rioja (UNIR)

This project originated from a group academic laboratory completed by:

- Nicolás Gualteros Pérez
- Camilo Linares Villegas
- Esteban José Puello Fuentes

The academic laboratory involved completing and analyzing anomaly detection experiments using a provided notebook structure.

The original group contributions should be acknowledged when presenting or extending the project.

## Responsible Use

This project is intended for educational and experimental purposes.

Detected anomalies should not be interpreted as verified equipment failures without additional operational context, domain expertise, and validated fault records.

The project is not intended for direct use in safety-critical industrial monitoring.

## Author and Contributor

**Nicolás Gualteros Pérez**

Systems Engineer | Master's in Artificial Intelligence

Application & Production Support | Machine Learning | Automation | Observability

LinkedIn: https://www.linkedin.com/in/nicogualteros
