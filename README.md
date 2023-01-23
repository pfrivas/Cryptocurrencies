# Cryptocurrencies

Full code for Cryptocurrencies, unsupervised machine learning challenge can be found at: [Crypto_Clustering](https://github.com/pfrivas/Cryptocurrencies/blob/main/Challenge/crypto_clustering.ipynb)

## Deliverable 1: Preprocessing the Data for PCA

### All cryptocurrencies that are not being traded are removed
- **Code:**
```
crypto_df = crypto_df[crypto_df.IsTrading != 0]
```
- **Output:**

### The IsTrading column is dropped
- **Code:**
```
crypto_df = crypto_df.drop(["IsTrading"], axis=1)
```
-**Output:**

### All the rows that have at least one null value are removed
- **Code:**
```
crypto_df = crypto_df[~(crypto_df.isna() == True).any(axis=1)]
```
- **Output:**

### All the rows that do not have coins being mined are removed
- **Code:**
```
crypto_df = crypto_df[~(crypto_df.TotalCoinsMined <= 0)]
```
- **Output:**

### The CoinName column is dropped
- **Code:**
```
crypto_df = crypto_df.drop(["CoinName"], axis=1)
```
- **Output:**

### A new DataFrame is created that stores all cryptocurrency names from the CoinName column and retains the index from the crypto_df DataFrame
- **Code:**
```
crypto_df = crypto_df.drop(["CoinName"], axis=1)
print(crypto_df.shape)
crypto_df.head(10)
```
- **Output:**

### The get_dummies() method is used to create variables for the text features, which are then stored in a new DataFrame, X
- **Code:**
```
X = pd.get_dummies(crypto_df, columns=["Algorithm", "ProofType"])
```
- **Output:**

### The features from the X DataFrame have been standardized using the StandardScaler fit_transform() function
- **Code:**
```
crypto_scaled = StandardScaler().fit_transform(X)
```
- **Output:**



## Deliverable 2: Reducing Data Dimensions Using PCA

### The PCA algorithm reduces the dimensions of the X DataFrame down to three principal components
- **Code:**
```
pca = PCA(n_components=3)
crypto_pca = pca.fit_transform(crypto_scaled)
```
- **Output:**

### The pcs_df DataFrame is created and has the following three columns, PC 1, PC 2, and PC 3, and has the index from the crypto_df DataFrame
- **Code:**
```
pcs_df = pd.DataFrame(crypto_pca, columns=["PC 1", "PC 2", "PC 3"], index=crypto_name_df.index)
```
- **Output:**


## Deliverable 3: Clustering Cryptocurrencies Using K-means

### An elbow curve is created using hvPlot to find the best value for K
- **Code:**
```
elbow_data = {"k": k, "inertia": inertia}
elbow_df = pd.DataFrame(elbow_data)
elbow_df.hvplot.line(x="k", y="inertia", title="Elbow Curve", xticks=k)
```
- **Output:**

### Predictions are made on the K clusters of the cryptocurrencies’ data
- **Code:**
```
predictions = km_model.predict(pcs_df)
```
- **Output:**

### A new DataFrame is created with the same index as the crypto_df DataFrame and has the following columns: Algorithm, ProofType, TotalCoinsMined, TotalCoinSupply, PC 1, PC 2, PC 3, CoinName, and Class
- **Code:**
```
# Concatentate the crypto_df and pcs_df DataFrames on the same columns.
clustered_df = pd.concat([crypto_df, pcs_df], axis=1, join='inner')

#  Add a new column, "CoinName" to the clustered_df DataFrame that holds the names of the cryptocurrencies. 
clustered_df["CoinName"] = crypto_name_df.CoinName

#  Add a new column, "Class" to the clustered_df DataFrame that holds the predictions.
clustered_df["Class"] = km_model.labels_
```
- **Output:**


## Deliverable 4: Visualizing Cryptocurrencies Results

### The clusters are plotted using a 3D scatter plot, and each data point shows the CoinName and Algorithm on hover
- **Code:**
```
fig = px.scatter_3d(clustered_df, 
                    x="PC 1", 
                    y="PC 2", 
                    z="PC 3", 
                    color="Class", 
                    symbol="Class", 
                    width=800, 
                    hover_name="CoinName", 
                    hover_data=["Algorithm"])
fig.update_layout(legend=dict(x=0, y=1))
fig.show()
```
- **Output:**


### A table with tradable cryptocurrencies is created using the hvplot.table() function
- **Code:**
```
tradable_crypto= clustered_df.hvplot.table(columns=['CoinName', 
                                                   'Algorithm', 
                                                   'ProofType', 
                                                   'TotalCoinSupply', 
                                                   'TotalCoinsMined', 
                                                   'Class'], 
                                          sortable=True, 
                                          selectable=True)
tradable_crypto
```
- **Output:**


### The total number of tradable cryptocurrencies is printed
- **Code:**
```
print(f'There are {len(clustered_df)} tradable cryptocurrencies.')
```
- **Output:**


### A DataFrame is created that contains the clustered_df DataFrame index, the scaled data, and the CoinName and Class columns
- **Code:**
```
plot_df = pd.DataFrame(tc_cluster_scaled, 
                       columns=['TotalCoinSupply', 
                                'TotalCoinsMined'], 
                       index=clustered_df.index)

# Add the "CoinName" column from the clustered_df DataFrame to the new DataFrame.
plot_df['CoinName'] = clustered_df.CoinName

# Add the "Class" column from the clustered_df DataFrame to the new DataFrame. 
plot_df['Class'] = clustered_df.Class
```
- **Output:**


### A hvplot scatter plot is created where the X-axis is "TotalCoinsMined", the Y-axis is "TotalCoinSupply", the data is ordered by "Class", and it shows the CoinName when you hover over the data point
- **Code:**
```
plot_df.hvplot.scatter(
    x='TotalCoinsMined', 
    y='TotalCoinSupply', 
    hover_cols='CoinName', 
    by='Class')
```
- **Output:**

