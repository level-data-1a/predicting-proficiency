# architectures
- linear / logistic regression (maya)
- decision tree / random forest (lmm)
- gradient boosted trees (manjari)

## to do
- generally try binary and continuous (`proficient_score`, `proficient_diff`) but architecture can only support one type do that
- focus on feature selection using non encoded dataframes (act\*_11, scantron\*_38)
- feature importance
- please document your code really really well! explain every step such that someone could understand it easily and add on

# Log

| Architecture | District | Year (Features) | Grades | Values / dataSource | Features (courses, vendors, schools, past proficiency) | # of features | Label | Accuracy | F1 | Other metrics (RSME, R^2) | Notes | Person | 
| :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | 
| Linear regression | 45 | 2017 | 3-8 | scantronMath | past_proficiency | 1 | `proficient_score` | | | RMSE=0.09, R^2=0.23 | Concerned about overfitting | Maya | 
| Linear regression | 45 | 2017 | 3-8 | scantronReading | past_proficiency | 1 | `proficient_score` | | | RMSE=0.04, R^2=0.85 |  Concerned about overfitting | Maya |
| Logistic regression | 18 | 2023 | 11 | actMath | schools, vendors, encoded courses | 23 | `is_proficient` | 0.7198 | | Log loss: 0.5703 | |  Maya |
| Decision tree | 45 | 2017 | 11 | actReading | schools, vendors, courses  | 241 | `proficient_score` | | | RMSE=0.2273, R^2=0.49046499 | | Louise Marie | 
| Decision tree | 45 | 2017 | 3–8 | scantronReading | schools, vendors, courses, past proficiency | 140| `proficient_score` | | | RMSE=0.0393, R^2=0.8632 | | Louise Marie | 
| Random forest | 45 | 2017 | 11 | actReading | schools, vendors, courses | 242 | `proficient_score` | | | RMSE=0.1802, R^2=0.6795 | | Louise Marie | 
| Random forest | 45 | 2017 | 3-8 | scantronReading | schools, vendors, courses, past proficiency | 140 | `proficient_score` | | | RMSE=0.0393, R^2=0.8632 | | Louise Marie | 


**Issues:**
- @LMM, for Features column, could you just list if you included courses, vendors, schools, or past proficiency? I assume you used courses but couldn't see the others so I didn't want to assume. - allison