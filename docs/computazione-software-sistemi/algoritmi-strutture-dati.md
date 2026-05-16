# Algoritmi, Strutture Dati e Complessità

## 📖 Introduzione

Gli algoritmi e le strutture dati sono i fondamenti dell'informatica. Un **algoritmo** è una sequenza finita di istruzioni ben definite per risolvere un problema, mentre le **strutture dati** sono modi di organizzare e memorizzare i dati per un accesso e una modifica efficienti.

---

## 📊 Analisi della Complessità

### Notazione Big-O

La notazione Big-O descrive come cresce il tempo di esecuzione di un algoritmo al crescere della dimensione dell'input (n).

**Classi di complessità comuni (dalla più veloce alla più lenta):**

| Notazione | Nome | Esempio | Descrizione |
|-----------|------|---------|-------------|
| O(1) | Costante | Accesso array[i] | Tempo fisso, indipendente da n |
| O(log n) | Logaritmica | Ricerca binaria | Dimezza lo spazio di ricerca ad ogni passo |
| O(n) | Lineare | Scansione array | Esamina ogni elemento una volta |
| O(n log n) | Linearitmica | Merge Sort, Quick Sort | Divide e conquista efficiente |
| O(n²) | Quadratica | Bubble Sort, nested loops | Due cicli annidati |
| O(2ⁿ) | Esponenziale | Fibonacci ricorsivo | Raddoppia ad ogni incremento |
| O(n!) | Fattoriale | Permutazioni | Cresce estremamente veloce |

**Esempio pratico:**
```python
# O(1) - Costante
def get_first(arr):
    return arr[0]  # Sempre 1 operazione

# O(n) - Lineare
def find_max(arr):
    max_val = arr[0]
    for num in arr:  # n operazioni
        if num > max_val:
            max_val = num
    return max_val

# O(n²) - Quadratica
def has_duplicates(arr):
    for i in range(len(arr)):      # n volte
        for j in range(i+1, len(arr)):  # n volte
            if arr[i] == arr[j]:
                return True
    return False
```

### Complessità Spaziale

Oltre al tempo, si analizza anche lo **spazio** (memoria) usato:

- **O(1)**: Spazio costante (poche variabili)
- **O(n)**: Spazio proporzionale all'input (array ausiliario)
- **O(log n)**: Stack ricorsivo di algoritmi divide-et-impera

---

## 🗂️ Strutture Dati Fondamentali

### Array e Liste

**Array:**
- Accesso diretto: O(1) - `arr[5]`
- Inserimento/rimozione: O(n) - deve spostare elementi
- Ricerca: O(n) non ordinato, O(log n) se ordinato (binary search)
- **Uso:** Quando serve accesso veloce per indice

**Lista Concatenata:**
- Accesso: O(n) - deve scorrere dalla testa
- Inserimento/rimozione in testa: O(1)
- Ricerca: O(n)
- **Uso:** Inserimenti/rimozioni frequenti, dimensione variabile

```python
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None

class LinkedList:
    def __init__(self):
        self.head = None
    
    def insert_at_beginning(self, data):
        new_node = Node(data)
        new_node.next = self.head
        self.head = new_node  # O(1)
```

### Stack e Queue

**Stack (LIFO - Last In First Out):**
- Push/Pop: O(1)
- **Uso:** Undo/redo, parsing espressioni, backtracking
- **Esempio:** Stack di chiamate funzioni

```python
stack = []
stack.append(1)  # push
stack.append(2)
top = stack.pop()  # pop → 2
```

**Queue (FIFO - First In First Out):**
- Enqueue/Dequeue: O(1)
- **Uso:** BFS, task scheduling, buffer
- **Esempio:** Coda di stampa

```python
from collections import deque
queue = deque()
queue.append(1)  # enqueue
queue.append(2)
first = queue.popleft()  # dequeue → 1
```

### Hash Table (Dizionari)

Struttura che mappa chiavi a valori con accesso O(1) medio.

**Operazioni:**
- Inserimento: O(1) medio
- Ricerca: O(1) medio
- Rimozione: O(1) medio

