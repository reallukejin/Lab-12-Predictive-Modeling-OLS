# Lab-12-Predictive-Modeling-OLS
# OLS as a Line of Best Fit: Estimation versus Prediction

## Objective
I used the California Housing data to fit a linear regression and see how the same model gets judged differently when the job is explaining a coefficient versus predicting new rows.

## Methodology
- Fitted median house value on median income three ways (the closed-form formula, statsmodels, and scikit-learn) and checked that all three gave the same intercept and slope.
- Added 0, 1, 5, 20 and 100 columns of pure random noise to the regression and compared R-squared with adjusted R-squared.
- Split the data into training and test rows (80/20) before fitting anything, then fitted an eight-feature model on the training rows and scored it on both sets.
- Used one specification (median income and rooms per household) for two jobs: reading the AveRooms coefficient with its 95% confidence interval, and measuring test RMSE.
- Wrote a function, `holdout_rmse()`, that splits the data the same way every time, fits a model on the training rows, and returns the test RMSE for any list of predictors. I used it on median income alone.
- Take-home challenge: counted the districts censored at the 5.0 ($500,000) ceiling, refitted the eight-feature model without them, and compared the coefficients and test RMSE.

## Key Findings
- All three methods gave the same line: intercept 0.450856 and slope 0.417938. So each extra $10,000 in median income goes with about $41,800 more in median house value.
- R-squared was 0.4734 with no noise and went up to 0.4758 with 100 noise columns, even though the noise has nothing to do with house value. Adjusted R-squared stayed around 0.473 (0.4732 at 100 columns), so R-squared on its own isn't a good way to choose between models.
- The eight-feature model had a train RMSE of 0.7197 and a test RMSE of 0.7456. The two are close, so it isn't badly overfitting, but a typical miss is still about $74,560.
- The AveRooms coefficient was -0.0381, with a 95% CI of [-0.0430, -0.0333]. It's clearly different from zero, but that same two-variable model had a test RMSE of 0.8379, so a significant coefficient doesn't mean the model predicts well.
- With median income alone, the test RMSE was 0.8421, compared with 0.7456 for all eight features. The typical error is about $9,650 bigger. This comparison is fair because both models are scored on the same test rows they never saw.
- Challenge: 965 districts (4.7%) are coded at the ceiling. Dropping them shrank most coefficients toward zero. Test RMSE only looked better (0.7456 to 0.6531) when I also dropped them from the test set. On the same test rows, the two models were basically tied (0.7456 vs 0.7441). The model without them is really a model of districts worth under $500,000, not of all of California.
