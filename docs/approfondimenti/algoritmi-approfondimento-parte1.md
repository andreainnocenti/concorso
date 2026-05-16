# Algoritmi e Strutture Dati - Approfondimento per Esame Orale

## 📚 Guida Completa per l'Esposizione

Questo documento fornisce un approfondimento dettagliato pensato per preparare un'esposizione orale completa sull'argomento "Algoritmi, Strutture Dati e Complessità". Include teoria, storia, esempi pratici e possibili domande d'esame.

---

## 🎓 PARTE 1: INTRODUZIONE E CONTESTO STORICO

### 1.1 Origini del Concetto di Algoritmo

Il termine "algoritmo" ha radici antiche e affascinanti. Deriva dal nome del matematico persiano **Muhammad ibn Musa al-Khwarizmi** (780-850 d.C.), il cui nome latinizzato divenne "Algoritmi". Al-Khwarizmi scrisse il trattato "Al-Kitāb al-mukhtaṣar fī ḥisāb al-jabr wa-l-muqābala" (Il libro conciso sul calcolo per completamento e bilanciamento), da cui deriva anche il termine "algebra".

**Perché è importante per l'esame**: Dimostra che comprendi le radici storiche dell'informatica e puoi contestualizzare i concetti moderni.

#### Algoritmi nell'Antichità

**Algoritmo di Euclide (300 a.C.)**
Il più antico algoritmo non banale ancora in uso. Calcola il Massimo Comun Divisore (MCD) di due numeri.

```
Procedimento:
1. Dati due numeri a e b (con a > b)
2. Dividi a per b e ottieni il resto r
3. Se r = 0, allora MCD(a,b) = b
4. Altrimenti, ripeti con a = b e b = r

Esempio: MCD(48, 18)
48 = 18 × 2 + 12
18 = 12 × 1 + 6
12 = 6 × 2 + 0
Quindi MCD(48, 18) = 6
```

**Perché è rilevante oggi**: 
- Usato in crittografia (RSA)
- Esempio perfetto di algoritmo efficiente (O(log n))
- Dimostra che algoritmi antichi possono essere ottimali

#### Evoluzione nel XX Secolo

**Alan Turing (1936)**: Introduce la Macchina di Turing, modello teorico di computazione che definisce cosa significa "computabile".

**John von Neumann (1945)**: Architettura von Neumann, base dei computer moderni, dove programmi e dati condividono la stessa memoria.

**Donald Knuth (1968)**: Pubblica "The Art of Computer Programming", sistematizzando lo studio degli algoritmi.

### 1.2 Definizione Formale di Algoritmo

Un algoritmo è una **sequenza finita, ordinata e non ambigua di istruzioni** che, dato un input, produce un output in tempo finito.

#### Le 5 Proprietà Fondamentali

1. **Finitezza**: Deve terminare dopo un numero finito di passi
   - Esempio: Un ciclo `while(true)` NON è un algoritmo
   - Controesempio: Calcolo di π con precisione infinita

2. **Definitezza**: Ogni passo deve essere definito precisamente
   - Esempio: "Dividi per 2" è definito
   - Controesempio: "Scegli un numero a caso" è ambiguo senza specificare la distribuzione

3. **Input**: Può ricevere zero o più input
   - Esempio con 0 input: Generatore di numeri casuali con seed fisso
   - Esempio con n input: Ordinamento di un array

4. **Output**: Produce almeno un output
   - L'output può essere un valore di ritorno, una modifica dello stato, o un effetto collaterale

5. **Efficacia**: Ogni operazione deve essere sufficientemente basilare
   - Esempio: Addizione, confronto, assegnazione
   - Controesempio: "Risolvi questo problema NP-completo" non è un'operazione basilare

### 1.3 Algoritmo vs Programma

**Domanda tipica d'esame**: "Qual è la differenza tra algoritmo e programma?"

**Risposta articolata**:
- **Algoritmo**: Concetto astratto, indipendente dal linguaggio, descrive la logica di risoluzione
- **Programma**: Implementazione concreta di un algoritmo in un linguaggio specifico

**Analogia**: L'algoritmo è come una ricetta di cucina (astratta), il programma è il piatto cucinato (concreto).

