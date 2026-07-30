Wholesale Cust. Segmentation Using Hierarchical and K-Means Clustering
================
AhmedLubis
2026-07-30

## Preparation

``` r
#import_data
customer_data <- read.csv("wholesale_customers_data.csv")

head(customer_data)
```

    ##   Channel Region Fresh Milk Grocery Frozen Detergents_Paper Delicassen
    ## 1       2      3 12669 9656    7561    214             2674       1338
    ## 2       2      3  7057 9810    9568   1762             3293       1776
    ## 3       2      3  6353 8808    7684   2405             3516       7844
    ## 4       1      3 13265 1196    4221   6404              507       1788
    ## 5       2      3 22615 5410    7198   3915             1777       5185
    ## 6       2      3  9413 8259    5126    666             1795       1451

``` r
#data_structure
str(customer_data)
```

    ## 'data.frame':    440 obs. of  8 variables:
    ##  $ Channel         : int  2 2 2 1 2 2 2 2 1 2 ...
    ##  $ Region          : int  3 3 3 3 3 3 3 3 3 3 ...
    ##  $ Fresh           : int  12669 7057 6353 13265 22615 9413 12126 7579 5963 6006 ...
    ##  $ Milk            : int  9656 9810 8808 1196 5410 8259 3199 4956 3648 11093 ...
    ##  $ Grocery         : int  7561 9568 7684 4221 7198 5126 6975 9426 6192 18881 ...
    ##  $ Frozen          : int  214 1762 2405 6404 3915 666 480 1669 425 1159 ...
    ##  $ Detergents_Paper: int  2674 3293 3516 507 1777 1795 3140 3321 1716 7425 ...
    ##  $ Delicassen      : int  1338 1776 7844 1788 5185 1451 545 2566 750 2098 ...

``` r
summary(customer_data)
```

    ##     Channel          Region          Fresh             Milk      
    ##  Min.   :1.000   Min.   :1.000   Min.   :     3   Min.   :   55  
    ##  1st Qu.:1.000   1st Qu.:2.000   1st Qu.:  3128   1st Qu.: 1533  
    ##  Median :1.000   Median :3.000   Median :  8504   Median : 3627  
    ##  Mean   :1.323   Mean   :2.543   Mean   : 12000   Mean   : 5796  
    ##  3rd Qu.:2.000   3rd Qu.:3.000   3rd Qu.: 16934   3rd Qu.: 7190  
    ##  Max.   :2.000   Max.   :3.000   Max.   :112151   Max.   :73498  
    ##     Grocery          Frozen        Detergents_Paper    Delicassen     
    ##  Min.   :    3   Min.   :   25.0   Min.   :    3.0   Min.   :    3.0  
    ##  1st Qu.: 2153   1st Qu.:  742.2   1st Qu.:  256.8   1st Qu.:  408.2  
    ##  Median : 4756   Median : 1526.0   Median :  816.5   Median :  965.5  
    ##  Mean   : 7951   Mean   : 3071.9   Mean   : 2881.5   Mean   : 1524.9  
    ##  3rd Qu.:10656   3rd Qu.: 3554.2   3rd Qu.: 3922.0   3rd Qu.: 1820.2  
    ##  Max.   :92780   Max.   :60869.0   Max.   :40827.0   Max.   :47943.0

``` r
#data_preparation
data_cluster <- customer_data[, c(
  "Fresh",
  "Milk",
  "Grocery",
  "Frozen",
  "Detergents_Paper",
  "Delicassen"
)]

head(data_cluster)
```

    ##   Fresh Milk Grocery Frozen Detergents_Paper Delicassen
    ## 1 12669 9656    7561    214             2674       1338
    ## 2  7057 9810    9568   1762             3293       1776
    ## 3  6353 8808    7684   2405             3516       7844
    ## 4 13265 1196    4221   6404              507       1788
    ## 5 22615 5410    7198   3915             1777       5185
    ## 6  9413 8259    5126    666             1795       1451

## Exploration

``` r
#scatterplot_matrix
pairs(data_cluster)
```

![](Wholesale-Cust.-Segmentation-Using-Hierarchical-and-K-Means-Clustering_files/figure-gfm/unnamed-chunk-2-1.png)<!-- -->

