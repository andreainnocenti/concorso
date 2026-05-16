# Reti Neurali e Deep Learning

## 📖 Introduzione

Le **reti neurali artificiali** sono modelli computazionali ispirati al cervello umano, composti da neuroni artificiali interconnessi. Il **Deep Learning** usa reti neurali profonde (molti layer) per apprendere rappresentazioni gerarchiche dei dati.

**Perché "Deep"?**
- Reti con molti layer nascosti (10, 50, 100+)
- Ogni layer impara features di livello crescente
- Layer bassi: edges, texture
- Layer alti: oggetti complessi, concetti astratti

---

## 🧠 Perceptron: Il Neurone Artificiale

### Struttura

Il perceptron è l'unità base di una rete neurale.

```
Inputs: x₁, x₂, ..., xₙ
Weights: w₁, w₂, ..., wₙ
Bias: b

Output: y = activation(Σ(wᵢ × xᵢ) + b)
```

**Visivamente:**
```
x₁ ──w₁──┐
x₂ ──w₂──┤
x₃ ──w₃──┼──→ Σ ──→ Activation ──→ y
  ...    │
xₙ ──wₙ──┘
    b ───┘
```

### Funzioni di Attivazione

**1. Sigmoid:**
```
σ(x) = 1 / (1 + e^(-x))
Output: (0, 1)
```
- Usata per output probabilistici
- Problema: Vanishing gradient

**2. Tanh:**
```
tanh(x) = (e^x - e^(-x)) / (e^x + e^(-x))
Output: (-1, 1)
```
- Centrata su zero (meglio di sigmoid)
- Ancora vanishing gradient

**3. ReLU (Rectified Linear Unit):**
```
ReLU(x) = max(0, x)
```
- **Più usata** in deep learning
- Veloce da calcolare
- No vanishing gradient per x > 0
- Problema: "Dying ReLU" (neuroni sempre 0)

**4. Leaky ReLU:**
```
LeakyReLU(x) = max(0.01x, x)
```
- Risolve dying ReLU

**5. Softmax (output layer):**
```
softmax(xᵢ) = e^xᵢ / Σ(e^xⱼ)
```
- Converte logits in probabilità
- Σ output = 1
- Usata per classificazione multi-classe

### Esempio Python

```python
import numpy as np

def sigmoid(x):
    return 1 / (1 + np.exp(-x))

def perceptron(inputs, weights, bias):
    # Weighted sum
    z = np.dot(inputs, weights) + bias
    # Activation
    output = sigmoid(z)
    return output

# Esempio
inputs = np.array([1.0, 2.0, 3.0])
weights = np.array([0.5, -0.3, 0.8])
bias = 0.1

output = perceptron(inputs, weights, bias)
print(f"Output: {output}")  # ~0.88
```

---

## 🔗 Multi-Layer Perceptron (MLP)

### Architettura

Rete con layer nascosti tra input e output.

```
Input Layer → Hidden Layer 1 → Hidden Layer 2 → Output Layer
   (3)            (4)              (4)              (2)
```

**Esempio:**
```python
from tensorflow.keras.models import Sequential
from tensorflow.keras.layers import Dense

model = Sequential([
    Dense(64, activation='relu', input_shape=(10,)),  # Hidden 1
    Dense(32, activation='relu'),                      # Hidden 2
    Dense(1, activation='sigmoid')                     # Output
])

model.compile(optimizer='adam', loss='binary_crossentropy', metrics=['accuracy'])
model.fit(X_train, y_train, epochs=10, batch_size=32)
```

### Backpropagation

Algoritmo per addestrare reti neurali.

**Processo:**
1. **Forward pass**: Calcola output
2. **Calcola loss**: Confronta output con target
3. **Backward pass**: Calcola gradienti (chain rule)
4. **Update weights**: w = w - learning_rate × gradient

**Formula gradient descent:**
```
w_new = w_old - α × ∂Loss/∂w
```

**Chain rule (backprop):**
```
∂Loss/∂w₁ = ∂Loss/∂output × ∂output/∂z × ∂z/∂w₁
```

### Ottimizzatori

**SGD (Stochastic Gradient Descent):**
```python
optimizer = tf.keras.optimizers.SGD(learning_rate=0.01)
```

**Adam (Adaptive Moment Estimation):**
```python
optimizer = tf.keras.optimizers.Adam(learning_rate=0.001)
```
- **Più usato**
- Adatta learning rate per ogni parametro
- Combina momentum + RMSprop

**Altri:** RMSprop, AdaGrad, AdaDelta

