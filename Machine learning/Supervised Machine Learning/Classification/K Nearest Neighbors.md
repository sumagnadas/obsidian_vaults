This algorithm basically predicts features of a test point based on the k nearest points of the test point to see which is more common(for classification) or the weighted average(for regression).
## Coding Aspect
---
- `sklearn`
	- `impute`-> For imputing missing values in a dataset
		- `SimpleImputer`-> Impute missing values by mean of the feature
		- `KNNImputer`-> Impute missing values based on the K nearest Neighbors of the data point.
	- `metrics.accuracy_score` -> used for getting accuracy of the model
## Algorithm for imputation
---
1. For a column with missing values, take the data point with missing values and find pairwise distance between them. This is done using NaN Euclidean distance which works as follows :-   ![[Pasted image 20250625192825.png]]
2. Now the value of the k nearest neighbours according to distance is taken and weighted average is calculated. We can take uniform weights ($w=1$) or distance-based weights ($w=1/d$).