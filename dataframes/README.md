This folder contains .csv files that are ready to be used for modeling. 

# Conventions
File naming: `{dataSource}_{grade}.csv`

Note: this may change in the future if we do subject-specific modeling.

Labels:
- `is_proficient` - boolean label representing whether the student's score is above the benchmark score for the given dataSource and year
- `proficient_score` - score / threshold representing the benchmark for proficient
- `proficient_diff` - score - threshold representing the benchmark for proficient

# Overview of DataFrames
The table below shows which features and labels are included in each dataframe. 
- `Feature year` - the year that the features are from
- `Label year` - the year that the labels are from
- `schools` - whether the school a student attended is used as a feature
- `courseRosters` - whether the courses a student was enrolled in are used as a features
- `vendorUsage` - whether the vendor usage data is used as a feature
- `is_proficient` - whether this label is present
- `proficient_score` - whether this label is present
- `proficient_diff` - whether this label is present


| File name |  Feature year | Label year |  schools | courseRosters |  vendorUsage | is_proficient | proficient_score | proficient_diff |
| --- | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| actEnglish_11.csv | 2017 | 2018 | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| actMath_11.csv    | 2017 | 2018 | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| actReading_11.csv | 2017 | 2018 | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| actScience_11.csv | 2017 | 2018 | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |