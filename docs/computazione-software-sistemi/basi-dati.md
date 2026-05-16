# Basi di Dati Relazionali e NoSQL

## 📖 Introduzione

Le **basi di dati** sono sistemi per memorizzare, organizzare e recuperare dati in modo efficiente e affidabile. Esistono due paradigmi principali: **relazionali (SQL)** e **NoSQL**.

---

## 🗄️ Database Relazionali (SQL)

### Modello Relazionale

Organizza dati in **tabelle** (relazioni) con:
- **Righe** (tuple/record): Singole entità
- **Colonne** (attributi): Proprietà delle entità
- **Chiavi primarie**: Identificatori univoci
- **Chiavi esterne**: Relazioni tra tabelle

**Esempio schema:**
```
Tabella: Utenti
+----+----------+-------------------+
| id | nome     | email             |
+----+----------+-------------------+
| 1  | Mario    | mario@example.com |
| 2  | Laura    | laura@example.com |
+----+----------+-------------------+

Tabella: Ordini
+----+-----------+--------+--------+
| id | utente_id | totale | data   |
+----+-----------+--------+--------+
| 1  | 1         | 99.99  | 2024-01|
| 2  | 1         | 149.99 | 2024-02|
+----+-----------+--------+--------+
```

### SQL (Structured Query Language)

**DDL (Data Definition Language):**
```sql
-- Creazione tabella
CREATE TABLE utenti (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nome VARCHAR(100) NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    data_registrazione TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    INDEX idx_email (email)
);

-- Modifica tabella
ALTER TABLE utenti ADD COLUMN telefono VARCHAR(20);

-- Eliminazione
DROP TABLE utenti;
```

**DML (Data Manipulation Language):**
```sql
-- INSERT
INSERT INTO utenti (nome, email) 
VALUES ('Mario Rossi', 'mario@example.com');

-- SELECT
SELECT nome, email 
FROM utenti 
WHERE data_registrazione > '2024-01-01'
ORDER BY nome
LIMIT 10;

-- UPDATE
UPDATE utenti 
SET email = 'nuovo@example.com' 
WHERE id = 1;

-- DELETE
DELETE FROM utenti WHERE id = 1;
```

**JOIN:**
```sql
-- INNER JOIN (solo match)
SELECT u.nome, o.totale, o.data
FROM utenti u
INNER JOIN ordini o ON u.id = o.utente_id;

-- LEFT JOIN (tutti utenti, anche senza ordini)
SELECT u.nome, COUNT(o.id) as num_ordini
FROM utenti u
LEFT JOIN ordini o ON u.id = o.utente_id
GROUP BY u.id, u.nome;

-- Subquery
SELECT nome 
FROM utenti 
WHERE id IN (
    SELECT utente_id 
    FROM ordini 
    WHERE totale > 100
);
```

**Aggregazioni:**
```sql
SELECT 
    categoria,
    COUNT(*) as num_prodotti,
    AVG(prezzo) as prezzo_medio,
    MAX(prezzo) as prezzo_max,
    MIN(prezzo) as prezzo_min
FROM prodotti
GROUP BY categoria
HAVING AVG(prezzo) > 50;
```

### Normalizzazione

Processo per ridurre ridondanza e anomalie.

**1NF (Prima Forma Normale):**
- Valori atomici (no liste)
- Ogni colonna ha tipo definito

```sql
-- ❌ Non 1NF
CREATE TABLE ordini (
    id INT,
    prodotti VARCHAR(255)  -- "Laptop, Mouse, Keyboard"
);

-- ✅ 1NF
CREATE TABLE ordini (id INT);
CREATE TABLE ordini_prodotti (
    ordine_id INT,
    prodotto_id INT
);
```

**2NF (Seconda Forma Normale):**
- 1NF + no dipendenze parziali
- Ogni attributo non-chiave dipende da tutta la chiave primaria

**3NF (Terza Forma Normale):**
- 2NF + no dipendenze transitive
- Attributi non-chiave dipendono solo dalla chiave primaria

**BCNF (Boyce-Codd):**
- 3NF + ogni determinante è chiave candidata

### Transazioni ACID

**Atomicity (Atomicità):**
- Tutto o niente
- Se una parte fallisce, rollback completo

```sql
START TRANSACTION;
UPDATE conti SET saldo = saldo - 100 WHERE id = 1;
UPDATE conti SET saldo = saldo + 100 WHERE id = 2;
COMMIT;  -- O ROLLBACK se errore
```

**Consistency (Consistenza):**
- Database passa da stato valido a stato valido
- Vincoli sempre rispettati

**Isolation (Isolamento):**
- Transazioni concorrenti non interferiscono
- Livelli: Read Uncommitted, Read Committed, Repeatable Read, Serializable

**Durability (Durabilità):**
- Commit permanente (anche con crash)
- Write-Ahead Logging (WAL)

### Indici

Strutture per velocizzare query.

```sql
-- Indice singolo
CREATE INDEX idx_email ON utenti(email);

-- Indice composto
CREATE INDEX idx_nome_cognome ON utenti(nome, cognome);

-- Indice unico
CREATE UNIQUE INDEX idx_email_unique ON utenti(email);

-- Full-text search
CREATE FULLTEXT INDEX idx_descrizione ON prodotti(descrizione);
```

