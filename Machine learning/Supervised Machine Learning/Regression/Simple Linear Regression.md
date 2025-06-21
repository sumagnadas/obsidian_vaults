Basically an algorithm to find a $m$ and $b$ for a  $y = mx+b$ line which has minimum error/difference from the data points in a sort of linear data i.e. find the best fit line that passes closest to the data points.

Here $m$ can be taken as weightage of the variable i.e. how much the package depends on the cgpa of an individual. $b$ is also called the offset telling what happens if the input turns to 0.

## Coding Aspect so far
---
`sklearn`:-
- `model_selection.train_test_split` -> For splitting an available dataset into training and testing dataset based on test/train dataset size, etc
- `linear_model.LinearRegressor` -> Class representing a linear regression model	which finds the parameters using OLS or Ordinary Least Squares
	- `fit` -> function to train the model on data / find $m$ and $b$ for the best fit line
	- `predict` -> function to test/get the prediction for a value
- `linear_model.SGDRegressor` -> Class representing a regression model which finds $m$ and $b$ using Gradient Descent.
- `datasets.make_regression` -> Make a linearly related dataset which can be used for regression.
## Mathematical formulation
---
We have to find the $m$ and $b$ for the best fit line using mathematical operations on a data
- **[[#Closed Form Expression|Closed Form Expression]]** -> Basically a mathematical equation expressed using known operators and functions but no limit, differentiation or integration. Eg. *OLS* -> Mostly used for 1D data?
	- $b=\bar{y}-m\bar{x}$
	- $m=\frac{\sum_{i=1}^n {(x_i-\bar{x_i})(y_i-\bar{y_i})}}{\sum_{i=1}^n {(x_i-\bar{x_i})^2}}$ where $\bar{x},\bar{y}$ are both means of $x$ and $y$ variable.
- **[[#Non-closed Form Expression|Non-closed Form Expression]]** -> Approximate the $m$ and $b$ using some method. Eg. Gradient Descent -> Mostly used for higher dimensional data

#### Closed Form Expression
***
Let $d_1,d_2,...,d_n$ be the difference of the actual values from the line we are choosing to represent the linear relation between the variables. We need to minimize all the $d_j$, $j=1,2,...,n$ as much as possible for it to be the best fit line for the linear model.

![[Data point line.png]]
Loss function, $L = \sum_{i=1}^n d_i^2 = d_1^2 + d_2^2+d_3^2+...+d_n^2$ which we have to minimize for the best fit line.
We could have chosen $L=d_1+d_2+...d_n$ but the difference can be negative and positive so it might cancel out. We could have used the absolute value too but didn't as we are gonna do differentiation with this. Hence we squared them.

Now $d_i=y_i - \hat{y_i}$ where $y_i$ denotes the actual value and $\hat{y_i}$ denotes the predicted value. Again $\hat{y_i}=mx_i + b$
Hence,
$L = \sum_{i=1}^n {(y_i-\hat{y}_i)^2}$ =$\sum_{i=1}^n {(y_i-mx_i - b)^2}$ 

$\therefore L(m,b)=\sum_{i=1}^n {(y_i-mx_i - b)^2}$ which is logical as rotating the line by fixing the y-intercept ($b$) or moving the line up and down by fixing the slope ($m$) will affect the error function. ($y_i$ and $x_i$ are constant with respect to the point).
We use gradient descent to find the least value by taking the grad of L = 0 to find the minima/maxima (though we need the minima).
Derivation for $b$ ->
![[b derivation.png]]
We get, $b=\bar{y} - m\bar{x}$
Derivation for $m$ ->
![[m derivation.png]]
We get,
$$
m=\frac{ \sum_{i=1}^n {(x_i-\bar{x_i})(y_i-\bar{y_i})}}{\sum_{i=1}^n {(x_i-\bar{x_i})^2}}
$$

#### Non-closed Form Expression
***
We use gradient descent for this to approximate the values of the variables in the loss function for which it achieves minima. It is basically an algorithm which takes iterative steps to go to the local minimum of the loss function using the gradient of the function. This is mostly useful for higher dimension data where calculating the values using the closed form expression will be computationally heavy and inefficient.

For a n-dimensional plane, a gradient basically gives the direction of steepest ascent or in which direction the curve/whatever the thing is rises up at that point. You can take the slope of a curve in 2D plane as its gradient (well technically). 

Since the gradient is the direction of steepest ascent at that point, we can take the opposite of to find the steepest descent, following which we will find the minima, like finding the downhill road to go back to ground level. We take small steps in that direction, gradually reaching close to the minimum of the function. 

For example, let us assume that $L(m,b)=\sum_{i=1}^n {(y_i-mx_i - b)^2}$ is the loss function and $m$  or the slope of the line is fixed. Then $L$ will be a parabolic curve w.r.t $b$ as can be seen from the equation. 

Thus, to go to the minima of the function, we may take small steps as such, 
$b_{new} = b_{old} - slope$.
where $slope$ is the slope of the curve at the point $b_{old}$. Now this might lead to us bouncing around on the curve and we might miss the minima value as the value of $slope$ can be very big.
Hence, we might use it as $b_{new} = b_{old} -\eta *slope$  where $\eta$ is the learning rate to decrease the value of $slope$ to reduce the bouncing around. After this, we just fix either the number of iterations or how low we want $b_{new} - b_{old}$.

Now if we want to generalize this whole thing for a loss function of $n$ variables $L(a_1,a_2,...,a_n)$ we can apply the same $b_{new}$ formula we used earlier except this time,for the slope part we do partial differentiation w.r.t the variable we are updating and use that formula i.e. basically the equation becomes 
$\begin{bmatrix}a_1 \\ a_2\\ ... \\ a_n \end{bmatrix}_{new} = \begin{bmatrix}a_1 \\ a_2\\ ... \\ a_n \end{bmatrix}_{curr}  - \eta\begin{bmatrix}\frac{\partial L}{\partial a_1} \\ \frac{\partial L}{\partial a_2}\\ ... \\ \frac{\partial L}{\partial a_n} \end{bmatrix}_{old}$   
where the 2nd gradient vector is with respect to the current coordinates.