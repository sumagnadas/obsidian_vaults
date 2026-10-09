## Small Notes
---
- Questions for the data:-
	- How big is the data? -> basically rows and columns
	- How does the data look like? -> see data columns and how might the data look like for them (ba-dum-tssss)
	- What is the data type of cols?  -> whatever the question says
	- Any missing values? (follow up -> how to account for them but thats for EDA)
	- How does data look mathematically? -> count, mean, std. dev, min,max, IQRs look like (useful for numerical columns)
	- Any dups? -> remove columns or rows that might unnecessarily make the model heavy.
	- Correlation between cols? -> Helps when thinking about the model. might also help with missing, dups.
- Interquartile Range - Q1/25th Percentile marks the place/value in data that is greater than 25% of the data. Similarly, there is Q3 which marks the 75th Percentile. *IQR* or *Interquartile Range* is the value/range between Q1 and Q3 i.e. IQR=Q3-Q1. Middle of the IQR is median.
  This is also helpful to mark outliers by setting a stipulated maximum (Q3+1.5\*IQR) and minimum(Q1-1.5\*IQR). Anything outside this range is designated as an outlier.
## Univariate
---
#### Useful in Categorical data
- **Countplot**: Counts frequency of different values in a column/array.
- **Piechart**: See data as percentage.
#### Useful in numerical data
- **Histogram**: Helps in seeing distribution by sorting data into a number of bins.
- **Distplot**: Histogram but gives an idea of the distribution of the data. Gives the PDF of the data i.e. what are the chances that the value you pick is a certain value from the data. Also helps in finding skewness
- **Boxplot**: Marks the Q1,Q3 and IQR, maximum and minimum as well as outliers in the data.
## Multivariate
---
All of the following lists are bivariate but adding `hue`, `style` and `size` (of point) based on other features/columns makes it multivariate analysis
### Numerical - Numerical
- **Scatterplot**: Helps in seeing mathematical(?) relation between two variables. 
- **Pairplot**: Scatterplot between all the numerical columns to see relationship at a glance
- **Lineplot**: Draws a line connecting the points ($X_i$, $Y_i$)
### Numerical - Categorical
- **Barplot**: Shows an average value of the categories
- **Boxplot**: Shows a boxplot wrt. categories now
- **Distplot**: Gives distplot for categories
### Categorical - Categorical
- **Crosstab**: Useful to see general count of data between two columns
- **Heatmap**(?)
- **Clustermap** -> what even is this