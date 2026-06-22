
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
