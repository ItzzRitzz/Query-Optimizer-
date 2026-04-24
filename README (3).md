# 🏥 Hospital Readmission Analysis

![Dashboard Preview](dashboard/dashboard_preview.png)

> **An end-to-end data analytics project** that identifies key drivers of 30-day hospital readmissions in diabetic patients, combining Python-based data processing with an interactive Power BI dashboard to support clinical decision-making.

---

## 📋 Table of Contents

- [Project Overview](#-project-overview)
- [Business Problem](#-business-problem)
- [Dataset](#-dataset)
- [Project Structure](#-project-structure)
- [Technologies Used](#-technologies-used)
- [Project Workflow](#-project-workflow)
- [Key DAX Measures](#-key-dax-measures)
- [Dashboard Features](#-dashboard-features)
- [Key Findings](#-key-findings)
- [Getting Started](#-getting-started)
- [Results & Impact](#-results--impact)

---

## 📌 Project Overview

Hospital readmission within 30 days is a critical quality metric and a major cost driver in healthcare. This project analyzes **69,973 diabetic patient encounters** to uncover which patient characteristics, diagnoses, and treatment patterns are associated with early readmission.

The project delivers:
- A **cleaned, feature-engineered dataset** ready for analysis
- An **interactive 2-page Power BI dashboard** with KPI cards, trend charts, demographic breakdowns, and risk segmentation
- **Actionable clinical insights** to help hospitals target high-risk patients for intervention

---

## 🎯 Business Problem

> *"Which patients are most likely to be readmitted within 30 days, and what factors drive that risk?"*

Unplanned readmissions cost the U.S. healthcare system billions annually and are a key metric used by CMS (Centers for Medicare & Medicaid Services) to assess hospital performance. Identifying high-risk patients at discharge allows clinical teams to intervene proactively — with follow-up calls, adjusted medication regimens, or extended care plans.

---

## 📊 Dataset

| Property | Value |
|---|---|
| **Source** | UCI Machine Learning Repository — Diabetes 130-US Hospitals (1999–2008) |
| **Records** | 69,973 patient encounters |
| **Features** | 30 columns (after cleaning and feature engineering) |
| **Target Variable** | `readmitted` — `<30` (within 30 days), `>30` (after 30 days), `NO` (not readmitted) |

### Key Columns

| Column | Description |
|---|---|
| `encounter_id` | Unique identifier for each hospital visit |
| `patient_nbr` | Unique patient identifier |
| `race`, `gender`, `age` | Patient demographics |
| `age_group` | Bucketed age: `<40`, `40-59`, `60-74`, `75+` |
| `time_in_hospital` | Length of stay (days) |
| `num_lab_procedures` | Number of lab tests during the encounter |
| `num_medications` | Number of distinct medications administered |
| `diag_1`, `diag_2`, `diag_3` | ICD-9 primary and secondary diagnosis codes |
| `insulin` | Insulin dosage change (`Up`, `Down`, `Steady`, `No`) |
| `diabetesMed` | Whether diabetes medication was prescribed |
| `readmitted` | Readmission outcome (target variable) |
| `readmitted_binary` | Binary readmission flag (1 = readmitted within 30 days) |
| `high_utiliser` | Flag for patients with high prior inpatient use |
| `los_bucket` | Length-of-stay bucket (Short, Medium, Long, Very Long) |
| `diagnosis_category` | Derived: Circulatory, Respiratory, Digestive, Diabetes, etc. |

### Readmission Distribution

| Outcome | Count | % |
|---|---|---|
| Not Readmitted (NO) | 41,474 | 59.3% |
| Readmitted > 30 days | 22,222 | 31.8% |
| Readmitted < 30 days | 6,277 | 8.9% |

---

## 📁 Project Structure

```
Hospital-Readmission-Analysis/
│
├── data/
│   └── raw/                        # Original unmodified source data
│       └── diabetic_data.csv
│
├── processed/                      # Cleaned and feature-engineered data
│   └── cleaned_readmission_data.csv
│
├── notebook/                       # Jupyter notebooks for EDA & cleaning
│   └── hospital_readmission_eda.ipynb
│
├── src/                            # Python scripts for data processing
│   └── data_cleaning.py
│
├── dashboard/                      # Power BI dashboard files
│   └── Hospital_Readmission.pbix
│
└── README.md
```

---

## 🛠️ Technologies Used

### Data Processing & Analysis

| Technology | Version | Purpose |
|---|---|---|
| **Python** | 3.10+ | Core data processing language |
| **Pandas** | 2.x | Data cleaning, transformation, feature engineering |
| **NumPy** | 1.x | Numerical operations |
| **Matplotlib / Seaborn** | Latest | Exploratory data visualizations |
| **Jupyter Notebook** | Latest | Interactive EDA environment |

### Business Intelligence & Visualization

| Technology | Purpose |
|---|---|
| **Power BI Desktop** | Interactive dashboard, DAX calculations, visual reporting |
| **DAX (Data Analysis Expressions)** | Custom measures and calculated columns |
| **Power Query (M Language)** | In-Power BI data transformation (if applicable) |

### Version Control

| Technology | Purpose |
|---|---|
| **Git** | Source control |
| **GitHub** | Repository hosting and collaboration |

---

## 🔄 Project Workflow

### Step 1 — Data Acquisition
- Download raw dataset from the UCI Machine Learning Repository
- Place in `data/raw/diabetic_data.csv`

### Step 2 — Exploratory Data Analysis (EDA)
Open the notebook:
```bash
cd notebook/
jupyter notebook hospital_readmission_eda.ipynb
```
Explores distributions, missing values, outliers, and correlations across all 50 original features.

### Step 3 — Data Cleaning (`src/data_cleaning.py`)
Run the cleaning script:
```bash
python src/data_cleaning.py
```

This script performs:
- Removal of duplicate patient encounters (keeps first visit per patient)
- Replacement of `?` placeholders with `NaN` / meaningful labels
- Dropping high-null columns (e.g., `weight`, `payer_code`, `medical_specialty` > 50% null)
- Standardising `age` from ranges (`[70-80)`) to midpoint numeric values
- Encoding the target variable (`readmitted`) as both categorical and binary
- Feature engineering (see below)
- Output saved to `processed/cleaned_readmission_data.csv`

### Step 4 — Feature Engineering
New derived columns created during cleaning:

| New Column | Logic |
|---|---|
| `age_numeric` | Midpoint of age bucket (e.g., `[70-80)` → `75`) |
| `age_group` | Grouped buckets: `<40`, `40-59`, `60-74`, `75+` |
| `readmitted_binary` | `1` if readmitted `<30`, else `0` |
| `high_utiliser` | `1` if `number_inpatient >= 3` |
| `on_insulin` | `1` if insulin is not `No` |
| `los_bucket` | Length-of-stay: Short (1-3d), Medium (4-7d), Long (8-14d), Very Long (15+d) |
| `total_medications_changed` | Count of medication columns with `Up`/`Down` change |
| `diagnosis_category` | ICD-9 code grouped into clinical categories (see DAX below) |

### Step 5 — Power BI Dashboard
1. Open `dashboard/Hospital_Readmission.pbix` in Power BI Desktop
2. Load `processed/cleaned_readmission_data.csv` as the data source
3. Add the `diagnosis_category` **calculated column** (not measure) via **Data View → New Column**
4. Explore the 2-page dashboard

---

## 📐 Key DAX Measures & Columns

### Calculated Column — `diagnosis_category`
> ⚠️ Must be created as a **Calculated Column** (Data View → New Column), NOT a measure.

```dax
diagnosis_category =
VAR d = 'cleaned_readmission_data'[diag_1]
VAR dNum = IFERROR(VALUE(d), BLANK())
RETURN
SWITCH(
    TRUE(),
    d = "" || ISBLANK(d), "Other",
    LEFT(d, 3) = "250", "Diabetes",
    NOT ISBLANK(dNum) && dNum >= 390 && dNum <= 459, "Circulatory",
    NOT ISBLANK(dNum) && dNum >= 460 && dNum <= 519, "Respiratory",
    NOT ISBLANK(dNum) && dNum >= 520 && dNum <= 579, "Digestive",
    NOT ISBLANK(dNum) && dNum >= 580 && dNum <= 629, "Genitourinary",
    NOT ISBLANK(dNum) && dNum >= 710 && dNum <= 739, "Musculoskeletal",
    "Other"
)
```

### Key Measures

```dax
-- 30-day Readmission Rate
30-Day Readmission Rate =
DIVIDE(
    COUNTROWS(FILTER('cleaned_readmission_data', 'cleaned_readmission_data'[readmitted] = "<30")),
    COUNTROWS('cleaned_readmission_data'),
    0
)

-- High Risk %
High Risk % =
DIVIDE(
    COUNTROWS(FILTER('cleaned_readmission_data', 'cleaned_readmission_data'[high_utiliser] = 1)),
    COUNTROWS('cleaned_readmission_data'),
    0
)
```

---

## 📈 Dashboard Features

### Page 1 — Executive Overview
- **KPI Cards**: Total encounters, 30-day readmission rate, average length of stay, high-utiliser %
- **Readmission by Age Group**: Bar chart showing readmission rates across `<40`, `40-59`, `60-74`, `75+`
- **Readmission by Gender**: Donut/bar comparison
- **Readmission by Diagnosis Category**: Breakdown across Circulatory, Diabetes, Respiratory, Digestive, etc.
- **Readmission by LOS Bucket**: How length of stay correlates with readmission

### Page 2 — Clinical Deep Dive
- **Prior Inpatient Visits vs Readmission Rate**: Line chart showing how prior visits predict future readmission
- **Medication Count Distribution**: Histogram of medication burden by readmission outcome
- **Insulin Use Impact**: Readmission rates segmented by insulin change status
- **Slicers**: Filter by age group, gender, diagnosis category, LOS bucket

---

## 🔍 Key Findings

1. **Prior inpatient visits are the strongest predictor** — patients with 3+ prior inpatient visits have dramatically higher 30-day readmission rates (~11–12%) versus patients with 0 prior visits
2. **Circulatory and Respiratory diagnoses** drive the highest readmission volumes
3. **Longer stays paradoxically correlate with higher readmission** — patients with stays > 8 days are more clinically complex and return sooner
4. **High utilisers (3+ prior inpatient visits)** represent a disproportionate share of readmissions and should be prioritised for discharge planning
5. **Insulin management matters** — patients with insulin dose changes show different readmission patterns compared to those on stable regimens
6. **Older patients (75+)** have the highest absolute readmission counts but the pattern is consistent across age groups when controlling for diagnosis

---

## 🚀 Getting Started

### Prerequisites
- Python 3.10 or higher
- Power BI Desktop (free download from Microsoft)
- Git

### Installation

```bash
# 1. Clone the repository
git clone https://github.com/vjallewar5858/Hospital-Readmission-Analysis.git
cd Hospital-Readmission-Analysis

# 2. Install Python dependencies
pip install pandas numpy matplotlib seaborn jupyter

# 3. Run data cleaning
python src/data_cleaning.py

# 4. (Optional) Launch EDA notebook
jupyter notebook notebook/hospital_readmission_eda.ipynb

# 5. Open dashboard
# Open dashboard/Hospital_Readmission.pbix in Power BI Desktop
```

### Data Source
The raw dataset is available from the [UCI Machine Learning Repository](https://archive.ics.uci.edu/ml/datasets/Diabetes+130-US+hospitals+for+years+1999-2008).

---

## 📊 Results & Impact

| Metric | Value |
|---|---|
| Total patient encounters analysed | 69,973 |
| Overall 30-day readmission rate | **8.97%** |
| Patients flagged as high utilisers | ~11% |
| Diagnosis categories identified | 7 |
| Dashboard pages | 2 |

---

## 👤 Author

**Vaibhav Jallewar**
- GitHub: [@vjallewar5858](https://github.com/vjallewar5858)

---

## 📄 License

This project is for educational and portfolio purposes. The dataset is sourced from the UCI Machine Learning Repository under their terms of use.

---

*Built with Python 🐍 and Power BI 📊*
