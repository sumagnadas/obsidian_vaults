This is basically an algorithm which draws a line according to the data such that it divides it into two parts. Cannot be used if the line cannot be drawn.

## Coding aspect so far
---
- `sklearn`
	- `linear_model.LogisticRegression` -> class for logistic regression based on sigmoid function.
	- `datasets.make_classification` -> function for creating data for classification models.
- `np.insert` -> insert a array before the index specified on the specified axis.
## Mathematical aspect
---
#### Perceptron trick
***
Basically we use the fact that a line has a negative and positive side.
Each side corresponds to a category(?) and we denote it on our own to make it a model.
Here we use $Ax+By+C=0$ equation of a line.

![[Pasted image 20250619032140.png]]
For making a point $(x,y)$ go to the positive side of a line, we add the coordinates after multiplying with the learning rate $\eta$ i.e $\eta\cdot(x,y,1)$ to $(A,B,C)$ and put the $(A,B,C)$ back it into the line. For going to the negative side, we subtract the coordinates $\eta\cdot(x,y,1)$ from $(A,B,C)$.
Now to understand which side is positive, just use the ole' "put (0,0)" method and see if its positive or negative.

For the algorithm, instead of $x$ and $y$, we use $x_1$ and $x_2$ which represent the two columns and hence, write the line as
$W_0 + W_1x_1+W_2x_2=0$  where $W_0=C, W_1=A, W_2=B$. Now, to predict outcome for $(x_1,x_2)=(x,y)$, we will calculate $W_0+W_1x+W_2y$ and see if it is positive or negative and apply our results accordingly. Basically do a $\begin{bmatrix}1 \\ x \\ y\end{bmatrix}\cdot\begin{bmatrix}W_0 \\ W_1 \\ W_2\end{bmatrix}$  and see if its positive or negative.
We can also generalize this to $m$ number of columns as 
$$
\begin{bmatrix}1 \\ x_1 \\ x_2 \\ ... \\ x_m\end{bmatrix}\cdot\begin{bmatrix}W_0 \\ W_1 \\ W_2 \\ ... \\ W_m \end{bmatrix}
$$
This method has a problem -> it only takes into account the misclassified point during calibrating or fitting the model. That means that for future unknown data, it might or might not be accurate; it might overfit to the training data. (see the `logistic-reg1.ipynb` file for more details.)
Okay, P.S. by ***adding*** something to weights vector, we are ***rotating*** it ***CCW*** and ***moving*** it ***down*** and by ***subtracting***, we are ***rotating*** it ***CW*** and ***moving*** it ***up***.
So short note -> (+) => rotate CCW, move down => dure shoraye from +ve side
	   		  (-) =>	move up, rotate CW => dure shoraye from -ve side
#### Sigmoid function
---
![[Pasted image 20250619184820.png|300]]
The problem with the perceptron trick is that after accommodating to the misclassified points, the line won't move any further and this might lead to overfitting, a situation where a model is accurate only for the training data and doesn't work very well for the test data or actual IRL data. 
On the other hand, the `LogisticRegression` class of `sklearn` is more accurate since it not only accommodates to the misclassified data, it also takes it into account the classified points. For example let us assume the data we got is very separated from each other, like we have either the minimum part or the maximum part.  This might make it easier for actually knowing how different it is but how do we approximate for the middle empty part.
![[Pasted image 20250619184952.png|300]]
As we can see, `sklearn` aside from the perceptron trick system, it went further and tried to give roughly the same amount of space on each side i.e. right in the middle of the empty space whereas the perceptron trick only goes up to until all the misclassified points are classified perfectly.

To fix that, we have to take into account the classified points as well. In the perceptron trick, the misclassified points basically pull the line towards themselves. Now along with that, the classified points will also push the line away from themselves. The amount of push will also depend on the distance of the point; for classified points, farther the point from the line, lesser the push and for misclassified points, farther the point, greater the pull.

For that, we will use the sigmoid function instead of the step function which goes as 
$$
\sigma(z)=\frac{1}{1+exp({-z})}
$$
 with a graph ->
