- Each exam type (ACT Math, ACT Reading, Scantron Math, and Scantron Reading) will hvae 3 files. A file for the main model (continuous label), a model with a binary label, and a calculation of feature importances. 

- Any file containing the word with 'binary' has the label of 'is_proficient' (i.e., a binary label)
  - All files with that label use the same dataset as their continuous counterpart.
    The only difference is that it is a classification model, rather than a regression model.
  - Any binary model calculates:
    - F1
    - Accuracy
    - A confusion matrix
- files without the ending 'binary' have more models other than XGBoost, but XGBoost yielded the best results.

- any files with containing the word "feature_importances" calculates the importance of different features using a non-PCA dataset.

- any files containing the phrase "main_model" is the model used to predict proficiency. It will contain PCA, and a continuous label (called 'proficient_score')

- ACT reading has preliminary outlier deteciton added in the main model. There is one older version of outlier detection (which has all of the code that didn't work). 

- the file laveled "Clustering_original_scores_df" aims to use clustering with a different dataframe to identify key features. it does not use the same dataset as the other models.
  
