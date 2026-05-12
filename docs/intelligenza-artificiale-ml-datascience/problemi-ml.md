# Problemi di ML: Classificazione, Predizione, Clustering

## 📖 Introduzione

Tipologie di problemi risolti con machine learning.

## 🎯 Classificazione

Assegnare etichette a istanze.

### Classificazione Binaria

```python
from sklearn.linear_model import LogisticRegression

# Spam detection
model = LogisticRegression()
model.fit(X_train, y_train)  # y in {0, 1}
predictions = model.predict(X_test)
```

### Classificazione Multiclasse

```python
from sklearn.ensemble import RandomForestClassifier

# Riconoscimento cifre (0-9)
model = RandomForestClassifier()
model.fit(X_train, y_train)  # y in {0,1,2,...,9}
```

## 📈 Regressione

Predire valori continui.

```python
from sklearn.linear_model import LinearRegression

# Predizione prezzi case
model = LinearRegression()
model.fit(X_train, y_train)  # y continuo
price_prediction = model.predict([[100, 3, 2]])  # mq, camere, bagni
```

## 🔍 Clustering

Raggruppare dati simili.

### K-Means

```python
from sklearn.cluster import KMeans

# Segmentazione clienti
kmeans = KMeans(n_clusters=3)
clusters = kmeans.fit_predict(X)
```

### DBSCAN

```python
from sklearn.cluster import DBSCAN

# Density-based clustering
dbscan = DBSCAN(eps=0.5, min_samples=5)
clusters = dbscan.fit_predict(X)
```

## 🚨 Anomaly Detection

Identificare outliers.

```python
from sklearn.ensemble import IsolationForest

detector = IsolationForest(contamination=0.1)
anomalies = detector.fit_predict(X)
```

## 📉 Dimensionality Reduction

### PCA

```python
from sklearn.decomposition import PCA

pca = PCA(n_components=2)
X_reduced = pca.fit_transform(X)
```

### t-SNE

```python
from sklearn.manifold import TSNE

tsne = TSNE(n_components=2)
X_embedded = tsne.fit_transform(X)
```

## 🔗 Collegamenti
- **Precedente:** [Agenti e Multiagente](agenti-multiagente.md)
- **Prossimo:** [Tipi di ML](tipi-ml.md)
