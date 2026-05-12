# Reti e Protocolli di Comunicazione

## 📖 Introduzione

Le reti di computer permettono la comunicazione e la condivisione di risorse tra dispositivi.

## 🌐 Modello OSI e TCP/IP

### Modello OSI (7 livelli)
1. **Fisico**: trasmissione bit
2. **Data Link**: frame, MAC address
3. **Network**: routing, IP
4. **Transport**: TCP/UDP
5. **Session**: gestione sessioni
6. **Presentation**: formato dati
7. **Application**: applicazioni utente

### Stack TCP/IP (4 livelli)
1. **Network Access**
2. **Internet** (IP)
3. **Transport** (TCP/UDP)
4. **Application** (HTTP, FTP, DNS)

## 🔌 Protocolli Principali

### IP (Internet Protocol)
- IPv4: 32 bit (es. 192.168.1.1)
- IPv6: 128 bit (es. 2001:0db8::1)

### TCP vs UDP

**TCP (Transmission Control Protocol)**
- Orientato alla connessione
- Affidabile, ordinato
- Controllo di flusso
- Uso: HTTP, FTP, email

**UDP (User Datagram Protocol)**
- Senza connessione
- Veloce, non affidabile
- Uso: streaming, gaming, DNS

### HTTP/HTTPS

```http
GET /api/users HTTP/1.1
Host: example.com
Authorization: Bearer token123

HTTP/1.1 200 OK
Content-Type: application/json

{"users": [...]}
```

### DNS (Domain Name System)
```
www.example.com → 93.184.216.34
```

## 🔒 Sicurezza di Rete

### Firewall
Filtra traffico in entrata/uscita

### VPN (Virtual Private Network)
Tunnel crittografato per connessioni sicure

### TLS/SSL
Crittografia per HTTPS

## 📊 Quality of Service (QoS)

Prioritizzazione del traffico per garantire performance.

## 🔗 Collegamenti
- **Precedente:** [Basi di Dati](basi-dati.md)
- **Prossimo:** [Sistemi Distribuiti e Cloud](sistemi-distribuiti-cloud.md)