**Quando usare:**
- ✅ Colonne in WHERE, JOIN, ORDER BY
- ✅ Chiavi esterne
- ❌ Tabelle piccole
- ❌ Colonne con pochi valori distinti
- ❌ Tabelle con molti INSERT/UPDATE (overhead)

---

## 📊 Database NoSQL

### Motivazioni

**Limiti SQL per alcuni scenari:**
- Schema rigido
- Scalabilità verticale (costosa)
- Performance con dati non strutturati

**Vantaggi NoSQL:**
- Schema flessibile
- Scalabilità orizzontale
- Performance con big data
- Modelli dati specializzati

### Tipi di NoSQL

#### 1. Document Store

Memorizza documenti (JSON, BSON).

**MongoDB:**
```javascript
// Inserimento
db.utenti.insertOne({
    nome: "Mario Rossi",
    email: "mario@example.com",
    indirizzo: {
        via: "Via Roma 1",
        città: "Milano"
    },
    ordini: [
        { id: 1, totale: 99.99, data: ISODate("2024-01-15") },
        { id: 2, totale: 149.99, data: ISODate("2024-02-20") }
    ],
    tags: ["premium", "attivo"]
});

// Query
db.utenti.find({
    "indirizzo.città": "Milano",
    "ordini.totale": { $gt: 100 }
});

// Aggregation
db.utenti.aggregate([
    { $match: { "ordini.totale": { $gt: 50 } } },
    { $unwind: "$ordini" },
    { $group: {
        _id: "$_id",
        totale_speso: { $sum: "$ordini.totale" }
    }}
]);
```

**Quando usare:**
- Dati semi-strutturati
- Schema evolve frequentemente
- Relazioni embedded (no JOIN complessi)

#### 2. Key-Value Store

Semplice: chiave → valore.

**Redis:**
```bash
# String
SET user:1:name "Mario Rossi"
GET user:1:name

# Hash
HSET user:1 name "Mario" email "mario@example.com"
HGETALL user:1

# List
LPUSH queue:tasks "task1" "task2"
RPOP queue:tasks

# Set
SADD user:1:tags "premium" "active"
SMEMBERS user:1:tags

# Sorted Set (con score)
ZADD leaderboard 100 "player1" 95 "player2"
ZRANGE leaderboard 0 -1 WITHSCORES

# Expiration
SETEX session:abc123 3600 "user_data"  # Scade dopo 1 ora
```

**Quando usare:**
- Cache
- Session storage
- Real-time analytics
- Leaderboards

#### 3. Column-Family Store

Ottimizzato per query su colonne.

**Cassandra:**
```cql
CREATE TABLE utenti (
    user_id UUID PRIMARY KEY,
    nome TEXT,
    email TEXT,
    created_at TIMESTAMP
);

CREATE TABLE ordini_per_utente (
    user_id UUID,
    order_id UUID,
    totale DECIMAL,
    data TIMESTAMP,
    PRIMARY KEY (user_id, data)
) WITH CLUSTERING ORDER BY (data DESC);

-- Query
SELECT * FROM ordini_per_utente 
WHERE user_id = 123e4567-e89b-12d3-a456-426614174000
AND data > '2024-01-01';
```

**Quando usare:**
- Time-series data
- Analytics su grandi dataset
- Write-heavy workloads

#### 4. Graph Database

Ottimizzato per relazioni.

**Neo4j (Cypher):**
```cypher
// Creazione nodi e relazioni
CREATE (m:User {name: 'Mario', email: 'mario@example.com'})
CREATE (l:User {name: 'Laura', email: 'laura@example.com'})
CREATE (m)-[:FRIEND]->(l)

// Query: Amici di amici
MATCH (u:User {name: 'Mario'})-[:FRIEND]->(friend)-[:FRIEND]->(fof)
WHERE NOT (u)-[:FRIEND]->(fof) AND u <> fof
RETURN DISTINCT fof.name

// Shortest path
MATCH path = shortestPath(
    (u1:User {name: 'Mario'})-[:FRIEND*]-(u2:User {name: 'Laura'})
)
RETURN path

// Recommendation
MATCH (u:User {name: 'Mario'})-[:LIKES]->(p:Product)<-[:LIKES]-(other)
MATCH (other)-[:LIKES]->(rec:Product)
WHERE NOT (u)-[:LIKES]->(rec)
RETURN rec.name, COUNT(*) as score
ORDER BY score DESC
LIMIT 5
```

**Quando usare:**
- Social networks
- Recommendation engines
- Fraud detection
- Knowledge graphs

---

## ⚖️ CAP Theorem

Un sistema distribuito può garantire solo **2 su 3**:

**C (Consistency):**
- Tutti i nodi vedono gli stessi dati
- Read dopo write vede update

**A (Availability):**
- Ogni richiesta riceve risposta (success/failure)
- Sistema sempre operativo

**P (Partition Tolerance):**
- Sistema funziona anche con partizioni di rete
- Nodi possono non comunicare