```python
# In Python: dict
hash_table = {}
hash_table["nome"] = "Alice"  # O(1)
valore = hash_table.get("nome")  # O(1)
```

**Come funziona:**
1. Calcola hash della chiave: `hash("nome") = 12345`
2. Usa hash come indice: `array[12345 % size]`
3. Gestisce collisioni (chaining o open addressing)

**Uso:** Cache, conteggio frequenze, rimozione duplicati

---

## 🌳 Strutture Dati Avanzate

### Alberi Binari di Ricerca (BST)

Albero dove ogni nodo ha al massimo 2 figli, con proprietà:
- Sottoalbero sinistro: valori < nodo
- Sottoalbero destro: valori > nodo

**Operazioni:**
- Ricerca/Inserimento/Rimozione: O(log n) medio, O(n) peggiore (albero sbilanciato)

**Uso:** Database index, set ordinati

**Varianti bilanciate (sempre O(log n)):**
- AVL Tree
- Red-Black Tree (usato in TreeMap Java, map C++)

### Heap

Albero binario completo con proprietà:
- **Min-Heap**: Ogni nodo ≤ figli
- **Max-Heap**: Ogni nodo ≥ figli

**Operazioni:**
- Inserimento: O(log n)
- Estrazione min/max: O(log n)
- Peek (guarda min/max): O(1)

**Uso:** Priority Queue, Heap Sort, algoritmo di Dijkstra

```python
import heapq
heap = []
heapq.heappush(heap, 5)
heapq.heappush(heap, 3)
heapq.heappush(heap, 7)
min_val = heapq.heappop(heap)  # 3
```

### Grafi

Insieme di nodi (vertici) connessi da archi.

**Rappresentazioni:**
- **Lista di adiacenza**: `{A: [B, C], B: [A, D], ...}` - O(V+E) spazio
- **Matrice di adiacenza**: Array 2D - O(V²) spazio

**Algoritmi di visita:**

**BFS (Breadth-First Search):**
- Esplora livello per livello
- Usa Queue
- Trova cammino minimo (grafi non pesati)
- Complessità: O(V + E)

**DFS (Depth-First Search):**
- Esplora in profondità
- Usa Stack (o ricorsione)
- Rileva cicli, componenti connesse
- Complessità: O(V + E)

---

## 🔄 Algoritmi di Ordinamento

### Confronto Algoritmi

| Algoritmo | Tempo Medio | Tempo Peggiore | Spazio | Stabile* |
|-----------|-------------|----------------|--------|----------|
| Bubble Sort | O(n²) | O(n²) | O(1) | Sì |
| Selection Sort | O(n²) | O(n²) | O(1) | No |
| Insertion Sort | O(n²) | O(n²) | O(1) | Sì |
| **Merge Sort** | **O(n log n)** | **O(n log n)** | O(n) | Sì |
| **Quick Sort** | **O(n log n)** | O(n²) | O(log n) | No |
| Heap Sort | O(n log n) | O(n log n) | O(1) | No |

*Stabile = mantiene l'ordine relativo di elementi uguali

### Merge Sort (Divide et Impera)

**Idea:** Dividi array a metà, ordina ricorsivamente, unisci (merge).

```python
def merge_sort(arr):
    if len(arr) <= 1:
        return arr
    
    mid = len(arr) // 2
    left = merge_sort(arr[:mid])
    right = merge_sort(arr[mid:])
    
    return merge(left, right)

def merge(left, right):
    result = []
    i = j = 0
    while i < len(left) and j < len(right):
        if left[i] <= right[j]:
            result.append(left[i])
            i += 1
        else:
            result.append(right[j])
            j += 1
    result.extend(left[i:])
    result.extend(right[j:])
    return result
```

**Pro:** Sempre O(n log n), stabile
**Contro:** Richiede O(n) spazio extra

### Quick Sort (Divide et Impera)

**Idea:** Scegli pivot, partiziona (elementi < pivot a sinistra, > pivot a destra), ordina ricorsivamente.

