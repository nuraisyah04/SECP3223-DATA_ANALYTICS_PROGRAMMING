# 📊 SECP3223: Data Analytic Programming — Assignment Reflection

---

## Assignment 1 — QuickRide: Lists, Dictionaries, Functions & File I/O

We analysed ride-sharing data from a fictional company called QuickRide, completing 8 tasks: writing and reading a CSV file, computing city-level fare averages, summarising driver performance, slicing the dataset, lambda-based sorting, and writing results to a text file.

**Key things I learned:**

- `csv.DictReader` automatically maps each row to a dictionary using the header — a clean way to parse CSV without manual indexing.
- Every field comes back as a string when reading CSV; explicit type conversion (e.g. `float(row['fare'])`) is always needed before any calculation.
- Building aggregation dictionaries from scratch (accumulating totals and counts per city/driver) gave me a real appreciation for what pandas does automatically under the hood.
- Lambda functions with `sorted()` — e.g. `key=lambda x: x[1]['total_fare']` — make ranking operations concise and readable without needing a separate named function.

**Challenge:** Python slicing is exclusive at the upper bound, so `data[start:end+1]` was needed to include the last element — a small but easy mistake to make.

---

## Assignment 3 — Rainfall Data Analysis (pandas, Aggregation & Visualisation)

This assignment used monthly rainfall data from weather stations across four regions, split into three parts: data exploration, grouping and aggregation, and visualisation.

**Key things I learned:**

- Starting every analysis with `.head()`, `.isnull().sum()`, and `.describe()` quickly reveals data quality issues before they cause silent errors downstream.
- `.groupby().agg()` with multiple functions in one call is far more efficient than looping — and produces multi-index DataFrames that, once understood, are powerful to work with.
- Different chart types answer different questions: bar charts for comparisons, boxplots for spread and outliers, heatmaps for patterns across two categorical dimensions, and faceted line plots (`sns.relplot`) for seeing trends broken down by multiple groups simultaneously.
- Urban stations recorded higher average rainfall (72.4 mm) than rural ones (56.8 mm) — a finding that raised the question of whether urbanisation affects rainfall or just where stations are placed.

**Challenge:** The CSV was not clean on load — manual column assignment and `pd.to_numeric(errors='coerce')` were required before any analysis could run reliably.

---

## Group Project — CO2 Emissions from Cars

The most substantial piece of work in the course, assessed across a Jupyter Notebook report (Part A), the Python code (Part B), and a 10-minute video presentation (Part C).

We worked with two datasets — `FuelConsumption.csv` (2014 global vehicle emission data) and `Cars2025.csv` (Malaysian car registrations) — and had to link them to estimate the emissions profile of cars on Malaysian roads today. Phase I covered data preparation and cleaning; Phase II covered analytics, machine learning, visualisation, and the video.

**Key things I learned:**

- Merging two real-world datasets is rarely straightforward — car names across the two CSVs used different conventions, requiring string normalisation (lowercasing, stripping punctuation) before any join could work.
- The full analytics pipeline (load → clean → wrangle → aggregate → model → visualise → communicate) showed me how each step depends on the previous one; errors caught early prevent corrupted results later.
- Machine learning for clustering car models by emission level shifted the task from describing the data to discovering structure within it — choosing between unsupervised clustering and supervised classification required understanding what question each method actually answers.
- Code quality is not just about correctness — with the code directly assessed (Part B), clear variable names, no unused cells, and explanatory comments matter because they let others trust and reproduce the analysis.

**Challenge:** Deciding between clustering and classification was not obvious. We ran trial approaches for both before settling on one that best matched our research question — a process that taught me that method selection is an analytical decision, not just a technical one.

---

## Overall Reflection

| | Assignment 1 | Assignment 3 | Group Project |
|---|---|---|---|
| **Data handling** | Manual CSV I/O | pandas, missing value imputation | Multi-dataset merging |
| **Aggregation** | Dictionary loops | `.groupby().agg()` | Cross-dataset group ops |
| **Analysis** | Summary stats | Regional/temporal patterns | Machine learning |
| **Output** | Text file | DataFrames + visualisations | Report, charts, video |

The biggest takeaway across all three: data analysis is not just producing correct numbers — it is asking the right question, preparing data carefully, and communicating findings clearly enough that someone else can understand and act on them.

---

*Last updated: June 2025 · ePortfolio maintained on GitHub*
