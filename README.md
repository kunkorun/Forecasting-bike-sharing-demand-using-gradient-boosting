\## What I Learned



This project helped me understand and practice the full machine learning workflow for a regression problem, with a particular focus on time-dependent data.



\### Data and EDA



\* Learned to perform a structured data audit before modelling.

\* Learned to distinguish between numerical, categorical, and binary features based on their meaning, not only on their data types.

\* Learned to use visual analysis to identify non-linear relationships and interactions between features.

\* Learned that a low correlation does not necessarily mean that a feature is useless.

\* Learned to pay attention to relationships between features, such as the strong correlation between `temp` and `atemp`.



\### Target Transformation and Evaluation



\* Learned why a strongly right-skewed target can benefit from a logarithmic transformation.

\* Practiced using `log1p()` for the target and `expm1()` to transform predictions back to the original scale.

\* Better understood the connection between logarithmic target transformation and RMSLE.

\* Learned that calculating RMSE in `log1p` space is equivalent to calculating RMSLE on the original target scale.



\### Validation and Data Leakage



\* Learned why a random train/validation split is not appropriate for time-dependent data.

\* Practiced using chronological validation instead.

\* Learned how target leakage can occur when features such as `casual` and `registered` directly contain information about the target.

\* Learned that an unexpectedly good metric can sometimes be a warning sign and should lead to a check for leakage or an incorrect validation strategy.



\### Machine Learning Models



\* Compared Linear Regression, KNN, Decision Tree, Random Forest, and Gradient Boosting.

\* Learned the difference between distance-based models such as KNN and tree-based models.

\* Learned why feature scaling is important for KNN but is generally not required for tree-based models.

\* Learned how Random Forest and Gradient Boosting capture non-linear relationships in different ways.

\* Practiced hyperparameter tuning with `RandomizedSearchCV` and `TimeSeriesSplit`.



\### Error Analysis



\* Learned to analyse residuals instead of relying only on the final metric.

\* Learned to look for systematic patterns in residual plots and use them to better understand model limitations.

\* Saw that different models can have different error patterns even when they are evaluated using the same metric.



\### Feature Engineering



\* Learned to extract useful temporal information from `datetime`.

\* Practiced creating `year`, `month`, `dayofweek`, and `hour` features.

\* Learned that feature engineering should be tested experimentally rather than adding features without a hypothesis.

\* Learned that differences between train and test distributions can make seemingly useful features harmful for the final model.



\### General Workflow



The main lesson from this project was that improving a model is not only about trying more algorithms.



A better approach is:



\*\*EDA → hypothesis → experiment → error analysis → feature engineering → tuning → final validation\*\*



Each modelling decision should be supported by an observation from the data or by the result of a controlled experiment.



