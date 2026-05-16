# Tipi di Machine Learning

## 📖 Introduzione

Il **Machine Learning (ML)** è un sottoinsieme dell'intelligenza artificiale che permette ai computer di imparare dai dati senza essere esplicitamente programmati. Si divide in diverse categorie in base al tipo di apprendimento.

---

## 📚 Supervised Learning (Apprendimento Supervisionato)

### Concetto

Apprendimento da **dati etichettati** (labeled data): ogni esempio ha input (features) e output desiderato (label).

**Obiettivo:** Imparare funzione `f: X → Y` che mappa input a output.

```
Training Data: (x₁, y₁), (x₂, y₂), ..., (xₙ, yₙ)
Modello impara: f(x) ≈ y
```

### Tipi di Problemi

**1. Classificazione** (output discreto)
- **Binaria**: 2 classi (spam/non-spam, malato/sano)
- **Multi-classe**: >2 classi (riconoscimento cifre 0-9)
- **Multi-label**: Più etichette per esempio (tag articolo)

**2. Regressione** (output continuo)
- Predire prezzo casa
- Previsione temperatura
- Stima vendite

### Algoritmi Principali

**Regressione Lineare:**
```python
from sklearn.linear_model import LinearRegression

# Training
model = LinearRegression()
model.fit(X_train, y_train)

# Prediction
y_pred = model.predict(X_test)
```

**Formula:** `y = w₁x₁ + w₂x₂ + ... + wₙxₙ + b`

**Logistic Regression (Classificazione):**
```python
from sklearn.linear_model import LogisticRegression

model = LogisticRegression()
model.fit(X_train, y_train)
accuracy = model.score(X_test, y_test)
```

**Decision Trees:**
```python
from sklearn.tree import DecisionTreeClassifier

model = DecisionTreeClassifier(max_depth=5)
model.fit(X_train, y_train)
```

**Random Forest (Ensemble):**
```python
from sklearn.ensemble import RandomForestClassifier

model = RandomForestClassifier(n_estimators=100)
model.fit(X_train, y_train)
```

**Support Vector Machines (SVM):**
```python
from sklearn.svm import SVC

model = SVC(kernel='rbf')
model.fit(X_train, y_train)
```

**Neural Networks:**
```python
from sklearn.neural_network import MLPClassifier

model = MLPClassifier(hidden_layer_sizes=(100, 50))
model.fit(X_train, y_train)
```

### Metriche di Valutazione

**Classificazione:**
- **Accuracy**: (TP + TN) / Total
- **Precision**: TP / (TP + FP) - "Dei positivi predetti, quanti sono veri?"
- **Recall**: TP / (TP + FN) - "Dei positivi reali, quanti ho trovato?"
- **F1-Score**: 2 × (Precision × Recall) / (Precision + Recall)

**Confusion Matrix:**
```
                Predicted
              Pos    Neg
Actual Pos    TP     FN
       Neg    FP     TN
```

**Regressione:**
- **MSE** (Mean Squared Error): Σ(y - ŷ)² / n
- **RMSE** (Root MSE): √MSE
- **MAE** (Mean Absolute Error): Σ|y - ŷ| / n
- **R²** (R-squared): Quanto varianza è spiegata dal modello

### Overfitting e Underfitting

**Underfitting:**
- Modello troppo semplice
- Non cattura pattern nei dati
- Alto errore su training E test

**Overfitting:**
- Modello troppo complesso
- Memorizza training data (anche rumore)
- Basso errore su training, alto su test

**Soluzioni:**
- **Regularization**: L1 (Lasso), L2 (Ridge)
- **Cross-validation**: K-fold per valutare generalizzazione
- **Early stopping**: Ferma training quando validation error aumenta
- **Dropout**: (Neural networks) Disattiva random neurons
- **Data augmentation**: Aumenta training data

---

## 🔍 Unsupervised Learning (Apprendimento Non Supervisionato)

### Concetto

Apprendimento da **dati non etichettati**: solo input, nessun output desiderato.

**Obiettivo:** Scoprire struttura nascosta nei dati.

### 1. Clustering

Raggruppare dati simili.

