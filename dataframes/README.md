This folder contains .csv files that are ready to be used for modeling. 

File naming convention: `{dataSource}_{grade}.csv`

Conventions:
- `is_proficient` - boolean label representing whether the student's score is above the benchmark score for the given dataSource and year
- `proficient_score` - score / threshold representing the benchmark for proficient
- `proficient_diff` - score - threshold representing the benchmark for proficient

NOTE: currently dataframes only have courses + schools as features. currently working on adding vendor data.