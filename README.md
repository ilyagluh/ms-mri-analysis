# Project MMD: Analysis of MRI Data of Patients with Multiple Sclerosis

**Author:** Ilya Glukhov

---

## Description

This project is an end-to-end data analysis of MRI data from the MosMedData SCLEROSIS dataset. The goal is to identify differences in MRI scanning protocols between patients with multiple sclerosis (MS) and a control group (normal).

**Key tasks completed:**
- ETL pipeline for extracting DICOM metadata
- Exploratory Data Analysis (EDA)
- Statistical A/B testing
- Interactive dashboard in Yandex DataLens

---

## Dataset

This project uses the **MosMedData MRI with signs of multiple sclerosis type II (SCLEROSIS)** dataset.

**Permanent link:** [https://mosmed.ai/datasets/](https://mosmed.ai/datasets/)

**Release Date:** 13.06.2023

**Dataset Version:** 1.0

**Citation:**
> Datasets of Research and Practical Clinical Center for Diagnostics and Telemedicine Technologies of the Moscow Health Department [Electronic resource]. – 2022. – URL: https://mosmed.ai/datasets/

**Contributors:**
- Vladzymyrskyy A.V. [1]
- Shulkin I.M. [1]
- Omelyanskaya O.V. [1]
- Chetverikov S.F. [1]
- Arzamasov K.M. [1]
- Khoruzhaya A.N. [1]
- Kremneva E.I. [1]
- Smorchkova A.K. [1]
- Andreychenko A.E. [1]

**Affiliation:**
> [1] Research and Practical Clinical Center for Diagnostics and Telemedicine Technologies of the Moscow Health Care Department.

**License:** Creative Commons Attribution-NonCommercial-NoDerivs 3.0 Unported (CC BY-NC-ND 3.0)

**Dataset structure:**
- 172 patients (86 MS / 86 Normal)
- Age range: 18–85 years (median 46)
- Gender: 56 M / 116 F
- Format: DICOM

---

## Technologies

| Category | Tools |
|----------|-------|
| Data extraction | Python, pydicom |
| Data processing | pandas, numpy |
| Visualization | matplotlib, seaborn |
| Statistical analysis | scipy (Mann-Whitney U test) |
| Database | SQLite |
| Dashboard | Yandex DataLens |

---

## ETL Pipeline

### 1. Extract
- Loaded `labels.xlsx` (sclerosis labels)
- Extracted DICOM metadata: manufacturer, echo time (TE), repetition time (TR), slice thickness, number of series, number of slices

### 2. Transform
- Cleaned missing values
- Categorized Echo Time: short (<60ms), medium (60-100ms), long (>100ms)
- Grouped manufacturers: TOSHIBA vs OTHER

### 3. Load
- Saved cleaned data to CSV and SQLite
- Created interactive dashboard in DataLens

---

## Results

### A/B Test: Number of Series (num_series)

**Hypothesis:** The number of MRI series differs between MS and Normal groups.

**Test:** Mann-Whitney U test (non-parametric)

| Metric | MS (1) | Normal (0) |
|--------|--------|------------|
| Median num_series | 11 | 10 |
| Mean num_series | 11.13 | 10.10 |

**p-value:** 0.0346

**Conclusion:** Statistically significant difference (p < 0.05). Patients with MS undergo on average 1 more MRI series, which may reflect extended protocols (post-contrast T1, DWI, FLAIR).

---

### Key Findings from EDA

| Parameter | MS (1) | Normal (0) |
|-----------|--------|------------|
| Echo Time (TE) median | 100 ms | 100 ms |
| Slice thickness median | 5.0 mm | 5.0 mm |
| Flip angle median | 90° | 90° |
| Manufacturer | Toshiba dominates (84%) | Toshiba dominates (84%) |

**Outliers:**
- slice_thickness: 33.3% (variability in protocols)
- num_slices: 34.5% (correlates with slice thickness)

---

## Dashboard

Interactive dashboard is available at:

🔗 **[Yandex DataLens Dashboard](https://datalens.ru/lyugqv1y8kiq5)**

The dashboard includes:
- Distribution of patients by scanner manufacturer
- Echo Time categories distribution
- Proportion of MS by Echo Time categories
- Summary statistics table

---

## Repository Structure

Project MMD/
├── Project MMD ENG.ipynb # Main Jupyter notebook with ETL + A/B test
├── cleaned_data.csv # Cleaned metadata (ready for analysis)
├── sclerosis.db # SQLite database (patients + manufacturers)
├── requirements.txt # Python dependencies
├── .gitignore # Ignored files (DICOM, temp files)
├── README.md # Project documentation
│
├── plots/ # Visualization outputs
│ ├── 01_manufacturer_bar.png
│ ├── 02_te_category_bar.png
│ ├── 03_te_category_vs_ms_stacked.png
│ └── ab_test_num_series.png
│
└── SCLEROSIS21_172/ # NOT included in repo (DICOM files)
└── ... (dataset folder - see .gitignore)

## How to Run

### Prerequisites

- Python 3.8 or higher
- Git
- Access to the MosMedData SCLEROSIS dataset (download from mosmed.ai)

### Step 1: Clone the repository

git clone https://github.com/your-username/ms-mri-analysis.git
cd ms-mri-analysis

### Step 2: Install dependencies

pip install -r requirements.txt

### Step 3: Download and place the dataset

1. Download the MosMedData MRI with signs of multiple sclerosis type II dataset from https://mosmed.ai/datasets/
2. Extract the archive
3. Place the SCLEROSIS21_172 folder in the project root directory

Expected structure after this step:

ms-mri-analysis/
├── SCLEROSIS21_172/          # Dataset folder
│   ├── labels.xlsx
│   ├── EXCELMR1_GB1_anon/
│   └── ...
├── Project MMD ENG.ipynb
├── cleaned_data.csv
├── requirements.txt
└── ...

### Step 4: Run the Jupyter notebook

jupyter notebook "Project MMD ENG.ipynb"

### Step 5: Execute cells in order

Run all cells sequentially from top to bottom. The notebook will:
- Extract metadata from DICOM files 
- Transform and clean the data
- Save results to CSV and SQLite
- Generate plots in the plots/ folder
- Perform A/B test and display results

### Step 6: Explore the dashboard (optional)

The interactive dashboard is available at:
https://datalens.yandex/your-link-here

### Notes

- The DICOM extraction step processes 172 patients and may take several minutes
- Raw DICOM files are not included in this repository (see .gitignore)
- The dataset is licensed under CC BY-NC-ND 3.0 (non-commercial use only)
