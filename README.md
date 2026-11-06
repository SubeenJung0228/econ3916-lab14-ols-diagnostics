# OLS Diagnostics: When the Standard Error Is Lying

## Objective

I tested how ordinary least squares standard errors can mislead when the model's assumptions break, and how diagnostics and robust (HC3) standard errors help catch it.

## Methodology

- Simulated data where the error spread grows with x, then fitted one regression and reported both classical and robust (HC3) standard errors for the slope.
- Ran 400 simulated samples and recorded how often each 95% confidence interval covered the true slope, along with the Monte Carlo standard error of each coverage rate.
- Read four residual plots side by side with Breusch-Pagan (heteroskedasticity) and Jarque-Bera (normality) tests.
- Deleted a single row out of 600 and measured how far the slope moved, in units of its own standard error, and how much the standard error changed.
- Fitted a regression on the California Housing dataset and compared the HC3 and classical standard errors on AveRooms.
- Wrote my own function, `hc3_se()`, and used it to compute the HC3 standard error on AveRooms with all districts and again after dropping the 965 districts sitting at the 5.0 ceiling.
- (Take-home Challenge) Built a regression that passes every standard diagnostic but is still wrong: y regressed on x alone, with a confounder u left out, then refitted with u included.

## Key Findings

- **Same slope, different uncertainty.** Classical and HC3 standard errors gave the same slope, 2.0147, but different standard errors: 0.0264 classical and 0.0290 robust. The point estimate doesn't change; only how much I should trust it does.
- **Classical intervals under-cover.** Across 400 samples, the classical 95% interval covered the true slope 91.8% of the time (Monte Carlo SE 1.4%), and the HC3 interval 93.5% of the time (Monte Carlo SE 1.2%). The robust interval came closer to the 95% it promises.
- **Plots and tests work best together.** I read the residual plots alongside the Breusch-Pagan and Jarque-Bera tests rather than relying on either alone.
- **One row can change the story.** Deleting one row out of 600 moved the slope by 0.58 of its own standard error and cut the standard error by 33%.
- **Real data can be far worse.** On California Housing, the HC3 standard error on AveRooms was 5.02 times the classical one.
- **Censored values matter.** With my `hc3_se()` function, the HC3 standard error on AveRooms was 0.0121 with all districts and 0.0082 after dropping the 965 districts at the 5.0 ceiling.
- **(Challenge) Passing diagnostics isn't the same as being right.** Leaving out the confounder u, the regression passed Breusch-Pagan (p = 0.47), Jarque-Bera (p = 0.56), and Cook's distance (largest 0.026), yet estimated the slope at 3.42 against a true value of 2.0. Adding u brought it back to 2.00. Diagnostics check the residuals; they can't detect a variable that was never in the model.