**Esempio**:
```
ALGORITMO (pseudocodice):
Per ogni elemento nell'array:
    Se l'elemento è maggiore del massimo corrente:
        Aggiorna il massimo

PROGRAMMA (Python):
def find_max(arr):
    max_val = arr[0]
    for num in arr:
        if num > max_val:
            max_val = num
    return max_val
```

---

## 🎓 PARTE 2: ANALISI DELLA COMPLESSITÀ

### 2.1 Perché Studiare la Complessità?

**Scenario reale per l'esame**:
Immagina di dover ordinare 1 milione di record in un database. La differenza tra un algoritmo O(n²) e uno O(n log n) è:
- O(n²): 1.000.000² = 1.000.000.000.000 operazioni (~ 11 giorni su un computer moderno)
- O(n log n): 1.000.000 × 20 = 20.000.000 operazioni (~ 0.02 secondi)

**Questo dimostra che la scelta dell'algoritmo può fare la differenza tra un sistema utilizzabile e uno inutilizzabile.**

### 2.2 Notazione Asintotica: Teoria Completa

#### Big-O (O): Limite Superiore

**Definizione formale**:
f(n) = O(g(n)) se esistono costanti positive c e n₀ tali che:
0 ≤ f(n) ≤ c·g(n) per ogni n ≥ n₀

**Interpretazione**: "f(n) cresce al massimo come g(n), a meno di costanti"

**Esempio pratico**:
```
f(n) = 3n² + 5n + 2

È O(n²) perché:
3n² + 5n + 2 ≤ 3n² + 5n² + 2n² = 10n² per n ≥ 1
Quindi c = 10, n₀ = 1
```

**Errori comuni da evitare**:
- ❌ "O(n) è meglio di O(1)" → Falso! O(1) è costante, sempre meglio
- ❌ "O(2n) è diverso da O(n)" → Falso! Le costanti si ignorano
- ❌ "O(n + m) = O(n)" → Falso! Se m può essere grande quanto n

#### Omega (Ω): Limite Inferiore

**Definizione formale**:
f(n) = Ω(g(n)) se esistono costanti positive c e n₀ tali che:
0 ≤ c·g(n) ≤ f(n) per ogni n ≥ n₀

**Interpretazione**: "f(n) cresce almeno come g(n)"

**Uso pratico**: Dimostrare che un problema richiede almeno un certo tempo.

**Esempio**: L'ordinamento basato su confronti è Ω(n log n) perché:
- Ci sono n! possibili permutazioni
- Un albero di decisione binario ha altezza minima log₂(n!) ≈ n log n
- Quindi servono almeno n log n confronti nel caso peggiore

#### Theta (Θ): Limite Stretto

**Definizione formale**:
f(n) = Θ(g(n)) se f(n) = O(g(n)) E f(n) = Ω(g(n))

**Interpretazione**: "f(n) cresce esattamente come g(n)"

**Esempio**:
```
f(n) = 2n² + 3n + 1 è Θ(n²)

Perché:
- È O(n²): 2n² + 3n + 1 ≤ 6n² per n ≥ 1
- È Ω(n²): 2n² + 3n + 1 ≥ 2n² per ogni n
```

### 2.3 Classi di Complessità: Analisi Dettagliata

#### O(1) - Costante

**Caratteristiche**:
- Tempo di esecuzione indipendente dalla dimensione dell'input
- Ideale, ma raro per problemi complessi

**Esempi reali**:
```python
# Accesso array
elemento = array[5]  # O(1)

# Operazioni aritmetiche
risultato = a + b  # O(1)

# Hash table lookup (caso medio)
valore = dizionario[chiave]  # O(1) medio
```

**Quando è possibile**:
- Accesso diretto tramite indice
- Operazioni matematiche semplici
- Lookup in hash table ben bilanciata

**Limitazioni**:
- Non può risolvere problemi che richiedono esaminare tutti i dati
- Esempio: Non puoi trovare il massimo in un array non ordinato in O(1)

#### O(log n) - Logaritmica

**Caratteristiche**:
- Cresce molto lentamente
- Tipica di algoritmi "divide et impera" che dimezzano il problema ad ogni passo

**Perché è efficiente**:
```
n = 1.000.000 → log₂(n) ≈ 20
n = 1.000.000.000 → log₂(n) ≈ 30

Anche con un miliardo di elementi, servono solo 30 operazioni!
```

