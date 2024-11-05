# architectures
- linear / logistic regression (maya)
- decision tree / random forest (lmm)
- gradient boosted trees (manjari)

## to do
- generally try binary and continuous (`proficient_score`, `proficient_diff`) but architecture can only support one type do that
- focus on feature selection using non encoded dataframes (act\*_11, scantron\*_38)
- feature importance

# Log

| Architecture | District | Year (Features) | Grades | Values / dataSource | Features (courses, vendors, schools, past proficiency) | # of features | Label | Accuracy | F1 | Other metrics (RSME, R^2) | Notes | Person | 
| :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: | 
| Linear regression | 45 | 2017 | 3-8 | scantronMath_38 | past_proficiency | 1 | `proficient_score` | | | RMSE=0.09, R^2=0.23 | Concerned about overfitting | Maya | 
| Linear regression | 45 | 2017 | 3-8 | scantronReading_38 | past_proficiency | 1 | `proficient_score` | | | RMSE=0.04, R^2=0.85 |  Concerned about overfitting | Maya |
| Logistic regression | 45 | 2023 | 11 | actMath_11 | schools, vendors, encoded courses | 23 | `is_proficient` | 0.7198 | | Log loss: 0.5703 | |  Maya |
| Decision tree | 45 | 2017 | 11 | actReading_11 | ? | 242 | `proficient_score` | | | RMSE=0.2273, R^2=0.49046499 | | Louise Marie | 
| Decision tree | 45 | 2017 | 3–8 | scantronReading_38 | ? | 142 | `proficient_score` | | | RMSE=0.0393, R^2=0.0.8632 | | Louise Marie | 
| Random forest | 45 | 2017 | 11 | actReading_11 | ? | 242 | `proficient_score` | | | RMSE=0.1802, R^2=0.6795 | | Louise Marie | 
| Random forest | 45 | 2017 | 11 | scantronReading_38 | ? | 140 | `proficient_score` | | | RMSE=0.0393, R^2=0.8632 | | Louise Marie | 


