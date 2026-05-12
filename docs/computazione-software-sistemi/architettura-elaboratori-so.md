# Architettura degli Elaboratori e Sistemi Operativi

## 📖 Introduzione

L'architettura degli elaboratori studia come sono progettati e funzionano i computer a livello hardware, mentre i sistemi operativi gestiscono le risorse hardware e forniscono servizi alle applicazioni.

## 🎯 Obiettivi di Apprendimento

- ✅ Comprendere l'architettura von Neumann e le sue varianti
- ✅ Conoscere il funzionamento di CPU, memoria e I/O
- ✅ Capire come i sistemi operativi gestiscono processi e thread
- ✅ Comprendere la gestione della memoria virtuale
- ✅ Conoscere i principali file system e la gestione delle risorse

---

## 🖥️ Architettura degli Elaboratori

### Architettura von Neumann

!!! info "Componenti Principali"
    - **CPU (Central Processing Unit)**: esegue le istruzioni
    - **Memoria**: memorizza dati e programmi
    - **Bus**: collega i componenti
    - **I/O**: interfaccia con dispositivi esterni

### CPU e Pipeline

```
Fetch → Decode → Execute → Memory Access → Write Back
```

**Concetti chiave:**
- Instruction Set Architecture (ISA)
- RISC vs CISC
- Pipeline e hazards
- Cache memory (L1, L2, L3)
- Parallelismo a livello di istruzione

### Gerarchia della Memoria

```
Registri (più veloce, più costoso)
    ↓
Cache L1
    ↓
Cache L2
    ↓
Cache L3
    ↓
RAM
    ↓
Disco (più lento, meno costoso)
```

---

## 🔧 Sistemi Operativi

### Funzioni Principali

1. **Gestione dei Processi**
2. **Gestione della Memoria**
3. **Gestione del File System**
4. **Gestione dell'I/O**
5. **Sicurezza e Protezione**

### Processi e Thread

```python
# Esempio concettuale di processo
class Process:
    def __init__(self, pid):
        self.pid = pid
        self.state = "NEW"  # NEW, READY, RUNNING, WAITING, TERMINATED
        self.program_counter = 0
        self.registers = {}
        self.memory_space = {}
        self.open_files = []
```

**Stati di un Processo:**
- NEW: processo creato
- READY: pronto per l'esecuzione
- RUNNING: in esecuzione
- WAITING: in attesa di I/O
- TERMINATED: terminato

### Scheduling della CPU

**Algoritmi di Scheduling:**
- **FCFS** (First-Come, First-Served)
- **SJF** (Shortest Job First)
- **Round Robin**
- **Priority Scheduling**
- **Multilevel Queue**

### Gestione della Memoria

**Memoria Virtuale:**
- Paginazione
- Segmentazione
- TLB (Translation Lookaside Buffer)
- Page replacement algorithms (LRU, FIFO, Optimal)

```
Indirizzo Virtuale → MMU → Indirizzo Fisico
```

### Sincronizzazione

**Problemi classici:**
- Producer-Consumer
- Readers-Writers
- Dining Philosophers

**Meccanismi:**
- Semafori
- Mutex
- Monitor
- Condition Variables

```python
import threading

# Esempio di mutex
lock = threading.Lock()

def critical_section():
    lock.acquire()
    try:
        # Sezione critica
        pass
    finally:
        lock.release()
```

---

## 💾 File System

### Strutture Comuni

- **FAT32**: semplice, compatibile
- **NTFS**: Windows, journaling
- **ext4**: Linux, performante
- **APFS**: macOS, ottimizzato per SSD

### Operazioni su File

```c
// Esempio in C
FILE *fp = fopen("file.txt", "r");
if (fp != NULL) {
    char buffer[100];
    fread(buffer, sizeof(char), 100, fp);
    fclose(fp);
}
```

---

## 🔗 I/O e Dispositivi

### Gestione I/O

- **Polling**: CPU controlla continuamente
- **Interrupt**: dispositivo notifica la CPU
- **DMA** (Direct Memory Access): trasferimento diretto

---

## 📚 Risorse per Approfondire

!!! tip "Libri Consigliati"
    - **"Computer Systems: A Programmer's Perspective"** - Bryant, O'Hallaron
    - **"Operating System Concepts"** - Silberschatz, Galvin, Gagne
    - **"Modern Operating Systems"** - Andrew Tanenbaum

!!! info "Risorse Online"
    - [OSDev Wiki](https://wiki.osdev.org/)
    - [Linux Kernel Documentation](https://www.kernel.org/doc/)

---

## ✅ Checklist di Studio

- [ ] Comprendere l'architettura von Neumann
- [ ] Conoscere il funzionamento della pipeline CPU
- [ ] Capire la gerarchia della memoria
- [ ] Studiare gli algoritmi di scheduling
- [ ] Comprendere la memoria virtuale e paginazione
- [ ] Conoscere i meccanismi di sincronizzazione
- [ ] Studiare almeno 2 file system in dettaglio

---

## 🔗 Collegamenti

- **Precedente:** [Algoritmi e Strutture Dati](algoritmi-strutture-dati.md)
- **Prossimo:** [Paradigmi e Linguaggi di Programmazione](paradigmi-linguaggi.md)
- **Correlato:** [Sistemi Distribuiti e Cloud](sistemi-distribuiti-cloud.md)