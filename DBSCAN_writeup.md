DBSCAN
Our goal right now is to use DBSCAN to create clusters and make another, hopefully information gaining feature.
First thing I noticed when working with this algorithm was the sensitivity of the parameters: by varying them in increments of 0.1 for epsilon and 1 for min_samples, the number of clusters could vary from around 5 to above 30. For this dataset, there seems to be an average of about 3-5% noise points with certain parameters, which is not too bad. However, one cluster seems to get the vast majority of data points--using 100,000 points 96% were in the 'primary' cluster, probably signifying that the algorithm cannot tell apart the vast majority of the comments.
I made my own parallel-coordinates like plot for graphing the points and their attributes. Cluster is determined by the color, attribute is on the X axis and value is on the Y axis; I didn't graph any attribute of a point where the attribute value was 0. While it is easier this way to see if certain attributes count for certain clusters, it is harder to tell the same for multiple attributes at a time--maybe some kind of graph plotting each attribute in each cluster's relationship with each other?
Going back to the goal, we were targeting around 20 clusters for the dataset--the issue is that (1) there is a large proportion of points just in one cluster, so for most it wouldn't be helpful and (2) the mutual information and silhouette coefficients, which we want to maximize, only take on larger values when there are around 2-4 clusters made, then goes down as the number of clusters made increases.

With eps=1, min_samples=50, datapts=159571, number of features=67, method=SelectKBest:
percent of points in first cluster 90.3%
Estimated number of clusters: 14
Estimated percent of noise points: 9.2%
Homogeneity: 0.006
Completeness: 0.005
V-measure: 0.005
Adjusted Rand Index: -0.04
Adjusted Mutual Information: 0.005
Silhouette Coefficient: 0.388
Time: 945s

With eps=0.3, min_samples=25, datapts=100000, number of features=67, method=TruncatedSVD:
percent of points in first cluster: 50.7%
Estimated number of clusters: 81
Estimated percent of noise points: 32.1% 
Homogeneity: .118
Completeness: .024
V-measure: .04
Adjusted Rand Index: .04
Adjusted Mutual Information: .04
Silhouette Coefficient: -.16
Time: 742s

So far the TSVD method has taken much longer to train than selectkbest: we believe this is because the TSVD returns dense data, but the original method involved sparse data. There would not be as much data to process with a sparse dataset, which DBSCAN takes.