``` r
#correlation
cor(data_cluster)
```

    ##                        Fresh      Milk     Grocery      Frozen Detergents_Paper
    ## Fresh             1.00000000 0.1005098 -0.01185387  0.34588146       -0.1019529
    ## Milk              0.10050977 1.0000000  0.72833512  0.12399376        0.6618157
    ## Grocery          -0.01185387 0.7283351  1.00000000 -0.04019274        0.9246407
    ## Frozen            0.34588146 0.1239938 -0.04019274  1.00000000       -0.1315249
    ## Detergents_Paper -0.10195294 0.6618157  0.92464069 -0.13152491        1.0000000
    ## Delicassen        0.24468997 0.4063683  0.20549651  0.39094747        0.0692913
    ##                  Delicassen
    ## Fresh             0.2446900
    ## Milk              0.4063683
    ## Grocery           0.2054965
    ## Frozen            0.3909475
    ## Detergents_Paper  0.0692913
    ## Delicassen        1.0000000

``` r
#histogram
hist(data_cluster$Fresh,
     main = "Distribusi Fresh",
     xlab = "Fresh")
```

![](Wholesale-Cust.-Segmentation-Using-Hierarchical-and-K-Means-Clustering_files/figure-gfm/unnamed-chunk-2-2.png)<!-- -->

``` r
hist(data_cluster$Milk,
     main = "Distribusi Milk",
     xlab = "Milk")
```

![](Wholesale-Cust.-Segmentation-Using-Hierarchical-and-K-Means-Clustering_files/figure-gfm/unnamed-chunk-2-3.png)<!-- -->

``` r
hist(data_cluster$Grocery,
     main = "Distribusi Grocery",
     xlab = "Grocery")
```

![](Wholesale-Cust.-Segmentation-Using-Hierarchical-and-K-Means-Clustering_files/figure-gfm/unnamed-chunk-2-4.png)<!-- -->

``` r
hist(data_cluster$Frozen,
     main = "Distribusi Frozen",
     xlab = "Frozen")
```

![](Wholesale-Cust.-Segmentation-Using-Hierarchical-and-K-Means-Clustering_files/figure-gfm/unnamed-chunk-2-5.png)<!-- -->

``` r
hist(data_cluster$Detergents_Paper,
     main = "Distribusi Detergents_Paper",
     xlab = "Detergents_Paper")
```

![](Wholesale-Cust.-Segmentation-Using-Hierarchical-and-K-Means-Clustering_files/figure-gfm/unnamed-chunk-2-6.png)<!-- -->

``` r
hist(data_cluster$Delicassen,
     main = "Distribusi Delicassen",
     xlab = "Delicassen")
```

![](Wholesale-Cust.-Segmentation-Using-Hierarchical-and-K-Means-Clustering_files/figure-gfm/unnamed-chunk-2-7.png)<!-- -->

It can be seen that every histogram is right-skewed, so i have to
transform into log-form.

``` r
#all_histogram_right-skewed
#copy_og_data
data_log <- data_cluster
#log_transformation-(log_base_e): column 1-6
data_log[, 1:6] <- log(data_cluster[, 1:6] + 1)
```

Exploration data_log

``` r
#scatterplot_matrix
pairs(data_log)
```

![](Wholesale-Cust.-Segmentation-Using-Hierarchical-and-K-Means-Clustering_files/figure-gfm/unnamed-chunk-4-1.png)<!-- -->

``` r
#correlation
cor(data_log)
```

    ##                        Fresh        Milk    Grocery      Frozen
    ## Fresh             1.00000000 -0.02109638 -0.1329889  0.38625790
    ## Milk             -0.02109638  1.00000000  0.7611276 -0.05522851
    ## Grocery          -0.13298890  0.76112760  1.0000000 -0.16452522
    ## Frozen            0.38625790 -0.05522851 -0.1645252  1.00000000
    ## Detergents_Paper -0.15870598  0.67872480  0.7971412 -0.21277086
    ## Delicassen        0.25644186  0.34231006  0.2399975  0.25631819
    ##                  Detergents_Paper Delicassen
    ## Fresh                  -0.1587060  0.2564419
    ## Milk                    0.6787248  0.3423101
    ## Grocery                 0.7971412  0.2399975
    ## Frozen                 -0.2127709  0.2563182
    ## Detergents_Paper        1.0000000  0.1675729
    ## Delicassen              0.1675729  1.0000000