---

## 🖼️ CNN (Convolutional Neural Networks)

### Architettura per Immagini

Le CNN sono specializzate per dati con struttura spaziale (immagini, video).

**Layer principali:**

**1. Convolutional Layer:**
- Applica filtri (kernel) all'immagine
- Rileva features locali (edges, texture, pattern)

```python
from tensorflow.keras.layers import Conv2D

Conv2D(filters=32, kernel_size=(3,3), activation='relu')
```

**Come funziona:**
```
Immagine 5×5:        Kernel 3×3:
1 2 3 4 5            1 0 -1
6 7 8 9 0            1 0 -1
1 2 3 4 5            1 0 -1
6 7 8 9 0
1 2 3 4 5

Convoluzione → Feature Map
```

**2. Pooling Layer:**
- Riduce dimensioni spaziali
- Mantiene features importanti

```python
from tensorflow.keras.layers import MaxPooling2D

MaxPooling2D(pool_size=(2,2))
```

**Max Pooling 2×2:**
```
Input 4×4:           Output 2×2:
1  3  2  4           3  4
5  6  7  8    →      6  8
9  2  1  3
4  5  6  7
```

**3. Flatten + Dense:**
- Appiattisce feature maps
- Fully connected layers per classificazione

### Architettura Completa

```python
from tensorflow.keras import Sequential
from tensorflow.keras.layers import Conv2D, MaxPooling2D, Flatten, Dense, Dropout

model = Sequential([
    # Block 1
    Conv2D(32, (3,3), activation='relu', input_shape=(28,28,1)),
    MaxPooling2D((2,2)),
    
    # Block 2
    Conv2D(64, (3,3), activation='relu'),
    MaxPooling2D((2,2)),
    
    # Block 3
    Conv2D(64, (3,3), activation='relu'),
    
    # Classifier
    Flatten(),
    Dense(64, activation='relu'),
    Dropout(0.5),
    Dense(10, activation='softmax')  # 10 classi
])

model.compile(optimizer='adam', loss='categorical_crossentropy', metrics=['accuracy'])
```

### Architetture Famose

**LeNet-5 (1998):**
- Prima CNN di successo
- Riconoscimento cifre scritte a mano

**AlexNet (2012):**
- Vinse ImageNet 2012
- 8 layer, 60M parametri
- Usò ReLU e Dropout

**VGG16/19 (2014):**
- Architettura molto profonda (16-19 layer)
- Kernel piccoli (3×3)
- Semplice ma efficace

**ResNet (2015):**
- Residual connections (skip connections)
- Permette reti molto profonde (50, 101, 152 layer)
- Risolve vanishing gradient

**Inception/GoogLeNet:**
- Inception modules (convoluzioni parallele)
- Efficiente computazionalmente

**EfficientNet (2019):**
- Scala bilanciata (depth, width, resolution)
- State-of-the-art efficiency

### Applicazioni CNN

- **Image Classification**: Riconoscimento oggetti
- **Object Detection**: YOLO, Faster R-CNN
- **Semantic Segmentation**: U-Net per immagini mediche
- **Face Recognition**: FaceNet
- **Style Transfer**: Neural style transfer

---

## 🔁 RNN (Recurrent Neural Networks)

### Architettura per Sequenze

Le RNN processano sequenze (testo, audio, time series) mantenendo "memoria" degli input precedenti.

**Struttura:**
```
Input: x₁, x₂, x₃, ...
Hidden state: h₀ → h₁ → h₂ → h₃ → ...
Output: y₁, y₂, y₃, ...
```

**Formula:**
```
hₜ = tanh(Wₓₕ × xₜ + Wₕₕ × hₜ₋₁ + bₕ)
yₜ = Wₕᵧ × hₜ + bᵧ
```

**Problema:** Vanishing/exploding gradient per sequenze lunghe.

### LSTM (Long Short-Term Memory)

Risolve problema di memoria a lungo termine.

**Componenti:**
- **Forget gate**: Cosa dimenticare
- **Input gate**: Cosa aggiungere
- **Output gate**: Cosa output

```python
from tensorflow.keras.layers import LSTM

model = Sequential([
    LSTM(128, return_sequences=True, input_shape=(timesteps, features)),
    LSTM(64),
    Dense(num_classes, activation='softmax')
])
```

**Applicazioni:**
- Language modeling
- Machine translation
- Speech recognition
- Time series prediction
- Video analysis

### GRU (Gated Recurrent Unit)

Versione semplificata di LSTM.

