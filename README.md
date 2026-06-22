# 📊 SECP3223: Data Analytic Programming — Assignment Reflection

---

## Overview

This page documents my reflections on two assignments completed for the Data Analytic Programming course. Together, they formed a progressive journey — from foundational Python data structures and file handling in Assignment 1, to real-world data analysis with pandas, aggregation, and visualisation in Assignment 3. Both assignments used Python in Jupyter Notebook and required not just writing working code, but understanding the *why* behind each tool we used.

---

## Assignment 1 — QuickRide: Lists, Tuples, Dictionaries, Functions & File I/O

### What the Assignment Covered

The assignment tasked us with analysing ride-sharing data from a fictional company called QuickRide. Working with a CSV dataset of 10 rides (fields: `ride_id`, `driver_name`, `rider_name`, `city`, `distance`, `duration`, `fare`), we had to complete 8 tasks: writing the data to a CSV file, reading and parsing it back, computing city-level statistics, summarising driver performance, slicing the dataset, applying lambda-based sorting, and writing results back to a file.

### What I Learned

**File I/O** was the first concept tested, and it was more hands-on than I expected. Writing the raw CSV string to `ride.csv` with `open("ride.csv", "w")` and later reading it back with `csv.DictReader` taught me that file I/O is not just about persistence — it is the bridge between raw data and structured, usable Python objects. The `DictReader` approach was particularly elegant because it automatically mapped each row to a dictionary using the header row as keys, saving a lot of manual parsing.

**Type conversion** was something I had not thought much about before Question 2. When CSV data is read, every field comes back as a string. Explicitly converting `distance`, `duration`, and `fare` to `float` inside the loading function was a small step, but it reminded me that data always needs to be validated and typed correctly before analysis begins — a principle that matters enormously at scale.

**Dictionary-based aggregation** (Questions 3 and 4) was the heart of the assignment. Building `city_fare` and `driver_summary` dictionaries by iterating through the data list — checking if a key exists, initialising it if not, then accumulating values — is a fundamental pattern for grouping data without any external libraries. Writing this from scratch, rather than calling a library function, gave me a deeper appreciation for what higher-level tools like pandas actually do under the hood.

**Lambda functions and sorting** in Questions 6 and 7 were a highlight. Using `sorted(summary.items(), key=lambda x: x[1]['total_fare'], reverse=True)` to rank drivers by earnings was concise and powerful. The lambda allowed me to express the sort key inline without defining a separate function, which made the code much more readable. The same approach for sorting cities by average duration reinforced that lambdas are most useful when the logic is simple and the intent is clear from context.

**List slicing** in `analyze_peak_slice(data, start_idx, end_idx)` was a short task but an important one. Using `data[start_idx:end_idx + 1]` to extract a subrange and then computing totals, averages, and a `set` of unique drivers on that slice taught me to think about data analysis as a series of transformations on subsets — not always the full dataset.

### Challenges I Faced

The trickiest part was building `average_fare_by_city` without pandas. I initially tried accumulating just the total fare per city, forgetting I also needed the count of rides to compute the average. After fixing that, using a dictionary-of-dictionaries (`{'total fare': 0.0, 'count': 0}`) became the pattern I used for all subsequent aggregation tasks.

For Question 5, the `+ 1` in `data[start_idx:end_idx + 1]` tripped me up initially — Python slicing is exclusive at the upper end, so without the `+ 1`, the last intended ride was always excluded.

---

## Assignment 3 — Rainfall Data Analysis (pandas, Aggregation & Visualisation)

### What the Assignment Covered

This assignment used a rainfall dataset recording monthly measurements from weather stations across four regions (North, South, East, West), with columns for `Rainfall_mm`, `RainyDays`, `MaxTemp_C`, `MinTemp_C`, and `UrbanRural`. The work was divided into three parts: data exploration (15 marks), grouping and aggregation (30 marks), and data visualisation (55 marks), all completed in a Jupyter Notebook.

### What I Learned

**Part 1 — Data Exploration**

Loading data with pandas and immediately calling `.head()`, `.isnull().sum()`, and `.describe()` became a workflow I now apply instinctively at the start of any data task. These three steps reveal the shape of the data, surface any quality issues, and give a statistical snapshot — all before any analysis begins.

Handling missing values by imputing the mean for numerical columns and the mode for categorical columns was important context. It taught me that missing data is not just an inconvenience to remove — the right fill strategy depends on the column type and the downstream analysis. Using the mean preserves the central tendency without distorting aggregates; using the mode for categoricals avoids introducing spurious categories.

