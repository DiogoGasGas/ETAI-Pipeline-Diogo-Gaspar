# What was done this week

1. Pulled the week 3 changes from the professor's repo into my main branch. The biggest change is in preprocessing.py, which now cleans the data (fixes categories written in different ways, removes duplicates, fills missing values) instead of just dropping every row with a missing value.
2. Put the two notebooks from Moodle in Practical/W3/notebooks and added the libraries they need with uv.
3. Ran main.py with the logistic regression and then with the decision tree, and compared the results with last week.

# Results analysis

One thing I noticed first: the test set is bigger this week (1443 people vs 1252). Last week every row with a missing value was deleted, and now those rows are kept and filled in. So the models are being tested on more (and messier) data than last week, which makes the comparison not 100% fair.

## 1. Logistic Regression

| | Week 2 | Week 3 |
|---|---|---|
| Train accuracy | 0.679 | 0.676 |
| Test accuracy | 0.678 | 0.657 |

- The accuracy went down about 2%. My guess is that it's because of the bigger and messier test set, and not because the model got worse.
- Train and test accuracy are still close, so I don't think there is overfitting.

| Race group | n | Our model FPR | COMPAS FPR |
|---|---|---|---|
| African-American | 349 | 0.28 | 0.44 |
| Caucasian | 290 | 0.14 | 0.24 |
| Hispanic | 85 | 0.11 | 0.16 |
| Other | 54 | 0.19 | 0.20 |

- Our model now has a lower FPR than COMPAS in all the bigger groups, that's good.
- Compared to last week, the FPR went down a lot for African-American (0.33 -> 0.28) and Caucasian (0.24 -> 0.14), but went a bit up for Hispanic and Other.

## 2. Decision Tree

| | Week 2 | Week 3 |
|---|---|---|
| Train accuracy | 0.829 | 0.792 |
| Test accuracy | 0.628 | 0.610 |

- Both went down, and the train accuracy is still much higher than the test accuracy, so I think it is still overfitting.
- The logistic regression is still the better model.

| Race group | n | Our model FPR | COMPAS FPR |
|---|---|---|---|
| African-American | 349 | 0.32 | 0.44 |
| Caucasian | 290 | 0.23 | 0.24 |
| Hispanic | 85 | 0.20 | 0.16 |
| Other | 54 | 0.19 | 0.20 |

- Last week the tree was worse than COMPAS for Hispanic and Other. Now it's better for Other, but still worse for Hispanic.

# Other notes

- Last week I said some race groups were the same group written in different ways. This week that got fixed by the new cleaning step: the fairness report now shows 6 groups instead of 16.
- I haven't gone through the two notebooks properly yet, so I don't fully understand all the new preprocessing decisions. That's what I want to do next.
- Decision trees are known to overfit. In this week's theory class we started ensemble methods like random forests, so I'm guessing we'll add one to the pipeline next week. If it brings the train and test accuracy closer together, I think the results will get a lot better.
