Basically an algorithm to find a $m$ and $b$ for a  $y = mx+b$ line which has minimum error/difference from the data points in a sort of linear data i.e. find the best fit line that passes closest to the data points.

Here $m$ can be taken as weightage of the variable i.e. how much the package depends on the cgpa of an individual. $b$ is also called the offset telling what happens if the input turns to 0.

## Coding Aspect so far
---
`sklearn`:-
- `model_selection.train_test_split` -> For splitting an available dataset into training and testing dataset based on test/train dataset size, etc
- `linear_model.LinearRegressor` -> Class representing a linear regression model	which finds the parameters using OLS or Ordinary Least Squares
	- `fit` -> function to train the model on data / find $m$ and $b$ for the best fit line
	- `predict` -> function to test/get the prediction for a value
- `linear_model.SGDRegressor` -> Class representing a regression model which finds $m$ and $b$ using Gradient Descient.
## Mathematical formulation
---
We have to find the $m$ and $b$ for the best fit line using mathematical operations on a data
- **Closed Form Expression** -> Basically a mathematical equation expressed using known operators and functions but no limit, differentiation or integration. Eg. *OLS* -> Mostly used for 1D data?
- **Non-closed From Expression** -> Approximate the $m$ and $b$ using some method. Eg. Gradient Descent -> Mostly used for higher dimensional data