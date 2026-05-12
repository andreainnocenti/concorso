# Data Mining, Visualization e Dati Sintetici

## 📖 Introduzione

Estrazione di conoscenza, visualizzazione efficace e generazione di dati sintetici.

## ⛏️ Data Mining

### Association Rules (Apriori)

```python
from mlxtend.frequent_patterns import apriori, association_rules
import pandas as pd

# Dataset transazionale
df = pd.DataFrame({
    'pane': [1, 1, 0, 1],
    'latte': [1, 1, 1, 0],
    'burro': [0, 1, 1, 0]
})

# Trova itemset frequenti
frequent_itemsets = apriori(df, min_support=0.5, use_colnames=True)

# Genera regole di associazione
rules = association_rules(frequent_itemsets, metric="confidence", min_threshold=0.7)
print(rules[['antecedents', 'consequents', 'support', 'confidence', 'lift']])
```

**Metriche:**
- **Support**: frequenza dell'itemset
- **Confidence**: probabilità condizionata
- **Lift**: quanto è più probabile rispetto al caso

## 📊 Data Visualization

### Matplotlib

```python
import matplotlib.pyplot as plt
import numpy as np

# Line plot
x = np.linspace(0, 10, 100)
y = np.sin(x)

plt.figure(figsize=(10, 6))
plt.plot(x, y, label='sin(x)')
plt.xlabel('X')
plt.ylabel('Y')
plt.title('Sine Wave')
plt.legend()
plt.grid(True)
plt.show()
```

### Seaborn

```python
import seaborn as sns

# Heatmap di correlazione
correlation = df.corr()
plt.figure(figsize=(10, 8))
sns.heatmap(correlation, annot=True, cmap='coolwarm', center=0)
plt.title('Correlation Matrix')
plt.show()

# Distribution plot
sns.histplot(data=df, x='age', kde=True)
plt.show()

# Scatter plot con categorie
sns.scatterplot(data=df, x='feature1', y='feature2', hue='category')
plt.show()
```

### Plotly (Interattivo)

```python
import plotly.express as px

# Scatter plot interattivo
fig = px.scatter(df, x='x', y='y', color='category', 
                 size='size', hover_data=['name'])
fig.show()

# 3D scatter
fig = px.scatter_3d(df, x='x', y='y', z='z', color='category')
fig.show()
```

## 📈 Dashboard

### Streamlit

```python
import streamlit as st
import pandas as pd

st.title('Dashboard Interattiva')

# Sidebar
option = st.sidebar.selectbox('Seleziona visualizzazione', 
                               ['Grafico 1', 'Grafico 2'])

# Upload file
uploaded_file = st.file_uploader("Carica CSV", type=['csv'])

if uploaded_file:
    df = pd.read_csv(uploaded_file)
    st.dataframe(df)
    
    # Grafico
    st.line_chart(df['column'])
    
    # Metriche
    col1, col2, col3 = st.columns(3)
    col1.metric("Media", df['column'].mean())
    col2.metric("Mediana", df['column'].median())
    col3.metric("Std Dev", df['column'].std())
```

## 🎨 Best Practices Visualizzazione

!!! tip "Principi Fondamentali"
    1. **Chiarezza**: messaggio immediato
    2. **Accuratezza**: no distorsioni
    3. **Efficienza**: ink-to-data ratio
    4. **Estetica**: design pulito
    5. **Accessibilità**: colori per daltonici

### Scelta del Grafico

| Obiettivo | Grafico Consigliato |
|-----------|-------------------|
| Confronto | Bar chart |
| Trend temporale | Line chart |
| Distribuzione | Histogram, Box plot |
| Correlazione | Scatter plot |
| Composizione | Pie chart, Stacked bar |
| Relazioni | Network graph |

## 🔬 Dati Sintetici

### Generazione Semplice

```python
import numpy as np
from sklearn.datasets import make_classification, make_regression

# Classificazione
X, y = make_classification(
    n_samples=1000,
    n_features=20,
    n_informative=15,
    n_redundant=5,
    random_state=42
)

# Regressione
X, y = make_regression(
    n_samples=1000,
    n_features=10,
    noise=0.1,
    random_state=42
)
```

### GANs per Dati Sintetici

