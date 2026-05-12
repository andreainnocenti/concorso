# Rappresentazione della Conoscenza e Ragionamento

## 📖 Introduzione

Come rappresentare e manipolare la conoscenza in sistemi IA.

## 🧩 Logica

### Logica Proposizionale

```
P: "Piove"
Q: "Prendo l'ombrello"

P → Q  (Se piove, allora prendo l'ombrello)
P      (Piove)
---
Q      (Quindi prendo l'ombrello)
```

### Logica del Primo Ordine (FOL)

```
∀x (Umano(x) → Mortale(x))
Umano(Socrate)
---
Mortale(Socrate)
```

## 🕸️ Knowledge Graphs

Rappresentazione grafica della conoscenza.

```
(Mario) --[lavora_per]--> (Google)
(Mario) --[vive_in]--> (Milano)
(Milano) --[è_in]--> (Italia)
```

### RDF (Resource Description Framework)

```turtle
@prefix ex: <http://example.org/> .

ex:Mario ex:lavoraPer ex:Google .
ex:Mario ex:viveIn ex:Milano .
ex:Milano ex:èIn ex:Italia .
```

## 🦉 Ontologie

Definizioni formali di concetti e relazioni.

### OWL (Web Ontology Language)

```xml
<owl:Class rdf:ID="Persona"/>
<owl:Class rdf:ID="Studente">
  <rdfs:subClassOf rdf:resource="#Persona"/>
</owl:Class>
```

## 🔗 Collegamenti
- **Precedente:** [Tipi di IA](tipi-ia.md)
- **Prossimo:** [Agenti e Multiagente](agenti-multiagente.md)