**Vantaggi:**
- Meno parametri (più veloce)
- Performance simile a LSTM

```python
from tensorflow.keras.layers import GRU

model = Sequential([
    GRU(128, return_sequences=True),
    GRU(64),
    Dense(num_classes, activation='softmax')
])
```

---

## 🎯 Attention e Transformers

### Meccanismo di Attention

Permette al modello di "focalizzarsi" su parti rilevanti dell'input.

**Idea:** Non tutte le parole sono ugualmente importanti per predire la prossima.

**Self-Attention:**
```
Query (Q), Key (K), Value (V) da input

Attention(Q, K, V) = softmax(Q × K^T / √d) × V
```

**Esempio:**
```
Frase: "Il gatto mangia il pesce"
Predire: prossima parola dopo "mangia"

Attention weights:
Il: 0.05
gatto: 0.15
mangia: 0.10
il: 0.05
pesce: 0.65  ← Focus qui!
```

### Transformer Architecture

Architettura rivoluzionaria (2017) che ha sostituito RNN per NLP.

**Componenti:**
1. **Multi-Head Attention**: Attention parallele
2. **Feed-Forward Networks**: MLP dopo attention
3. **Positional Encoding**: Codifica posizione parole
4. **Layer Normalization**: Stabilizza training

**Vantaggi vs RNN:**
- ✅ Parallelizzabile (no sequenziale)
- ✅ Cattura dipendenze a lungo raggio
- ✅ Più veloce da addestrare
- ✅ Scala meglio

**Architetture basate su Transformer:**
- **BERT**: Encoder-only (comprensione testo)
- **GPT**: Decoder-only (generazione testo)
- **T5**: Encoder-decoder (seq2seq tasks)

```python
from transformers import BertTokenizer, BertModel

tokenizer = BertTokenizer.from_pretrained('bert-base-uncased')
model = BertModel.from_pretrained('bert-base-uncased')

inputs = tokenizer("Hello world!", return_tensors="pt")
outputs = model(**inputs)
```

---

## 📝 Embeddings

### Word Embeddings

Rappresentazione densa di parole in spazio vettoriale.

**Word2Vec (2013):**
- Impara embeddings da contesto
- Parole simili → vettori vicini

```python
from gensim.models import Word2Vec

sentences = [["cat", "sat", "mat"], ["dog", "ran", "park"]]
model = Word2Vec(sentences, vector_size=100, window=5, min_count=1)

# Similarità
similarity = model.wv.similarity('cat', 'dog')

# Analogie: king - man + woman ≈ queen
result = model.wv.most_similar(positive=['king', 'woman'], negative=['man'])
```

**GloVe (Global Vectors):**
- Basato su co-occorrenze globali
- Pre-addestrato su Wikipedia, Common Crawl

**FastText:**
- Considera sub-word (caratteri)
- Gestisce parole fuori vocabolario

### Contextual Embeddings

Embeddings che cambiano in base al contesto.

**BERT Embeddings:**
```python
from transformers import BertTokenizer, BertModel
import torch

tokenizer = BertTokenizer.from_pretrained('bert-base-uncased')
model = BertModel.from_pretrained('bert-base-uncased')

# "bank" ha significato diverso nei due contesti
text1 = "I went to the bank to deposit money"
text2 = "I sat on the river bank"

inputs1 = tokenizer(text1, return_tensors='pt')
inputs2 = tokenizer(text2, return_tensors='pt')

with torch.no_grad():
    outputs1 = model(**inputs1)
    outputs2 = model(**inputs2)

# Embeddings di "bank" sono diversi!
```

---

## 🎯 Domande Tipiche per Esame

### 1. Differenza tra CNN e RNN

**Risposta:**

**CNN (Convolutional Neural Networks):**
- Per dati con **struttura spaziale** (immagini)
- Usa **convoluzioni** (filtri locali)
- Rileva features gerarchiche (edges → texture → oggetti)
- Parallelizzabile
- **Uso:** Computer vision

**RNN (Recurrent Neural Networks):**
- Per **sequenze** (testo, audio, time series)
- Ha **memoria** (hidden state)
- Processa input sequenzialmente
- Non parallelizzabile
- **Uso:** NLP, speech recognition

**Esempio:**
- CNN: Classificare se immagine contiene gatto
- RNN: Predire prossima parola in frase

### 2. Cos'è il vanishing gradient problem?

**Risposta:**

Problema in reti profonde dove gradienti diventano estremamente piccoli durante backpropagation.

**Causa:**
- Moltiplicazione ripetuta di gradienti < 1
- Dopo molti layer: gradient → 0

