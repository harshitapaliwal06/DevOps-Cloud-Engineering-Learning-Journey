# Day 02 — Networking Basic

## What I learned today:

### What is Networking & Why is it Needed?
- A **network** is formed when two or more devices connect to share information/resources
- **Example:** Suppose a company's office is located in Delhi, and other employees work from Jaipur. All employees need to use the same application, which requires access to the same application server. It's not physically possible for everyone to travel to where the server is kept.
- **Networking solves this through:**
  - Resource sharing
  - Shared storage
  - Communication
  - Centralized management

### Network Architectures
- **Client-Server** — clients send requests; a central server provides/manages responses
- **Peer-to-Peer (P2P)** — no dedicated server; all devices act as equals and share resources directly

### Types of Networks (by size/range)
- **PAN (Personal Area Network)** — very small range (1-10 meters), e.g., phone to earbuds
- **LAN (Local Area Network)** — single office/building, few meters to a few hundred meters
- **MAN (Metropolitan Area Network)** — city-wide, connects multiple offices in one city
- **WAN (Wide Area Network)** — large geographical area, e.g., the internet itself

### Internet 
- **Internet** - network of networks
- interconnected networks around the world

### Internet vs Intranet
- **Internet** — public network, accessible by everyone, no restriction on access, a network of networks
- **Intranet** — private network, restricted to a single organization's employees only

### How Data Travels (Path to the Internet)
- Local device → Router → ISP → Data Center → Router (destination)
- **ISP (Internet Service Provider)** — provides internet access and connects the local network to the wider internet
- If using mobile data: mobile device → mobile tower → core network → SIM authentication
- If using WiFi/laptop: device → modem → WiFi router → ISP → internet

---

### Network Devices (Hardware Components)

**Basic components:** Client, Server, Switch, Router, Transmission Medium, NIC

**NIC (Network Interface Card)**
- Connects a computer to a network
- Without NIC, no network connection is possible
- Sends, receives, and stores MAC address; converts signals
- **Wired NIC** — connects via physical Ethernet cable
- **Wireless NIC** — connects via WiFi/Bluetooth

**Hub**
- Connects multiple computers together in a network
- Receives data and copies/sends it to **all** connected devices (not just the intended one)
- The device that doesn't need the data simply discards it
- Causes **collisions** and shares bandwidth among all devices — inefficient

**Switch**
- Solves the Hub's problem — sends data only to the intended device
- Maintains a MAC address table to know exactly where to send data
- Smarter and more efficient than a Hub

**Router**
- Connects multiple networks together
- Directs data from local network to the internet (and vice versa)
- Uses IP addresses to decide the packet's route

**Modem**
- Connects to the Internet Service Provider
- Converts the ISP signal into a format usable by the local network

**Access Point**
- Creates a wireless LAN
- Connects to a wired router via Ethernet and broadcasts WiFi signals

**Repeater**
- Boosts/amplifies a signal so it can travel further

**Bridge**
- Connects two LAN segments together

**Gateway**
- Acts as a translator between different types of networks/protocols

---
**Takeaway:** Covered networking fundamentals — why networking exists, key architectures (client-server vs P2P), network types (PAN/LAN/MAN/WAN), internet vs intranet, how data travels via ISPs, and the core hardware devices (NIC, Hub, Switch, Router, Modem, Access Point, Repeater, Bridge, Gateway) that make networking possible.
