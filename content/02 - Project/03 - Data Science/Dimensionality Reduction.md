---
creation_date: 2023年11月16日
banner: "![[daily-note-banner.gif]]"
banner_icon: "🌞"
tags: "#笔记"
banner_y: 0.4705
---

# Dimensionality Reduction

*Find a new set of multivariate variables that are uncorrected and explain as much variance as possible*

*If you put all the variables together in one matrix, find the best matrix created with fewer variables (lower rank) that explains the original data*

# 01 Background

# 02 Algorithms
### t-SNE

### PCA Principal Component Analysis
Use lower dimensional representation taht explains most of the variation in the data.
Factor Analysis
Independent COmponent Analysis
Latent Semantic Analysis

The paper by Jonathon Shlens of Google Research is
| called, A Tutorial on Principal Component Analysis. |

 A matrix X has the singular value decomposition UDV^t. The principal components of X are ? COlumns of V

| A matrix X has the singular value decomposition UDV^t. The singular values of X are found where?

1: the columns of V
2: the columns of U
3: the columns of D
**4: the diagonal elements of D**

| True or False? D gives the singular values of a matrix in decreasing order of weight.

1: False
**2: True**

 To see LEFT singular vectors of sub1, which component of svd1 would we examine?

1: v
**2: u**
3: d
4: x


### SVD Singular Value Decomposition
If *X* is a matrix with each variable in a column and each observation in a row then the SVD is a "matrix decomposition".
$$X = UDV^T$$
where the columns of U are orthogonal (left singular vectors), the colums of V are orthogonal (right singular vectors) and D is a diagonal matrix (singular values)