# econ3916-lab19-random-forests
# Tree-Based Models: Random Forests

## Objective
I compared a random forest with a single decision tree and two linear models on the California Housing data, and looked at which features the forest relies on most.

## Methodology
- Split the California Housing data into training and test sets.
- Fit four models: a single decision tree, OLS, Ridge regression and a random forest. I scored each one on the test set using R².
- Measured feature importance for the forest in two ways: impurity-based importance (MDI) and permutation importance.
- Tuned the forest's `max_depth` and `max_features` with cross-validation, then scored the tuned model on the test set.
- Wrote my own prompt asking an AI for a function that computes permutation importance for any fitted model, charts the top features, and returns them as a table. I went through 2 rounds and checked the function's numbers against my own.

## Key Findings
- **The forest did best.** It reached a test R² of 0.8046, compared with 0.6228 for the single tree and 0.5758 for Ridge. Averaging many trees fixed much of the overfitting that holds a single tree back.
- **A single tree beat Ridge.** This suggests the relationship between the features and house values is not linear, which a linear model can't capture.
- **The two importance methods disagreed on Latitude.** MDI gave it 0.0890, but permutation importance gave it 0.4427. MDI weights each split by how many training rows reach it. Location does much of its work in deep splits that each carve out a small region, so MDI gives it little credit. Shuffling Latitude on the test rows moves every block group to a random place on the map, which hurts the forest a lot. My takeaway is that MDI alone would have led me to undervalue Latitude.
- **Tuning helped a little.** Cross-validation raised the forest's test R² from 0.8046 to 0.8094. The default settings were already close to the best I found.
- **The AI's function matched my numbers.** In both rounds, the importances it computed matched mine exactly for all 8 features, in the same order, without refitting any forest. I also ran it on a different model it had not seen. There it matched scikit-learn's `permutation_importance` run directly on that model, which shows it uses the model and data it is given and not my notebook's variables.
