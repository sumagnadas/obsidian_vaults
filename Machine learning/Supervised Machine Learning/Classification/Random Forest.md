Decision trees, although ideal for machine learning models, tend to overfit on the training data as the ordering of the conditions matter a lot when categorizing a data. Hence, to make it work better, the Random Forest algorithm was made.
## Algorithm for building and using a Random Forest model
---
1. First create a bootstrapped dataset of the same size with the original training dataset i.e. sample random points from the original dataset taking duplicates until the dataset is of the same dimensions as the original one.
2. We then make a decision tree out of the bootstrapped dataset. Take a random subset of columns instead of all the columns (only 3 of 8 columns or such) and iteratively decide from the random selections of the remaining columns until a decision tree is made from all the columns.
3. We make more decision trees repeating the steps of 1 and 2 iteratively until we are satisfied or for a number of times. 
4. To classify a new sample, we just pass it through all the decision trees and take the output which is more probable according to all the decision trees.
 As the datasets and consequently the decision trees are randomly made, the model is highly effective.
## Evaluation
---
When creating the bootstrapped datasets, we had points which were excluded due to there being duplicates. We take those points and pass it through each of the decision trees they were not included in the making of and see how many are correctly predicted according to our model. This helps us score our model.