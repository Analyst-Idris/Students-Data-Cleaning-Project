# Students-Data-Cleaning-Project
A compact, real-world data cleaning workflow using Python (pandas + NumPy) on a synthetic student dataset. The focus is on clarity of decisions, why each step matters, and how to reproduce the pipeline end-to-end.

![Image](https://github.com/user-attachments/assets/bf0e1333-05f9-4a19-b579-58e45f7991f1)

## 🔍 Problem Statement

**Raw education data often comes messy:** duplicate rows, inconsistent text casing, mixed date formats, and non-numeric scores hidden as strings. Left uncleaned, this leads to misleading insights (wrong averages, broken time trends, unreliable KPIs).

**Goal:** transform raw student records into a clean, analysis-ready table with consistent types, standardized categories, and no missing critical values.


🧾 Dataset (Columns & Definitions)
| Column Name | ColumnType (final) | Description | Notes / Constraints |
| :--- | :--- | :--- | :--- |
| `Student_Id` | `int64` | Unique identifier per student | Must be unique, no nulls |
| `Name` | `object` | Student full name | Standardized to Title Case |
| `Age` | `float64` | Student age | Missing filled with rounded mean; sanity range check recommended |
| `Gender` | `object` | Gender label | Standardized to Title Case (e.g., “Male”, “Female”) |
| `Grade` | `int` | Academic grade level | Normalized from strings like “10th” → 10 |
| `Math Score` | `float64` | Math score | Missing filled with mean |
| `English Score` | `float64` | English score | Coerced to numeric (invalid → NaN) |
| `Science Score` | `int64` | Science score | Expected 0–100 (validate as needed) |
| `Enrolled Date` | `datetime64[ns]` | Enrollment date | Mixed formats standardized to ISO; parsed to datetime |
| `Remarks` | `object` | Qualitative remark | Title Case for consistent grouping |

## 🧠 Thought Process & Key Decisions

Below is the **why** behind each action, what I looked for, what I fixed, and the trade-offs I accepted. Code references align with the notebook.

## 1.	Load & Inspect (shape, head/tail, info)

*	**Why:** Understand structure, size, likely issues (nulls, wrong dtypes).
* **Decision:** Keep the raw DataFrame intact; iterate with light transforms to avoid irreversible changes too early.

## 2.	Duplicates (duplicated().sum() → drop_duplicates)

* **Why:** Duplicates inflate counts, bias averages.
* **Decision:** Drop full-row duplicates; if your domain requires a key check, also verify Student_Id uniqueness after.

## 3.	Rename columns to readable, business-friendly names

* **Why:** Clear names reduce downstream errors and make reports more understandable.
* **Decision:** Use Title Case with spaces for human readability (e.g., Math Score). (Accepted trade-off: column names with spaces require bracketed access in code; I prefer readability for stakeholders.)

## 4.	Name standardization (lower → Title Case)

* **Why:** Prevents “John Doe” vs “john doe” mismatch in grouping and deduplication.
* **Decision:** Title case is a neutral, readable standard.

## 5.	Age cleaning (inspect unique → fillna with rounded mean)
* **Why:** Age is numeric and often missing.
* **Decision:** Use rounded mean to maintain distribution.
* **Trade-off:** Median is more robust to outliers; mean is fine here given the small, fairly normal distribution.

## 6.	Gender standardization (lower → Title Case)
* **Why:** Avoids “male/Male/MALE” variants.
* **Decision:** Normalize to a readable canonical form. 

## 7.	Grade normalization (map “10th”→10, “11th”→11 → cast to int)
* **Why:** Mixed formats break sorting and numeric operations.
* **Decision:** Map string grades to integers and enforce dtype.

## 8.	Score cleaning
* **Math Score:** fill missing with mean.
* **English Score:** coerce to numeric (errors='coerce') then handle any resulting NaNs.
* **Why:** Scores should be numeric; strings like "85 " or "N/A" must be fixed.
* **Decision:** Use coercion to systematically identify non-numeric values. Fill missing in a consistent way (mean)

## 9.	Date normalization (replace mixed formats → to_datetime)
* **Why:** Mixed formats ('10-06-2022', '06/12/2022', '2022/06/11') block time analysis.
* **Decision:** Standardize to ISO (YYYY-MM-DD) and parse to datetime with errors='coerce' to surface invalid rows as NaT for explicit handling.

## 10.	Remarks standardization (lower → Title Case)
* **Why:** Consistent labels for grouping (“Good”, “Excellent”).
* **Decision:** Title case for presentation; if using as category features, consider a curated mapping.

## 11.	Final Audit (info)
* **Why:** Validate dtypes, null counts, memory usage.
* **Decision:** Assert no remaining nulls in required fields; log any intentional coercions.


![Image](https://github.com/user-attachments/assets/926aec17-f155-412b-a29e-207f55c56901)

For any collaborative work or gigs, reach out to me at:

📧 Email: oladejoidris55@gmail.com 📞 Phone: 07025062857






  







