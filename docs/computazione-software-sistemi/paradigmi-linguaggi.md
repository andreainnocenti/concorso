# Paradigmi e Linguaggi di Programmazione

## 📖 Introduzione

I **paradigmi di programmazione** sono approcci fondamentali alla risoluzione di problemi attraverso il codice. Ogni linguaggio supporta uno o più paradigmi, influenzando come pensiamo e strutturiamo le soluzioni.

---

## 🎯 Paradigmi Principali

### 1. Programmazione Imperativa

**Concetto:** Sequenza di comandi che modificano lo stato del programma.

**Caratteristiche:**
- Istruzioni eseguite sequenzialmente
- Variabili mutabili
- Controllo di flusso esplicito (if, for, while)

**Esempio C:**
```c
int sum = 0;
for (int i = 1; i <= 10; i++) {
    sum += i;
}
printf("Sum: %d\n", sum);
```

**Pro:** Intuitivo, vicino all'hardware, efficiente
**Contro:** Difficile da parallelizzare, side effects

### 2. Programmazione Orientata agli Oggetti (OOP)

**Concetto:** Organizzazione del codice in oggetti con dati (attributi) e comportamenti (metodi).

**Principi fondamentali:**

**Incapsulamento:**
```python
class BankAccount:
    def __init__(self, balance):
        self.__balance = balance  # Privato
    
    def deposit(self, amount):
        if amount > 0:
            self.__balance += amount
    
    def get_balance(self):
        return self.__balance
```

**Ereditarietà:**
```python
class Animal:
    def speak(self):
        pass

class Dog(Animal):
    def speak(self):
        return "Woof!"

class Cat(Animal):
    def speak(self):
        return "Meow!"
```

**Polimorfismo:**
```python
def make_sound(animal):
    print(animal.speak())

make_sound(Dog())  # "Woof!"
make_sound(Cat())  # "Meow!"
```

**Astrazione:**
```python
from abc import ABC, abstractmethod

class Shape(ABC):
    @abstractmethod
    def area(self):
        pass

class Circle(Shape):
    def __init__(self, radius):
        self.radius = radius
    
    def area(self):
        return 3.14 * self.radius ** 2
```

**Pro:** Riusabilità, manutenibilità, modellazione naturale
**Contro:** Overhead, complessità per problemi semplici

### 3. Programmazione Funzionale

**Concetto:** Computazione come valutazione di funzioni matematiche pure.

**Principi:**
- **Immutabilità**: Dati non modificabili
- **Funzioni pure**: Stesso input → stesso output, no side effects
- **First-class functions**: Funzioni come valori
- **Higher-order functions**: Funzioni che prendono/restituiscono funzioni

**Esempio Haskell:**
```haskell
-- Funzione pura
square :: Int -> Int
square x = x * x

-- Higher-order function
map square [1, 2, 3, 4]  -- [1, 4, 9, 16]

-- Composizione
(square . (+1)) 5  -- square(5+1) = 36
```

**Python (stile funzionale):**
```python
# Map, filter, reduce
numbers = [1, 2, 3, 4, 5]

squared = list(map(lambda x: x**2, numbers))
evens = list(filter(lambda x: x % 2 == 0, numbers))

from functools import reduce
sum_all = reduce(lambda a, b: a + b, numbers)
```

**Pro:** Facile da testare, parallelizzabile, no side effects
**Contro:** Curva apprendimento, performance (immutabilità)

### 4. Programmazione Dichiarativa

**Concetto:** Descrizione di "cosa" fare, non "come" farlo.

**SQL (dichiarativo):**
```sql
SELECT name, age 
FROM users 
WHERE age > 18 
ORDER BY name;
```

vs **Imperativo:**
```python
result = []
for user in users:
    if user.age > 18:
        result.append((user.name, user.age))
result.sort(key=lambda x: x[0])
```

**Pro:** Più conciso, ottimizzazione automatica
**Contro:** Meno controllo, debugging più difficile

### 5. Programmazione Logica