```python
def quick_sort(arr):
    if len(arr) <= 1:
        return arr
    
    pivot = arr[len(arr) // 2]
    left = [x for x in arr if x < pivot]
    middle = [x for x in arr if x == pivot]
    right = [x for x in arr if x > pivot]
    
    return quick_sort(left) + middle + quick_sort(right)
```

**Pro:** O(n log n) medio, in-place, veloce in pratica
**Contro:** O(n²) peggiore (pivot sempre min/max)

---

## 🎨 Tecniche di Progettazione Algoritmica

### 1. Divide et Impera

**Strategia:** Dividi problema in sottoproblemi, risolvi ricorsivamente, combina soluzioni.

**Esempi:**
- Merge Sort, Quick Sort
- Binary Search
- Moltiplicazione di Karatsuba

**Schema:**
```
function divideEtImpera(problema):
    if problema è piccolo:
        risolvi direttamente
    else:
        dividi in sottoproblemi
        risolvi ricorsivamente ogni sottoproblema
        combina le soluzioni
```

### 2. Programmazione Dinamica

**Strategia:** Risolvi sottoproblemi una volta, memorizza risultati, riusa per problemi più grandi.

**Quando usarla:**
- Sottoproblemi sovrapposti (stessi calcoli ripetuti)
- Sottostruttura ottima (soluzione ottima contiene soluzioni ottime dei sottoproblemi)

**Esempio: Fibonacci**
```python
# Ricorsivo ingenuo: O(2ⁿ) - LENTO
def fib_recursive(n):
    if n <= 1:
        return n
    return fib_recursive(n-1) + fib_recursive(n-2)

# Programmazione dinamica: O(n) - VELOCE
def fib_dp(n):
    if n <= 1:
        return n
    dp = [0] * (n + 1)
    dp[1] = 1
    for i in range(2, n + 1):
        dp[i] = dp[i-1] + dp[i-2]
    return dp[n]
```

**Problema classico: 0/1 Knapsack (Zaino)**
- Hai n oggetti con peso e valore
- Zaino con capacità massima
- Massimizza valore senza superare capacità

### 3. Algoritmi Greedy

**Strategia:** Fai scelta localmente ottima ad ogni passo, sperando nell'ottimo globale.

**Quando funziona:**
- Proprietà greedy choice (scelta locale porta a ottimo globale)
- Sottostruttura ottima

**Esempi:**
- Algoritmo di Dijkstra (cammino minimo)
- Algoritmo di Kruskal (minimum spanning tree)
- Huffman coding (compressione)

**Esempio: Resto monete**
```python
def min_coins(amount, coins=[25, 10, 5, 1]):
    """Minimo numero di monete per dare resto"""
    coins.sort(reverse=True)
    count = 0
    for coin in coins:
        count += amount // coin
        amount %= coin
    return count
```

**Attenzione:** Greedy non sempre trova l'ottimo (es. con monete [1, 3, 4] per resto 6, greedy dà 4+1+1=3 monete, ottimo è 3+3=2)

---

## 🔍 Algoritmi di Ricerca

### Ricerca Lineare vs Binaria

**Ricerca Lineare:**
- Scorre tutti gli elementi
- O(n)
- Funziona su array non ordinati

**Ricerca Binaria:**
- Dimezza spazio di ricerca ad ogni passo
- O(log n)
- **Richiede array ordinato**

```python
def binary_search(arr, target):
    left, right = 0, len(arr) - 1
    
    while left <= right:
        mid = (left + right) // 2
        
        if arr[mid] == target:
            return mid
        elif arr[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    
    return -1  # Non trovato
```

### Dijkstra (Cammino Minimo)

Trova il cammino più corto da un nodo sorgente a tutti gli altri in un grafo pesato.

**Complessità:** O((V + E) log V) con heap

**Idea:**
1. Inizializza distanze: sorgente=0, altri=∞
2. Usa min-heap con (distanza, nodo)
3. Estrai nodo con distanza minima
4. Aggiorna distanze dei vicini
5. Ripeti fino a heap vuoto

**Limitazione:** Non funziona con pesi negativi (usa Bellman-Ford)

---

## 🎯 Domande Tipiche per Esame

### 1. Differenza tra O(n) e O(n²)