![[Pasted image 20250619211200.png|400]]
As we can see, it also gets the value between 0 to 1 and instead of a definitive 0 or 1, it will technically give the probability of the point being on the one side more than the other. At 0, its 50-50 on the two sides.
Hence the step function => `1 if line(x,y) >=0 else 0` is replaced by the sigmoid function and its done.
See below how the line up-down works with the $y_i - \hat{y_i}$ as $\hat{y}_i$ changed.
![[Pasted image 20250619210520.png]]
#### Maximum Likelihood
---
Now the sigmoid function has made it somewhat better but how to make sure the model is choosing the best line out of all the possible lines? To do that, we would have to find some kind of loss function which would help use compare the lines to find the best one among them.
Hence we find maximum likelihood for that model, which for this model is basically the product of all the probabilities of it belonging to its correct class according to the current model. 
![[Pasted image 20250620181950.png]]
For example, a red point according to model 1 above the line has a probability of it belonging to red class of 0.4, which in model 2 is higher as it is correctly classified.This shows that model 2 has a higher probability labeling the classes correctly according to the probability. Now we multiply all these probabilities to find the maximum likelihood.
$$
M.L. = (\prod_{i=1}^k {P_i(R)})\cdot(\prod_{i=1}^l {P_i(G)})
$$
where there are $k$ points belonging to red class/-ve region and $l$ points belonging to the green class/+ve region.

For the ideal model, this will be 1 as all the points are correctly classified and positioned as such that they are situated faraway from the line
For the best real-life model though, this will be maximum(probably not 1) compared to the other lines as the points in the dataset are correctly classified with their high probabilities showing the amount of correctness it has.

But using it in an algorithm is problematic as the multiplication will go near to 0 for a massive dataset and hence, we have to find a better summation alternative from this. To do that, just apply $log$ on it.
$$\begin{align}
log(M.L.) = log(\prod_{i=1}^k {P_i(R)}) + log(\prod_{i=1}^l {P_i(G)}) \\
= \sum_{i=1}^k log(P_i(R)) +\sum_{i=1}^l log(P_i(G))\end{align}
$$

We have to maximize this function for it to be a better model.
Now, since this is lesser than 1, it will be negative, so we will just apply a minus sign to make it positive but now we will have minimize the new function instead of maximizing it i.e.
$$
L=-log(M.L.)= -\sum_{i=1}^k log(P_i(R)) -\sum_{i=1}^l log(P_i(G)) 
$$
Hence the generalized function for finding the best model by minimizing it is this function. Now we have to convert it into data point notation.

The probabilities in the data point's view is basically the predicted value but in our case it gives the probability of the point being green so we can't put it directly in the log function. So how can we differentiate it for green points and red points?
We know that, $y_i=0$ for a red point and $y_i=1$ for green points. Also we have to take $P(G) = \hat{y_i}$ for green points and $P(R) = 1- \hat{y_i}$ for red points. 
Hence the formula becomes,
$$L= -(\sum_{i=1}^n {y_ilog(P(G)) + (1-y_i)log(P(R))})$$
i.e.
$$L= -(\sum_{i=1}^n {y_ilog(\hat{y_i}) + (1-y_i)log(1-\hat{y_i})})$$
Here the summation term will turn to $log(\hat{y_i})$ for green points due to $y_i$ being 1 and to $log(1-\hat{y_i})$  for red points due to $y_i$ being 0.