``` r
#histogram_log_form
#fresh
hist(data_log$Fresh,
     main = "Distribusi Fresh",
     xlab = "Fresh")
```

![](Wholesale-Cust.-Segmentation-Using-Hierarchical-and-K-Means-Clustering_files/figure-gfm/unnamed-chunk-4-2.png)<!-- -->

``` r
#milk
hist(data_log$Milk,
     main = "Distribusi Milk",
     xlab = "Milk")
```

![](Wholesale-Cust.-Segmentation-Using-Hierarchical-and-K-Means-Clustering_files/figure-gfm/unnamed-chunk-4-3.png)<!-- -->

``` r
#grocery
hist(data_log$Grocery,
     main = "Distribusi Grocery",
     xlab = "Grocery")
```

![](Wholesale-Cust.-Segmentation-Using-Hierarchical-and-K-Means-Clustering_files/figure-gfm/unnamed-chunk-4-4.png)<!-- -->

``` r
#frozen
hist(data_log$Frozen,
     main = "Distribusi Frozen",
     xlab = "Frozen")
```

![](Wholesale-Cust.-Segmentation-Using-Hierarchical-and-K-Means-Clustering_files/figure-gfm/unnamed-chunk-4-5.png)<!-- -->

``` r
#detergents_paper
hist(data_log$Detergents_Paper,
     main = "Distribusi Detergents_Paper",
     xlab = "Detergents_Paper")
```

![](Wholesale-Cust.-Segmentation-Using-Hierarchical-and-K-Means-Clustering_files/figure-gfm/unnamed-chunk-4-6.png)<!-- -->

``` r
#delicassen
hist(data_log$Delicassen,
     main = "Distribusi Delicasse",
     xlab = "Delicassen")
```

![](Wholesale-Cust.-Segmentation-Using-Hierarchical-and-K-Means-Clustering_files/figure-gfm/unnamed-chunk-4-7.png)<!-- -->

After exploration data, i have to do standardization data.

``` r
#standardization_data
data_scaled <- scale(data_log)

head(data_scaled)
```

    ##           Fresh       Milk     Grocery     Frozen Detergents_Paper Delicassen
    ## [1,] 0.48563170  0.9751888  0.43965422 -1.5075338        0.6434109  0.4085010
    ## [2,] 0.08778870  0.9898294  0.65142933  0.1338998        0.7651721  0.6272121
    ## [3,] 0.01633769  0.8901377  0.45417004  0.3764707        0.8034903  1.7748131
    ## [4,] 0.51688887 -0.9568836 -0.08469525  1.1402764       -0.3283378  0.6324134
    ## [5,] 0.87962962  0.4391620  0.39539643  0.7564611        0.4044781  1.4549317
    ## [6,] 0.28364930  0.8305900  0.09003047 -0.6243428        0.4103703  0.4711141

Next, the step should be done by me is measuring of distance.

``` r
#distance_measurement
#euclidean_distance
euclidean_distance <- dist(
  data_scaled,
  method = "euclidean"
)

as.matrix(euclidean_distance)[1:5,1:5]
```

    ##          1        2        3        4        5
    ## 1 0.000000 1.720554 2.381090 3.466144 2.592739
    ## 2 1.720554 0.000000 1.196349 2.593122 1.482768
    ## 3 2.381090 1.196349 0.000000 2.668694 1.165347
    ## 4 3.466144 2.593122 2.668694 0.000000 1.916216
    ## 5 2.592739 1.482768 1.165347 1.916216 0.000000

``` r
#manhattan_distance
manhattan_distance <- dist(
  data_scaled,
  method = "manhattan"
)

as.matrix(manhattan_distance)[1:5,1:5]
```

    ##          1        2        3        4        5
    ## 1 0.000000 2.606165 3.979257 6.331150 4.523641
    ## 2 2.606165 0.000000 1.796892 5.217025 3.409516
    ## 3 3.979257 1.796892 0.000000 5.924471 2.471925
    ## 4 6.331150 5.217025 5.924471 0.000000 4.178028
    ## 5 4.523641 3.409516 2.471925 4.178028 0.000000

