
# Electrical Transformer Sensor Dataset

## Overview

This directory is used for the dataset analyzed in the Electrical Transformer Anomaly Detection project.

The original academic laboratory uses electrical voltage and current measurements collected through IoT devices.

## Dataset File

Required local filename:

`muinar07_act3_CurrentVoltage.csv`

The dataset contains:

- 19,352 observations.
- 11 columns.
- A measurement timestamp.
- Voltage and current measurements.

## Dataset Structure

| Column | Description |
|---|---|
| DeviceTimeStamp | Measurement timestamp |
| VL1, VL2, VL3 | Phase voltages |
| IL1, IL2, IL3 | Phase currents |
| VL12, VL23, VL31 | Line-to-line voltages |
| INUT | Neutral current |

## Local Setup

Copy the original dataset into:

```text
data/
├── README.md
└── muinar07_act3_CurrentVoltage.csv
```

The dataset is excluded from version control.

## Data Loading

When the notebook runs from the `notebooks/` directory:

```python
import pandas as pd

df = pd.read_csv(
    "../data/muinar07_act3_CurrentVoltage.csv"
)
```

## Data Availability

The dataset was provided as part of an academic activity.

Its original publication source and redistribution permissions should be verified before making it publicly available.
