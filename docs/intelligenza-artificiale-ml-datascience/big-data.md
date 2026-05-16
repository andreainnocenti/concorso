# Big Data: Architetture e Tecniche

## 📖 Introduzione

**Big Data** si riferisce a dataset così grandi e complessi che i tradizionali strumenti di elaborazione dati non sono sufficienti. Richiede architetture distribuite e tecniche specializzate.

---

## 📊 Le 5 V del Big Data

### 1. Volume
- **Scala**: Terabyte, Petabyte, Exabyte
- **Esempio**: Facebook genera 4 petabyte/giorno
- **Sfida**: Storage e processing distribuito

### 2. Velocity
- **Velocità**: Dati generati e processati in tempo reale
- **Esempio**: Transazioni finanziarie, IoT sensors
- **Sfida**: Stream processing, low latency

### 3. Variety
- **Tipi**: Strutturati (SQL), semi-strutturati (JSON), non strutturati (testo, immagini)
- **Esempio**: Log, social media, video
- **Sfida**: Schema flessibile, data integration

### 4. Veracity
- **Qualità**: Accuratezza, affidabilità dei dati
- **Esempio**: Dati mancanti, rumore, bias
- **Sfida**: Data cleaning, validation

### 5. Value
- **Valore**: Estrarre insights utili
- **Esempio**: Recommendation, fraud detection
- **Sfida**: Analytics, ML su larga scala

---

## 🐘 Hadoop Ecosystem

### HDFS (Hadoop Distributed File System)

**Architettura:**
```
NameNode (master)
    ↓
DataNode 1 | DataNode 2 | DataNode 3
  Block A1  |  Block A2  |  Block A3
  Block B1  |  Block B2  |  Block B3
```

**Caratteristiche:**
- File divisi in blocchi (128 MB default)
- Replicazione (3 copie default)
- Fault tolerance
- Write-once, read-many

### MapReduce

**Paradigma di programmazione distribuita:**

```
Input → Map → Shuffle & Sort → Reduce → Output
```

**Esempio: Word Count**
```python
# Map phase
def map(document):
    for word in document.split():
        emit(word, 1)

# Reduce phase
def reduce(word, counts):
    emit(word, sum(counts))
```

**Processo:**
1. **Map**: Processa input, emette coppie (key, value)
2. **Shuffle**: Raggruppa per chiave
3. **Reduce**: Aggrega valori per chiave

**Limitazioni:**
- Lento (scrittura su disco)
- Solo batch processing
- Difficile da programmare

---

## ⚡ Apache Spark

### Architettura

**Componenti:**
- **Driver**: Coordina esecuzione
- **Executors**: Eseguono task
- **Cluster Manager**: Gestisce risorse (YARN, Mesos, K8s)

**RDD (Resilient Distributed Dataset):**
- Collezione distribuita immutabile
- Lazy evaluation
- Fault tolerance tramite lineage

### Spark vs MapReduce

| Aspetto | MapReduce | Spark |
|---------|-----------|-------|
| Velocità | Lento (disk I/O) | 100x più veloce (in-memory) |
| Facilità | Complesso | API semplici |
| Streaming | No | Sì (Spark Streaming) |
| ML | Difficile | MLlib integrato |

### Spark APIs

**RDD (low-level):**
```python
from pyspark import SparkContext

sc = SparkContext("local", "WordCount")
text = sc.textFile("file.txt")
counts = text.flatMap(lambda line: line.split()) \
             .map(lambda word: (word, 1)) \
             .reduceByKey(lambda a, b: a + b)
counts.saveAsTextFile("output")
```

**DataFrame (high-level):**
```python
from pyspark.sql import SparkSession

spark = SparkSession.builder.appName("Analysis").getOrCreate()
df = spark.read.json("data.json")

df.filter(df.age > 21) \
  .groupBy("city") \
  .count() \
  .show()
```

**Spark SQL:**
```python
df.createOrReplaceTempView("users")
result = spark.sql("SELECT city, COUNT(*) FROM users WHERE age > 21 GROUP BY city")
```

---

## 🌊 Stream Processing

### Apache Kafka

**Architettura:**
```
Producer → Kafka Broker (Topics/Partitions) → Consumer
```

**Caratteristiche:**
- Pub/Sub messaging
- Persistenza su disco
- Scalabilità orizzontale
- Throughput elevato (milioni msg/sec)

**Esempio:**
```python
from kafka import KafkaProducer, KafkaConsumer

# Producer
producer = KafkaProducer(bootstrap_servers='localhost:9092')
producer.send('topic', b'message')

# Consumer
consumer = KafkaConsumer('topic', bootstrap_servers='localhost:9092')
for message in consumer:
    print(message.value)
```

### Apache Flink

**Stream processing nativo:**
- Event time processing
- Exactly-once semantics
- Low latency (<100ms)
- Stateful computations