**Concetto:** Definizione di fatti e regole, il sistema deduce soluzioni.

**Prolog:**
```prolog
% Fatti
parent(tom, bob).
parent(tom, liz).
parent(bob, ann).

% Regole
grandparent(X, Z) :- parent(X, Y), parent(Y, Z).

% Query
?- grandparent(tom, ann).
% true
```

**Uso:** AI, sistemi esperti, NLP

---

## 💻 Linguaggi Moderni

### Python

**Paradigmi:** Multi-paradigma (imperativo, OOP, funzionale)
**Tipizzazione:** Dinamica, duck typing
**Gestione memoria:** Garbage collection

**Caratteristiche:**
- Sintassi leggibile
- Librerie ricchissime
- Interpretato

**Uso:** Data science, ML, web, scripting

```python
# OOP
class Calculator:
    def add(self, a, b):
        return a + b

# Funzionale
add = lambda a, b: a + b

# List comprehension
squares = [x**2 for x in range(10)]
```

### Java

**Paradigmi:** OOP principalmente
**Tipizzazione:** Statica, forte
**Gestione memoria:** Garbage collection

**Caratteristiche:**
- Write once, run anywhere (JVM)
- Fortemente tipizzato
- Enterprise-ready

**Uso:** Enterprise, Android, backend

```java
public class Calculator {
    public int add(int a, int b) {
        return a + b;
    }
}

// Generics
List<String> names = new ArrayList<>();

// Lambda (Java 8+)
names.forEach(name -> System.out.println(name));
```

### JavaScript

**Paradigmi:** Multi-paradigma
**Tipizzazione:** Dinamica, debole
**Gestione memoria:** Garbage collection

**Caratteristiche:**
- Linguaggio del web
- Event-driven
- Asincrono (Promises, async/await)

**Uso:** Frontend, backend (Node.js), mobile (React Native)

```javascript
// Funzionale
const square = x => x * x;
[1, 2, 3].map(square);  // [1, 4, 9]

// OOP
class Person {
    constructor(name) {
        this.name = name;
    }
    greet() {
        return `Hello, ${this.name}`;
    }
}

// Async
async function fetchData() {
    const response = await fetch(url);
    return await response.json();
}
```

### Rust

**Paradigmi:** Multi-paradigma, focus su systems programming
**Tipizzazione:** Statica, forte
**Gestione memoria:** Ownership system (no GC)

**Caratteristiche:**
- Memory safety senza GC
- Zero-cost abstractions
- Concurrency sicura

**Uso:** Systems programming, embedded, WebAssembly

```rust
fn main() {
    let numbers = vec![1, 2, 3, 4, 5];
    
    // Ownership
    let s1 = String::from("hello");
    let s2 = s1;  // s1 non più valido (moved)
    
    // Borrowing
    let len = calculate_length(&s2);
    
    // Pattern matching
    match numbers.get(0) {
        Some(first) => println!("First: {}", first),
        None => println!("Empty"),
    }
}
```

### Go

**Paradigmi:** Imperativo, concorrenza
**Tipizzazione:** Statica
**Gestione memoria:** Garbage collection

**Caratteristiche:**
- Semplicità
- Goroutines (concurrency)
- Compilazione veloce

**Uso:** Backend, microservizi, cloud

```go
func main() {
    // Goroutines
    go func() {
        fmt.Println("Concurrent")
    }()
    
    // Channels
    ch := make(chan int)
    go func() {
        ch <- 42
    }()
    value := <-ch
}
```

### Haskell

**Paradigmi:** Funzionale puro
**Tipizzazione:** Statica, forte, inferenza
**Gestione memoria:** Garbage collection

**Caratteristiche:**
- Lazy evaluation
- Purezza (no side effects)
- Type system avanzato

**Uso:** Ricerca, finanza, compilatori

```haskell
-- Funzioni pure
factorial :: Integer -> Integer
factorial 0 = 1
factorial n = n * factorial (n - 1)

-- Lazy evaluation
infiniteList = [1..]
take 5 infiniteList  -- [1,2,3,4,5]

-- Monads
main :: IO ()
main = do
    putStrLn "Enter name:"
    name <- getLine
    putStrLn ("Hello, " ++ name)
```

