# Reti e Protocolli di Comunicazione

## 📖 Introduzione

Le **reti di computer** permettono la comunicazione e la condivisione di risorse tra dispositivi. I **protocolli** definiscono regole standard per questa comunicazione.

---

## 🌐 Modello OSI e TCP/IP

### Modello OSI (7 livelli)

**Open Systems Interconnection** - modello teorico di riferimento.

| Livello | Nome | Funzione | Protocolli/Tecnologie |
|---------|------|----------|----------------------|
| 7 | **Application** | Interfaccia utente | HTTP, FTP, SMTP, DNS |
| 6 | **Presentation** | Formato dati, crittografia | SSL/TLS, JPEG, ASCII |
| 5 | **Session** | Gestione sessioni | NetBIOS, RPC |
| 4 | **Transport** | Consegna end-to-end | TCP, UDP |
| 3 | **Network** | Routing, indirizzamento | IP, ICMP, ARP |
| 2 | **Data Link** | Frame, MAC address | Ethernet, Wi-Fi, PPP |
| 1 | **Physical** | Trasmissione bit | Cavi, fibra, radio |

### Stack TCP/IP (4 livelli)

**Modello pratico** usato in Internet.

```
Application (HTTP, FTP, DNS)
    ↓
Transport (TCP, UDP)
    ↓
Internet (IP, ICMP)
    ↓
Network Access (Ethernet, Wi-Fi)
```

**Confronto:**
- OSI: Teorico, 7 livelli
- TCP/IP: Pratico, 4 livelli
- TCP/IP combina livelli OSI 5-6-7 in Application

---

## 🔌 Protocolli Principali

### IP (Internet Protocol)

**Funzione:** Indirizzamento e routing pacchetti.

**IPv4:**
- 32 bit (4 byte)
- Formato: `192.168.1.1`
- Spazio: ~4.3 miliardi indirizzi
- **Problema:** Esaurimento indirizzi

**Classi IPv4:**
```
Classe A: 0.0.0.0 - 127.255.255.255 (grandi reti)
Classe B: 128.0.0.0 - 191.255.255.255 (medie reti)
Classe C: 192.0.0.0 - 223.255.255.255 (piccole reti)
```

**IPv6:**
- 128 bit (16 byte)
- Formato: `2001:0db8:85a3:0000:0000:8a2e:0370:7334`
- Spazio: 340 undecilioni indirizzi
- **Vantaggi:** No NAT necessario, sicurezza integrata, auto-configurazione

**Subnetting:**
```
IP: 192.168.1.0/24
Subnet mask: 255.255.255.0
Network: 192.168.1.0
Broadcast: 192.168.1.255
Host range: 192.168.1.1 - 192.168.1.254
```

### TCP vs UDP

#### TCP (Transmission Control Protocol)

**Caratteristiche:**
- **Orientato alla connessione** (3-way handshake)
- **Affidabile**: Conferme (ACK), ritrasmissione
- **Ordinato**: Pacchetti in sequenza
- **Controllo di flusso**: Sliding window
- **Controllo congestione**: Slow start, congestion avoidance

**3-Way Handshake:**
```
Client → Server: SYN
Server → Client: SYN-ACK
Client → Server: ACK
[Connessione stabilita]
```

**Uso:** HTTP, HTTPS, FTP, SMTP, SSH

**Esempio Python:**
```python
import socket

# Server
server = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
server.bind(('localhost', 8080))
server.listen(1)
conn, addr = server.accept()
data = conn.recv(1024)
conn.send(b"Response")
conn.close()

# Client
client = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
client.connect(('localhost', 8080))
client.send(b"Request")
response = client.recv(1024)
client.close()
```

#### UDP (User Datagram Protocol)

**Caratteristiche:**
- **Senza connessione**: No handshake
- **Non affidabile**: No conferme, no ritrasmissione
- **Non ordinato**: Pacchetti possono arrivare fuori ordine
- **Veloce**: Meno overhead

**Uso:** DNS, DHCP, streaming video, gaming, VoIP

**Esempio Python:**
```python
import socket

# Server
server = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
server.bind(('localhost', 8080))
data, addr = server.recvfrom(1024)
server.sendto(b"Response", addr)

# Client
client = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
client.sendto(b"Request", ('localhost', 8080))
response, addr = client.recvfrom(1024)
```