**K-Means:**
```python
from sklearn.cluster import KMeans

# Trova 3 cluster
kmeans = KMeans(n_clusters=3)
labels = kmeans.fit_predict(X)
centers = kmeans.cluster_centers_
```

**Algoritmo:**
1. Inizializza K centroidi random
2. Assegna ogni punto al centroide più vicino
3. Ricalcola centroidi come media dei punti
4. Ripeti 2-3 fino a convergenza

**DBSCAN (Density-Based):**
```python
from sklearn.cluster import DBSCAN

dbscan = DBSCAN(eps=0.5, min_samples=5)
labels = dbscan.fit_predict(X)
```

**Vantaggi:**
- Trova cluster di forma arbitraria
- Identifica outlier automaticamente

**Hierarchical Clustering:**
```python
from sklearn.cluster import AgglomerativeClustering

model = AgglomerativeClustering(n_clusters=3)
labels = model.fit_predict(X)
```

**Applicazioni:**
- Segmentazione clienti
- Compressione immagini
- Anomaly detection

### 2. Dimensionality Reduction

Ridurre numero di features mantenendo informazione.

**PCA (Principal Component Analysis):**
```python
from sklearn.decomposition import PCA

# Riduci a 2 dimensioni
pca = PCA(n_components=2)
X_reduced = pca.fit_transform(X)

# Varianza spiegata
print(pca.explained_variance_ratio_)
```

**Idea:** Trova direzioni di massima varianza.

**t-SNE (t-Distributed Stochastic Neighbor Embedding):**
```python
from sklearn.manifold import TSNE

tsne = TSNE(n_components=2, perplexity=30)
X_embedded = tsne.fit_transform(X)
```

**Uso:** Visualizzazione dati ad alta dimensione (es. embeddings)

**Applicazioni:**
- Visualizzazione
- Feature extraction
- Noise reduction
- Compressione

### 3. Association Rules

Scoprire relazioni tra variabili.

**Esempio:** Market basket analysis
- "Chi compra pane compra anche latte" (80% delle volte)

**Metriche:**
- **Support**: Frequenza itemset
- **Confidence**: P(B|A)
- **Lift**: Confidence / P(B)

---

## 🎯 Semi-Supervised Learning

### Concetto

Combina **pochi dati etichettati** + **molti dati non etichettati**.

**Motivazione:** Etichettare dati è costoso (tempo, esperti).

### Approcci

**1. Self-Training:**
```
1. Addestra modello su dati etichettati
2. Predici su dati non etichettati
3. Aggiungi predizioni più confident al training set
4. Ri-addestra
5. Ripeti
```

**2. Co-Training:**
- Due modelli addestrati su feature diverse
- Ogni modello etichetta dati per l'altro

**3. Graph-Based:**
- Costruisci grafo di similarità
- Propaga label attraverso grafo

**Applicazioni:**
- Classificazione testo (pochi documenti etichettati)
- Riconoscimento immagini (poche immagini etichettate)
- Speech recognition

---

## 🎮 Reinforcement Learning (Apprendimento per Rinforzo)

### Concetto

**Agent** impara tramite interazione con **environment**, ricevendo **reward** o **punishment**.

**Componenti:**
- **State (s)**: Situazione corrente
- **Action (a)**: Cosa può fare agent
- **Reward (r)**: Feedback numerico
- **Policy (π)**: Strategia (state → action)
- **Value function (V)**: Valore atteso di uno state

### Processo

```
Agent → Action → Environment
  ↑                    ↓
  └─── Reward + State ─┘
```

**Obiettivo:** Massimizzare reward cumulativo nel tempo.

### Algoritmi Principali

**Q-Learning:**
```python
# Q-table: Q(state, action) → expected reward
Q = {}

for episode in range(num_episodes):
    state = env.reset()
    
    while not done:
        # Epsilon-greedy: esplora vs sfrutta
        if random() < epsilon:
            action = random_action()
        else:
            action = argmax(Q[state])
        
        next_state, reward, done = env.step(action)
        
        # Update Q-value
        Q[state][action] = Q[state][action] + α * (
            reward + γ * max(Q[next_state]) - Q[state][action]
        )
        
        state = next_state
```

