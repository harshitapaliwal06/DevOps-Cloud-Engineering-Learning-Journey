# Day 04 — Cloud Computing & AWS Basics

## What I learned today:

### Traditional Infrastructure (On-Premises)
- Also called On-Premises Infra — own building/office, everything belongs to you
- You repair, you upgrade, you maintain

**Problems with traditional infra:**
- If you buy 50 servers but only 20 users are working, the remaining 30 sit idle — wasted money
- If suddenly traffic jumps to 10,000 users, the server can crash (not enough capacity)
- If a server dies — repair is your responsibility
- Maintenance, repairing, hardware failure — all your responsibility

### Solution: Cloud Computing
- Renting someone else's server (data center) instead of owning your own
- Delivery of computing services (server, storage, network, security) over the internet on a pay-as-you-go basis
- Everything is handled by the provider

**Advantages:**
- Scalability
- Pay for what you use
- Fast setup
- Maintenance is reduced
- Automatic backup
- Better disaster recovery

**Disadvantages:**
- Vendor lock-in
- Depends on internet connectivity
- Less control compared to owning your own infra

**Definition:** Cloud Computing is the practice of accessing and using computing resources over the internet rather than maintaining physical infrastructure yourself.

---

### Service Models — IaaS, PaaS, SaaS

For a website/application, you generally need:
- Application (your web app/code — e.g., Java, Python, Node.js)
- Runtime, Middleware
- OS
- VM
- Physical server, storage, networking

**Middleware** — acts as a bridge between different databases and applications.

#### IaaS (Infrastructure as a Service)
- AWS gives you a blank virtual computer (VM)
- AWS manages: physical server, storage, networking
- You manage: OS, data, application, runtime
- Example: renting an empty apartment — you handle everything inside it
- Example service: EC2
- Requires good Linux/networking knowledge to manage and maintain

#### PaaS (Platform as a Service)
- Provider installs and manages the runtime and OS
- You just have to write and deploy code
- Example: fully furnished apartment — just move in and use it
- Example service: Elastic Beanstalk
- Downsides: less control, platform restrictions, limited customization

#### SaaS (Software as a Service)
- Simply open the application and use it
- You just manage your own data and how you use the application
- Example: Gmail, Zoom

---

### Deployment Models (based on where the infra is located)

**Public Cloud**
- Owned and managed by a cloud provider
- Many customers share it, but each customer's data stays isolated and secure
- Providers: AWS, GCP, Azure

**Private Cloud**
- You build or rent dedicated infrastructure only for yourself
- Expensive
- Requires skilled people to manage

**Hybrid Cloud**
- Some workloads stay private, some run on public cloud
- Combines public + private together

**Multi-Cloud**
- Using more than one public cloud provider (no private infra involved)

---

### Virtualization
- One physical server (hardware) → many virtual computers (VMs)
- All VMs are managed by a **Hypervisor** — the "brain" of virtualization
- Hypervisor creates, starts, and stops VMs

**Types of Hypervisors:**

**Type 1 (Bare Metal)**
- Structure: App → VM → Hypervisor → Bare Metal Hardware
- Fast and secure (runs directly on hardware, no host OS in between)

**Type 2**
- Structure: App → VM → Hypervisor → Host OS → Bare Metal Hardware
- Slower and less secure (runs on top of a host OS)

---

### Pricing Model Basics
Cloud pricing is generally based on:
- **Compute** — processing power used
- **Storage** — data stored
- **Data Transfer** — how much data leaves/enters the network (network usage)

---
**Takeaway:** Covered core cloud computing concepts today — the problems with traditional on-premises infrastructure, how cloud computing solves them, the three service models (IaaS/PaaS/SaaS) with real-world analogies, deployment models (public/private/hybrid/multi-cloud), virtualization and hypervisor types, and the basics of cloud pricing. Strong foundation for understanding AWS and cloud infrastructure roles.