**Confronto:**

| Aspetto | TCP | UDP |
|---------|-----|-----|
| Connessione | Sì | No |
| Affidabilità | Alta | Bassa |
| Velocità | Più lento | Più veloce |
| Overhead | Alto | Basso |
| Ordinamento | Sì | No |
| Uso | File transfer, web | Streaming, gaming |

### HTTP/HTTPS

**HTTP (HyperText Transfer Protocol):**

**Metodi:**
- **GET**: Recupera risorsa
- **POST**: Invia dati
- **PUT**: Aggiorna risorsa
- **DELETE**: Elimina risorsa
- **PATCH**: Modifica parziale

**Esempio richiesta:**
```http
GET /api/users/123 HTTP/1.1
Host: example.com
User-Agent: Mozilla/5.0
Accept: application/json
Authorization: Bearer token123

HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 85

{
  "id": 123,
  "name": "Mario Rossi",
  "email": "mario@example.com"
}
```

**Status codes:**
- **2xx**: Success (200 OK, 201 Created)
- **3xx**: Redirection (301 Moved, 304 Not Modified)
- **4xx**: Client error (400 Bad Request, 404 Not Found, 401 Unauthorized)
- **5xx**: Server error (500 Internal Error, 503 Service Unavailable)

**HTTP/2:**
- Multiplexing (più richieste su una connessione)
- Server push
- Header compression
- Binario (non testuale)

**HTTP/3:**
- Basato su QUIC (UDP)
- Più veloce
- Migliore su reti instabili

**HTTPS = HTTP + TLS/SSL:**
- Crittografia end-to-end
- Autenticazione server (certificati)
- Integrità dati

### DNS (Domain Name System)

**Funzione:** Traduce nomi dominio in indirizzi IP.

```
www.example.com → 93.184.216.34
```

**Gerarchia:**
```
. (root)
  ↓
.com (TLD - Top Level Domain)
  ↓
example.com (Second Level Domain)
  ↓
www.example.com (Subdomain)
```

**Processo risoluzione:**
```
1. Client → Local DNS cache
2. Client → Recursive DNS resolver
3. Resolver → Root DNS server
4. Resolver → TLD DNS server (.com)
5. Resolver → Authoritative DNS server (example.com)
6. Resolver → Client (con IP)
```

**Record types:**
- **A**: IPv4 address
- **AAAA**: IPv6 address
- **CNAME**: Alias
- **MX**: Mail server
- **TXT**: Text (SPF, DKIM)
- **NS**: Name server

**Esempio query:**
```bash
nslookup example.com
dig example.com
host example.com
```

---

## 🔒 Sicurezza di Rete

### Firewall

**Funzione:** Filtra traffico in entrata/uscita basato su regole.

**Tipi:**
- **Packet filtering**: Esamina header pacchetti
- **Stateful inspection**: Traccia connessioni
- **Application layer**: Esamina payload

**Esempio regole:**
```
ALLOW TCP port 80 (HTTP)
ALLOW TCP port 443 (HTTPS)
ALLOW TCP port 22 (SSH) from 192.168.1.0/24
DENY all other
```

### VPN (Virtual Private Network)

**Funzione:** Tunnel crittografato per connessioni sicure su rete pubblica.

**Tipi:**
- **Site-to-Site**: Connette reti
- **Remote Access**: Connette utenti remoti

**Protocolli:**
- **IPsec**: Layer 3, sicuro
- **OpenVPN**: SSL/TLS based
- **WireGuard**: Moderno, veloce

**Vantaggi:**
- Crittografia traffico
- Nasconde IP reale
- Accesso risorse remote

### TLS/SSL

**TLS (Transport Layer Security)** - successore di SSL.

**Handshake TLS:**
```
1. Client Hello (cipher suites supportati)
2. Server Hello (cipher scelto, certificato)
3. Client verifica certificato
4. Key exchange (ECDHE)
5. Finished (handshake completo)
6. Application data (cifrato)
```

**Cipher suite esempio:**
```
TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384
 │    │     │        │    │    │    │
 │    │     │        │    │    │    └─ Hash
 │    │     │        │    │    └────── Modo
 │    │     │        │    └─────────── Cifrario
 │    │     │        └──────────────── Lunghezza chiave
 │    │     └───────────────────────── Autenticazione
 │    └─────────────────────────────── Key exchange
 └──────────────────────────────────── Protocollo
```

