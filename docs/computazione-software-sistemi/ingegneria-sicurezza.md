# Ingegneria e Sicurezza del Software

## 📖 Introduzione

L'**ingegneria del software** fornisce metodologie sistematiche per sviluppare software di qualità, mentre la **sicurezza del software** si occupa di proteggere applicazioni da minacce e vulnerabilità.

**Obiettivi ingegneria software:**
- Qualità: Affidabilità, manutenibilità, efficienza
- Tempi e costi prevedibili
- Scalabilità e manutenibilità
- Soddisfazione requisiti utente

---

## 🔄 Ciclo di Vita del Software (SDLC)

### Fasi Principali

**1. Requisiti:**
- Raccolta (interviste, questionari, osservazione)
- Analisi (funzionali vs non-funzionali)
- Specifica (documento requisiti)
- Validazione (review con stakeholder)

**2. Design:**
- Architettura sistema (componenti, interfacce)
- Design dettagliato (algoritmi, strutture dati)
- Design pattern (MVC, Observer, Factory)
- Database schema

**3. Implementazione:**
- Coding standards
- Version control (Git)
- Code review
- Pair programming

**4. Testing:**
- Unit testing
- Integration testing
- System testing
- Acceptance testing

**5. Deployment:**
- Release planning
- Deployment automation
- Rollback strategy
- Monitoring

**6. Manutenzione:**
- Bug fixing
- Feature enhancement
- Performance optimization
- Security patches

### Modelli SDLC

**Waterfall (Cascata):**
```
Requisiti → Design → Implementazione → Testing → Deployment → Manutenzione
```
- ✅ Semplice, ben documentato
- ❌ Rigido, difficile gestire cambiamenti

**Iterativo/Incrementale:**
- Sviluppo in cicli
- Ogni iterazione produce versione funzionante
- ✅ Flessibile, feedback continuo

**Spiral:**
- Combina iterativo + risk analysis
- Ogni ciclo: pianificazione → risk analysis → engineering → evaluation

---

## 🏃 Metodologie Agile

### Manifesto Agile

**Valori:**
1. Individui e interazioni > processi e strumenti
2. Software funzionante > documentazione esaustiva
3. Collaborazione con cliente > negoziazione contratti
4. Rispondere al cambiamento > seguire un piano

**Principi chiave:**
- Consegne frequenti di software funzionante
- Accogliere cambiamenti anche tardivi
- Collaborazione quotidiana business-developers
- Team auto-organizzati
- Riflessione e adattamento continui

### Scrum

**Ruoli:**
- **Product Owner**: Definisce priorità, gestisce backlog
- **Scrum Master**: Facilita processo, rimuove impedimenti
- **Development Team**: Auto-organizzato, cross-functional

**Artefatti:**
- **Product Backlog**: Lista prioritizzata features
- **Sprint Backlog**: Task per sprint corrente
- **Increment**: Software potenzialmente rilasciabile

**Eventi:**
```
Sprint Planning (inizio sprint)
    ↓
Daily Standup (ogni giorno, 15 min)
    ↓
Sprint Review (demo a stakeholder)
    ↓
Sprint Retrospective (miglioramento processo)
```

**Sprint:**
- Durata fissa: 1-4 settimane (tipicamente 2)
- Goal definito
- No cambiamenti che compromettono goal

**Daily Standup:**
- Cosa ho fatto ieri?
- Cosa farò oggi?
- Ci sono impedimenti?

### Kanban

**Principi:**
- Visualizza workflow
- Limita WIP (Work In Progress)
- Gestisci flusso
- Rendi esplicite le policy
- Migliora collaborativamente

**Board Kanban:**
```
| To Do | In Progress (WIP: 3) | Review | Done |
|-------|---------------------|--------|------|
| Task1 | Task2               | Task5  | Task7|
| Task3 | Task4               |        | Task8|
| Task6 |                     |        |      |
```

**Metriche:**
- **Lead Time**: Tempo da richiesta a completamento
- **Cycle Time**: Tempo da inizio lavoro a completamento
- **Throughput**: Task completati per unità tempo

### XP (Extreme Programming)

