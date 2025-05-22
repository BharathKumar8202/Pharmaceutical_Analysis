# Clinical Trials Analysis Project

## 📌 Introduction

This notebook analyses clinical trials registered in the USA as part of a project for a pharmaceutical company. The primary objective is to extract meaningful insights into the clinical research landscape by examining trends, characteristics, and key attributes within the provided dataset.

The dataset contains comprehensive information on a wide range of clinical trials. It was cleaned and preprocessed to address issues such as missing or inconsistent data, standardise formats, and ensure readiness for detailed exploratory analysis and interpretation.

---

## 📊 Data Import and Preprocessing

The dataset was imported into **Spark** using the `read.csv()` function with automatic schema inference enabled and the first row treated as headers, as outlined in the assessment brief.

To prepare the data for analysis, several preprocessing steps were implemented:

- **Column Selection:** Unnecessary columns like `Study Title`, `Acronym`, `Sponsor`, `Collaborators`, `Enrollment`, and `Study Design` were removed.
- **Schema Standardisation:** Column names were standardised by replacing spaces with underscores and converting them to lowercase.
- **Data Standardisation:** All string values were converted to uppercase to eliminate case sensitivity issues.
- **Data Quality Assessment:** A null value analysis was conducted. No imputation was performed as artificial values could compromise the integrity of medical records.
- **Data Type Verification:** Column data types were verified and converted as needed.

The resulting clean DataFrame was then registered as a **temporary view** to enable SQL querying for the following analysis steps.

---

## 📈 Question 1: Frequency of Clinical Trial Types

The objective here was to list all clinical trial types along with their corresponding frequencies, ordered from most to least frequent.

**SQL Query Approach:**

- Filtered out null `study_type` values.
- Grouped data by `study_type`.
- Counted each type using `COUNT(*)`.
- Sorted results in descending order of frequency.

**Reference image:**

<img width="1196" alt="Screenshot 2025-05-22 at 18 46 22" src="https://github.com/user-attachments/assets/e0e800ba-df57-4f49-afd7-78bf646523de" />


### 🔍 Findings — Question 1

- **Interventional trials:** 399,888 (76.6%)
- **Observational trials:** 120,906 (23.2%)
- **Expanded Access trials:** 966 (0.2%)

This highlights the predominance of interventional research and the rarity of expanded access trials for life-threatening conditions.

---

## 📈 Question 2: Top 10 Conditions in Clinical Trials

Objective: Identify the top 10 conditions based on frequency.

**Steps:**

- Identified `Conditions` values are pipe-delimited (`|`).
- Applied:
  - `SPLIT(TRIM(Conditions), '\\|')`
  - `ARRAY_DISTINCT()`
  - `EXPLODE()`  
- Grouped and counted occurrences.
- Filtered out nulls.
- Sorted and limited to the top 10.

**Reference image:**

<img width="1194" alt="Screenshot 2025-05-22 at 18 47 16" src="https://github.com/user-attachments/assets/7e25eb94-57c8-4a67-869a-a4db74bb0737" />


### 🔍 Findings — Question 2

Top 3 conditions:

1. **HEALTHY** (~9,500)
2. **BREAST CANCER** (~5,800)
3. **OBESITY** (~4,000)

Key observations:

- Chronic issues like STROKE, HYPERTENSION, and DEPRESSION each appeared 3,000–4,000 times.
- OBESITY appeared prominently due to its link with chronic conditions.
- CANCER as a category may have been divided into specific types like lung or colon cancer.

---

## 📈 Question 3: Average Clinical Trial Length (Months)

Objective: Calculate the average clinical trial duration in months.

**Steps:**

- Converted `Completion_Date` and `Start_Date` to timestamps using `TO_TIMESTAMP()`.
- Extracted date portions with `DATE()`.
- Calculated duration using `MONTHS_BETWEEN()`.
- Averaged with `AVG()`.
- Rounded result with `ROUND()`.

**Reference image:**

<img width="1195" alt="Screenshot 2025-05-22 at 18 47 38" src="https://github.com/user-attachments/assets/982b4204-90cf-4aad-9f29-b5f01c028d09" />


### 🔍 Findings — Question 3

- **Average clinical trial duration:** 36 months (3 years)

This aligns with typical trial complexities including recruitment, treatment, and follow-ups, serving as a valuable benchmark for research planning.

---

## 📈 Question 4: Yearly Count of Completed Diabetes Studies

**Steps:**

- Split and extracted individual conditions.
- Filtered for diabetes-related studies.
- Selected studies with "COMPLETED" status.
- Counted occurrences by year using `YEAR()`.

**Reference image:**

<img width="1199" alt="Screenshot 2025-05-22 at 18 48 03" src="https://github.com/user-attachments/assets/842e3896-4528-465a-9f58-0bc2d3133c29" />


### 🔍 Findings — Question 4

Trends observed from **1995 to 2024**:

- **Growth Phase (1995–2010):** Steady increase, peaking at 144 in 2010.
- **Stabilisation (2011–2019):** High activity, peaking again in 2015 (147).
- **Recent Decline (2020–2024):** Drop-off likely from COVID-19 disruptions and shifts in research priorities.

This captured the evolving focus on diabetes research over the past 30 years.

---

## 📌 Conclusion

The analysis revealed:

- Interventional studies dominate clinical trials (76%).
- Most common focus areas include breast cancer, obesity, stroke, and hypertension.
- Average trial duration is 3 years.
- Diabetes research trends reflect global health priorities, peaking in the mid-2010s before recent decline.

The findings provide essential context for understanding clinical trial dynamics and health research trends in the USA.

## Running the Project

> This notebook is best run on [Databricks Community Edition](https://community.cloud.databricks.com/).

### Steps:
1. Upload the notebook `00787835_Task2.html`
2. Attach to a Spark cluster
3. Run cells in sequence


---