---

## 📊 Quality of Service (QoS)

**Funzione:** Prioritizzazione traffico per garantire performance.

**Tecniche:**
- **Traffic shaping**: Limita bandwidth
- **Prioritization**: Code diverse per tipo traffico
- **Bandwidth reservation**: Garantisce minimo

**Classi traffico:**
1. **Voice**: Massima priorità (VoIP)
2. **Video**: Alta priorità (streaming)
3. **Data**: Media priorità (web)
4. **Best effort**: Bassa priorità (download)

---

## 🎯 Domande Tipiche per Esame

### 1. Differenza tra TCP e UDP

**Risposta:**

**TCP:**
- Orientato connessione (handshake)
- Affidabile (ACK, ritrasmissione)
- Ordinato
- Più lento
- **Uso:** Web, email, file transfer

**UDP:**
- Senza connessione
- Non affidabile
- Non ordinato
- Più veloce
- **Uso:** Streaming, gaming, DNS

**Quando usare:**
- TCP: Quando serve affidabilità (dati critici)
- UDP: Quando serve velocità (real-time)

### 2. Come funziona DNS?

**Risposta:**
DNS traduce nomi dominio in IP.

**Processo:**
1. Client chiede IP di `www.example.com`
2. Controlla cache locale
3. Chiede a recursive resolver
4. Resolver interroga:
   - Root server → TLD server (.com) → Authoritative server
5. Resolver restituisce IP al client
6. Client si connette all'IP

**Caching:** Riduce query, velocizza risoluzione

### 3. Cos'è HTTPS?

**Risposta:**
HTTPS = HTTP + TLS/SSL

**Fornisce:**
1. **Crittografia**: Dati cifrati in transito
2. **Autenticazione**: Certificato verifica identità server
3. **Integrità**: Rileva modifiche ai dati

**Handshake:**
- Client e server negoziano cipher
- Scambiano chiavi (ECDHE)
- Verificano certificato
- Comunicano cifrato

**Porta:** 443 (vs HTTP 80)

### 4. Differenza tra IPv4 e IPv6

**Risposta:**

**IPv4:**
- 32 bit (4 byte)
- Formato: `192.168.1.1`
- ~4.3 miliardi indirizzi
- **Problema:** Esaurimento

**IPv6:**
- 128 bit (16 byte)
- Formato: `2001:0db8::1`
- 340 undecilioni indirizzi
- **Vantaggi:**
  - No NAT necessario
  - IPsec integrato
  - Auto-configurazione
  - Header semplificato

**Transizione:** Dual stack, tunneling, NAT64

### 5. Cos'è un firewall?

**Risposta:**
Sistema che filtra traffico di rete basato su regole.

**Funzioni:**
- Blocca traffico non autorizzato
- Permette traffico legittimo
- Log e monitoring

**Tipi:**
1. **Packet filtering**: Esamina header (IP, porta)
2. **Stateful**: Traccia stato connessioni
3. **Application layer**: Esamina payload (DPI)

**Esempio regole:**
- ALLOW HTTP (80), HTTPS (443)
- ALLOW SSH (22) solo da rete interna
- DENY tutto il resto

---

## 📊 Tabella Riassuntiva Protocolli

| Protocollo | Livello OSI | Porta | Funzione | Transport |
|------------|-------------|-------|----------|-----------|
| HTTP | Application | 80 | Web | TCP |
| HTTPS | Application | 443 | Web sicuro | TCP |
| FTP | Application | 21 | File transfer | TCP |
| SSH | Application | 22 | Remote shell | TCP |
| SMTP | Application | 25 | Email invio | TCP |
| DNS | Application | 53 | Name resolution | UDP/TCP |
| DHCP | Application | 67/68 | IP assignment | UDP |
| TCP | Transport | - | Reliable transport | - |
| UDP | Transport | - | Fast transport | - |
| IP | Network | - | Routing | - |

---

## 🔗 Collegamenti

- **Precedente:** [Basi di Dati](basi-dati.md)
- **Successivo:** [Sistemi Distribuiti e Cloud](sistemi-distribuiti-cloud.md)
- **Correlato:** [Sicurezza Software](ingegneria-sicurezza.md)