**Parametri:**
- **α (learning rate)**: Quanto velocemente impara
- **γ (discount factor)**: Importanza reward futuri (0-1)
- **ε (epsilon)**: Probabilità di esplorazione

**Deep Q-Network (DQN):**
- Usa neural network invece di Q-table
- Funziona con state space enormi (es. immagini)

**Policy Gradient:**
- Ottimizza direttamente policy invece di value function
- Funziona con action space continui

**Actor-Critic:**
- Combina policy gradient (actor) + value function (critic)

**Applicazioni:**
- Gaming (AlphaGo, Dota 2, StarCraft)
- Robotica (controllo robot)
- Autonomous driving
- Resource management
- Trading algoritmico

---

## 🔄 Transfer Learning

### Concetto

**Riutilizzare** modello pre-addestrato su task diverso ma correlato.

**Motivazione:**
- Training da zero richiede molti dati e tempo
- Modelli pre-addestrati hanno già imparato features generali

### Approcci

**1. Feature Extraction:**
```python
from tensorflow.keras.applications import VGG16

# Carica modello pre-addestrato (ImageNet)
base_model = VGG16(weights='imagenet', include_top=False)

# Congela layer
base_model.trainable = False

# Aggiungi nuovi layer per tuo task
model = Sequential([
    base_model,
    GlobalAveragePooling2D(),
    Dense(256, activation='relu'),
    Dense(num_classes, activation='softmax')
])

model.compile(optimizer='adam', loss='categorical_crossentropy')
model.fit(X_train, y_train)
```

**2. Fine-Tuning:**
```python
# Dopo feature extraction, sblocca alcuni layer
base_model.trainable = True

# Congela solo primi layer
for layer in base_model.layers[:15]:
    layer.trainable = False

# Ri-addestra con learning rate basso
model.compile(optimizer=Adam(lr=1e-5), loss='categorical_crossentropy')
model.fit(X_train, y_train)
```

**Quando usare:**
- **Feature extraction**: Pochi dati, task molto diverso
- **Fine-tuning**: Più dati, task simile

**Modelli Pre-addestrati Popolari:**

**Computer Vision:**
- VGG16/19, ResNet, Inception, EfficientNet
- Pre-addestrati su ImageNet (1.4M immagini, 1000 classi)

**NLP:**
- BERT, GPT, RoBERTa, T5
- Pre-addestrati su enormi corpus di testo

**Applicazioni:**
- Classificazione immagini mediche (pochi dati etichettati)
- Sentiment analysis (usa BERT pre-addestrato)
- Object detection (usa YOLO/Faster R-CNN pre-addestrati)

---

## 🎯 Domande Tipiche per Esame

### 1. Differenza tra supervised e unsupervised learning

**Risposta:**

**Supervised Learning:**
- Dati etichettati (input + output desiderato)
- Obiettivo: Imparare mapping input → output
- Esempi: Classificazione spam, predizione prezzi
- Algoritmi: Linear regression, Random Forest, Neural Networks

**Unsupervised Learning:**
- Dati non etichettati (solo input)
- Obiettivo: Scoprire struttura nascosta
- Esempi: Clustering clienti, riduzione dimensionalità
- Algoritmi: K-Means, PCA, DBSCAN

**Analogia:**
- Supervised: Insegnante mostra esempi con risposte corrette
- Unsupervised: Studente trova pattern da solo

### 2. Cos'è l'overfitting e come si previene?

**Risposta:**

**Overfitting:** Modello memorizza training data (incluso rumore) invece di imparare pattern generali.

**Sintomi:**
- Accuracy alta su training set
- Accuracy bassa su test set
- Modello troppo complesso per dati disponibili

**Prevenzione:**

1. **Più dati**: Più esempi → meno memorizzazione
2. **Regularization**: Penalizza pesi grandi (L1/L2)
3. **Cross-validation**: Valuta su dati mai visti
4. **Early stopping**: Ferma quando validation error aumenta
5. **Dropout**: (NN) Disattiva random neurons
6. **Semplifica modello**: Meno layer/parametri
7. **Data augmentation**: Crea variazioni dei dati