``` r
#chebyshev_distance
chebyshev_distance <- dist(
  data_scaled,
  method = "maximum"
)

as.matrix(chebyshev_distance)[1:5,1:5]
```

    ##          1         2         3        4         5
    ## 1 0.000000 1.6414336 1.8840045 2.647810 2.2639949
    ## 2 1.641434 0.0000000 1.1476010 1.946713 0.8277196
    ## 3 1.884004 1.1476010 0.0000000 1.847021 0.8632919
    ## 4 2.647810 1.9467130 1.8470213 0.000000 1.3960457
    ## 5 2.263995 0.8277196 0.8632919 1.396046 0.0000000

``` r
#based_on_max_spare
minkowski_distance <- dist(
  data_scaled,
  method = "minkowski",
  p = 3
)

as.matrix(minkowski_distance)[1:5,1:5]
```

    ##          1        2         3        4         5
    ## 1 0.000000 1.651851 2.1064009 2.994390 2.3500836
    ## 2 1.651851 0.000000 1.1534834 2.167439 1.1536910
    ## 3 2.106401 1.153483 0.0000000 2.152236 0.9606147
    ## 4 2.994390 2.167439 2.1522358 0.000000 1.5721402
    ## 5 2.350084 1.153691 0.9606147 1.572140 0.0000000

After measure of distance, I will do cluster analysis.

``` r
#hierarchial_clustering
#complete_linkage
hc_complete <- hclust(
  euclidean_distance,
  method = "complete"
)

plot(
  hc_complete,
  hang = -1,
  main = "Dendrogram Complete Linkage"
)
```

![](Wholesale-Cust.-Segmentation-Using-Hierarchical-and-K-Means-Clustering_files/figure-gfm/unnamed-chunk-8-1.png)<!-- -->

``` r
#creating_cluster
cluster_hc <- cutree(
  hc_complete,
  k = 5
)

cluster_hc
```

    ##   [1] 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1
    ##  [38] 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 2 2 1 1 1 1 1 1 1
    ##  [75] 1 3 1 1 1 1 1 1 1 1 1 1 1 1 4 1 4 1 1 1 1 2 2 4 4 1 1 1 1 1 1 1 1 1 1 5 1
    ## [112] 1 1 1 1 1 1 1 1 1 1 1 3 1 1 1 1 1 5 1 1 4 1 1 1 1 1 5 1 1 1 1 5 1 1 1 1 1
    ## [149] 1 1 1 1 1 1 4 1 1 1 1 1 1 3 1 1 1 1 1 1 1 1 1 2 1 1 1 1 1 1 1 1 1 1 1 1 5
    ## [186] 1 1 5 1 1 1 4 1 2 1 1 1 1 1 1 1 1 1 5 1 1 1 1 1 1 1 1 1 1 1 1 1 1 2 4 1 1
    ## [223] 1 1 1 1 1 1 1 1 1 1 1 5 1 1 1 1 4 1 1 1 1 1 1 1 1 3 1 1 1 1 1 1 1 4 1 1 1
    ## [260] 1 1 4 1 1 1 1 1 1 1 4 4 1 1 1 1 4 1 4 1 1 1 1 1 1 1 4 1 1 1 4 1 1 1 1 1 1
    ## [297] 1 1 1 4 1 1 1 1 2 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 4 1 1 1 1 1
    ## [334] 1 1 1 1 1 4 1 1 1 1 1 1 1 1 1 1 1 1 1 4 2 1 4 3 2 1 1 1 1 1 1 1 1 1 1 1 4
    ## [371] 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 4 1 1 1 1 1 1 1 1
    ## [408] 1 1 1 1 1 2 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 5

``` r
#adding_cluster_to_data
customer_data$cluster_hc <- cluster_hc

head(customer_data)
```

    ##   Channel Region Fresh Milk Grocery Frozen Detergents_Paper Delicassen
    ## 1       2      3 12669 9656    7561    214             2674       1338
    ## 2       2      3  7057 9810    9568   1762             3293       1776
    ## 3       2      3  6353 8808    7684   2405             3516       7844
    ## 4       1      3 13265 1196    4221   6404              507       1788
    ## 5       2      3 22615 5410    7198   3915             1777       5185
    ## 6       2      3  9413 8259    5126    666             1795       1451
    ##   cluster_hc
    ## 1          1
    ## 2          1
    ## 3          1
    ## 4          1
    ## 5          1
    ## 6          1

``` r
#cluster_visualization
library(factoextra)
```

    ## Loading required package: ggplot2

    ## Warning: package 'ggplot2' was built under R version 4.5.3

    ## Welcome! Want to learn more? See two factoextra-related books at https://goo.gl/ve3WBa