```python
import torch
import torch.nn as nn

class Generator(nn.Module):
    def __init__(self, latent_dim, output_dim):
        super().__init__()
        self.model = nn.Sequential(
            nn.Linear(latent_dim, 128),
            nn.ReLU(),
            nn.Linear(128, 256),
            nn.ReLU(),
            nn.Linear(256, output_dim),
            nn.Tanh()
        )
    
    def forward(self, z):
        return self.model(z)

class Discriminator(nn.Module):
    def __init__(self, input_dim):
        super().__init__()
        self.model = nn.Sequential(
            nn.Linear(input_dim, 256),
            nn.LeakyReLU(0.2),
            nn.Linear(256, 128),
            nn.LeakyReLU(0.2),
            nn.Linear(128, 1),
            nn.Sigmoid()
        )
    
    def forward(self, x):
        return self.model(x)

# Training loop
generator = Generator(latent_dim=100, output_dim=784)
discriminator = Discriminator(input_dim=784)

for epoch in range(epochs):
    # Train discriminator
    real_data = next(data_loader)
    fake_data = generator(torch.randn(batch_size, 100))
    
    d_loss_real = criterion(discriminator(real_data), torch.ones(batch_size, 1))
    d_loss_fake = criterion(discriminator(fake_data.detach()), torch.zeros(batch_size, 1))
    d_loss = d_loss_real + d_loss_fake
    
    # Train generator
    g_loss = criterion(discriminator(fake_data), torch.ones(batch_size, 1))
```

### CTGAN (Conditional Tabular GAN)

```python
from ctgan import CTGAN

# Dati tabulari reali
real_data = pd.read_csv('data.csv')

# Addestra CTGAN
ctgan = CTGAN(epochs=300)
ctgan.fit(real_data, discrete_columns=['category_col'])

# Genera dati sintetici
synthetic_data = ctgan.sample(1000)
```

## 🔒 Privacy-Preserving Synthesis

### Differential Privacy

```python
from diffprivlib.models import GaussianNB

# Classificatore con differential privacy
clf = GaussianNB(epsilon=1.0)
clf.fit(X_train, y_train)
predictions = clf.predict(X_test)
```

### Synthetic Data Vault (SDV)

```python
from sdv.tabular import GaussianCopula

# Modella distribuzione
model = GaussianCopula()
model.fit(real_data)

# Genera dati sintetici
synthetic_data = model.sample(num_rows=1000)

# Valuta qualità
from sdv.metrics.tabular import KSTest
score = KSTest.compute(real_data, synthetic_data)
```

## 📊 Valutazione Dati Sintetici

### Metriche di Qualità

```python
from sklearn.metrics import mean_squared_error
from scipy.stats import ks_2samp

# Distribuzione marginale
for col in real_data.columns:
    statistic, pvalue = ks_2samp(real_data[col], synthetic_data[col])
    print(f"{col}: KS statistic = {statistic}, p-value = {pvalue}")

# Correlazioni
real_corr = real_data.corr()
synth_corr = synthetic_data.corr()
corr_diff = mean_squared_error(real_corr.values.flatten(), 
                                synth_corr.values.flatten())
print(f"Correlation MSE: {corr_diff}")
```

### Utility Testing

```python
from sklearn.ensemble import RandomForestClassifier
from sklearn.metrics import accuracy_score

# Addestra su dati reali
model_real = RandomForestClassifier()
model_real.fit(X_real_train, y_real_train)
acc_real = accuracy_score(y_test, model_real.predict(X_test))

# Addestra su dati sintetici
model_synth = RandomForestClassifier()
model_synth.fit(X_synth_train, y_synth_train)
acc_synth = accuracy_score(y_test, model_synth.predict(X_test))

print(f"Real data accuracy: {acc_real}")
print(f"Synthetic data accuracy: {acc_synth}")
print(f"Utility preservation: {acc_synth/acc_real * 100}%")
```

## 📚 Risorse

!!! tip "Tool e Librerie"
    - **Visualization**: Matplotlib, Seaborn, Plotly, Altair
    - **Dashboard**: Streamlit, Dash, Tableau
    - **Synthetic Data**: CTGAN, SDV, DataSynthesizer
    - **Privacy**: diffprivlib, PySyft

!!! info "Best Practices"
    - Esplora i dati prima di visualizzare
    - Scegli il grafico appropriato
    - Mantieni design semplice e pulito
    - Valuta sempre la qualità dei dati sintetici
    - Considera privacy e bias

## 🔗 Collegamenti

- **Precedente:** [Big Data](big-data.md)
- **Sezione:** [Intelligenza Artificiale, ML, Data Science](index.md)
- **Correlato:** [Privacy-Enhancing Technologies](../crittografia-dlt-privacy/privacy-enhancing.md)