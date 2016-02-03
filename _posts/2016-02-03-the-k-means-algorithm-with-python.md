---
layout: post
title: The k-Means algorithm with Python
modified:
categories: 
excerpt:
tags: []
image:
  feature:
date: 2016-02-03T21:12:35+02:00
---

The k-means algorithm works by spliting a given unlabeled dataset into a fixed number of clusters. Every cluster is represented by centroid . The goal of k-means algorithm is to find the best division of n enteties in k groups , so that the total distance between the clusters members and its coresponding centroid is minimized. The distance between cluster members is calculated using Eculided distance .

~~~ python
import numpy as np

a = np.array((1.0,1.5,3.0,5.0,3.5,4.5,3.5))
b = np.array((1.0,2.0,4.0,7.0,5.0,5.0,4.5))
y = np.column_stack((a,b))

~~~