**Esempi reali**:

1. **Ricerca Binaria**:
```python
def binary_search(arr, target):
    """
    Cerca in array ordinato dimezzando lo spazio ad ogni passo
    """
    left, right = 0, len(arr) - 1
    
    while left <= right:
        mid = (left + right) // 2
        
        if arr[mid] == target:
            return mid
        elif arr[mid] < target:
            left = mid + 1  # Elimina metà sinistra
        else:
            right = mid - 1  # Elimina metà destra
    
    return -1

# Analisi:
# Iterazione 1: n elementi
# Iterazione 2: n/2 elementi
# Iterazione 3: n/4 elementi
# ...
# Iterazione k: n/2^k elementi
# Termina quando n/2^k = 1, quindi k = log₂(n)
```

2. **Alberi Binari Bilanciati**:
- Altezza = O(log n)
- Ricerca, inserimento, cancellazione: O(log n)

3. **Algoritmi su Heap**:
- Insert: O(log n)
- Extract-min/max: O(log n)

**Applicazioni pratiche**:
- Database con indici B-tree
- Ricerca in dizionari ordinati
- Sistemi di file gerarchici

#### O(n) - Lineare

**Caratteristiche**:
- Tempo proporzionale alla dimensione dell'input
- Spesso inevitabile quando devi esaminare tutti i dati

**Esempi reali**:

1. **Ricerca Lineare**:
```python
def linear_search(arr, target):
    """
    Deve controllare ogni elemento nel caso peggiore
    """
    for i, element in enumerate(arr):
        if element == target:
            return i
    return -1
```

2. **Somma di un Array**:
```python
def sum_array(arr):
    """
    Deve visitare ogni elemento
    """
    total = 0
    for num in arr:
        total += num
    return total
```

3. **Copia di una Struttura Dati**:
```python
def copy_list(original):
    """
    Deve copiare ogni elemento
    """
    return [x for x in original]
```

**Quando è ottimale**:
- Quando devi processare ogni elemento almeno una volta
- Esempio: Calcolare la media richiede vedere tutti i numeri

**Limite inferiore**:
Molti problemi hanno limite inferiore Ω(n):
- Trovare il massimo: devi vedere tutti gli elementi
- Verificare se un array contiene duplicati: devi vedere tutti (caso peggiore)

#### O(n log n) - Linearitmica

**Caratteristiche**:
- Tipica dei migliori algoritmi di ordinamento
- Limite inferiore teorico per ordinamento basato su confronti

**Perché n log n è il limite per l'ordinamento?**

**Dimostrazione con albero di decisione**:
```
Per ordinare n elementi:
- Ci sono n! possibili permutazioni (output possibili)
- Un albero di decisione binario con n! foglie ha altezza minima log₂(n!)
- log₂(n!) = log₂(n × (n-1) × ... × 1) ≈ n log₂(n) per la formula di Stirling
- Quindi servono almeno n log n confronti
```

**Algoritmi O(n log n)**:

1. **Merge Sort**:
```python
def merge_sort(arr):
    if len(arr) <= 1:
        return arr
    
    mid = len(arr) // 2
    left = merge_sort(arr[:mid])   # T(n/2)
    right = merge_sort(arr[mid:])  # T(n/2)
    
    return merge(left, right)      # O(n)

# Equazione di ricorrenza:
# T(n) = 2T(n/2) + O(n)
# Soluzione: T(n) = O(n log n) per il Master Theorem
```

2. **Quick Sort (caso medio)**:
```python
def quick_sort(arr):
    if len(arr) <= 1:
        return arr
    
    pivot = arr[len(arr) // 2]
    left = [x for x in arr if x < pivot]    # O(n)
    middle = [x for x in arr if x == pivot]
    right = [x for x in arr if x > pivot]
    
    return quick_sort(left) + middle + quick_sort(right)

# Caso medio: pivot divide circa a metà
# T(n) = 2T(n/2) + O(n) = O(n log n)
```