---

## 🎯 Domande Tipiche per Esame

### 1. Differenza tra OOP e Programmazione Funzionale

**Risposta:**

**OOP:**
- Stato mutabile in oggetti
- Metodi modificano stato
- Ereditarietà per riuso
- **Esempio:** Java, C++

**Funzionale:**
- Immutabilità
- Funzioni pure (no side effects)
- Composizione funzioni
- **Esempio:** Haskell, Lisp

**Quando usare:**
- OOP: Modellazione domini complessi, GUI
- Funzionale: Concurrency, data transformation, testing

### 2. Cos'è il polimorfismo?

**Risposta:**
Capacità di trattare oggetti di classi diverse attraverso interfaccia comune.

**Tipi:**

**Polimorfismo di sottotipo (OOP):**
```python
def make_sound(animal):
    print(animal.speak())

make_sound(Dog())  # "Woof!"
make_sound(Cat())  # "Meow!"
```

**Polimorfismo parametrico (Generics):**
```java
<T> T identity(T value) {
    return value;
}
```

**Benefici:** Riusabilità, estensibilità, manutenibilità

### 3. Cosa sono le funzioni pure?

**Risposta:**
Funzioni che:
1. Stesso input → stesso output (deterministiche)
2. No side effects (no modifica stato esterno)

**Esempio puro:**
```python
def add(a, b):
    return a + b  # ✅ Puro
```

**Esempio impuro:**
```python
total = 0
def add_to_total(x):
    global total
    total += x  # ❌ Modifica stato globale
```

**Vantaggi:**
- Facili da testare
- Parallelizzabili
- Memoizzabili

### 4. Differenza tra tipizzazione statica e dinamica

**Risposta:**

**Statica (Java, C++, Rust):**
- Tipi verificati a compile-time
- Errori catturati prima dell'esecuzione
- Performance migliori
- Più verboso

```java
int x = 5;
x = "hello";  // ❌ Errore compile-time
```

**Dinamica (Python, JavaScript):**
- Tipi verificati a runtime
- Più flessibile
- Meno verboso
- Errori solo a runtime

```python
x = 5
x = "hello"  # ✅ OK
```

### 5. Cos'è il garbage collection?

**Risposta:**
Gestione automatica della memoria che libera oggetti non più referenziati.

**Come funziona:**
1. Mark: Identifica oggetti raggiungibili
2. Sweep: Libera oggetti non raggiungibili

**Pro:**
- ✅ No memory leaks
- ✅ No dangling pointers
- ✅ Più sicuro

**Contro:**
- ❌ Pause (stop-the-world)
- ❌ Overhead performance
- ❌ Meno controllo

**Linguaggi:**
- Con GC: Java, Python, JavaScript, Go
- Senza GC: C, C++, Rust (ownership)

---

## 📊 Tabella Comparativa Linguaggi

| Linguaggio | Paradigma | Tipizzazione | GC | Performance | Uso Principale |
|------------|-----------|--------------|----|-----------|----|
| Python | Multi | Dinamica | Sì | Media | Data science, scripting |
| Java | OOP | Statica | Sì | Alta | Enterprise, Android |
| JavaScript | Multi | Dinamica | Sì | Media | Web frontend/backend |
| C++ | Multi | Statica | No | Altissima | Games, systems |
| Rust | Multi | Statica | No | Altissima | Systems, safety-critical |
| Go | Imperativo | Statica | Sì | Alta | Backend, cloud |
| Haskell | Funzionale | Statica | Sì | Alta | Ricerca, finanza |

---

## 🔗 Collegamenti

- **Precedente:** [Architettura Elaboratori e SO](architettura-elaboratori-so.md)
- **Successivo:** [Caratteristiche del Software](caratteristiche-software.md)
- **Correlato:** [Algoritmi e Strutture Dati](algoritmi-strutture-dati.md)
