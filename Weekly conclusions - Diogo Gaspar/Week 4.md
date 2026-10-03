# What was done this week

1. Pulled the week 4 changes from the professor's repo into my main branch. The biggest change is that the models are now evaluated with a stratified 5-fold cross-validation instead of a single train/test split
2. Put the two week 4 notebooks from Moodle in Practical/W4/notebooks
3. Ran main.py with the 4 models that are now available: dummy, logistic regression, decision tree and random forest
4. Compared the cross-validation results with last week's holdout results
5. Changed the preprocessing in config.yaml to see if it improves the logistic regression: I tried changing the scaler from robust to standard and then the encoder from target to onehot

# Results analysis

## 1. Holdout (week 3) vs cross-validation (week 4)

| Model | Week 3 holdout (test) | Week 4 CV (validation mean ± std)
|---|---|---|
| Dummy | (not used) | 0.549 ± 0.000 |
| Logistic Regression | 0.657 | 0.672 ± 0.013 |
| Decision Tree | 0.610 | 0.607 ± 0.018 |
| Random Forest | (not used) | 0.650 ± 0.018 |

- The week 3 logistic regression result (0.657) is slightly outside the Weak 4 mean ± std range. So I think last week we got a slightly unfavorable split. Last week I guessed the accuracy dropped because of the bigger and messier test set, but part of it was probably just the split
- Only by changing which rows are used for validation, the logistic regression goes from 0.654 to 0.688. So with a single split we can get an unfavorable split, or a favorable one and have inflated results. This shows the importance of cross-validation and to look at the average
- The comparison is not 100% fair, because the preprocessing also changed between week 3 and week 4 (robust scaler and the target encoder)

## 2. Comparing the models

- The dummy model always says "won't reoffend" and still gets 54.9% right. So our models are not that much better. Only up about 12 points for the logistic regression, 10 for the random forest and only 6 for the decision tree
- The logistic regression is still the best model

| Model | Train mean | Validation mean | Gap |
|---|---|---|---|
| Logistic Regression | 0.675 | 0.672 | +0.003 |
| Decision Tree | 0.695 | 0.607 | +0.088 |
| Random Forest | 0.733 | 0.650 | +0.083 |

- Last week I guessed that a random forest would bring the train and test accuracy closer together. That didn't really happen: the gap went down for both the new decision tree and the random forest, and it ended up about the same for both. It wasn't the random forest that reduced the overfit
- But the random forest has 4 points more validation accuracy than the decision tree (0.650 vs 0.607), so it did improve the results, it's just still worse than the logistic regression

## 3. Fairness

| Race group | n | Logistic Regression FPR | COMPAS FPR |
|---|---|---|---|
| African-American | 1420 | 0.26 | 0.45 |
| Caucasian | 1161 | 0.13 | 0.23 |
| Hispanic | 311 | 0.15 | 0.23 |
| Other | 185 | 0.14 | 0.14 |

- Our logistic regression still has a lower FPR than COMPAS in the bigger groups, and the same for Other
- The fairness table now uses the out-of-fold predictions of the whole development set, so the groups are about 4 times bigger than last week. I think this makes these FPRs more reliable

## 4. Changing the preprocessing (logistic regression)

| Preprocessing | Validation mean ± std | Recall class 1 | FPR African-American | FPR Caucasian | FPR Hispanic |
|---|---|---|---|---|---|
| target + robust (week 4 config) | 0.672 ± 0.013 | 0.51 | 0.26 | 0.13 | 0.15 |
| target + standard (week 3 winner) | 0.672 ± 0.013 | 0.51 | 0.26 | 0.13 | 0.15 |
| onehot + robust | 0.670 ± 0.016 | 0.55 | 0.30 | 0.16 | 0.20 |

- Scaler: in week 3 the grid chose target + standard, but this week the config uses target + robust, chosen by hand. I tested the week 3 winner and the results are practically the same. So for the logistic regression the scaler doesn't matter here. I didn't test it on the tree models because trees aren't affected by the scaling
- Encoder: onehot gives the same accuracy (the difference is much smaller than the std), but the model predicts "will reoffend" more often. It catches more people who reoffend, but the FPR goes up in all the bigger groups
- So two preprocessings with the same accuracy can treat people differently. Only looking at the accuracy I would say they are the same. I would keep target encoding because of the lower FPR
- None of the changes I tried improved the accuracy

# Other notes

- The decision tree's train accuracy went down a lot from week 3 (from 0.792 to 0.695), and that's why its gap got smaller, not because the validation accuracy went up. Not sure why