3. **Heap Sort**:
```python
def heap_sort(arr):
    # Build heap: O(n)
    heapify(arr)
    
    # Extract n volte: n × O(log n)
    for i in range(len(arr) - 1, 0, -1):
        arr[0], arr[i] = arr[i], arr[0]
        sift_down(arr, 0, i)
    
    return arr
# Totale: O(n) + O(n log n) = O(n log n)
```

**Applicazioni pratiche**:
- Ordinamento di grandi dataset
- Algoritmi di compressione (Huffman coding)
- Algoritmi su grafi (alcuni casi)

#### O(n²) - Quadratica

**Caratteristiche**:
- Tempo cresce con il quadrato dell'input
- Accettabile per piccoli input (n < 1000)
- Problematico per grandi dataset

**Quando si verifica**:
- Cicli annidati che iterano su n elementi
- Confronto di ogni elemento con ogni altro

**Esempi reali**:

1. **Bubble Sort**:
```python
def bubble_sort(arr):
    n = len(arr)
    for i in range(n):           # n iterazioni
        for j in range(n - i - 1):  # n iterazioni (medio)
            if arr[j] > arr[j + 1]:
                arr[j], arr[j + 1] = arr[j + 1], arr[j]
    return arr

# Analisi:
# Iterazione esterna: n volte
# Iterazione interna: (n-1) + (n-2) + ... + 1 = n(n-1)/2 ≈ n²/2
# Totale: O(n²)
```

2. **Selection Sort**:
```python
def selection_sort(arr):
    for i in range(len(arr)):
        min_idx = i
        for j in range(i + 1, len(arr)):  # Trova minimo
            if arr[j] < arr[min_idx]:
                min_idx = j
        arr[i], arr[min_idx] = arr[min_idx], arr[i]
    return arr
```

3. **Verifica Duplicati (Naive)**:
```python
def has_duplicates_naive(arr):
    for i in range(len(arr)):
        for j in range(i + 1, len(arr)):
            if arr[i] == arr[j]:
                return True
    return False

# Versione O(n) con set:
def has_duplicates_optimal(arr):
    return len(arr) != len(set(arr))
```

**Impatto pratico**:
```
n = 100:    10.000 operazioni      → 0.01 ms
n = 1.000:  1.000.000 operazioni   → 1 ms
n = 10.000: 100.000.000 operazioni → 100 ms
n = 100.000: 10.000.000.000 ops    → 10 secondi!
```

**Quando è accettabile**:
- Input piccoli (n < 100)
- Semplicità del codice è prioritaria
- Insertion Sort per array quasi ordinati

#### O(2ⁿ) - Esponenziale

**Caratteristiche**:
- Cresce estremamente rapidamente
- Diventa intrattabile anche per n moderati
- Tipica di algoritmi che esplorano tutte le possibilità

**Impatto drammatico**:
```
n = 10:  1.024 operazioni        → Istantaneo
n = 20:  1.048.576 operazioni    → Veloce
n = 30:  1.073.741.824 ops       → 1 secondo
n = 40:  1.099.511.627.776 ops   → 18 minuti
n = 50:  1.125.899.906.842.624   → 13 giorni!
```

**Esempi reali**:

1. **Fibonacci Ricorsivo Naive**:
```python
def fibonacci_recursive(n):
    if n <= 1:
        return n
    return fibonacci_recursive(n-1) + fibonacci_recursive(n-2)

# Albero di ricorsione:
#           fib(5)
#          /      \
#      fib(4)    fib(3)
#      /    \    /    \
#   fib(3) fib(2) ...
#
# Numero di chiamate: 2^n (approssimato)
# fib(40) richiede 2^40 ≈ 1 trilione di chiamate!
```

2. **Subset Sum (Brute Force)**:
```python
def subset_sum_bruteforce(arr, target):
    """
    Trova se esiste un sottoinsieme che somma a target
    Prova tutti i 2^n sottoinsiemi possibili
    """
    n = len(arr)
    for i in range(2**n):  # 2^n iterazioni
        subset = [arr[j] for j in range(n) if (i >> j) & 1]
        if sum(subset) == target:
            return True
    return False
```

