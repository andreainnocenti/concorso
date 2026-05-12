# Algoritmi, Strutture Dati e Complessità

## 📖 Introduzione

Gli algoritmi e le strutture dati sono i fondamenti dell'informatica. Un algoritmo è una sequenza finita di istruzioni ben definite per risolvere un problema, mentre le strutture dati sono modi di organizzare e memorizzare i dati per un accesso e una modifica efficienti.

## 🎯 Obiettivi di Apprendimento

Dopo aver studiato questo argomento, dovresti essere in grado di:

- ✅ Analizzare la complessità temporale e spaziale degli algoritmi
- ✅ Scegliere la struttura dati appropriata per un problema specifico
- ✅ Implementare algoritmi fondamentali di ordinamento e ricerca
- ✅ Applicare tecniche di progettazione algoritmica (divide et impera, programmazione dinamica, greedy)
- ✅ Comprendere e utilizzare strutture dati avanzate (alberi, grafi, heap)

---

## 📊 Analisi della Complessità

### Notazione Big-O

La notazione Big-O descrive il comportamento asintotico di un algoritmo, ovvero come cresce il tempo di esecuzione al crescere della dimensione dell'input.

!!! info "Classi di Complessità Comuni"
    - **O(1)** - Costante: accesso diretto a un elemento
    - **O(log n)** - Logaritmica: ricerca binaria
    - **O(n)** - Lineare: scansione di un array
    - **O(n log n)** - Linearitmica: merge sort, quick sort (caso medio)
    - **O(n²)** - Quadratica: bubble sort, selection sort
    - **O(2ⁿ)** - Esponenziale: problemi NP-completi
    - **O(n!)** - Fattoriale: permutazioni

### Esempio di Analisi

```python
def find_max(arr):
    """
    Trova il massimo in un array
    Complessità: O(n) tempo, O(1) spazio
    """
    if not arr:
        return None
    
    max_val = arr[0]  # O(1)
    for num in arr:   # O(n)
        if num > max_val:
            max_val = num
    return max_val
```

---

## 🗂️ Strutture Dati Fondamentali

### Array e Liste

**Array**
- Accesso: O(1)
- Inserimento/Rimozione: O(n)
- Ricerca: O(n) non ordinato, O(log n) ordinato

**Liste Concatenate**
- Accesso: O(n)
- Inserimento/Rimozione in testa: O(1)
- Ricerca: O(n)

```python
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None

class LinkedList:
    def __init__(self):
        self.head = None
    
    def insert_at_beginning(self, data):
        """Inserimento in O(1)"""
        new_node = Node(data)
        new_node.next = self.head
        self.head = new_node
```

### Stack e Queue

**Stack (LIFO - Last In First Out)**
```python
class Stack:
    def __init__(self):
        self.items = []
    
    def push(self, item):
        """O(1)"""
        self.items.append(item)
    
    def pop(self):
        """O(1)"""
        if not self.is_empty():
            return self.items.pop()
    
    def is_empty(self):
        return len(self.items) == 0
```

**Queue (FIFO - First In First Out)**
```python
from collections import deque

class Queue:
    def __init__(self):
        self.items = deque()
    
    def enqueue(self, item):
        """O(1)"""
        self.items.append(item)
    
    def dequeue(self):
        """O(1)"""
        if not self.is_empty():
            return self.items.popleft()
    
    def is_empty(self):
        return len(self.items) == 0
```

### Hash Table (Dizionari)

Le hash table offrono accesso, inserimento e rimozione in tempo medio O(1).

```python
# In Python, i dizionari sono implementati come hash table
hash_table = {}
hash_table["chiave"] = "valore"  # O(1)
valore = hash_table.get("chiave")  # O(1)
```

!!! warning "Collisioni"
    Le collisioni possono degradare le prestazioni a O(n) nel caso peggiore. Tecniche di risoluzione:
    - Chaining (liste concatenate)
    - Open addressing (probing lineare, quadratico, double hashing)

---

## 🌳 Strutture Dati Avanzate

### Alberi Binari di Ricerca (BST)

```python
class TreeNode:
    def __init__(self, val):
        self.val = val
        self.left = None
        self.right = None

class BST:
    def __init__(self):
        self.root = None
    
    def insert(self, val):
        """O(log n) medio, O(n) peggiore"""
        if not self.root:
            self.root = TreeNode(val)
        else:
            self._insert_recursive(self.root, val)
    
    def _insert_recursive(self, node, val):
        if val < node.val:
            if node.left is None:
                node.left = TreeNode(val)
            else:
                self._insert_recursive(node.left, val)
        else:
            if node.right is None:
                node.right = TreeNode(val)
            else:
                self._insert_recursive(node.right, val)
    
    def search(self, val):
        """O(log n) medio, O(n) peggiore"""
        return self._search_recursive(self.root, val)
    
    def _search_recursive(self, node, val):
        if node is None or node.val == val:
            return node
        if val < node.val:
            return self._search_recursive(node.left, val)
        return self._search_recursive(node.right, val)
```