**Risposta:**
- **O(n)**: Tempo cresce linearmente. Se input raddoppia, tempo raddoppia.
  - Esempio: Scorrere array una volta
- **O(n²)**: Tempo cresce quadraticamente. Se input raddoppia, tempo quadruplica.
  - Esempio: Due cicli annidati

**Impatto pratico:**
- n=1000: O(n)=1000 operazioni, O(n²)=1.000.000 operazioni
- n=10000: O(n)=10000 operazioni, O(n²)=100.000.000 operazioni

### 2. Quando usare array vs lista concatenata?

**Risposta:**

**Array:**
- ✅ Accesso casuale frequente (arr[i])
- ✅ Dimensione nota/fissa
- ✅ Memoria contigua (cache-friendly)
- ❌ Inserimenti/rimozioni costosi

**Lista concatenata:**
- ✅ Inserimenti/rimozioni frequenti (specialmente in testa)
- ✅ Dimensione variabile
- ❌ Accesso casuale lento
- ❌ Overhead memoria (puntatori)

### 3. Perché Merge Sort è O(n log n)?

**Risposta:**
- **Divisione:** Dividi array a metà ricorsivamente → log n livelli
- **Merge:** Ad ogni livello, unisci tutti gli n elementi → O(n) per livello
- **Totale:** log n livelli × O(n) per livello = O(n log n)

**Esempio con n=8:**
```
Livello 0: [8 elementi] → 1 array di 8
Livello 1: [4][4] → 2 array di 4
Livello 2: [2][2][2][2] → 4 array di 2
Livello 3: [1][1][1][1][1][1][1][1] → 8 array di 1
Totale livelli: log₂(8) = 3
```

### 4. Cos'è la programmazione dinamica?

**Risposta:**
Tecnica per ottimizzare algoritmi ricorsivi che risolvono sottoproblemi ripetuti.

**Principi:**
1. **Memoization (top-down):** Memorizza risultati in cache
2. **Tabulation (bottom-up):** Riempi tabella dai casi base

**Esempio:** Fibonacci ricorsivo fa 2ⁿ chiamate (molte ripetute). Con DP: O(n) chiamate.

**Quando usarla:**
- Sottoproblemi sovrapposti
- Sottostruttura ottima

### 5. Differenza tra BFS e DFS

**Risposta:**

**BFS (Breadth-First Search):**
- Esplora livello per livello
- Usa **Queue**
- Trova **cammino minimo** (grafi non pesati)
- Più memoria (tiene tutti nodi di un livello)

**DFS (Depth-First Search):**
- Esplora in profondità prima
- Usa **Stack** (o ricorsione)
- Rileva **cicli**, componenti connesse
- Meno memoria (solo path corrente)

**Esempio visivo:**
```
    A
   / \
  B   C
 / \   \
D   E   F

BFS: A → B,C → D,E,F
DFS: A → B → D → E → C → F
```

---

## 📊 Tabella Riassuntiva Strutture Dati

| Struttura | Accesso | Ricerca | Inserimento | Rimozione | Uso Principale |
|-----------|---------|---------|-------------|-----------|----------------|
| Array | O(1) | O(n) | O(n) | O(n) | Accesso diretto |
| Lista Concatenata | O(n) | O(n) | O(1)* | O(1)* | Inserimenti frequenti |
| Stack | O(n) | O(n) | O(1) | O(1) | LIFO, backtracking |
| Queue | O(n) | O(n) | O(1) | O(1) | FIFO, BFS |
| Hash Table | - | O(1)† | O(1)† | O(1)† | Lookup veloce |
| BST | O(log n)† | O(log n)† | O(log n)† | O(log n)† | Dati ordinati |
| Heap | - | O(n) | O(log n) | O(log n) | Priority Queue |

*In testa/coda, †caso medio

---

## 🔗 Collegamenti

- **Prossimo:** [Architettura degli Elaboratori e Sistemi Operativi](architettura-elaboratori-so.md)
- **Correlato:** [Paradigmi e Linguaggi di Programmazione](paradigmi-linguaggi.md)
- **Applicazioni:** [Big Data](../intelligenza-artificiale-ml-datascience/big-data.md)