3. **Torre di Hanoi**:
```python
def hanoi(n, source, target, auxiliary):
    if n == 1:
        print(f"Move disk 1 from {source} to {target}")
        return
    hanoi(n-1, source, auxiliary, target)
    print(f"Move disk {n} from {source} to {target}")
    hanoi(n-1, auxiliary, target, source)

# Numero di mosse: 2^n - 1
# Con 64 dischi: 2^64 - 1 ≈ 18 quintilioni di mosse
# A 1 mossa/secondo: 585 miliardi di anni!
```

**Quando si incontra**:
- Problemi NP-completi risolti con brute force
- Backtracking senza pruning
- Generazione di tutte le combinazioni

**Come migliorare**:
- Programmazione dinamica (Fibonacci: O(2ⁿ) → O(n))
- Memoization
- Algoritmi greedy (se applicabili)
- Algoritmi approssimati

---

## 🎓 PARTE 3: DOMANDE TIPICHE D'ESAME

### Domanda 1: "Spiega la differenza tra O, Ω e Θ"

**Risposta strutturata**:

"Le tre notazioni descrivono diversi aspetti della complessità:

- **Big-O (O)** fornisce un **limite superiore**: indica che l'algoritmo non può essere peggiore di quella complessità. È la più usata perché ci interessa il caso peggiore.

- **Omega (Ω)** fornisce un **limite inferiore**: indica che l'algoritmo richiede almeno quella complessità. Utile per dimostrare che un problema è intrinsecamente difficile.

- **Theta (Θ)** fornisce un **limite stretto**: indica che l'algoritmo ha esattamente quella complessità, sia come limite superiore che inferiore.

**Esempio pratico**: La ricerca lineare in un array non ordinato:
- È O(n): nel caso peggiore scorre tutto l'array
- È Ω(1): nel caso migliore trova subito l'elemento
- NON è Θ(n) perché i casi migliore e peggiore differiscono

Mentre la ricerca binaria in un array ordinato:
- È O(log n)
- È Ω(log n)
- È Θ(log n): ha sempre complessità logaritmica"

### Domanda 2: "Perché l'ordinamento basato su confronti non può essere più veloce di O(n log n)?"

**Risposta articolata**:

"Questa è una domanda di teoria della complessità fondamentale. La dimostrazione usa il concetto di **albero di decisione**:

1. **Setup**: Per ordinare n elementi, esistono n! possibili permutazioni (output possibili)

2. **Albero di decisione**: Ogni algoritmo basato su confronti può essere rappresentato come un albero binario dove:
   - Ogni nodo interno è un confronto (a < b?)
   - Ogni foglia è un output (una permutazione ordinata)

3. **Altezza minima**: Un albero binario con n! foglie ha altezza minima log₂(n!)

4. **Formula di Stirling**: log₂(n!) ≈ n log₂(n) - n log₂(e) ≈ n log₂(n)

5. **Conclusione**: Servono almeno n log n confronti nel caso peggiore

**Implicazioni pratiche**:
- Merge Sort, Quick Sort (medio), Heap Sort sono ottimali asintoticamente
- Per fare meglio, serve informazione extra (es: Counting Sort con range limitato)
- Questo è un esempio di **limite inferiore** per una classe di problemi"

### Domanda 3: "Quando useresti Quick Sort invece di Merge Sort?"

**Risposta comparativa**:

"La scelta dipende da vari fattori:

**Quick Sort è preferibile quando**:
- Memoria limitata (in-place, O(log n) stack vs O(n) di Merge Sort)
- Dati casuali (caso medio O(n log n) molto veloce in pratica)
- Cache performance importante (più cache-friendly)
- Implementazione più semplice

**Merge Sort è preferibile quando**:
- Serve garanzia O(n log n) anche nel caso peggiore
- Stabilità richiesta (mantiene ordine relativo di elementi uguali)
- Ordinamento di linked list (non richiede accesso casuale)
- Parallelizzazione (più facile da parallelizzare)

**Esempio pratico**:
- Java usa Quick Sort per tipi primitivi (int, double) dove stabilità non serve
- Java usa Merge Sort (TimSort) per oggetti dove stabilità è importante
- Python usa TimSort (variante di Merge Sort) come default

**Ottimizzazione ibrida**:
Molte implementazioni moderne usano:
- Quick Sort per grandi partizioni
- Insertion Sort per piccole partizioni (< 10 elementi)
- Mediana-di-tre per scegliere il pivot"

---

*Continua nella Parte 2...*
