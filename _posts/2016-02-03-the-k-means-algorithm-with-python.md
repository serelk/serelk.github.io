---
layout: post
title: The k-means algorithm with Python
modified:
categories: 
excerpt:
tags: []
image:
  feature:
date: 2016-02-03T21:12:35+02:00
---
The k-means is an algorithm for unsupervised clustering. 
Every cluster is represented by centroid , which is the arithmetic mean of data points. The goal of k-means algorithm is to find the best division  _n_ members in  _k_ clusters , so that the total distance between the clusters members and its coresponding centroid is minimized.


### k-means clustering
* **Pros:** Easy to implement
* **Cons:** Can converge at local minima; slow on very large datasets 
* **Works with:** Numeric values

Let's look at how the k-means operates on the simple dataset. First off , we will generate some data  and use scatter plot to visualize a relationships.


~~~python
np.random.seed(7)
km = np.random.random_sample((20, 2))
~~~


<figure>
	<a href="/images/km1.png"><img src="/images/km1.png"></a>
</figure>

Alright, lets start by assigning a random data points as the initial cluster centroids.For the sake of simplicity, we limit ourselves to only two clusters. 


~~~ python 
np.random.seed(15)
centroid = np.random.random_sample((2, 2))
~~~

<figure>
	<a href="/images/km2.png"><img src="/images/km2.png"></a>
</figure>

On the next step each point in the dataset is assigned to a cluster that has the closest centroid. In order to find the cluster with the most similar centroid, the algorithm must calculate the distance between all points and each centroid. The common practice is to use simple Euclidean distance as  measure  of distance between the cluster members.\\
We start by finding the closest centroid and assigning the point to that cluster. 


~~~ python 

from scipy.spatial import distance
clst_list1 = []
clst_list2 = []
for i in range(km.shape[0]):
     dst1 = distance.euclidean(centroid[0],km[i])
     dst2 = distance.euclidean(centroid[1],km[i])
     if dst1 < dst2:
        clst_list1.append(km[i])
     else:
        clst_list2.append(km[i])
clst1 = np.array(clst_list1)
clst2 = np.array(clst_list2)
~~~

<figure>
	<a href="/images/km1.png"><img src="/images/km3.png"></a>
</figure>

The assignment is done and we  can observe some pattern  Now we can update the centroids with the new value by calculating the mean of clusters data points .

~~~ python 
centroid1 = clst1.mean(0)
centroid2 = clst2.mean(0)
~~~

<figure>
    <a href="/images/km1.png"><img src="/images/km4.png"></a>
</figure>

It makes sense , but we cannot yet be sure that each member has been assigned to the right cluster.  So, we  proceed by comparing the distance between each member to its own cluster centroid and one of the opposite cluster.


~~~ python 

clst_list11 = []
clst_list21 = []
for i in range(km.shape[0]):
     dst11 = distance.euclidean(centroid1,km[i])
     dst21 = distance.euclidean(centroid2,km[i])
     if dst11 < dst21:
        clst_list11.append(km[i])
     else:
        clst_list21.append(km[i])
clst11 = np.array(clst_list11)
clst21 = np.array(clst_list21)
print clst11
~~~
<figure>
    <a href="/images/km1.png"><img src="/images/km5.png"></a>
</figure>

As we can see two data points have moved to the opposite cluster. 
The process should be repeated until no more relocations occur.\\
Finally , lets compare our results to result of scikit learn implementation of k-mens .
We only need these five lines of code to do it.  

~~~ python 

from sklearn.cluster import KMeans
est = KMeans(2) 
est.fit(km)
y_kmeans = est.predict(km)
plt.scatter(km[:, 0], km[:, 1], c=y_kmeans, s=30, cmap='rainbow');
~~~

<figure>
    <a href="/images/km1.png"><img src="/images/km6.png"></a>
</figure>

Looks pretty much the same.