**Trade-offs:**
- **CP**: Consistency + Partition (MongoDB, HBase)
  - Sacrifica availability durante partizioni
- **AP**: Availability + Partition (Cassandra, DynamoDB)
  - Eventual consistency
- **CA**: Consistency + Availability (RDBMS tradizionali)
  - Non tollerante a partizioni (single-node)

**Esempio pratico:**
```
Scenario: Network partition separa datacenter

CP (MongoDB):
- Minority partition rifiuta write
- Garantisce consistency
- Sacrifica availability

AP (Cassandra):
- Entrambe partizioni accettano write
- Garantisce availability
- Eventual consistency (conflitti risolti dopo)
```

---

## 🎯 Domande Tipiche per Esame

### 1. Differenza tra SQL e NoSQL

**Risposta:**

**SQL (Relazionale):**
- Schema fisso (tabelle, colonne)
- ACID transactions
- JOIN per relazioni
- Scalabilità verticale
- **Quando**: Dati strutturati, relazioni complesse, transazioni critiche

**NoSQL:**
- Schema flessibile
- Eventual consistency (spesso)
- Denormalizzazione
- Scalabilità orizzontale
- **Quando**: Big data, schema evolve, scalabilità, performance

**Non è "o/o"**: Spesso si usano entrambi (polyglot persistence).

### 2. Cos'è una transazione ACID?

**Risposta:**

Insieme di operazioni che devono essere eseguite come unità atomica.

**A - Atomicity**: Tutto o niente
```sql
-- Trasferimento denaro
BEGIN;
UPDATE conti SET saldo = saldo - 100 WHERE id = 1;
UPDATE conti SET saldo = saldo + 100 WHERE id = 2;
COMMIT;  -- Entrambe o nessuna
```

**C - Consistency**: Vincoli sempre rispettati
- Saldo non negativo
- Chiavi esterne valide

**I - Isolation**: Transazioni concorrenti non interferiscono
- Livelli isolamento prevengono anomalie

**D - Durability**: Commit permanente
- Anche con crash sistema

### 3. Cos'è la normalizzazione?

**Risposta:**

Processo per organizzare dati riducendo ridondanza.

**Problema senza normalizzazione:**
```
Ordini: id | cliente_nome | cliente_email | prodotto | prezzo
```
- Ridondanza: cliente_nome ripetuto
- Anomalie update: cambiare email in un posto solo
- Anomalie delete: eliminare ordine perde info cliente

**Soluzione normalizzata:**
```
Clienti: id | nome | email
Ordini: id | cliente_id | prodotto | prezzo
```

**Forme normali:**
- 1NF: Valori atomici
- 2NF: No dipendenze parziali
- 3NF: No dipendenze transitive

### 4. Quando usare NoSQL invece di SQL?

**Risposta:**

**Usa NoSQL quando:**
1. **Schema flessibile**: Dati semi-strutturati, schema evolve
2. **Scalabilità orizzontale**: Milioni di utenti, petabyte dati
3. **Performance**: Letture/scritture massive
4. **Modello dati specifico**: Grafi, time-series, cache

**Usa SQL quando:**
1. **Relazioni complesse**: Molti JOIN
2. **Transazioni ACID**: Banking, e-commerce
3. **Query complesse**: Aggregazioni, analytics
4. **Schema stabile**: Dati strutturati

**Esempio:**
- E-commerce: SQL per ordini/pagamenti (ACID), NoSQL per catalogo prodotti (flessibilità)

### 5. Cos'è il CAP theorem?

**Risposta:**

Sistema distribuito può garantire solo 2 su 3:
- **C**onsistency: Tutti vedono stessi dati
- **A**vailability: Sempre risponde
- **P**artition tolerance: Funziona con network partition

**Trade-off:**
- **CP** (MongoDB): Consistency + Partition → Sacrifica availability
- **AP** (Cassandra): Availability + Partition → Eventual consistency
- **CA** (RDBMS): Consistency + Availability → No partition tolerance

**Pratica:** Sistemi distribuiti devono tollerare partizioni (P), quindi scegli tra C e A.

---

## 📊 Tabella Comparativa

| Aspetto | SQL | Document | Key-Value | Column | Graph |
|---------|-----|----------|-----------|--------|-------|
| **Schema** | Fisso | Flessibile | Nessuno | Flessibile | Flessibile |
| **Scalabilità** | Verticale | Orizzontale | Orizzontale | Orizzontale | Verticale |
| **Transazioni** | ACID | Limitate | Limitate | Limitate | ACID |
| **Query** | SQL | Query language | Get/Set | CQL | Cypher |
| **Relazioni** | JOIN | Embedded | No | No | Native |
| **Uso** | General | Semi-structured | Cache | Time-series | Networks |
| **Esempi** | PostgreSQL, MySQL | MongoDB | Redis | Cassandra | Neo4j |

---

## 🔗 Collegamenti

- **Precedente:** [Ingegneria e Sicurezza](ingegneria-sicurezza.md)
- **Successivo:** [Reti e Protocolli](reti-protocolli.md)
- **Correlato:** [Big Data](../intelligenza-artificiale-ml-datascience/big-data.md)
