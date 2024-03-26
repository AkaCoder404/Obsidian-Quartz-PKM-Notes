# K-Means
A clustering algorithm.

**Sources**:
- 📹 [StatQuest: K-means clustering](https://www.youtube.com/watch?v=4b5d3muPQmA)

**Background**

**Algorithm**
Let's take a look at the pseudo code for the algorithm.
```pseudo
\begin{algorithm}
    \caption{K-means Clustering} 
    \begin{algorithmic}
      \Procedure{kmeans}{}
	    \State Initialize K clusters
	    \State Calculate distance to each point from each centroid
	    \State Assign each point to its closest centroid
	    \State Compute new centroids, which is mean of data points in the cluster
	    \State Repeat 3,4,5 until the centroids no longer move 
      \EndProcedure
      \end{algorithmic}
    \end{algorithm}
```
This results in clusters with minimum error (most dense cluster).

There are three different variables that we have to consider, the cluster initialization algorithm, the type of distance calculation, and the optimal $K$ to chose.

Different centroid initialization methods may be used
	- K-means++: https://stackoverflow.com/questions/38497982/k-means-vs-random-for-initial-centers
	- Random

Different multi-dimensional similarity/distance measurements may be used
- euclidean distance
- cosine similarity
- average distance

**Key**
- Relatively efficient for medium and large sized databases. 
- It is a partitioning clustering algorithm that divides data into non-overlapping subsets without any **cluster-internal** structure.
- K-means tries to minimize **intra-cluster** distances and maximize **inter-cluster** distances.
- It is a heuristic algorithm, no guarantee that it will converge to the global optimum, and the result may depend on the initial clusters. Thus, it is good to start process with multiple starting random centroids.


## Accuracy
- External Approach
- Internal Approach
	- Average the distance between data points within a cluster

