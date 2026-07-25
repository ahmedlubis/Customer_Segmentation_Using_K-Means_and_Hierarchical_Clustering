# 🛒 Wholesale Customer Segmentation using K-Means Clustering

## 📖 Dataset Overview

This dataset contains purchasing information from clients of a **wholesale distributor** and is commonly used for customer segmentation and clustering analysis.

The dataset consists of **440 customer observations** with **8 variables**, including **6 numerical features** representing annual spending across product categories and **2 categorical features** describing customer characteristics.

---

# 📊 Dataset Variables

## Numerical Variables

The numerical variables represent each customer's **annual spending** (in monetary units) across different product categories.

| Variable | Description |
|----------|-------------|
| **FRESH** | Annual spending on fresh products |
| **MILK** | Annual spending on milk products |
| **GROCERY** | Annual spending on grocery products |
| **FROZEN** | Annual spending on frozen products |
| **DETERGENTS_PAPER** | Annual spending on detergents and paper products |
| **DELICATESSEN** | Annual spending on delicatessen products |

---

## Categorical Variables

The categorical variables provide additional information about each customer.

| Variable | Description |
|----------|-------------|
| **CHANNEL** | Customer type: **Horeca (Hotel, Restaurant, Café)** or **Retail** |
| **REGION** | Customer location: **Lisbon**, **Oporto**, or **Other** |

---

# 🎯 Objective

The objective of this analysis is to identify natural customer segments based on purchasing behavior using **unsupervised machine learning**, specifically the **K-Means Clustering** algorithm.

The resulting customer groups can be used to support:

- Customer segmentation
- Targeted marketing strategies
- Product recommendation
- Inventory planning
- Customer relationship management (CRM)

---

# 📈 Clustering Evaluation

Several clustering algorithms were evaluated and compared using multiple cluster validation metrics, including:

- K-Means Clustering
- Hierarchical Clustering (Complete Linkage)
- Hierarchical Clustering (Average Linkage)
- Hierarchical Clustering (Single Linkage)
- Hierarchical Clustering (Ward Linkage)

The evaluation aimed to determine the clustering method and number of clusters that best represented the underlying customer structure.

---

# 🏆 Optimal Number of Clusters

## K = 4

After comparing multiple clustering configurations, the analysis concludes that **four clusters** provide the most meaningful customer segmentation.

### Interpretation

Initial exploratory visualization using **five clusters (k = 5)** revealed noticeable overlap between several customer groups, making cluster boundaries difficult to interpret.

Reducing the number of clusters to **four** resulted in:

- Better separation between customer segments
- Reduced overlap
- More balanced cluster sizes
- Clearer customer profiles

Overall, **k = 4** provides a more interpretable and statistically robust segmentation.

---

# 📊 Clustering Performance Metrics

Several internal validation metrics were used to compare clustering quality.

---

## 1. Calinski–Harabasz Index

> **Higher values indicate better clustering performance.**

The Calinski–Harabasz Index evaluates how compact individual clusters are while simultaneously measuring the separation between different clusters.

### Results

| Algorithm | Score |
|-----------|------:|
| **K-Means** | **131.36** |
| Ward Linkage | 108.60 |
| Complete Linkage | 38.90 |
| Average Linkage | 8.38 |
| Single Linkage | 8.38 |

### Interpretation

K-Means achieved the **highest Calinski–Harabasz score**, indicating that it produced:

- Highly compact clusters
- Strong separation between customer groups
- Better overall cluster structure than all hierarchical methods

---

## 2. Silhouette Score

> **Values closer to 1 indicate better-defined clusters.**

The Silhouette Score measures how similar each observation is to its own cluster compared with neighboring clusters.

### Findings

Average Linkage and Single Linkage produced relatively high Silhouette scores (**0.498**).

### Interpretation

Although these methods showed good local cohesion, their very low Calinski–Harabasz scores suggest that the overall clustering structure remained weak, with poor separation and less balanced clusters.

---

## 3. Davies–Bouldin Index

> **Lower values indicate better clustering performance.**

The Davies–Bouldin Index evaluates the ratio between within-cluster similarity and between-cluster separation.

### Findings

Average Linkage and Single Linkage achieved relatively low scores (**0.355**).

### Interpretation

While these results indicate compact clusters, they do not fully capture overall clustering quality. Combined with their poor Calinski–Harabasz performance, the clusters appear less effective at representing meaningful customer segmentation.

---

# 📝 Key Findings

The comparative evaluation demonstrates that:

- **K-Means** consistently produced the strongest overall clustering performance.
- **Four clusters (k = 4)** provided clearer and more interpretable customer segments than five clusters.
- Hierarchical clustering methods showed strengths under individual evaluation metrics but failed to achieve balanced performance across all validation measures.
- The **Calinski–Harabasz Index** was the primary factor supporting the selection of K-Means, as it demonstrated the best balance between cluster compactness and separation.

---

# 📌 Conclusion

Based on both **visual inspection** and **statistical validation**, **K-Means with four clusters (k = 4)** is the most appropriate clustering solution for this dataset.

The selected model offers:

- Well-separated customer segments
- Minimal overlap between clusters
- High internal cohesion
- Strong overall structural quality

Although some hierarchical clustering methods performed well on individual metrics such as the Silhouette Score and Davies–Bouldin Index, they did not achieve consistently strong performance across all evaluation criteria.

Overall, **K-Means (k = 4)** provides the best balance between interpretability and clustering quality, making it the preferred approach for customer segmentation within this wholesale distribution dataset.
