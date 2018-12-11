
# Performing Principal Component Analysis (PCA)

## Introduction

In this lesson we shall look at the steps we needed to perform a Principal Components Analysis on a set of data. We will look at an explanation of what is happening at each point so that we can make informed decisions about when to use this technique.

## Objectives.
- Understand the steps required to perform PCA on a given dataset
- Understand and explain the role of Eigen decomposition in PCA


## Step 1: Get some data

Let's use a simple example for our dataset. Our data only has 2 dimensions, as this will allow to create plots of the data to show what the PCA analysis is doing at each step. The x and y variables in our data as shown below:

<img src="data.png" width=150>



## Step 2: Subtract the mean

For PCA to work properly, we have to subtract the mean from each of the data dimensions. The mean subtracted is the average across each dimension. So, all the $x$ values
have $\bar{x}$ (the mean of the $x$ values of all the data points) subtracted, and all the $y$ values
have $\bar{y}$ subtracted from them. This produces a data set whose mean is zero as shown below:
<img src="adj.png" width=220>

The plot for above data would look like below:
<img src="plot.png" width=300>

## Step 3: Calculate the covariance matrix

This is done in exactly the same way as was discussed earlier. Since the data is 2 dimensional, the covariance matrix will be 2 x 2 = 4 dimensional. Below is what the covariance matrix for above data would look like:

<img src="cov.png" width=300>

Since the non-diagonal elements in this covariance matrix are positive, we should expect that both the x and y variable increase together.

## Step 4: Calculate the eigenvectors and eigenvalues of the covariance matrix

Since the covariance matrix is square, we can calculate the eigenvectors and eigenvalues for this matrix. These are rather important, as they tell us useful information about our data. Here are the EIgenvectors and eigenvalues for above data. Calculating these by hand requires tedious matrix manipulation. We'll soon find out how to do this pretty easily in Python. 
<img src="eigen.png" width=400>

### So what do they mean? 

<img src="EIGENPLOT.png" width=400>

If we look at the plot above, the data has quite a strong pattern. As expected from the covariance matrix, they two variables do indeed increase together. On top of the data we have both the eigenvectors as well. They appear as diagonal dotted lines on the plot, perpendicular to each other. 

Eigenvectors provide us with information about the patterns in the data. See how one of the eigenvectors goes through the middle of the points, like drawing a line of best fit? That eigenvector is showing us how these two data sets are related along that line.

The second eigenvector gives us the other, less important, pattern in the data, that all the points follow the main line, but are off to the side of the main line by some amount.

So, by this process of taking the eigenvectors of the covariance matrix, we have been able to extract lines that characterize the data. The rest of the steps involve transforming the data so that it is expressed in terms of them lines.

## Step 5: Choosing components and forming a feature vector

If we look at the eigenvectors and eigenvalues above, we see that the eigenvalues are quite different values. In fact, it turns out that the eigenvector with the highest eigenvalue is the principal component of the data set.
In our example, the eigenvector with the larges eigenvalue was the one that pointed down the middle of the data. It is the most significant relationship between the data dimensions.

In general, once eigenvectors are found from the covariance matrix, the next step is to order them by eigenvalue, highest to lowest. This gives us the components in order of significance. We can also decide to ignore the components of lesser significance. We do lose some information in the process, but if the eigenvalues are small, we  don’t lose much. 
> If we leave out some components, the final data set will have less dimensions than the original.

*To be precise, if we originally have $n$ dimensions in our data, we can calculate $n$ eigenvectors and eigenvalues, and then you choose only the first $p$ eigenvectors, then the final data set has only $p$ dimensions.*

Next, we need to form a __feature vector__, which is just a fancy name for a matrix of vectors. This is constructed by taking the eigenvectors that we want to keep from the list of eigenvectors, and forming a matrix with these eigenvectors in the columns as shown below:

$$FeatureVector = [eig_1, eig_2, .., eig_n]$$

Given our example set of data, and the fact that we have 2 eigenvectors, we have two choices. We can either form a feature vector with both of the eigenvectors: 

<img src="feat1.png" width=300>

OR, we can choose to leave out the smaller, less significant component and only have a single column: 

<img src="feat2.png" width=150>

Next, we'll see the effect of this process on final data.

## Step 5: Deriving the new data set

This the final step in PCA, and is also the easiest. Once we have chosen the components (eigenvectors) that we wish to keep in our data and formed a feature vector, we simply take the transpose of the vector and multiply it on the left of the original data set, transposed.

$$FinalData = RowFeatureVector~*~RowDataAdjust$$

where $RowFeatureVector$ is the matrix with the eigenvectors in the columns transposed so that the eigenvectors are now in the rows, with the most significant eigenvector at the top, and $RowDataAdjust$  is the mean-adjusted data transposed, ie. the data items are in each column, with each row holding a separate dimension. 

*The transpose mentioned above may be a bit confusing at this stage, but according to matrix algebra and multiplication rules, all further calculations become much easier if we perform this step here. You'll get to see more of this as we work with real data later on.*

### Outcome of the process

This process will give us the original data solely in terms of the vectors we chose. Our original data set had two axes, x and y, so our data was in terms of them. It is possible to express data in terms of any two axes that we like. If these axes are perpendicular, then the expression is the most efficient. This was why it was important that eigenvectors are always perpendicular to each other.

We have changed our data from being in terms of the axes x and y, and now they are in terms of our 2 eigenvectors. In the case of when the new data set has reduced dimensionality, ie. we have left some of the eigenvectors out, the new data is only in terms of the vectors that we decided to keep.


The transpose of the result in each case is also taken, to bring the data back to the nice table-like format as shown below:

<img src="transplot.png" width=400>

The plot below shows the impact of this process on our data. The final transformation is performed with each of the possible feature vectors. The data is plotted with final points to show how they relate to the components.

<img src="transformed.png" width=400>

In the case of keeping both eigenvectors for the transformation, the plot is basically the original data, rotated so that the eigenvectors are the new axes. This is understandable since we have lost no information in
this decomposition.



### Reducing Data 
The other transformation we can make is by taking only the eigenvector with the largest eigenvalue. The table of data resulting from that is shown below. 

<img src="trans3.png" width=250>


As
expected, it only has a single dimension. If we compare this data set with the one resulting from using both eigenvectors, we see that this data set is exactly the first column of the other. So, if we were to plot this data, it would be 1 dimensional,

and would be points on a line in exactly the x positions of the points in the plot above. We have effectively thrown away the whole other axis, which is the other eigenvector.

## Conclusion 

Basically we have transformed our data so that is expressed in terms of the patterns between them, where the patterns are the lines that most closely describe the relationships between the data. This is helpful because we
have now classified our data point as a combination of the contributions from each of those lines.

Initially we had the simple x and y axes. This is fine, but the x and y values of each data point don’t really tell us exactly how that point relates to the rest of the data. Now, the values of the data points tell us exactly where (ie. above/below) the trend lines the data point sits. In the case of the transformation using both eigenvectors, we have simply altered the data so that it is in terms of those eigenvectors instead of
the usual axes. But the single-eigenvector decomposition has removed the contribution due to the smaller eigenvector and left us with data that is only in terms of the other.

## Further Reading
- [PCA process explained](https://coolstatsblog.com/2015/03/21/principal-component-analysis-explained/)

- [PCA in four steps](https://alexhwoods.com/pca/)

## Summary 

In this lesson, we looked at the steps required to perform PCA on a simple dataset. Next we shall try to implement this approach in Python, Numpy and scikit-learn to run PCA on a slightly complex dataset. 
