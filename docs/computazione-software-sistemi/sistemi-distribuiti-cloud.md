# Sistemi Distribuiti, Cloud e Infrastrutture Software-Defined

## 📖 Introduzione

I sistemi distribuiti sono collezioni di computer indipendenti che appaiono agli utenti come un unico sistema coerente.

## 🌍 Principi dei Sistemi Distribuiti

### Caratteristiche
- **Trasparenza**: nasconde la distribuzione
- **Scalabilità**: cresce con il carico
- **Affidabilità**: tolleranza ai guasti
- **Consistenza**: dati sincronizzati

### Sfide
- Latenza di rete
- Guasti parziali
- Concorrenza
- Sincronizzazione

## ☁️ Cloud Computing

### Modelli di Servizio

**IaaS (Infrastructure as a Service)**
- Risorse virtuali (VM, storage, network)
- Esempi: AWS EC2, Azure VMs, Google Compute Engine

**PaaS (Platform as a Service)**
- Piattaforma per sviluppo
- Esempi: Heroku, Google App Engine, Azure App Service

**SaaS (Software as a Service)**
- Applicazioni pronte all'uso
- Esempi: Gmail, Salesforce, Office 365

### Modelli di Deployment
- **Public Cloud**: condiviso
- **Private Cloud**: dedicato
- **Hybrid Cloud**: mix
- **Multi-Cloud**: più provider

## 🐳 Containerizzazione

### Docker

```dockerfile
FROM python:3.11-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt
COPY . .
CMD ["python", "app.py"]
```

```bash
docker build -t myapp .
docker run -p 8000:8000 myapp
```

### Kubernetes

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp
spec:
  replicas: 3
  selector:
    matchLabels:
      app: myapp
  template:
    metadata:
      labels:
        app: myapp
    spec:
      containers:
      - name: myapp
        image: myapp:latest
        ports:
        - containerPort: 8000
```

## 🏗️ Microservizi vs Monoliti

### Monolite
- Singola applicazione
- Deploy unico
- Semplice inizialmente

### Microservizi
- Servizi indipendenti
- Deploy separati
- Scalabilità granulare
- Complessità operativa

## 🔧 Infrastructure as Code (IaC)

### Terraform

```hcl
resource "aws_instance" "web" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = "t2.micro"
  
  tags = {
    Name = "WebServer"
  }
}
```

## 📊 Patterns Distribuiti

- **Load Balancing**: distribuzione carico
- **Service Discovery**: localizzazione servizi
- **Circuit Breaker**: gestione fallimenti
- **API Gateway**: punto di ingresso unico
- **Event Sourcing**: log eventi
- **CQRS**: separazione lettura/scrittura

## 🔗 Collegamenti
- **Precedente:** [Reti e Protocolli](reti-protocolli.md)
- **Prossimo:** [Architetture Applicative](architetture-applicative.md)