**Esempio:**
```python
from pyflink.datastream import StreamExecutionEnvironment

env = StreamExecutionEnvironment.get_execution_environment()
ds = env.from_collection([1, 2, 3, 4, 5])
ds.map(lambda x: x * 2).print()
env.execute()
```

---

## 🏗️ Architetture Big Data

### Lambda Architecture

**Tre layer:**
```
Batch Layer (Hadoop) → Batch Views
    ↓
Speed Layer (Storm/Flink) → Real-time Views
    ↓
Serving Layer → Query
```

**Pro:** Accuratezza (batch) + Latenza bassa (speed)
**Contro:** Complessità (due codebase)

### Kappa Architecture

**Semplificazione di Lambda:**
```
Stream Processing (Kafka + Flink) → Views → Query
```

**Pro:** Una sola pipeline, più semplice
**Contro:** Riprocessing più complesso

---

## 💾 Data Lakes vs Data Warehouses

### Data Warehouse

**Caratteristiche:**
- Schema-on-write
- Dati strutturati
- ETL (Extract, Transform, Load)
- Query SQL
- Costoso

**Esempio:** Amazon Redshift, Snowflake

**Uso:** Business intelligence, reporting

### Data Lake

**Caratteristiche:**
- Schema-on-read
- Tutti i tipi di dati (raw)
- ELT (Extract, Load, Transform)
- Economico (object storage)

**Esempio:** AWS S3 + Athena, Azure Data Lake

**Uso:** ML, data science, exploratory analysis

### Data Lakehouse

**Combina entrambi:**
- Flessibilità data lake
- Performance data warehouse
- ACID transactions
- Schema enforcement

**Esempio:** Databricks Delta Lake, Apache Iceberg

---

## 🎯 Domande Tipiche per Esame

### 1. Cosa sono le 5 V del Big Data?

**Risposta:**
- **Volume**: Quantità enorme di dati (petabyte)
- **Velocity**: Velocità generazione/processing
- **Variety**: Diversi tipi (strutturati, semi, non strutturati)
- **Veracity**: Qualità e affidabilità
- **Value**: Estrarre insights utili

### 2. Differenza tra Hadoop e Spark?

**Risposta:**

**Hadoop MapReduce:**
- Disk-based (lento)
- Solo batch processing
- Complesso da programmare

**Spark:**
- In-memory (100x più veloce)
- Batch + streaming
- API semplici (Python, Scala, Java)
- MLlib per machine learning

**Quando usare:**
- Hadoop: Batch processing su dati enormi, budget limitato
- Spark: Iterative algorithms, ML, streaming, velocità

### 3. Cos'è un Data Lake?

**Risposta:**
Repository centralizzato che memorizza dati raw in formato nativo.

**Caratteristiche:**
- Schema-on-read (flessibile)
- Tutti i tipi di dati
- Economico (object storage)
- Scalabile

**vs Data Warehouse:**
- Warehouse: Schema-on-write, solo strutturati, costoso
- Lake: Schema-on-read, tutti i tipi, economico

**Uso:** ML, data science, exploratory analysis

### 4. Come funziona MapReduce?

**Risposta:**

**Fasi:**
1. **Map**: Processa input, emette (key, value)
2. **Shuffle**: Raggruppa per chiave
3. **Reduce**: Aggrega valori

**Esempio Word Count:**
```
Input: "hello world hello"

Map: 
  ("hello", 1), ("world", 1), ("hello", 1)

Shuffle:
  "hello" → [1, 1]
  "world" → [1]

Reduce:
  ("hello", 2), ("world", 1)
```

### 5. Cos'è Kafka?

**Risposta:**
Piattaforma di streaming distribuita per pub/sub messaging.

**Caratteristiche:**
- Persistenza su disco
- Scalabilità orizzontale
- Throughput elevato
- Fault tolerance

**Componenti:**
- **Producer**: Pubblica messaggi
- **Broker**: Memorizza messaggi in topics
- **Consumer**: Legge messaggi

**Uso:** Event streaming, log aggregation, real-time analytics

---

## 📊 Tabella Comparativa Tecnologie

| Tecnologia | Tipo | Latenza | Throughput | Uso |
|------------|------|---------|------------|-----|
| Hadoop MapReduce | Batch | Alta (minuti) | Alto | Batch processing |
| Spark | Batch/Stream | Media (secondi) | Molto alto | ML, analytics |
| Flink | Stream | Bassa (<100ms) | Alto | Real-time |
| Kafka | Messaging | Bassa | Altissimo | Event streaming |
| Storm | Stream | Bassa | Medio | Real-time processing |

---

## 🔗 Collegamenti

- **Precedente:** [Foundation Models e LLM](foundation-models-llm.md)
- **Successivo:** [Data Mining e Visualization](data-mining-visualization.md)
- **Correlato:** [Basi di Dati](../computazione-software-sistemi/basi-dati.md)
