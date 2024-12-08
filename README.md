# level-data-1a
- `[process_data.ipynb](./process_data.ipynb)` - set up the file structure and sync data
- `[make_dfs.ipynb](./make_dfs.ipynb)` - make the dataframes from data
- [Model spreadsheet](https://docs.google.com/spreadsheets/d/17sNVnDQ4ZQKMnNjwo3k7TYPvQMVXpdHDBMfrAvNe6Jo/edit?usp=sharing) - contains metadata, metrics for each model

# Predicting Proficiency 
## Table of Contents
1. [Project Description](#project-description)
2. [Results and Key Findings](#results-and-key-findings)
4. [Potential Next Steps](#potential-next-steps)
5. [Usage](#usage)
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
| **Data Tables Provided** | **Data Attributes** |
|--------------------------|---------------------|
| - scores<br>- benchmarks<br>- courseSections<br>- courseSectionRosters<br>- schools<br>- vendorUsage | - District 18, 45 (where 18 is a superset of 45)<br>- student ID (all data tables)<br>- year (all data tables)<br>- grade level (all data tables)<br>- demographics (ethnicity, ELL, etc.) - scores<br>- course enrollment - courseRosterSections, courseRosters<br>- school - schools (anonymized)<br>- vendor usage - vendorUsage<br>**Note:** Data is anonymized |

#### Data Usage
Using benchmark threshold and student score, we created two data types:  
1. Boolean — `is_proficient`  
2. Continuous — `proficient_score` (score/threshold)  
   * < 1 is not proficient, >= 1 is proficient  
   * Captures “how” proficient a student is  

For example, if a student's score is 21 and the proficiency threshold is 18, then `is_proficient` will be True and `proficient_score` is 1.1666.

Then, for a given student, grade level, and subject, we merged dataframes in the following manner: 
| studentId | subgroup_ethnicity |...| course_Algebra II |...| school_A |...|iready_math |...| proficient_score |
|-----------|--------------------|---|-------------------|---|----------|---|-------------|---|------------------|
| 45440     | 1                  |   |1                  |   | 0        |   | 1           |   |0.941176         |
| 45054     | 0                   |   | 0                 |   |1         |   | 0           |   |0.529412         |

1 = a student is part of a subgroup, course, school, etc.  
0 = a student is not part of a subgroup, course, school, etc.   
Label: `proficient_score`

#### Final Datasets
| **Math and Reading Proficiency for Grades 3–8** | **Subject Proficiency for Grade 11** |
|--------------------------------------------------|--------------------------------------|
| - scantronMath, scantronReading<br>- 2017 features, 2018 labels<br>- Features: courses, schools, vendors, past_proficiency<br>- ~20,000 students in each DataFrame | - ACT (Reading, English, Math, Science)<br>- 2017 features, 2018 labels<br>- Features: courses, schools, vendors<br>- ~2,500 students in each DataFrame |

#### Dimension Reduction
We used two different methods to reduce the number of columns in our final dataframes: Encoding and Principal Component Analysis (PCA)

##### Encoding
In the raw data we were given, original course names had formats such as `English 5`, `LifeSci Gr7` etc. To encode this information in our dataframes, we processed it in the following way: 
1. Extract grade level
2. Create subject areas using keywords
3. Create feature for course
   0 = Not enrolled  
   1 = Below grade level  
   2 = At grade level  
   3 = Above grade level  
4. Create binary feature for electives

##### PCA
* What is PCA?
  * PCA reduces the number of columns (features) in a dataset.
* How does it work?
  * Combines original columns into fewer “principal components.”
  * Captures the most important differences (variation) in the data.
  * We kept 80% of the variation and removed excess columns.
* Why do we use it?
  * Hundreds of columns make data hard to analyze.
  * Removing unnecessary components didn’t hurt performance.
  * Eliminates redundancy and simplifies the data.

Outcomes of PCA
| **Metric**                        | **ACT_math** | **ACT_reading** | **Scantron_Math** | **Scantron_reading** |
|-----------------------------------|--------------|-----------------|-------------------|----------------------|
| **Columns in original dataframe** | 240          | 240             | 139               | 139                  |
| **Columns in dataframe after PCA**| 109          | 109             | 46                | 46                   |

Before: 142 Features
| **studentId** | **course_English 5** |...| **course_LifeSci Gr7** |
|---------------|----------------------|---|------------------------|
| 43588         | 1                    |   | 0                      |
| 30983         | 0                    |   | 1                      |

After: 26 Features
| **studentId** | **subject_english** |...| **subject_science** |
|---------------|----------------------|---|------------------------|
| 43588         | 2                    |   | 0                      |
| 30983         | 0                    |   | 3                      |


### Exploratory Data Analysis
#### Correlations
We used correlation matrices to find how related different columns of our data frames are. Here are some of our key findings from this process:  
* Strong connections between lunch status, gender, and ethnicity.
* ACT sections (e.g., math & reading) are closely related.
* Doing well in one ACT section often means doing well in another.
* Similar pattern for Scantron Math and Reading exams (not shown).

#### Class Imbalance
We found that only about 15-20% of students represented in the dataset are proficient (i.e., score at least the benchmark). This impacted our models’ ability to predict proficient students. 

### Modeling
#### Model Selection
We had three main requirements when choosing which machine learning models we wanted to use to predict proficiency
1. First, given that our intended audience is school administrators and educators, who likely do not have a data science background, we wanted our models to be interpretable. 
2. Second, we also decided to look at proficiency as a continuous label instead of a simple binary, yes or no label.
   After discussing with our Challenge Advisors and TA, we thought that using a continuous label would be more beneficial because it accounts for different levels of proficiency.
3. The third criteria is that we wanted our models to fit well to the data we were given, meaning they do not underfit (do not learn enough complexities) or overfit (learn too much of the training data’s complexities). It is important to note that model training and tuning also has an impact on how the model performs in the end.  
Ultimately, we decided to train linear regression, decision tree, random forest, and gradient boosted decision tree models.

#### Model Training
Our approach: Trying many different models and seeing which ones performed best.  
Here is our [Model spreadsheet](https://docs.google.com/spreadsheets/d/17sNVnDQ4ZQKMnNjwo3k7TYPvQMVXpdHDBMfrAvNe6Jo/edit?usp=sharing) - contains metadata, metrics for each model

Here are our highest performing models:  
Note that accuracy and macro F1 scores are computed after converting the continuous result into a binary value (e.g. 1.6 → true)    

##### Math, reading for grades 3-8 (Using Scantron Math/Reading data)
| **Model Name**            | **Features**                                           | **Evaluation Metrics**                                | **Insights**                                                                                                                                           |
|---------------------------|--------------------------------------------------------|------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Simple Linear Regression** | Past proficiency                                      | **Math:**<br>RMSE: 0.06<br>R^2: 0.44<br>Accuracy: 0.87<br>Macro F1: 0.83<br>**Reading:**<br>RMSE: 0.07<br>R^2: 0.59<br>Accuracy: 0.87<br>Macro F1: 0.86 | - Only 1 feature for this model because adding features either didn’t change or worsened RMSE and R^2 values.<br>- Predicts poorly for students with better or worse future scores.<br>- Treated as a baseline model for scantron math/reading. |
| **XG Boost**               | Schools, courses, vendor usage, past proficiency      | RMSE: 0.06<br>R^2: 0.59<br>Accuracy: 0.88<br>Macro F1: 0.87 | - Past_proficient_score is positively correlated with the label.<br>- Average performance varies by grade level.<br>- Co-enrollment in advanced classes.<br>- Scantron Math had no advanced courses in key features. |
| **Decision Tree**          | Schools, courses, vendor usage                        | RMSE: 0.07<br>R^2: 0.5<br>Accuracy: 0.87<br>Macro F1: 0.87 | - Encoded versions performed slightly better than PCA.<br>- School comes up as an important feature.                                                     |
| **Random Forest**          | Schools, courses, vendor usage                        | RMSE: 0.05<br>R^2: 0.5<br>Accuracy: 0.86<br>Macro F1: 0.86 | - Decision Tree performed slightly better.<br>- PCA performed better than encoded.<br>- Schools play an important factor.                                    |


##### Math, reading for grade 11 (Using ACT Math/Reading data)
| **Model Name**            | **Features**                                           | **Evaluation Metrics**                                | **Insights**                                                                                                                                           |
|---------------------------|--------------------------------------------------------|------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------|
| **XG Boost**               | Schools, courses, vendor usage                        | RMSE: 0.08<br>R^2: 0.6<br>Accuracy: 0.79<br>Macro F1: 0.82 | - **Positive Correlations with Proficiency:**<br>  - iReady Math (value of 1) positively correlates with ACT math scores.<br>  - Advanced courses with positive correlations: course_Alg II/Trig, course_Eng Gr10 Adv, course_USHis I Adv, course_Geometry Adv, course_ChemistryAdv.<br> - **Negative Correlations with Proficiency:**<br>  - Courses with negative correlations: Algebra I B, Physical Science, English Grade 10. |
| **Decision Tree**          | Schools, courses, vendor usage                        | RMSE: 0.18<br>R^2: 0.38<br>Accuracy: 0.79<br>Macro F1: 0.87 | - In the ACT Math model, science courses were important.<br>- In the ACT Reading model, STEM courses were more important than English 10 enrollment. |
| **Random Forest**          | Schools, courses, vendor usage                        | RMSE: 0.16<br>R^2: 0.5<br>Accuracy: 0.80<br>Macro F1: 0.79 | - Random Forest performs better than Decision Trees. |


## Results and Key Findings
  1. About the data:
     * A majority of students are not proficient (according to the benchmarks)
     * We have enough data to train predictive models for students in grades 3–8 and 11
  2. Past proficiency is a relatively good predictor of proficiency
  3. Proficiency in one subject (e.g. reading) correlates with proficiency in another (e.g. math)
  4. We can predict proficiency for the stated demographics with over 70% accuracy in most cases
  5. Our models have a harder time predicting proficient students 

## Potential Next Steps 
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

## Usage
* The `dataframes` folder contains all of the csv files that we used to train models on
* Individual models are in their respective folder
  * E.g. You can find all decision tree models in the `decision tree` folder
* Inside of each folder, you will find Jupyter Notebooks for each dataframe that the model was trained on
* You can download and run the Jupyter Notebooks on an IDE of your choosing (we used Visual Studio Code)
 

## Credits and Acknowledgements 
Student Team: Allison Huang, Manjari Muruganandam, Louise Marie Maganto, Maya Patel 
Thank you to our TA, Blessing Nwogu, and Challenge Advisors from Level Data, Pradnya Bhawalkar and Eddie Shek, for supporting us through this project!