**Esempio:**
```python
# Regularization L2
from sklearn.linear_model import Ridge
model = Ridge(alpha=1.0)  # alpha controlla regularization

# Cross-validation
from sklearn.model_selection import cross_val_score
scores = cross_val_score(model, X, y, cv=5)
```

### 3. Come funziona K-Means clustering?

**Risposta:**

**Algoritmo:**
1. Scegli K (numero cluster)
2. Inizializza K centroidi random
3. **Assegnazione**: Assegna ogni punto al centroide più vicino
4. **Update**: Ricalcola centroidi come media dei punti assegnati
5. Ripeti 3-4 fino a convergenza (centroidi non cambiano)

**Esempio visivo:**
```
Iterazione 0: Centroidi random
Iterazione 1: Punti assegnati, centroidi aggiornati
Iterazione 2: Riassegnazione, centroidi si muovono
...
Convergenza: Centroidi stabili
```

**Scegliere K:**
- **Elbow method**: Plotta inertia vs K, cerca "gomito"
- **Silhouette score**: Misura quanto cluster sono separati

**Limitazioni:**
- Assume cluster sferici
- Sensibile a inizializzazione
- Deve specificare K a priori

### 4. Cos'è il reinforcement learning?

**Risposta:**

Apprendimento tramite **trial-and-error** con feedback (reward/punishment).

**Componenti:**
- **Agent**: Chi impara (es. robot, AI giocatore)
- **Environment**: Mondo in cui opera
- **State**: Situazione corrente
- **Action**: Cosa può fare
- **Reward**: Feedback numerico (+/-)

**Processo:**
```
Agent osserva state → Sceglie action → 
Environment dà reward + nuovo state → 
Agent impara da esperienza
```

**Obiettivo:** Massimizzare reward totale nel tempo.

**Esempio:** Imparare a giocare a scacchi
- State: Posizione pezzi
- Action: Mossa possibile
- Reward: +1 se vinci, -1 se perdi, 0 altrimenti
- Agent impara strategia vincente giocando molte partite

**Applicazioni:** Gaming (AlphaGo), robotica, autonomous driving

### 5. Cos'è il transfer learning e quando usarlo?

**Risposta:**

Riutilizzare modello pre-addestrato su task diverso ma correlato.

**Perché:**
- Training da zero richiede molti dati e tempo
- Modelli pre-addestrati hanno già imparato features utili

**Approcci:**

1. **Feature Extraction:**
   - Congela layer pre-addestrati
   - Aggiungi nuovi layer per tuo task
   - Addestra solo nuovi layer
   - **Quando:** Pochi dati, task diverso

2. **Fine-Tuning:**
   - Sblocca alcuni layer pre-addestrati
   - Ri-addestra con learning rate basso
   - **Quando:** Più dati, task simile

**Esempio:**
- Modello pre-addestrato su ImageNet (1000 classi generali)
- Adatta per classificare razze di cani (120 classi specifiche)
- Invece di 1M immagini, servono solo 10k

**Vantaggi:**
- ✅ Meno dati necessari
- ✅ Training più veloce
- ✅ Migliori performance

---

## 📊 Tabella Riassuntiva

| Tipo | Dati | Obiettivo | Esempi | Algoritmi |
|------|------|-----------|---------|-----------|
| **Supervised** | Etichettati | Predire output | Spam detection, prezzi | Linear Regression, Random Forest, SVM |
| **Unsupervised** | Non etichettati | Trovare pattern | Segmentazione clienti | K-Means, PCA, DBSCAN |
| **Semi-Supervised** | Pochi etichettati + molti no | Sfruttare dati non etichettati | Classificazione testo | Self-training, Co-training |
| **Reinforcement** | Reward/Punishment | Massimizzare reward | Gaming, robotica | Q-Learning, DQN, Policy Gradient |
| **Transfer** | Pre-addestrato | Riusare conoscenza | Classificazione immagini | Fine-tuning, Feature extraction |

---

## 🔗 Collegamenti

- **Precedente:** [Problemi ML](problemi-ml.md)
- **Successivo:** [Reti Neurali e Deep Learning](reti-neurali-deep-learning.md)
- **Correlato:** [Data Mining](data-mining-visualization.md)