This is total error function of the model and this function is also known as log-loss error or binary cross-entropy. Using this function, we can do gradient descent to find the $W_0,W_1,W_2,...,W_m$ for a logistic regression where $m$ denotes the number of independent columns.
#### Gradient Descent
---
First of all, we need the derivative of the sigmoid function.
![[Pasted image 20250620191822.png]]
$= \sigma(x)[1 - \sigma(x)]$
$$\therefore\sigma^`(x)=\sigma(x)[1 - \sigma(x)]$$
For $m$ columns and $n$ rows, the prediction $\hat{y_i}$ becomes $\sigma(\begin{bmatrix}1 \\ x_{i1} \\ x_{i2} \\ ... \\ x_{im}\end{bmatrix}\cdot\begin{bmatrix}W_0 \\ W_1 \\ W_2 \\ ... \\ W_m \end{bmatrix})$.
We can write all the prediction values in a matrix form i.e.
$$\hat{y}
=\begin{bmatrix}\hat{y_1}\\\hat{y_2}\\\hat{y_3}\\...\\\hat{y_n}\end{bmatrix}
= \sigma(\begin{bmatrix}1 & x_{11} & x_{12} & ... & x_{1m} \\ 1 & x_{21} & x_{22} & ... & x_{2m}\\ 1 & x_{31} & x_{32} & ... & x_{3m}\\ ... \\1 & x_{n1} & x_{n2} & ... & x_{nm}\end{bmatrix}\cdot\begin{bmatrix}W_0 \\ W_1 \\ W_2 \\ ... \\ W_m \end{bmatrix})
$$
$$
\therefore\hat{y}=\sigma(xW)
$$
where $x$ denotes the variable matrix and $W$ denotes the weight matrix.
Now we can write, for each row,
$$\begin{align}
L = -&\frac{1}{n}(\sum_{i=1}^n {y_ilog(\hat{y_i}) + \sum_{i=1}^n (1-y_i)log(1-\hat{y_i})}) \\
  = -&\frac{1}{n}((y_1log(\hat{y_1})+y_2log(\hat{y_2})+...+y_nlog(\hat{y_n})) + \\
     &((1-y_1)log(1-\hat{y_1})+(1-y_2)log(1-\hat{y_2})+...+(1-y_n)log(1-\hat{y_n}))) \\\\
  = -&\frac{1}{n}(\begin{bmatrix}y_1 \\ y_{2} \\ ... \\ y_{n}\end{bmatrix}\cdot log(\begin{bmatrix}\hat{y_1}\\\hat{y_2}\\...\\\hat{y_n}\end{bmatrix})+(I_n-\begin{bmatrix}y_1 \\ y_{2} \\ ... \\ y_{n}\end{bmatrix})\cdot log(I_n-\begin{bmatrix}\hat{y_1}\\\hat{y_2}\\...\\\hat{y_n}\end{bmatrix}) \\\\
  = -&\frac{1}{n}(y \cdot log(\hat{y}) + (1-y) \cdot log(1-\hat{y}) \\
  = -&\frac{1}{n}(y \cdot log(\sigma(XW)) + (1-y) \cdot log(1-\sigma(XW))
\end{align}
$$
$y$ is the output variable matrix.
So the average loss function in matrix form comes out to be,
$$
\therefore L(W) = -\frac{1}{n}(y \cdot log(\sigma(xW)) + (1-y) \cdot log(1-\sigma(xW))
$$
where $x,y,W$ denotes the input variable matrix, output variable matrix and weights matrix

$\hat{y} = \sigma(xW)$  => $\sigma^`(xW) = \sigma(xW)[1-\sigma(xW)]=\hat{y}(1-\hat{y})$ 
For gradient descent, we need the derivative of $L$, which comes out to be
$$\begin{align}
L^`(W) &= -\frac{1}{n}((\frac{y}{\sigma(xW)})\sigma^`(xW)x-(\frac{1-y}{1-\sigma(xW)})(-\sigma^`(xW))x) \\
	&= \frac{1}{n}(\frac{1-y}{1-\hat{y}}\hat{y}(1-\hat{y})x -\frac{y}{\hat{y}}\hat{y}(1-\hat{y})x) \\
	&= \frac{1}{n}x(\hat{y}(1-y)-y(1-\hat{y})) = \frac{1}{n}x(\hat{y}-y\hat{y}-y+y\hat{y}) \\
	&= -\frac{1}{n}x(y-\hat{y}) 
\end{align}
$$
$$ 
\therefore L^`(W) =  \frac{1}{n}x(y-\sigma(xW))
$$
where $x,y,W$ denotes the input variable matrix, output variable matrix and weights matrix.

The gradient descent will be done using,
$$
W_{new} = W_{old} - \eta.L^`(W)
$$
