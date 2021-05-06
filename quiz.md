This quiz is part of Algoritma Academy assessment process. Congratulations on completing the Unsupervised Learning course! We will conduct an assessment quiz to test practical unsupervised learning techniques you have learned on the course. The quiz is expected to be taken in the classroom, please contact our team of instructors if you missed the chance to take it in class.

# Data Exploration

In this quiz, we will be using **Arabica Coffee Beans Reviews**. You can get the csv file stored within the folder under `coffee.csv` file. Before we perform data clustering and principle component analysis, we will need to perform exploratory data analysis. You can glimpse the structure of our dataset! You can choose either `str()` or `glimpse()` function.

```
# your code here

```

This dataset consisted of 13 variables with 1082 rows and contain reviews of varieties of Arabica coffee by highly trained individuals from the Coffee Quality Institute. Take a look at the following glossary:

- `coffeId` : id of coffee
- `Aroma` : the smell of coffee after adding hot water (e.g.floral, spicy, fruity, winery, sweety, earthy or nutty etc.).
- `Flavor` : the taste characteristics (e.g. fruity, sour, bitter, rich or balanced etc.).
- `Aftertaste` : overall impression of the coffee remains in the mouth.
- `Acidity` : the sharpness and liveliness of the acidity (e.g. sharp, thin, flat, mild, or neutral etc.).
- `Body` : the tactile feeling of the coffee in the mouth (e.g. full, thick, balanced, buttery or thin etc.).
- `Balance` : no single flavor dominates the other.
- `Uniformity` : how similarly sized the ground coffee particles are.
- `Clean cup` : no flavor defects present.
- `Sweetness` : mild, smooth taste sensation with no harsh flavors.
- `Cupper points` : points earned from a Cupper (a person who objectively review over the taste and aroma of a brewed coffee, to know whether it’s a Specialty Grade Coffee).
- `Moisture` : any amount of liquid diffused in small quantities within a green coffee beans, if humidity is stable, the coffee beans will retain that moisture until roasting.
- `Quakers`: unripened coffee beans, often with a wrinkled surface, not darken well when roasted.

If you look carefully, `coffeeId` doesn't have a meaningful information because they represent the row index of the observation and every coffee has their own id number. Therefore, it would be wise to remove them before we go further.

```
# your code here

```


# 1. Principal Component Analysis (PCA)

## Data Pre-Processing

PCA is very useful to retain information while reducing the dimension of the data. However, we need to make sure that our data is properly scaled in order to get a useful PCA. You may use `scale()` function to scale the numeric variable and store it as `coffee_scale`.

```{r}
# your code here

```

## Build your Principal Component

We have prepared the scaled data to be used for PCA. Next, we will try to generate the principal component from the `coffee_scale`. Recall how we use `FactoMineR` library to perform PCA. Use `PCA()` function from the library to generate a PCA and store it as `pca_coffee`. Remember to set the `scale.unit` parameter to `FALSE` since we already scaled the data manually. Check the summary of the `pca_coffee`.

```{r}
# your code here

```
___
1. Based on the summary, how many  Principal Components (PCs) you will use if you only tolerate no more than 20% of information loss?
 - [ ] 4 PC (PC 1 through 4)
 - [ ] 5 PC (PC 1 through 5)
 - [ ] 6 PC (PC 1 through 6)
 - [ ] 1 PC (PC 1 only)
___

Another great implementation of PCA is to visualize high dimensional data into 2 dimensional plot for various purposes, such as cluster analysis or detecting any outliers. In order to visualize the PCA, use `plot.PCA()` function to the `pca_coffee`. This will generate an individual PCA plot.

```
# your code here

```

___
2. Judging from the plot that you have created, which coffee id is considered as outlier?
 - [ ] 1082, 1080, 1081
 - [ ] 1082, 993, 998
 - [ ] 1081, 308, 201
 - [ ] 1080, 1082, 998
___

We can also create a variable PCA plot that shows the variable loading information of the PCA by simply add `choix = "var"` in the `plot.PCA()`. The loading information will be represented by the length of the arrow from the center of coordinates. The longer the arrow, the bigger loading information of those variable. However, this may not an efficient method if we have a lot of features. Some variables would overlap with each other, making it harder to see the variable names.