``` r
fviz_cluster(
  list(
    data = data_scaled,
    cluster = cluster_hc
  )
)
```

![](Wholesale-Cust.-Segmentation-Using-Hierarchical-and-K-Means-Clustering_files/figure-gfm/unnamed-chunk-11-1.png)<!-- -->

``` r
library(dplyr)
```

    ## Warning: package 'dplyr' was built under R version 4.5.3

    ## 
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

``` r
customer_data%>%
  group_by(cluster_hc) %>%
  summarise(
    Mean_fresh = mean(Fresh),
    Mean_milk = mean(Milk),
    Mean_grocery = mean(Grocery),
    Mean_frozen = mean(Frozen),
    Mean_detergents_paper = mean(Detergents_Paper),
    Mean_delicassen = mean(Delicassen)
  )
```

    ## # A tibble: 5 × 7
    ##   cluster_hc Mean_fresh Mean_milk Mean_grocery Mean_frozen Mean_detergents_paper
    ##        <int>      <dbl>     <dbl>        <dbl>       <dbl>                 <dbl>
    ## 1          1    12607.      6119.        8148.       3172.                3005. 
    ## 2          2       84.5     7589.       18507.        434.                7379. 
    ## 3          3    16260.       472.         248.       2545.                  16.8
    ## 4          4     9153.       619.        1505.       3400                  137. 
    ## 5          5     5419.      6344.        7991.       1365                  919. 
    ## # ℹ 1 more variable: Mean_delicassen <dbl>

``` r
#determine_optimal_number_of_cluster
#elbow_method
fviz_nbclust(
  data_scaled,
  kmeans,
  method = "wss"
)
```

![](Wholesale-Cust.-Segmentation-Using-Hierarchical-and-K-Means-Clustering_files/figure-gfm/unnamed-chunk-13-1.png)<!-- -->

``` r
#silhoutte_method
fviz_nbclust(
  data_scaled,
  kmeans,
  method = "silhouette"
)
```

![](Wholesale-Cust.-Segmentation-Using-Hierarchical-and-K-Means-Clustering_files/figure-gfm/unnamed-chunk-13-2.png)<!-- -->

Next step is k-means clustering. The “k” in k-means represents the
number of clusters you want to find. The “means” refers to the average
position of all the data points within a cluster (the cluster center, or
centroid).

``` r
#k-means_clsutering
set.seed(123)

km <- kmeans(
  data_scaled,
  centers = 4,
  nstart = 25
)

km
```

    ## K-means clustering with 4 clusters of sizes 130, 61, 119, 130
    ## 
    ## Cluster means:
    ##         Fresh        Milk    Grocery      Frozen Detergents_Paper Delicassen
    ## 1  0.54020468 -0.07960844 -0.2433786  0.74628688       -0.3597453  0.4546260
    ## 2 -1.30955829  0.52172112  0.7026482 -1.24479279        0.7882335 -0.8960177
    ## 3  0.12083561  0.91292274  0.9460461 -0.07623072        0.9826730  0.5562885
    ## 4 -0.03633069 -1.00087459 -0.9523217 -0.09241137       -0.9096418 -0.5434049
    ## 
    ## Clustering vector:
    ##   [1] 3 3 3 1 1 3 3 3 3 3 3 4 3 3 3 4 2 1 3 3 3 4 1 3 3 2 4 4 3 1 3 1 4 1 4 2 1
    ##  [38] 3 2 1 1 1 2 2 2 3 3 3 3 3 1 2 3 2 1 3 3 2 4 3 2 3 3 3 1 2 2 3 1 4 1 3 1 1
    ##  [75] 3 4 1 3 4 1 4 2 3 1 2 3 3 1 4 1 4 1 3 1 2 2 2 4 4 4 3 3 3 1 1 4 2 3 2 2 1
    ## [112] 3 1 1 4 4 4 1 1 1 1 4 4 3 1 1 1 3 2 1 1 4 4 4 4 4 3 2 1 1 1 1 4 4 4 3 4 1
    ## [149] 4 4 4 4 4 1 4 3 3 1 3 2 3 4 1 3 3 3 3 2 4 4 2 3 2 2 4 2 1 1 1 1 3 3 2 1 2
    ## [186] 4 4 2 3 2 4 4 4 2 4 1 1 3 1 4 3 3 1 4 4 3 4 2 3 3 1 3 4 3 3 3 2 1 2 4 4 2
    ## [223] 4 1 4 1 3 4 4 1 1 3 4 2 1 2 4 1 4 1 1 1 4 3 3 3 4 4 1 1 4 3 1 3 3 4 4 1 1
    ## [260] 1 4 4 4 1 2 3 3 1 3 4 4 4 2 1 4 4 1 4 1 3 4 3 1 1 1 4 4 1 4 4 4 1 1 3 1 2
    ## [297] 1 3 3 4 3 3 2 2 2 2 3 1 4 3 1 1 2 4 1 3 4 3 4 3 4 1 4 1 1 1 4 4 1 1 1 3 1
    ## [334] 3 1 3 4 1 4 1 2 3 2 2 4 2 3 3 4 3 4 3 4 2 1 4 4 2 1 2 1 4 4 4 4 3 4 4 4 4
    ## [371] 1 1 1 3 4 4 3 1 4 2 4 1 3 4 3 4 1 1 4 4 4 4 4 1 4 4 3 4 4 4 4 1 4 2 1 4 1
    ## [408] 3 3 1 1 1 2 1 1 3 3 3 2 1 2 3 1 1 3 1 3 1 1 4 3 1 4 4 3 1 1 3 1 2
    ## 
    ## Within cluster sum of squares by cluster:
    ## [1] 310.9328 307.7432 274.5418 490.2857
    ##  (between_SS / total_SS =  47.5 %)
    ## 
    ## Available components:
    ## 
    ## [1] "cluster"      "centers"      "totss"        "withinss"     "tot.withinss"
    ## [6] "betweenss"    "size"         "iter"         "ifault"

