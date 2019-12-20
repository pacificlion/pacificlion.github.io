---
published: false
---
### Option 1 – Good Visualization
 [link to course](https://www.coursera.org/learn/datavisualization/)
#### Background


Review the definition of data visualization and the differences between interactive visualization, presentation visualization, and online storytelling

#### Question

What web page URL would you point people to as a good example of presentation visualization or interactive storytelling? Which is it and why is it a good example of this type? What about the web page impresses you the most? How does the web page help you understand the data and gain insight into what the data represents?

#### Option 1 : Interactive Visualization : Clustering Visualization by [Naftali](https://www.naftaliharris.com/)

I found a [webpage](https://www.naftaliharris.com/blog/visualizing-k-means-clustering/) on Clustering data useful for interactive storytelling. It explains how K Means Clustering algorithm is used for grouping data which are closer to each by distance in same groups. . It is an excellent example of interactive storytelling because the number of data points are pre-configured by the developer, but it allows user to choose how data points are arranged like randomly or in gaussian distribution.

I found the visualization very impressive because it helps to see the effect of different input parameters like number of groups, i.e. centroids you wish to make and see how the groups change with change in number of groups. It also showcases each data point in different colors along with the centroid for each group which acts as a representative for that group allowing nice visualization. The centroids can be randomly assigned by the user. The centroids change with each iteration as the distances of each point is computed and the data point closest to a centroid is assigned that group. The centroid is recalculated as an average of data point coordinates. This iterative process is shown on click of “Update Centroids” which gives the user the actual insights of how clustering the data works.

The webpage has helped me to understand that data distribution is important for initial analysis of number of groups formed. This has also helped to gain insights into how the data can be clustered together based on distance and helped me to understand KMeans clustering algorithm on different distributions. It also brought me the insight how my initial centroids chosen by me can impact the groups formed. There is always a chance that the clusters formed on same data points for same number of groups are different because of the initial centroids chosen irrespective of how many iterations a user runs. It explains the limitation of this algorithm which is that the cluster formation is greatly dependent on initial selection of centroids.