**Pratiche:**
- **Pair Programming**: Due sviluppatori, una workstation
- **TDD**: Test-Driven Development
- **Continuous Integration**: Integrazione frequente
- **Refactoring**: Miglioramento continuo codice
- **Simple Design**: YAGNI (You Aren't Gonna Need It)

---

## 🔧 DevOps

### Concetto

**DevOps** = Development + Operations

Cultura e pratiche che uniscono sviluppo e operations per:
- Velocizzare delivery
- Migliorare qualità
- Aumentare affidabilità

### Continuous Integration (CI)

**Processo:**
```
Developer → Commit → Build → Test → Report
```

**Esempio con GitHub Actions:**
```yaml
name: CI
on: [push, pull_request]
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Run tests
        run: |
          npm install
          npm test
```

**Benefici:**
- Rileva bug presto
- Riduce integration hell
- Feedback rapido

### Continuous Delivery/Deployment (CD)

**Continuous Delivery:**
- Software sempre in stato rilasciabile
- Deploy manuale

**Continuous Deployment:**
- Deploy automatico in produzione
- Ogni commit che passa test → produzione

**Pipeline CD:**
```
Code → Build → Test → Stage → Production
```

### Infrastructure as Code (IaC)

Gestione infrastruttura tramite codice.

**Terraform:**
```hcl
resource "aws_instance" "web" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
  
  tags = {
    Name = "WebServer"
  }
}
```

**Docker:**
```dockerfile
FROM python:3.9
WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt
COPY . .
CMD ["python", "app.py"]
```

**Kubernetes:**
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: web-app
spec:
  replicas: 3
  selector:
    matchLabels:
      app: web
  template:
    metadata:
      labels:
        app: web
    spec:
      containers:
      - name: web
        image: myapp:1.0
        ports:
        - containerPort: 80
```

### Monitoring e Logging

**Metriche chiave:**
- **Uptime**: Disponibilità sistema
- **Response Time**: Tempo risposta
- **Error Rate**: Tasso errori
- **Throughput**: Richieste/secondo

**Tools:**
- Prometheus + Grafana (metriche)
- ELK Stack (Elasticsearch, Logstash, Kibana) per log
- Datadog, New Relic (APM)

---

## 🧪 Testing

### Piramide dei Test

```
        /\
       /  \  E2E Tests (pochi, lenti)
      /____\
     /      \
    / Integr.\  Integration Tests
   /__________\
  /            \
 /  Unit Tests  \  Unit Tests (molti, veloci)
/________________\
```

### Unit Testing

Test di singole unità (funzioni, metodi).

```python
import unittest

def add(a, b):
    return a + b

class TestMath(unittest.TestCase):
    def test_add(self):
        self.assertEqual(add(2, 3), 5)
        self.assertEqual(add(-1, 1), 0)
        self.assertEqual(add(0, 0), 0)

if __name__ == '__main__':
    unittest.main()
```

**Caratteristiche:**
- Veloci (millisecondi)
- Isolati (no dipendenze esterne)
- Ripetibili
- Self-checking

### Integration Testing

Test interazioni tra componenti.

```python
def test_database_integration():
    db = Database()
    user_service = UserService(db)
    
    user = user_service.create_user("test@example.com")
    retrieved = user_service.get_user(user.id)
    
    assert retrieved.email == "test@example.com"
```

### Test-Driven Development (TDD)

**Ciclo Red-Green-Refactor:**
```
1. Red: Scrivi test che fallisce
2. Green: Scrivi codice minimo per passare test
3. Refactor: Migliora codice mantenendo test verdi
4. Ripeti
```

**Esempio:**
```python
# 1. RED - Test fallisce
def test_calculate_discount():
    assert calculate_discount(100, 0.1) == 90

# 2. GREEN - Implementazione minima
def calculate_discount(price, discount):
    return price - (price * discount)

# 3. REFACTOR - Migliora
def calculate_discount(price, discount):
    if not 0 <= discount <= 1:
        raise ValueError("Discount must be between 0 and 1")
    return price * (1 - discount)
```

**Benefici:**
- Design migliore (testabilità)
- Meno bug
- Documentazione vivente
- Refactoring sicuro

---

## 🔒 Sicurezza del Software

### OWASP Top 10 (2021)

**1. Broken Access Control:**
- Utenti accedono a risorse non autorizzate
- **Prevenzione**: Implementa controlli accesso, principle of least privilege

**2. Cryptographic Failures:**
- Dati sensibili non protetti
- **Prevenzione**: Usa TLS, cifra dati sensibili, no algoritmi deboli

**3. Injection:**
- SQL, NoSQL, OS command injection
```python
# ❌ VULNERABILE
query = f"SELECT * FROM users WHERE id = {user_id}"

# ✅ SICURO
query = "SELECT * FROM users WHERE id = ?"
cursor.execute(query, (user_id,))
```

**4. Insecure Design:**
- Mancanza di security by design
- **Prevenzione**: Threat modeling, secure design patterns

**5. Security Misconfiguration:**
- Default credentials, errori esposti, servizi non necessari
- **Prevenzione**: Hardening, minimal install, automated config

**6. Vulnerable Components:**
- Librerie obsolete con vulnerabilità note
- **Prevenzione**: Dependency scanning, aggiornamenti regolari

**7. Authentication Failures:**
- Weak passwords, no MFA, session management debole
- **Prevenzione**: MFA, password policy, secure session management

**8. Software and Data Integrity Failures:**
- CI/CD insicuro, deserializzazione non sicura
- **Prevenzione**: Firma digitale, integrity checks

**9. Logging & Monitoring Failures:**
- Eventi critici non loggati
- **Prevenzione**: Log completi, alerting, SIEM

**10. Server-Side Request Forgery (SSRF):**
- Server fa richieste non validate
- **Prevenzione**: Whitelist URL, network segmentation

### Secure Coding Practices

**Input Validation:**
```python
import re

def validate_email(email):
    pattern = r'^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$'
    if not re.match(pattern, email):
        raise ValueError("Invalid email")
    return email
```

**Output Encoding:**
```python
from html import escape

def render_user_input(user_input):
    return f"<div>{escape(user_input)}</div>"
```

**Principle of Least Privilege:**
- Dare solo permessi minimi necessari
- Separazione dei privilegi

**Defense in Depth:**
- Multipli layer di sicurezza
- Se uno fallisce, altri proteggono

---

## 🎯 Domande Tipiche per Esame

### 1. Differenza tra Agile e Waterfall

**Risposta:**

**Waterfall:**
- Sequenziale: una fase dopo l'altra
- Requisiti fissi all'inizio
- Testing alla fine
- ✅ Semplice, ben documentato
- ❌ Rigido, difficile gestire cambiamenti

**Agile:**
- Iterativo: cicli brevi (sprint)
- Requisiti evolvono
- Testing continuo
- ✅ Flessibile, feedback rapido
- ❌ Meno documentazione, richiede disciplina

**Quando usare:**
- Waterfall: Requisiti chiari e stabili, progetti regolamentati
- Agile: Requisiti incerti, mercato dinamico

### 2. Cos'è DevOps?

**Risposta:**

Cultura e pratiche che uniscono Development e Operations per velocizzare delivery mantenendo qualità.

**Pilastri:**
1. **CI/CD**: Integrazione e deploy continui
2. **IaC**: Infrastruttura come codice
3. **Monitoring**: Osservabilità sistema
4. **Collaboration**: Team cross-functional

**Benefici:**
- Deploy più frequenti
- Meno fallimenti
- Recovery più veloce
- Time-to-market ridotto

### 3. Cos'è il TDD?

**Risposta:**

**Test-Driven Development**: Scrivi test PRIMA del codice.

**Ciclo:**
1. **Red**: Scrivi test che fallisce
2. **Green**: Scrivi codice minimo per passare
3. **Refactor**: Migliora codice

**Vantaggi:**
- Design migliore (testabile)
- Meno bug
- Refactoring sicuro
- Test come documentazione

**Sfide:**
- Curva apprendimento
- Più tempo iniziale (ma meno bug dopo)

### 4. Cos'è SQL Injection e come prevenirla?

**Risposta:**

Attacco dove input malevolo modifica query SQL.

**Esempio vulnerabile:**
```python
query = f"SELECT * FROM users WHERE username = '{username}'"
# Input: ' OR '1'='1
# Query: SELECT * FROM users WHERE username = '' OR '1'='1'
# Risultato: Tutti gli utenti!
```

**Prevenzione:**

1. **Prepared Statements:**
```python
cursor.execute("SELECT * FROM users WHERE username = ?", (username,))
```

2. **ORM:**
```python
User.objects.filter(username=username)
```

3. **Input Validation:**
```python
if not username.isalnum():
    raise ValueError("Invalid username")
```

### 5. Differenza tra CI e CD

**Risposta:**

**CI (Continuous Integration):**
- Integra codice frequentemente (più volte al giorno)
- Build e test automatici
- Rileva problemi presto

**CD (Continuous Delivery):**
- Software sempre in stato rilasciabile
- Deploy manuale in produzione

**CD (Continuous Deployment):**
- Deploy automatico in produzione
- Ogni commit che passa test → produzione

**Relazione:**
```
CI → CD (Delivery) → CD (Deployment)
```

---

## 📊 Tabella Riassuntiva

| Aspetto | Waterfall | Agile | DevOps |
|---------|-----------|-------|--------|
| **Approccio** | Sequenziale | Iterativo | Continuo |
| **Flessibilità** | Bassa | Alta | Alta |
| **Feedback** | Tardivo | Frequente | Real-time |
| **Testing** | Fase finale | Continuo | Automatizzato |
| **Deploy** | Raro | Frequente | Continuo |
| **Documentazione** | Estesa | Minima | Automatizzata |

---

## 🔗 Collegamenti

- **Precedente:** [Caratteristiche del Software](caratteristiche-software.md)
- **Successivo:** [Basi di Dati](basi-dati.md)
- **Correlato:** [Sistemi Distribuiti e Cloud](sistemi-distribuiti-cloud.md)
