# What was done this week

1. Forked the repo https://github.com/sofiacper/ETAI-Pipeline and cloned the fork into my machine. 
2. Then I set up a python enviornment with uv init and translated the requirements in requirements.txt from the original repo into the pyproject.toml. Syncronized the dependencies with uv sync.
3. I successfully ran the main.py with uv run main.py and the results showed up in the terminal and in \results.
4. This ran the intire ml pipeline (loading data, pre processing it, create and train the model, benchmark it and save the results) using a logistic regression model.
5. Then switched the training method from the logistic regression to a decision tree on config.yaml, ran the entire pipeline again with uv run main.py and got the new results on the terminal and on \results

# Results analysis

## 1. Logistic Regression

- The train and test accuracy are very balenced, which to me sounds that we don't have overfit
- Our model generally performs better, specially on the bigger groups

Looking at the bigger race groups (African-American, Caucasian, Hispanic and Other) and comparing COMPAS's FPR with our's:

| Race group | n | Our model FPR | COMPAS FPR |
|---|---|---|---|
| African-American | 303 | 0.33 | 0.44 |
| Caucasian | 232 | 0.24 | 0.25 |
| Hispanic | 61 | 0.10 | 0.15 |
| Other | 41 | 0.15 | 0.20 |

We see that our model is producing less false positives, that's good

## 2. Decision Tree

- The train accuracy is high compared to the test accuracy, which can mean that there may is some overfitting
- The test accuracy also dropped 5% in comparison to the logistic regression model
- I would say the logistic regression model is working better for now

Looking again at the bigger race groups and comparing the FPR's:

| Race group | n | Our model FPR | COMPAS FPR |
|---|---|---|---|
| African-American | 303 | 0.28 | 0.44 |
| Caucasian | 232 | 0.24 | 0.25 |
| Hispanic | 61 | 0.18 | 0.15 |
| Other | 41 | 0.27 | 0.20 |

We see that our model FPR's results are more or less on pair with  COMPAS's ones, as some FPR's are better for our model (African-American, Caucasian) and some are worst (Hispanic, Other)

# Other notes

- Some race groups on the dataset are separated but really represent the same race group. I would join these in the same group:
  - " African-American" + "AFRICAN-AMERICAN" + "African American" + "African-American" + "african-american" -> "African American"
  - " Caucasian" + "CAUCASIAN" + "Caucasian" + "caucasian" -> "Caucasian"
  - "-" + "?" + "Other" -> "Other"
  - "Hispanic" + "hispanic" -> "Hispanic"
- This leaves us with 6 groups instead of 16