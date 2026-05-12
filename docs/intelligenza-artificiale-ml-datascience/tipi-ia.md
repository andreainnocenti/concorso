# Tipi di Intelligenza Artificiale

## 📖 Introduzione

L'IA può essere classificata in base all'approccio e alle capacità.

## 🧠 IA Debole vs Forte

### IA Debole (Narrow AI)
Specializzata in compiti specifici.

**Esempi:**
- Riconoscimento vocale (Siri, Alexa)
- Raccomandazioni (Netflix, Spotify)
- Guida autonoma
- Traduzione automatica

### IA Forte (AGI - Artificial General Intelligence)
Intelligenza generale come quella umana.

**Status:** Ancora teorica

## 🎯 Approcci all'IA

### IA Simbolica (Deduttiva)

Ragionamento basato su regole e logica.

```prolog
% Esempio in Prolog
genitore(mario, luigi).
genitore(mario, anna).

fratello(X, Y) :- 
    genitore(Z, X),
    genitore(Z, Y),
    X \= Y.

?- fratello(luigi, anna).
% true
```

**Caratteristiche:**
- Interpretabile
- Richiede conoscenza esplicita
- Difficile con incertezza

### IA Subsimbolica (Induttiva)

Apprendimento dai dati.

```python
from sklearn.tree import DecisionTreeClassifier

# Apprende pattern dai dati
model = DecisionTreeClassifier()
model.fit(X_train, y_train)
predictions = model.predict(X_test)
```

**Caratteristiche:**
- Apprende automaticamente
- Gestisce incertezza
- Meno interpretabile

### IA Generativa

Crea nuovi contenuti.

**Esempi:**
- GPT (testo)
- DALL-E (immagini)
- MusicGen (musica)
- AlphaFold (strutture proteiche)

## 🔗 Collegamenti
- **Prossimo:** [Rappresentazione della Conoscenza](rappresentazione-conoscenza.md)