**Conseguenza:**
- Layer iniziali non imparano
- Training molto lento o bloccato

**Soluzioni:**

1. **ReLU activation**: No saturazione come sigmoid/tanh
2. **Batch Normalization**: Normalizza input di ogni layer
3. **Residual connections** (ResNet): Skip connections
4. **LSTM/GRU**: Gates per controllare flusso informazione
5. **Gradient clipping**: Limita magnitude gradienti

**Esempio ResNet:**
```
x → Conv → ReLU → Conv → + → Output
↓________________________↑
    (skip connection)
```

### 3. Come funziona il meccanismo di Attention?

**Risposta:**

Attention permette al modello di focalizzarsi su parti rilevanti dell'input.

**Processo:**
1. Calcola **Query** (cosa cerco), **Key** (cosa ho), **Value** (contenuto)
2. Calcola **attention scores**: similarità Query-Key
3. Applica **softmax**: Converte in probabilità
4. **Weighted sum**: Combina Values con pesi attention

**Formula:**
```
Attention(Q, K, V) = softmax(Q × K^T / √d) × V
```

**Esempio traduzione:**
```
Input: "The cat sat on the mat"
Output: "Il gatto sedeva sul tappeto"

Quando genero "gatto", attention si focalizza su "cat"
Quando genero "tappeto", attention si focalizza su "mat"
```

**Vantaggi:**
- Cattura dipendenze a lungo raggio
- Interpretabile (vedi dove guarda il modello)
- Parallelizzabile

### 4. Differenza tra LSTM e GRU

**Risposta:**

Entrambi risolvono vanishing gradient in RNN, ma con complessità diversa.

**LSTM (Long Short-Term Memory):**
- 3 gates: Forget, Input, Output
- Cell state separato da hidden state
- Più parametri
- Più espressivo

**GRU (Gated Recurrent Unit):**
- 2 gates: Reset, Update
- No cell state separato
- Meno parametri (~25% in meno)
- Più veloce

**Quando usare:**
- **LSTM**: Task complessi, molti dati
- **GRU**: Task semplici, meno dati, serve velocità

**Performance:** Simili nella maggior parte dei casi.

### 5. Cos'è il Transfer Learning nelle reti neurali?

**Risposta:**

Riutilizzare rete pre-addestrata su dataset grande per task diverso.

**Processo:**

1. **Pre-training**: Addestra su dataset enorme (es. ImageNet)
2. **Fine-tuning**: Adatta a tuo task specifico

**Approcci:**

**Feature Extraction:**
```python
# Carica modello pre-addestrato
base_model = VGG16(weights='imagenet', include_top=False)
base_model.trainable = False  # Congela

# Aggiungi classifier custom
model = Sequential([
    base_model,
    Flatten(),
    Dense(256, activation='relu'),
    Dense(num_classes, activation='softmax')
])
```

**Fine-Tuning:**
```python
# Sblocca ultimi layer
for layer in base_model.layers[-4:]:
    layer.trainable = True

# Ri-addestra con learning rate basso
model.compile(optimizer=Adam(lr=1e-5), ...)
```

**Vantaggi:**
- ✅ Serve meno dati
- ✅ Training più veloce
- ✅ Migliori performance

**Esempio:** Classificare razze cani usando ResNet pre-addestrato su ImageNet.

---

## 📊 Tabella Riassuntiva Architetture

| Architettura | Tipo Dati | Layer Chiave | Applicazioni | Esempio |
|--------------|-----------|--------------|--------------|---------|
| **MLP** | Tabellari | Dense | Classificazione generale | Iris dataset |
| **CNN** | Immagini | Conv2D, Pooling | Computer vision | ImageNet, object detection |
| **RNN** | Sequenze | Recurrent | Time series, NLP | Stock prediction |
| **LSTM/GRU** | Sequenze lunghe | Gates | NLP, speech | Machine translation |
| **Transformer** | Sequenze | Attention | NLP, vision | BERT, GPT, ViT |
| **Autoencoder** | Qualsiasi | Encoder-Decoder | Compression, denoising | Image denoising |
| **GAN** | Immagini | Generator-Discriminator | Generazione | StyleGAN, DALL-E |

---

## 🔗 Collegamenti

- **Precedente:** [Tipi di ML](tipi-ml.md)
- **Successivo:** [Foundation Models e LLM](foundation-models-llm.md)
- **Correlato:** [Computer Vision](../intelligenza-artificiale-ml-datascience/tipi-ia.md)
