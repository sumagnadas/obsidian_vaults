![[Pasted image 20250621140839.png]]
This is a decision tree. As you can see, its basically a lot of if-else statement which help us categorize data into multiple classes. This is one of the basic classification algorithms.
Also, the first node is known as root node(top blue). The ones which have arrows going both in and out of them are known as branches (blue other than the top one) and the ones which have arrows only going into them are known as leaves (green)
## Coding Aspect
---
- `sklearn`
	- `preprocessing.LabelEncoder` -> encode all the label categories to numerical categories
	- `tree.DecisionTreeClassifier` -> Make a decision tree model based on a dataset
		- `score` -> function to see how good it predicted for a test dataset (probably present in every models' class)
## Mathematical Aspect
---
Although this looks like many if-else statements, the way we order the if-else is very important. If we choose it right, we can optimize the categorization at the top before it goes down very deep. The way we choose the ordering is by seeing how impure the leaves are from each if-else.
#### Gini Impurity
---
One of the ways of checking which one is less impure is by seeing the Gini impurity of the branch.
[[HK Decision Tree.canvas|HK Decision Tree]]
Imagine, we want to predict if someone likes HK or not from the fact that they love platforming games and/or Metroidvanias.
To do that, we divide it into the Yes/No graphs for each column(for binary values) as shown in the canvas and count the number of people who like and doesn't like HK on the previous condition. (again binary classification for now)
Now from all these counting, we can calculate the total Gini impurity for each branch, but before that, we need to find the Gini impurity for each leaf.
It is calculated by
$$
G.I = 1 - P(Yes)^2 - P(No)^2
$$
Then, total Gini impurity is calculated using 
$$
T.G.I = (Probability\ of\ people\ going\ in\ the\ leaf)*G.I._{leaf}
$$
The one which is impure is on top. Then this just goes on in the branches until we reach a definitive classification.
For numerical/continuous data, this process is a bit more complicated. Same system of using the lower $G.I.$ but finding the $G.I.$ is a bit complicated. For that, we first sort the data in ascending order and then calculate the average value for each adjacent rows. These values are then used for calculating the $G.I.$ values by seeing how many people like/dislike HK by using the <`avg_value` as True and and >`avg_value` as False which again creates two branches for each average value and each average value will have a Gini Impurity value.
Eg.
![[Pasted image 20250622013803.png]]