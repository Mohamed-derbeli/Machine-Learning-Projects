# Machine-Learning-Projects


### When to Normalize and Standardize:

Standardization is a scaling technique that assumes your data conforms to a normal distribution.
If a given data attribute is normal or close to normal, this is probably the scaling method to use.
It is good practice to record the summary statistics used in the standardization process, so that you can apply them when standardizing data in the future that you may want to use with your model.

Normalization is a scaling technique that does not assume any specific distribution.
If your data is not normally distributed, consider normalizing it prior to applying your machine learning algorithm.
It is good practice to record the minimum and maximum values for each column used in the normalization process, again, in case you need to normalize new data in the future to be used with your model.

### Normalize data before or after split of training and testing data?
The answer would depend on situation. If you have the entire data you need for the project then normalizing before splitting should be fine. However, if you are training your model in anticipation that it should work on the data that is going to be generated in the future, then you would have to go ahead with the following steps:
* 1. split the raw data into training and test sets
* 2. normalize the training set and save the normalization parameters
* 3. built the model. 
* 4. normalize the test set using the training normalization parameters
* 5. test the model.
* 6. denormalize predictions using those same parameters to return the user output in the original scale

### Which transformation to pick?
If you decide that your data should follow a normal distribution and needs transformation, there are simple and highly utilized power transformations we will have a look at. They transform your data to follow a normal distribution more closely. 

#### 1. Simple Transformations
##### Right (positive) skewed data:
* Root ⁿ√x. Weakest transformation, stronger with higher order root. For negative numbers special care needs to be taken with the sign while transforming negative numbers:
* Logarithm log(x). Commonly used transformation, the strength of this transformation can be somewhat altered by the root of the logarithm. It can not be used on negative numbers or 0, here you need to shift the entire data by adding at least |min(x)|+1.
* Reciprocal 1/x. Strongest transformation, the transformation is stronger with higher exponents, e.g. 1/x³. This transformation should not be done with negative numbers and numbers close to zero, hence the data should be shifted similar as the log transform.
##### Left (negative) skewed data:
* Reflect Data and use the appropriate transformation for right skew. Reflect every data point by subtracting it from the maximum value. Add 1 to every data point to avoid having one or multiple 0 in your data.
* Square x². Stronger with higher power. Can not be used with negative values.
* Exponential eˣ. Strongest transformation and can be used with negative values. Stronger with higher base.
##### Light & heavy tailed data:
* Subtract the data points from the median and transform. Deviations of the tail from normality are usually less critical than skewness and might not need transformation after all. The subtraction from the median sets your data to a median of 0. After that use an appropriate transformation for skewed data on the absolute deviations from 0 on either side. For heavy-tailed data use transformations for right skew to pull in on the median and for light-tailed data use transformations for left skew to push data away from the median.
#### 2. Automatic Transformations
There are various implementations of automatic transformations in R that choose the optimal transformation expression for you. They determine a lambda value which is the power coefficient used to transform your data closest to a normal distribution.
* Use Lambert W x Gaussian transform. The R package LambertW has an implementation for automatically transforming heavy or light tailed data with Gaussianize().
* Tukey’s Ladder of Powers. For skewed data, the implementation transformTukey()from the R package rcompanion uses Shapiro-Wilk tests iteratively to find at which lambda value the data is closest to normality and transforms it. Left skewed data should be reflected to right skew and there should be no negative values.
* Box-Cox Transformation. The implementation BoxCox.lambda()from the R package forecast finds iteratively a lambda value which maximizes the log-likelihood of a linear model. However it can be used on a single variable with model formula x~1. The transformation with the resulting lambda value can be done via the forecast function BoxCox(). There is also an implementation in the R package MASS. Standard Box-Cox can not be used with negative values, two-parameter Box-Cox however can.
* Yeo-Johnson Transformation. This can be seen as an useful extension to the Box-Cox. It is the same as Box-Cox for non-negative values and handles negative and 0 values as well. There are various implementations in R via packages car, VGAM and recipes in the meta machine-learning framework tidymodels.

### Normality testing:
To test the normaliy 2 methos can be used:
* Graphs (Q-Q plot, Frequency distribution)
* Testing P using one of the foloowing technique: Shapiro-Wilk, D'agostino-Pearson omnibus, Kolmogorov-Smirnov. if P>0.05 --> accept the Null hypothesis (normally distributed), if P <= 0.05 --> reject the Null hypothesis.

