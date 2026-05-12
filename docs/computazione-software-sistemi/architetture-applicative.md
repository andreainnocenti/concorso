# Architetture Applicative

## 📖 Introduzione

Le architetture applicative definiscono la struttura e l'organizzazione di un'applicazione software.

## 🏛️ Pattern Architetturali

### Layered Architecture (A Livelli)

```
┌─────────────────────┐
│  Presentation Layer │
├─────────────────────┤
│   Business Layer    │
├─────────────────────┤
│ Data Access Layer   │
├─────────────────────┤
│     Database        │
└─────────────────────┘
```

### MVC (Model-View-Controller)

```
User → Controller → Model
         ↓           ↓
       View ←────────┘
```

### Clean Architecture

```
┌──────────────────────────┐
│      Frameworks & UI     │
├──────────────────────────┤
│   Interface Adapters     │
├──────────────────────────┤
│   Application Business   │
│        Rules             │
├──────────────────────────┤
│   Enterprise Business    │
│        Rules             │
└──────────────────────────┘
```

**Principi:**
- Indipendenza da framework
- Testabilità
- Indipendenza da UI
- Indipendenza da database

## 🔄 Event-Driven Architecture

### Componenti
- **Event Producers**: generano eventi
- **Event Consumers**: reagiscono agli eventi
- **Event Bus**: trasporta eventi

```python
# Esempio con pattern Observer
class EventBus:
    def __init__(self):
        self.subscribers = {}
    
    def subscribe(self, event_type, handler):
        if event_type not in self.subscribers:
            self.subscribers[event_type] = []
        self.subscribers[event_type].append(handler)
    
    def publish(self, event_type, data):
        if event_type in self.subscribers:
            for handler in self.subscribers[event_type]:
                handler(data)
```

## 📡 API Design

### REST (Representational State Transfer)

```http
GET    /api/users          # Lista utenti
GET    /api/users/123      # Dettaglio utente
POST   /api/users          # Crea utente
PUT    /api/users/123      # Aggiorna utente
DELETE /api/users/123      # Elimina utente
```

**Principi REST:**
- Stateless
- Cacheable
- Uniform interface
- Client-server separation

### GraphQL

```graphql
query {
  user(id: "123") {
    name
    email
    posts {
      title
      content
    }
  }
}
```

**Vantaggi:**
- Client specifica i dati necessari
- Singolo endpoint
- Strongly typed

### gRPC

```protobuf
service UserService {
  rpc GetUser (UserRequest) returns (UserResponse);
  rpc ListUsers (Empty) returns (stream UserResponse);
}
```

**Vantaggi:**
- Performance (Protocol Buffers)
- Streaming bidirezionale
- Code generation

## 🎯 CQRS (Command Query Responsibility Segregation)

Separazione tra operazioni di lettura e scrittura.

```
Commands (Write) → Write Model → Event Store
                                      ↓
                    Read Model ← Event Handlers
                         ↑
Queries (Read) ──────────┘
```

## 📦 Hexagonal Architecture (Ports & Adapters)

```
        ┌─────────────┐
        │   Domain    │
        │   Logic     │
        └──────┬──────┘
               │
    ┌──────────┼──────────┐
    │          │          │
  Port       Port       Port
    │          │          │
Adapter    Adapter    Adapter
  (DB)      (API)     (Queue)
```

## 🔗 Collegamenti
- **Precedente:** [Sistemi Distribuiti e Cloud](sistemi-distribuiti-cloud.md)
- **Sezione:** [Computazione, Software, Sistemi](index.md)