### Heap (Min-Heap)

```python
import heapq

# Python fornisce heapq per min-heap
heap = []
heapq.heappush(heap, 5)  # O(log n)
heapq.heappush(heap, 3)
heapq.heappush(heap, 7)

min_val = heapq.heappop(heap)  # O(log n) - restituisce 3
```

**Applicazioni:**
- Priority Queue
- Algoritmo di Dijkstra
- Heap Sort

### Grafi

```python
class Graph:
    def __init__(self):
        self.adj_list = {}
    
    def add_vertex(self, vertex):
        if vertex not in self.adj_list:
            self.adj_list[vertex] = []
    
    def add_edge(self, v1, v2):
        """Grafo non orientato"""
        self.adj_list[v1].append(v2)
        self.adj_list[v2].append(v1)
    
    def bfs(self, start):
        """Breadth-First Search - O(V + E)"""
        visited = set()
        queue = deque([start])
        visited.add(start)
        
        while queue:
            vertex = queue.popleft()
            print(vertex, end=" ")
            
            for neighbor in self.adj_list[vertex]:
                if neighbor not in visited:
                    visited.add(neighbor)
                    queue.append(neighbor)
    
    def dfs(self, start, visited=None):
        """Depth-First Search - O(V + E)"""
        if visited is None:
            visited = set()
        
        visited.add(start)
        print(start, end=" ")
        
        for neighbor in self.adj_list[start]:
            if neighbor not in visited:
                self.dfs(neighbor, visited)
```

---

## 🔄 Algoritmi di Ordinamento

### Confronto degli Algoritmi

| Algoritmo | Tempo Medio | Tempo Peggiore | Spazio | Stabile |
|-----------|-------------|----------------|--------|---------|
| Bubble Sort | O(n²) | O(n²) | O(1) | Sì |
| Selection Sort | O(n²) | O(n²) | O(1) | No |
| Insertion Sort | O(n²) | O(n²) | O(1) | Sì |
| Merge Sort | O(n log n) | O(n log n) | O(n) | Sì |
| Quick Sort | O(n log n) | O(n²) | O(log n) | No |
| Heap Sort | O(n log n) | O(n log n) | O(1) | No |

### Merge Sort

