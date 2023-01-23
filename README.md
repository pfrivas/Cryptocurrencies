# Cryptocurrencies

Full code for Cryptocurrencies, unsupervised machine learning challenge can be found at: [Crypto_Clustering](https://github.com/pfrivas/Cryptocurrencies/blob/main/Challenge/crypto_clustering.ipynb)

## Deliverable 1: Preprocessing the Data for PCA

### All cryptocurrencies that are not being traded are removed
- **Code:**
```
# Keep all the cryptocurrencies that have a working algorithm.
crypto_df = crypto_df[crypto_df.IsTrading != 0]
print(crypto_df.shape)
crypto_df.head(10)
```
- **Output:**

### The IsTrading column is dropped
- **Code:**
```
crypto_df = crypto_df.drop(["IsTrading"], axis=1)
print(crypto_df.shape)
crypto_df.head(10)
```
-**Output:**

### All the rows that have at least one null value are removed
- **Code:**
```

```
- **Output:**

### All the rows that do not have coins being mined are removed
- **Code:**
```

```
- **Output:**

### The CoinName column is dropped
- **Code:**
```

```
- **Output:**

### A new DataFrame is created that stores all cryptocurrency names from the CoinName column and retains the index from the crypto_df DataFrame
- **Code:**
```

```
- **Output:**

### The get_dummies() method is used to create variables for the text features, which are then stored in a new DataFrame, X
- **Code:**
```

```
- **Output:**

### The features from the X DataFrame have been standardized using the StandardScaler fit_transform() function
- **Code:**
```

```
- **Output:**



## Deliverable 2: Reducing Data Dimensions Using PCA

### The PCA algorithm reduces the dimensions of the X DataFrame down to three principal components
- **Code:**
```

```
- **Output:**

### The pcs_df DataFrame is created and has the following three columns, PC 1, PC 2, and PC 3, and has the index from the crypto_df DataFrame
- **Code:**
```

```
- **Output:**


## Deliverable 3: Clustering Cryptocurrencies Using K-means

### An elbow curve is created using hvPlot to find the best value for K
- **Code:**
```

```
- **Output:**

### Predictions are made on the K clusters of the cryptocurrencies’ data
- **Code:**
```

```
- **Output:**

### A new DataFrame is created with the same index as the crypto_df DataFrame and has the following columns: Algorithm, ProofType, TotalCoinsMined, TotalCoinSupply, PC 1, PC 2, PC 3, CoinName, and Class
- **Code:**
```

```
- **Output:**


## Deliverable 4: Visualizing Cryptocurrencies Results

### The clusters are plotted using a 3D scatter plot, and each data point shows the CoinName and Algorithm on hover
- **Code:**
```

```
- **Output:**


### A table with tradable cryptocurrencies is created using the hvplot.table() function
- **Code:**
```

```
- **Output:**


### The total number of tradable cryptocurrencies is printed
- **Code:**
```

```
- **Output:**


### A DataFrame is created that contains the clustered_df DataFrame index, the scaled data, and the CoinName and Class columns
- **Code:**
```

```
- **Output:**


### A hvplot scatter plot is created where the X-axis is "TotalCoinsMined", the Y-axis is "TotalCoinSupply", the data is ordered by "Class", and it shows the CoinName when you hover over the data point
- **Code:**
```

```
- **Output:**