**Part 2 — Grouping and Aggregation**

The `.groupby()` and `.agg()` combination in pandas was the most technically enriching part of the assignment. Computing total rainfall, average rainy days, and average temperatures per station with a single `.agg()` call — compared to the manual dictionary loops in Assignment 1 — showed me exactly why pandas exists and what it saves.

The multi-level aggregation in Question 3 (grouping by both `Month` and `Region`, then applying four different aggregations to `Rainfall_mm`) produced a hierarchical multi-index DataFrame. Navigating this output and understanding how pandas structures nested group results was initially confusing, but once I understood that the index now had two levels, reading and interpreting the table became straightforward.

Finding the highest recorded rainfall with `df.loc[df['Rainfall_mm'].idxmax()]` was a single line that in pure Python would require several more. `idxmax()` returns the integer index of the maximum value, and `df.loc[]` retrieves that entire row — a pattern I have started using for finding extremes in any column.

The Urban vs. Rural comparison (Question 5) was analytically interesting. Urban stations had a notably higher average rainfall (72.4 mm) compared to rural stations (56.8 mm), which prompted us to reflect in our written summary on whether urbanisation affects local rainfall or whether it reflects where weather stations happen to be placed.

**Part 3 — Data Visualisation**

Visualisation was where the analysis came alive. The progression from bar charts to heatmaps to scatter plots to faceted line plots built a complete narrative:

- **Bar charts** gave a quick ranked comparison of total rainfall by region.
- **Boxplots** revealed the spread and outliers of rainfall across urban vs. rural classifications, which simple averages would have hidden.
- **Heatmaps** (using seaborn) were new to me — mapping mean rainfall across regions and months as a colour gradient made seasonal patterns immediately visible in a way no table could.
- **Scatter plots** with a regression line showed the relationship between `MaxTemp_C` and `Rainfall_mm`, revealing that the two do not have a simple positive or negative relationship — the correlation varies by region, indicating that temperature alone is not a good predictor of rainfall.
- **Faceted line plots** (`sns.relplot` with `col='Region'` and `row='UrbanRural'`) were the most complex visualisation we built. Having eight subplots automatically generated from a single function call, each showing monthly rainfall for a specific region-urbanisation combination, demonstrated the power of seaborn's figure-level functions.

### Key Insights from the Analysis

Summarising our findings in a Markdown cell was a task I initially underestimated, but it turned out to be one of the most valuable parts of the assignment. It forced me to translate visual patterns back into clear, plain-language statements:

- Regional differences in rainfall are consistent and likely linked to geography and climate, not just data noise.
- Urban areas receive more rainfall on average, though this may partly reflect station placement rather than a pure urbanisation effect.
- Monthly variability is significant — certain months are clearly wetter across all regions, pointing to seasonal weather patterns.
- Temperature and rainfall do not correlate simply; more complex meteorological factors are at play.

### Challenges I Faced

The data loading step in Assignment 3 required some extra handling — the CSV was read without a proper header and needed manual column assignment and type conversion via `pd.to_numeric(errors='coerce')`. Getting this right before any analysis was essential; errors here would have silently corrupted every downstream result.

The multi-index DataFrame output from `.groupby(['Month', 'Region']).agg(...)` initially displayed in a way that was hard to read. Learning to interpret hierarchical indices — and understanding that pandas is not malfunctioning, it is just representing nested groupings — was a small but real conceptual hurdle.

---

## Overall Reflection

These two assignments together tell a coherent story of progression in data skills:

| Dimension | Assignment 1 | Assignment 3 |
|---|---|---|
| **Data handling** | Manual CSV I/O and parsing | pandas `read_csv`, type coercion, missing value imputation |
| **Aggregation** | Dictionary loops from scratch | `.groupby()` and `.agg()` |
| **Sorting** | Lambda with `sorted()` | Lambda with `sorted()` and pandas `.sort_values()` |
| **Output** | Print statements and text file | DataFrames and visualisations |
| **Insight layer** | Computed statistics | Visual patterns with written analysis |

The most lasting lesson is that data analysis is not just about producing correct numbers — it is about asking the right questions, choosing the right representation, and communicating findings clearly. Writing the summary in Part 3 of Assignment 3 reinforced that the final step of any analysis is always translation: turning output into meaning.

Working as a group also added a collaborative dimension. Dividing tasks by section and then reviewing each other's code and written summaries improved the quality of the final submission and gave me experience in reading and understanding code written by others — a skill just as important as writing it myself.

---

*Last updated: June 2025*  
*ePortfolio maintained on GitHub*
