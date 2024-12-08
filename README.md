# level-data-1a
- `[process_data.ipynb](./process_data.ipynb)` - set up the file structure and sync data
- `[make_dfs.ipynb](./make_dfs.ipynb)` - make the dataframes from data
- [Model spreadsheet](https://docs.google.com/spreadsheets/d/17sNVnDQ4ZQKMnNjwo3k7TYPvQMVXpdHDBMfrAvNe6Jo/edit?usp=sharing) - contains metadata, metrics for each model

# Predicting Proficiency 
## Table of Contents
## Project Description
### Overview, Objectives, and Goals
For a given district:
Identify which factors most strongly contribute to student proficiency (subject-based)
Utilize factors to build predictive models that flag students at risk of falling below proficiency level

For example:
Our ML model could help us flag a current 11th grader as at risk of falling below proficiency in math based on 10th grade features  
Educators can then provide extra help / attention / services  

Scope: Predict subject-specific proficiency given past year’s data and grade level (potentially generalize across grade levels)

### Methodology
#### Raw Dataset Overview
* **Results and Key Findings**
  1. About the data:
     * A majority of students are not proficient (according to the benchmarks)
     * We have enough data to train predictive models for students in grades 3–8 and 11
  2. Past proficiency is a relatively good predictor of proficiency
  3. Proficiency in one subject (e.g. reading) correlates with proficiency in another (e.g. math)
  4. We can predict proficiency for the stated demographics with over 70% accuracy in most cases
  5. Our models have a harder time predicting proficient students 

* **Visualizations**
* **Potential Next Steps**
  1. Add additional data to improve accuracy and representation
     * Performance on courses
     * Complete demographic data
     * More benchmark exams (e.g. Aspire)
  2. Try to get the model to generalize better
     * We can generalize across grade levels given enough data
     * Can we generalize across subjects?
  3. Represent proficiency more holistically
     * Different tiers of proficiency
     * Different data sources

## Installation

## Usage

## Contributing

## License

## Credits and Acknowledgements 
Student Team: Allison Huang, Manjari Muruganandam, Louise Marie Maganto, Maya Patel 
Thank you to our TA, Blessing Nwogu, and Challenge Advisors from Level Data, Pradnya Bhawalkar and Eddie Shek, for supporting us through this project!