An alternative way to extract the loading information is by using the `dimdesc()` function to the `pca_coffee`. Store the result as `pca_dimdesc`. Inspect the loading information from the first dimension/PC by calling `pca_dimdesc$Dim.1` since the first dimension is the one that hold the most information.

```
# your code here
```

___
3. Mention 3 most contributing variables on PC 1 based on the correlation between variables with the PC 1.
 - [ ] Aroma, Flavor, Aftertaste
 - [ ] Sweetness, Clean.cup, Uniformity
 - [ ] Balance, Flavor, Aftertaste
 - [ ] Moisture, Quakers, Clean.Cup
 - [ ] Acidity, Body, Balance
___

In the principal component analysis, each produced PC has an eigen value obtained from the covariance matrix. The greater the eigen value, the greater the variance captured by the PC.

___
4. Which of the following is **NOT TRUE** about PCA?
 - [ ] PCA requires data to be scaled so they have the same range of measurement
 - [ ] A Principal Component with an eigenvalue of 0.6 is not more helpful than a PC with an eigenvalue of 6.0
 - [ ] We cannot fully reconstruct the original data from a PCA even when we have eigen value and eigen vector
___

# 2. K-Means Clustering

Data clustering is a common data mining technique to create clusters of data that can be identified as "data with the same characteristics". Before performing data clustering, you will need to remove the identified *outlier* based the previous individual PCA plot. The observation with **coffeeId 1082** is a fairly extending outlier compared to the rest of the observation. Remove the observation from our initial dataset and once again scale the data.

```
# your code here

```

## 2.1 Choosing Optimum K

The next step in building a K-means clustering is to find the optimum cluster number to model our data. Use the defined `kmeansTunning()` function below to find the optimum K using Elbow method. Use a maximum of `maxK` as 5 to limit the plot into 5 distinct clusters.

```
RNGkind(sample.kind = "Rounding")
kmeansTunning <- function(data, maxK) {
  withinall <- NULL
  total_k <- NULL
  for (i in 2:maxK) {
    set.seed(101)
    temp <- kmeans(data,i)$tot.withinss
    withinall <- append(withinall, temp)
    total_k <- append(total_k,i)
  }
  plot(x = total_k, y = withinall, type = "o", xlab = "Number of Cluster", ylab = "Total within")
}

# kmeansTunning(your_data, maxK = 5)
```

Based on the elbow plot generated from the function above try to answer the following question:

___
5. What is the optimal number of clusters ?
  - [ ] 2
  - [ ] 3
  - [ ] 4
  - [ ] 5
___

K-means is a clustering algorithm that groups the data based on distance. The resulting clusters are stated to be optimum if the distance between data in the same cluster is low and the distance between data from different clusters is high.

___
6. Which of the following is **NOT TRUE** about K-Means?
  - [ ] The centroid in the first iteration is placed randomly
  - [ ] A good cluster are clusters with low `withinss` and high `betweenss`
  - [ ] Cluster with low `withinss` means the character of the data within 1 cluster are similar to each other
  - [ ] The greater the value of `betweenss`, indicates the greater the data variance in each cluster
___

## 2.2 Building Cluster

Once you find the optimum K from the previous section, try to do K-means clustering from your data and store it as `coffee_cluster`. Use `set.seed(101)` to guarantee a reproducible example. Extract the cluster information from the resulting K-means object using `cofee_cluster$cluster` and add them as a new column named `cluster` to the coffee dataset.

```
# your code here

set.seed(101)
```

## 2.3 Clusters Profiling

You can use the following chunk to answer question number 7.

```
# your code here


```

___
7. For a customer who enjoy coffee with `coffeId` 929, which of the following coffee beans may be characteristically similar enough to warrant a recommendation?
  - [ ] 1060
  - [ ] 208
  - [ ] 1011
___

Recall how we can perform a cluster profiling by using a combination of `group_by()` and `summarise_all()`, grouped by the previously created cluster column. After you extract the characteristics for each cluster, try to answer the following question:

```
# your code here

```

___
8. Can you describe the characteristic coffee that  in the same cluster as `coffeeId` 218!
  - [ ] Highest mean of Aroma, highest mean of Sweetness, and lowest mean of Acidity
  - [ ] Highest mean of Aroma, highest mean of Flavor, and lowest mean of Body
  - [ ] Highest mean of Aroma, highest mean of Flavor, and highest mean of Acidity
  - [ ] Highest mean of Aroma, lowest mean of Flavor, and lowest mean of Acidity
___
