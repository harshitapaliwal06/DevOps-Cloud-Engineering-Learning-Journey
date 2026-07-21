# Day 7 — Networking Fundamentals

## What I learned today:

### What is Networking?
- Two or more devices connected together so that they can communicate (exchange data) using agreed protocols

### Network Types (by Range)

- **PAN (Personal Area Network)** — 1-10 meters
- **LAN (Local Area Network)** — limited to office/geographic area
- **MAN (Metropolitan Area Network)** — connects multiple LANs across a city or metropolitan area
- **WAN (Wide Area Network)** — large geographical distance

---

### Client-Server & Peer-to-Peer

**Client-Server:**
- **Client** — computer that requests a service
- **Server** — provides the service, always listens/supports open ports
- Flow: Client1 → Server, Client2 → Server (server simply waits for new incoming requests — called "listening")
- Example: Linux servers provide services like SSH, web hosting, etc.

**Peer-to-Peer:**
- Each device can both request and send — no dedicated server

---

### Protocols
- Rules for communication
- If everyone spoke a different language, no one would understand each other — protocols solve this for devices

---

### OSI Model
- OSI = Open System Interconnection Model (a reference model)
- Created to explain how data moves through a network
- A conceptual model — real networks use TCP/IP
- Has **7 layers**, each with a different role

**Layers (top to bottom):**
1. Application
2. Presentation
3. Session
4. Transport
5. Network
6. Data Link
7. Physical

**Layer details:**
- **Application Layer** — provides network services directly to end-user applications (e.g., browsers)
  - Protocols: HTTP, HTTPS, FTP, SMTP, DNS, SSH
- **Presentation Layer** — handles encryption, decryption, compression, and data formatting
- **Session Layer** — manages and maintains sessions between devices
- **Transport Layer** — TCP & UDP; ensures reliable data transfer, handles flow control
- **Network Layer** — handles IP addressing and routing
- **Data Link Layer** — handles MAC addressing, framing, error detection
- **Physical Layer** — actual transmission of bits (cables, signals)

---

### TCP/IP Model
- A simplified, practical model (4 layers): Application → Transport → Internet → Network Access

### Encapsulation & Decapsulation
- **Encapsulation** — adding protocol headers as data moves down the networking stack (sender side)
- **Decapsulation** — removing protocol headers as data moves up the stack after receiving (receiver side)

### PDU (Protocol Data Unit) — name changes per layer:
- Application — Data
- Transport — Segment
- Network — Packet
- Data Link — Frame
- Physical — Bits

---

### IP Address
- A logical address assigned to a device so it can be identified on a network
- No two devices on the same network can have the same IP
- If two devices have the same IP on the same network — it causes an **IP conflict**

**Static IP vs Dynamic IP:**
- **Static IP** — permanent, doesn't change (commonly used for servers)
- **Dynamic IP** — assigned by DHCP, changes over time (commonly used for laptops/phones)
- IP address = logical and permanent (can be configured/changed) address

### MAC Address
- MAC = Media Access Control address
- Physical address of a NIC (Network Interface Card)
- Cannot be changed (assigned by the manufacturer)
- Used by switches to identify devices on a LAN

---

### IPv4 Address
- 32 bits long
- 4 parts (octets), 8 bits each — e.g., `255.255.255.255`
- Each octet ranges from **0 to 255** (min-max possible because of 8 bits: 2^8 = 256 combinations)
- Limited number of addresses possible

---

### Public IP vs Private IP
- **Public IP** — globally unique address, assigned on the internet
- **Private IP** — used within an internal/local network, not routable on the internet
- **NAT (Network Address Translation)** — allows private IPs to communicate with the internet using a shared public IP

**Common Private IP ranges:**
- `10.0.0.0 – 10.255.255.255`
- `172.16.0.0 – 172.31.255.255`
- `192.168.0.0 – 192.168.255.255`

---
**Takeaway:** Covered core networking fundamentals today — network types by range, client-server vs peer-to-peer models, the OSI model's 7 layers with their roles, the TCP/IP model, encapsulation/decapsulation, IP addressing (static vs dynamic, public vs private), and MAC addresses. This forms the foundation for understanding how devices communicate across networks — a must-have skill for infrastructure and DevOps roles.
