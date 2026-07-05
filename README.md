Dataset Overview
The dataset refers to clients of a wholesale distributor and includes 440 observations with 8 variables: six numerical and two categorical. The numerical variables represent annual spending in monetary units (m.u.) on different product categories:

1. FRESH: Spending on fresh products
2. MILK: Spending on milk products
3. GROCERY: Spending on grocery products
4. FROZEN: Spending on frozen products
5. DETERGENTS_PAPER: Spending on detergents and paper products
6. DELICATESSEN: Spending on delicatessen products

The categorical variables are:
1. CHANNEL: Customer channel, either Horeca (Hotel/Restaurant/Café) or Retail
2. REGION: Customer region, including Lisbon, Oporto, or Other 


Based on the evaluation table and the explanation provided at the end of this report, here is an explanation of the conclusion reached:  

1. The Final Decision: 4 Clusters is Optimal
I concluded that dividing the data into 4 clusters provides the most optimal structure. While my initial visual test using 5 clusters showed that some groups overlapped or intersected with each other, dropping it down to 4 clusters provided cleaner, more well-defined boundaries between the segments.

2. Understanding the Performance Metrics
To prove why 4 clusters under the K-Means algorithm was chosen over the Hierarchical Clustering methods (Complete, Average, Single, Ward), this cross-referenced several indices:
2.1. Calinski-Harabasz Index (Higher is Better): This metric measures how dense these clusters are internally and how well-separated they are from each other.
--> K-Means model scored 131.36.
--> This heavily outperformed the hierarchical techniques, where Ward scored 108.6, Complete scored 38.9, and Average/Single scored 8.38. This proves K-Means created much more distinct boundaries.
2.2. Silhouette Score (Closer to 1 is Better): This measures how similar an object is to its own cluster compared to other clusters. My Average and Single hierarchical models yielded a strong silhouette score (0.498), but their exceptionally low Calinski-Harabasz scores indicate that they likely created unbalanced or poorly separated group structures overall.
2.3. Davies-Bouldin Index (Lower is Better): This evaluates the ratio of within-cluster distances to between-cluster distances. My hierarchical Average and Single methods scored very low (0.355), which aligns with their higher Silhouette profiles, but again, failed when considering overall cluster variance and division mapped out by the Calinski-Harabasz score.

Summary of the Conclusion
My conclusion effectively justifies choosing K-Means with k=4. It balances physical visual clarity (avoiding the overlaps seen at k=5) with strong statistical performance—specifically yielding the highest structural distance and tightness across all tested clustering frameworks via the Calinski-Harabasz index.  