``` r
fviz_cluster(
  km,
  data = data_scaled
)
```

![](Wholesale-Cust.-Segmentation-Using-Hierarchical-and-K-Means-Clustering_files/figure-gfm/unnamed-chunk-15-1.png)<!-- -->

``` r
#adding_the_result_of_cluster
customer_data$Cluster_KMeans <- km$cluster

head(customer_data)
```

    ##   Channel Region Fresh Milk Grocery Frozen Detergents_Paper Delicassen
    ## 1       2      3 12669 9656    7561    214             2674       1338
    ## 2       2      3  7057 9810    9568   1762             3293       1776
    ## 3       2      3  6353 8808    7684   2405             3516       7844
    ## 4       1      3 13265 1196    4221   6404              507       1788
    ## 5       2      3 22615 5410    7198   3915             1777       5185
    ## 6       2      3  9413 8259    5126    666             1795       1451
    ##   cluster_hc Cluster_KMeans
    ## 1          1              3
    ## 2          1              3
    ## 3          1              3
    ## 4          1              1
    ## 5          1              1
    ## 6          1              3

``` r
#cluster_evaluation
#calinski-harabasz_index
library(clusterSim)
```

    ## Warning: package 'clusterSim' was built under R version 4.5.3

    ## Loading required package: cluster

    ## Loading required package: MASS

    ## 
    ## Attaching package: 'MASS'

    ## The following object is masked from 'package:dplyr':
    ## 
    ##     select

``` r
index.G1(
  data_scaled,
  km$cluster
)
```

    ## [1] 131.3613

``` r
#davies-bouldin_index
index.DB(
  data_scaled,
  km$cluster
)$DB
```

    ## [1] 1.692217

``` r
#comparison_clustering_model
# Complete linkage
hc_complete <- hclust(
  euclidean_distance,
  method = "complete"
)

# Average linkage
hc_average <- hclust(
  euclidean_distance,
  method = "average"
)

# Single linkage
hc_single <- hclust(
  euclidean_distance,
  method = "single"
)

# Ward linkage
hc_ward <- hclust(
  euclidean_distance,
  method = "ward.D2"
)
```

``` r
#comparison_4clusters
cluster_complete <- cutree(hc_complete, k = 4)

cluster_average <- cutree(hc_average, k = 4)

cluster_single <- cutree(hc_single, k = 4)

cluster_ward <- cutree(hc_ward, k = 4)
```

Final step, I have to do some evaluations.