```python
def merge_sort(arr):
    """
    Divide et Impera
    Tempo: O(n log n), Spazio: O(n)
    """
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

### Quick Sort

```python
def quick_sort(arr):
    """
    Divide et Impera
    Tempo medio: O(n log n), Peggiore: O(n²)
    """
    if len(arr) <= 1:
        return arr
    
    pivot = arr[len(arr) // 2]
    left = [x for x in arr if x < pivot]
    middle = [x for x in arr if x == pivot]
    right = [x for x in arr if x > pivot]
    
    return quick_sort(left) + middle + quick_sort(right)
```

---

## 🎨 Tecniche di Progettazione Algoritmica

### 1. Divide et Impera

Divide il problema in sottoproblemi più piccoli, risolvi ricorsivamente e combina le soluzioni.

**Esempi:** Merge Sort, Quick Sort, Binary Search

### 2. Programmazione Dinamica

Risolvi sottoproblemi una sola volta e memorizza i risultati per evitare calcoli ripetuti.

```python
def fibonacci_dp(n):
    """
    Fibonacci con programmazione dinamica
    Tempo: O(n), Spazio: O(n)
    """
    if n <= 1:
        return n
    
    dp = [0] * (n + 1)
    dp[1] = 1
    
    for i in range(2, n + 1):
        dp[i] = dp[i-1] + dp[i-2]
    
    return dp[n]

# Ottimizzazione spazio: O(1)
def fibonacci_optimized(n):
    if n <= 1:
        return n
    
    prev, curr = 0, 1
    for _ in range(2, n + 1):
        prev, curr = curr, prev + curr
    
    return curr
```

**Problema dello Zaino (Knapsack):**
```python
def knapsack(weights, values, capacity):
    """
    0/1 Knapsack Problem
    Tempo: O(n * capacity), Spazio: O(n * capacity)
    """
    n = len(weights)
    dp = [[0] * (capacity + 1) for _ in range(n + 1)]
    
    for i in range(1, n + 1):
        for w in range(1, capacity + 1):
            if weights[i-1] <= w:
                dp[i][w] = max(
                    values[i-1] + dp[i-1][w - weights[i-1]],
                    dp[i-1][w]
                )
            else:
                dp[i][w] = dp[i-1][w]
    
    return dp[n][capacity]
```

### 3. Algoritmi Greedy

Fai la scelta localmente ottima ad ogni passo, sperando di trovare l'ottimo globale.

```python
def activity_selection(start, finish):
    """
    Selezione di attività compatibili
    Tempo: O(n log n)
    """
    # Ordina per tempo di fine
    activities = sorted(zip(start, finish), key=lambda x: x[1])
    
    selected = [activities[0]]
    last_finish = activities[0][1]
    
    for s, f in activities[1:]:
        if s >= last_finish:
            selected.append((s, f))
            last_finish = f
    
    return selected
```

---

## 🔍 Algoritmi di Ricerca

### Ricerca Binaria

```python
def binary_search(arr, target):
    """
    Ricerca in array ordinato
    Tempo: O(log n), Spazio: O(1)
    """
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

### Ricerca in Grafi

**Dijkstra (Cammino Minimo):**
```python
import heapq

def dijkstra(graph, start):
    """
    Cammino minimo da un nodo sorgente
    Tempo: O((V + E) log V) con heap
    """
    distances = {node: float('inf') for node in graph}
    distances[start] = 0
    pq = [(0, start)]
    
    while pq:
        current_dist, current = heapq.heappop(pq)
        
        if current_dist > distances[current]:
            continue
        
        for neighbor, weight in graph[current].items():
            distance = current_dist + weight
            
            if distance < distances[neighbor]:
                distances[neighbor] = distance
                heapq.heappush(pq, (distance, neighbor))
    
    return distances
```

---

## 💡 Problemi Classici

### 1. Two Sum

```python
def two_sum(nums, target):
    """
    Trova due numeri che sommano a target
    Tempo: O(n), Spazio: O(n)
    """
    seen = {}
    for i, num in enumerate(nums):
        complement = target - num
        if complement in seen:
            return [seen[complement], i]
        seen[num] = i
    return []
```

### 2. Longest Common Subsequence

```python
def lcs(text1, text2):
    """
    Sottosequenza comune più lunga
    Tempo: O(m * n), Spazio: O(m * n)
    """
    m, n = len(text1), len(text2)
    dp = [[0] * (n + 1) for _ in range(m + 1)]
    
    for i in range(1, m + 1):
        for j in range(1, n + 1):
            if text1[i-1] == text2[j-1]:
                dp[i][j] = dp[i-1][j-1] + 1
            else:
                dp[i][j] = max(dp[i-1][j], dp[i][j-1])
    
    return dp[m][n]
```

### 3. Valid Parentheses

```python
def is_valid_parentheses(s):
    """
    Verifica se le parentesi sono bilanciate
    Tempo: O(n), Spazio: O(n)
    """
    stack = []
    mapping = {')': '(', '}': '{', ']': '['}
    
    for char in s:
        if char in mapping:
            top = stack.pop() if stack else '#'
            if mapping[char] != top:
                return False
        else:
            stack.append(char)
    
    return not stack
```

---

## 📚 Risorse per Approfondire

!!! tip "Libri Consigliati"
    - **"Introduction to Algorithms"** - Cormen, Leiserson, Rivest, Stein (CLRS)
    - **"Algorithm Design Manual"** - Steven Skiena
    - **"Grokking Algorithms"** - Aditya Bhargava (per principianti)

!!! info "Piattaforme di Pratica"
    - [LeetCode](https://leetcode.com/) - Problemi di coding interview
    - [HackerRank](https://www.hackerrank.com/) - Sfide algoritmiche
    - [Codeforces](https://codeforces.com/) - Competitive programming
    - [Project Euler](https://projecteuler.net/) - Problemi matematici

!!! note "Visualizzazioni"
    - [VisuAlgo](https://visualgo.net/) - Visualizzazione di algoritmi
    - [Algorithm Visualizer](https://algorithm-visualizer.org/)

---

## ✅ Checklist di Studio

- [ ] Comprendere la notazione Big-O e saper analizzare la complessità
- [ ] Implementare strutture dati fondamentali da zero
- [ ] Conoscere almeno 3 algoritmi di ordinamento
- [ ] Saper applicare ricerca binaria
- [ ] Comprendere BFS e DFS su grafi
- [ ] Risolvere problemi con programmazione dinamica
- [ ] Praticare su almeno 50 problemi su LeetCode
- [ ] Implementare algoritmi classici (Dijkstra, Knapsack, LCS)

---

## 🔗 Collegamenti

- **Prossimo:** [Architettura degli Elaboratori e Sistemi Operativi](architettura-elaboratori-so.md)
- **Correlato:** [Paradigmi e Linguaggi di Programmazione](paradigmi-linguaggi.md)
- **Applicazioni:** [Big Data](../intelligenza-artificiale-ml-datascience/big-data.md) - per algoritmi su larga scala