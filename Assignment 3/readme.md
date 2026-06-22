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