``` r
#evaluated_by_silhouette_index
library(cluster)

sil_complete <- silhouette(
  cluster_complete,
  euclidean_distance
)

sil_average <- silhouette(
  cluster_average,
  euclidean_distance
)

sil_single <- silhouette(
  cluster_single,
  euclidean_distance
)

sil_ward <- silhouette(
  cluster_ward,
  euclidean_distance
)

mean_complete <- mean(sil_complete[,3])

mean_average <- mean(sil_average[,3])

mean_single <- mean(sil_single[,3])

mean_ward <- mean(sil_ward[,3])

mean_complete
```

    ## [1] 0.2595294

``` r
mean_average
```

    ## [1] 0.4980345

``` r
mean_single
```

    ## [1] 0.4980345

``` r
mean_ward
```

    ## [1] 0.2020796

``` r
#evaluated_by_davies-bouldin_index
library(clusterSim)

db_complete <- index.DB(
  data_scaled,
  cluster_complete
)$DB

db_average <- index.DB(
  data_scaled,
  cluster_average
)$DB

db_single <- index.DB(
  data_scaled,
  cluster_single
)$DB

db_ward <- index.DB(
  data_scaled,
  cluster_ward
)$DB

db_complete
```

    ## [1] 1.381521

``` r
db_average
```

    ## [1] 0.3558376

``` r
db_single
```

    ## [1] 0.3558376

``` r
db_ward
```

    ## [1] 1.650321

``` r
#evaluated_by_calinski-harabasz_index
ch_complete <- index.G1(
  data_scaled,
  cluster_complete
)

ch_average <- index.G1(
  data_scaled,
  cluster_average
)

ch_single <- index.G1(
  data_scaled,
  cluster_single
)

ch_ward <- index.G1(
  data_scaled,
  cluster_ward
)

ch_complete
```

    ## [1] 38.94448

``` r
ch_average
```

    ## [1] 8.382897

``` r
ch_single
```

    ## [1] 8.382897

``` r
ch_ward
```

    ## [1] 108.6067

``` r
#resume_comparison_model
compared_result <- data.frame(
  Metode = c(
    "Complete",
    "Average",
    "Single",
    "Ward"
  ),

  Silhouette = c(
    mean_complete,
    mean_average,
    mean_single,
    mean_ward
  ),

  Davies_Bouldin = c(
    db_complete,
    db_average,
    db_single,
    db_ward
  ),

  Calinski_Harabasz = c(
    ch_complete,
    ch_average,
    ch_single,
    ch_ward
  )
)

compared_result
```

    ##     Metode Silhouette Davies_Bouldin Calinski_Harabasz
    ## 1 Complete  0.2595294      1.3815210         38.944481
    ## 2  Average  0.4980345      0.3558376          8.382897
    ## 3   Single  0.4980345      0.3558376          8.382897
    ## 4     Ward  0.2020796      1.6503208        108.606730

### **Determining the Optimal Number of Clusters**

Based on the evaluation results, the optimal clustering model is
**K-Means with $k = 4$**.

#### **Key Supporting Factors**

1.  **Elbow & Silhouette Methods ($k = 4$ vs $k = 5$)**
    - Visual evaluation using `fviz_nbclust` showed cluster overlap at
      $k = 5$.
    - Setting **$k = 4$** eliminated overlapping, yielding
      better-separated groups.
2.  **Superior Cluster Separation (Calinski-Harabasz Index)**
    - K-Means ($k = 4$) achieved a **Calinski-Harabasz score of
      131.36**.
    - Outperformed all tested hierarchical methods:
      - **K-Means ($k=4$):** `131.36` *(Highest)*
      - **Ward’s:** `108.60`
      - **Complete:** `38.90`
      - **Average:** `8.30`
3.  **Davies-Bouldin Index & Model Stability**
    - K-Means ($k = 4$) achieved a **Davies-Bouldin Index of 1.69**.
    - Although Average Linkage scored lower (0.35), it suffered from
      severe cluster imbalance/chaining effect.
4.  **Balanced Cluster Distribution**
    - K-Means ($k = 4$) produced well-balanced cluster sizes: **130, 61,
      119, and 130**.
    - Avoided single-cluster dominance seen in Average/Single linkage,
      making the segments practical for business interpretation.

------------------------------------------------------------------------

> **Conclusion:** **K-Means ($k = 4$)** is selected as the optimal model
> because it provides the highest internal validity score
> (Calinski-Harabasz) and the most stable, balanced cluster structure.
