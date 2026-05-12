# Basi di Dati Relazionali e NoSQL

## 📖 Introduzione

Le basi di dati sono sistemi per memorizzare, organizzare e recuperare dati in modo efficiente.

## 🗄️ Database Relazionali (SQL)

### Modello Relazionale
- Tabelle (relazioni)
- Righe (tuple)
- Colonne (attributi)
- Chiavi primarie e esterne

### SQL (Structured Query Language)

```sql
-- Creazione tabella
CREATE TABLE utenti (
    id INT PRIMARY KEY,
    nome VARCHAR(100),
    email VARCHAR(100) UNIQUE,
    data_registrazione DATE
);

-- Query
SELECT nome, email 
FROM utenti 
WHERE data_registrazione > '2024-01-01'
ORDER BY nome;

-- Join
SELECT o.id, u.nome, o.totale
FROM ordini o
JOIN utenti u ON o.utente_id = u.id;
```

### Normalizzazione
- **1NF**: valori atomici
- **2NF**: dipendenza funzionale completa
- **3NF**: nessuna dipendenza transitiva
- **BCNF**: forma normale di Boyce-Codd

### Transazioni ACID
- **Atomicity**: tutto o niente
- **Consistency**: stato valido
- **Isolation**: transazioni indipendenti
- **Durability**: persistenza

## 📊 Database NoSQL

### Tipi di NoSQL

#### Document Store (MongoDB, CouchDB)
```javascript
{
  "_id": "123",
  "nome": "Mario Rossi",
  "email": "mario@example.com",
  "ordini": [
    { "id": 1, "totale": 99.99 },
    { "id": 2, "totale": 149.99 }
  ]
}
```

#### Key-Value Store (Redis, DynamoDB)
```
SET user:123:name "Mario Rossi"
GET user:123:name
```

#### Column-Family (Cassandra, HBase)
Ottimizzato per query su colonne specifiche.

#### Graph Database (Neo4j, ArangoDB)
```cypher
// Neo4j Cypher
MATCH (u:User)-[:FRIEND]->(f:User)
WHERE u.name = 'Mario'
RETURN f.name
```

## ⚖️ CAP Theorem

Un sistema distribuito può garantire solo 2 su 3:
- **Consistency**: tutti vedono gli stessi dati
- **Availability**: ogni richiesta riceve risposta
- **Partition tolerance**: funziona anche con partizioni di rete

## 🔍 Quando Usare Cosa?

**SQL quando:**
- Dati strutturati
- Relazioni complesse
- Transazioni ACID necessarie

**NoSQL quando:**
- Dati non strutturati/semi-strutturati
- Scalabilità orizzontale
- Flessibilità dello schema

## 📚 Risorse

!!! tip "Database Popolari"
    - **SQL**: PostgreSQL, MySQL, Oracle, SQL Server
    - **NoSQL**: MongoDB, Redis, Cassandra, Neo4j

## 🔗 Collegamenti
- **Precedente:** [Ingegneria e Sicurezza](ingegneria-sicurezza.md)
- **Prossimo:** [Reti e Protocolli](reti-protocolli.md)
