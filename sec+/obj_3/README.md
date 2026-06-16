# Chapter 3 — Security Architecture (CompTIA Security+ SY0-701)


## Architecture Models (Objective 3.1)

The objective is to **compare and contrast the security implications** of different architecture models. This breaks into two halves: the **architecture & infrastructure concepts** (cloud, IaC, serverless, microservices, network designs, deployment models, specialized systems) and the **design considerations** you weigh for each (availability, cost, recovery, patching, and so on).


## Cloud Architecture

Cloud apps run as **IaaS (Infrastructure as a Service), PaaS (Platform as a Service), SaaS (Software as a Service)**, and other models. The central security question is always: **who is responsible for what?**

### Responsibility Matrix
A **shared responsibility matrix** (provided by the cloud provider) spells out who secures each layer.

- The split **shifts by service model** — e.g., for the **operating system**: the provider handles it under SaaS and PaaS, but the **customer** owns it under IaaS (and obviously on-premises).
- **Accounts and identities are almost always the customer's responsibility**, regardless of model — a major security concern you can't hand off.
- The matrix **varies by provider** and can be **modified by contract** to fit your needs.

### Hybrid Considerations
Using **multiple clouds across different providers** (a hybrid/multi-cloud setup) adds flexibility but also **complexity**:

- Providers **don't talk to each other directly**, and their systems work differently, so you often **configure each one separately**.
- That creates **mismatch risk** — authentication, server configs, and firewall settings can drift out of sync between providers.
- **Logs differ** — each provider uses different formats and terminology, making consolidation hard.
- Data routinely **moves between providers across the public internet**, so you must secure that **data in transit**.

### Third-Party Vendors
Beyond the cloud provider, you manage **third-party apps and devices** (e.g., a third-party firewall in front of your in-house app).

- Maintain a **vendor risk management policy**.
- Plan **incident response** that includes those third parties (not just your internal and provider processes).
- **Continuously monitor** third-party security and availability.


## Infrastructure as Code (IaC)

Defining infrastructure **as code rather than as physical hardware** — specifying the hosts to build, the web servers, the database servers, etc.

- Makes it easy to **build, modify, and rebuild** an infrastructure consistently.
- The same definition can **rebuild the instance on any provider at any time** — a core benefit of cloud.


## Serverless

A **serverless architecture** accesses **individual functions** (FaaS) instead of one monolithic application; each function handles a small piece.

- **Less emphasis on the OS** — each function runs on whatever OS is appropriate at the time.
- Functions **run only when needed**, built in real time and removed when done — **saving time and money**.
- Most processing happens **in the cloud**, so the **bulk of the security is in the cloud** too.


## Microservices

Breaking a **monolithic application** (one big executable handling UI, login, and business logic) into **separate services** accessed via **APIs**.

- The client talks to an **API gateway**, which routes requests to the right **microservice**.
- **Scalability** — spin up more instances of just the heavily-used services.
- **Resilience** — lose one microservice and the rest keeps working.
- **Per-service security** — apply controls appropriate to each service (authentication vs. database read/write).


## Network Infrastructure

### Physical Isolation / Air-Gapped
An **air gap** physically separates devices so there's **no connection** between them — compromise switch A and you still can't reach switch B.

- Used to separate, say, **web-server racks from database racks**, or to isolate **one MSP customer from another** on separate switches.
- **Doesn't scale** — 100 isolated customers would need 100 physical switches.

### Logical Segmentation (VLANs)
**VLANs (Virtual LANs)** segment a **single physical switch** into groups that **can't communicate directly** — the same effect as separate switches, but it **scales** with far less hardware.

### Software-Defined Networking (SDN)
SDN splits a network device into three **planes of operation** so they can be coded as software:

- **Data plane** — does the work of **forwarding traffic** (also NAT, encryption, trunking).
- **Control plane** — manages **how** to forward: routing tables, dynamic routing updates, NAT tables.
- **Management plane** — where you **configure** the device (SSH, SNMP, APIs).

This turns physical devices into **software** you can deploy in the cloud — letting you **dynamically add** load balancers, firewalls, etc. with a click.


## Deployment Models

### On-Premises vs. Cloud
| | On-premises | Cloud |
|---|---|---|
| **Control** | Complete — change things yourself | Provider-mediated |
| **Hardware/data center** | You buy, cool, staff, support it | None to support |
| **Security ownership** | Local, your trained (costlier) staff | Provider supplies it, centralized |
| **Adding capacity** | Cost + time to buy/configure/install | Fast, often programmatic |

The attacker **doesn't care where the security lives** — they work to get around it either way.

### Centralized vs. Decentralized
Most organizations are **decentralized** — multiple locations, multiple cloud providers, multiple operating systems — which is **hard to secure**.

- A **centralized management console** consolidates monitoring, alerts, and log analysis into one view.
- It **adds visibility** but becomes a **single point of failure** — lose the console and you lose visibility.
- As the org grows, that console needs **more storage** (logs) and **more CPU** (alerts).


## Virtualization vs. Containerization

### Virtualization
A **hypervisor** runs on the physical hardware and manages resources for multiple **virtual machines**, each with its **own guest OS** and applications.

- **Inefficiency:** even if every VM runs the *same* OS, you still run **separate full OS instances**.

### Containerization
**Containers** hold an application and its dependencies **but not the OS** — they **share the host operating system** via container software (e.g., **Docker**).

- Containers are **isolated** from one another; each app sees only its own container.
- **Efficient** to add, remove, or swap apps, since there's no separate OS per app.
- Apps are built for the **single host OS** (a Windows container host runs Windows apps; a Linux host runs Linux apps).

| | Virtualization | Containerization |
|---|---|---|
| **On the hardware** | Hypervisor | Host OS + container software (Docker) |
| **OS per workload** | Each VM has its own guest OS | All containers share the host OS |
| **Isolation** | VM-level | Container-level |
| **Efficiency** | Heavier (duplicate OSes) | Lighter, faster to swap |


## Specialized & Resilient Systems

- **IoT (Internet of Things)** — networked sensors and automation (thermostats, lighting, video doorbells, smartwatches, air-quality monitors). Convenient, but the makers **aren't necessarily security experts** — and a **single exploited IoT device can give an attacker access to your whole network**.
- **ICS / SCADA** — *Industrial Control Systems / Supervisory Control and Data Acquisition*, used in manufacturing and power generation. A central control room monitors and adjusts equipment remotely. These are **completely segmented from the outside** and are **among the most secure systems in the world**, given the stakes (e.g., power grids, refineries).
- **RTOS (Real-Time Operating System)** — a **deterministic** OS where one critical process can seize priority instantly (vs. the non-deterministic OS on your desktop). Used in cars (anti-lock brakes), military, and manufacturing. **Very security-sensitive** and typically **self-contained and hard to access**.
- **Embedded systems** — **purpose-built** hardware + software doing **one job** efficiently, with **no direct OS access** (traffic lights, digital watches, hospital monitoring equipment). Often a component inside a larger device.
- **High availability (HA)** — keeps a system **running even if one part fails**. This is a step beyond mere **redundancy** (a spare you might still have to rack, power, and configure). HA **fails over** automatically, and can be **active/passive** (backup waits) or **active/active** (both run, no one notices a failure). HA **costs more** — dual firewalls invite dual networks and dual power — so it ends in a **business decision**: pay for availability or accept the downtime risk.

### Quick Self-Check
1. Under a shared responsibility matrix, who almost always owns accounts and identities?
2. Name two sources of "mismatch risk" in a hybrid/multi-cloud setup.
3. What's the security advantage of microservices when one service fails?
4. How do VLANs achieve the same isolation as an air gap, but scale better?
5. Name SDN's three planes and what each one does.
6. Why is a centralized management console both a strength and a weakness?
7. What's the key efficiency difference between virtualization and containerization?
8. Why are IoT devices a disproportionate security risk?
9. Distinguish high availability from simple redundancy.


## Considerations

When comparing architectures, weigh these factors — each has a security angle:

- **Availability** — the resource is up *and* reachable, but **only by the right people**. Often summarized as uptime percentage (e.g., "99.999%").
- **Resilience** — how fast you recover. Measured by **MTTR (Mean Time to Repair)**; you must first find the **root cause** (hardware, software, redundant component) before fixing.
- **Cost** — not one number: **initial install** (varies by platform), **maintenance**, and **replacement**, plus accounting realities like **depreciation, capital vs. operational expense, and taxes**.
- **Responsiveness** — request-to-response speed; critical for **interactive applications** and hard to quantify when a transaction has many steps.
- **Scalability** — **elasticity** to expand/contract with load, often automatically. Security tooling must **scale with the app** so new resources stay monitored.
- **Ease of deployment** — cloud **orchestration** builds out infrastructure **automatically on demand**; good project planning (where, who, what resources) avoids delays.
- **Risk transference** — shifting risk to a third party, commonly via **cybersecurity insurance** (helps recoup losses from ransomware, downtime, and legal costs — it doesn't *prevent* the event).
- **Ease of recovery** — design for fast restore: an **image backup (~10 min)** beats reinstalling from original media (~1 hr) for the same result.
- **Patch availability** — patches fix bugs, close security holes, and improve uptime; **test, then deploy** quickly. A weak patching process is a weak security posture.
- **Inability to patch** — some **embedded systems** (HVAC controls, time clocks) have **no update path or connectivity**; mitigate by adding a layer like a **firewall** in front of them.
- **Power** — everything needs electricity; plan capacity (a licensed electrician helps), and provide backup with a **UPS (Uninterruptible Power Supply)** or **generator**.
- **Compute** — the processing "heavy lifting" (a cloud **compute engine**), scalable from a single CPU to many processors across cloud systems as demand requires.

### Considerations Quick-Reference

| Consideration | Security-relevant angle |
|---|---|
| **Availability** | Up *and* reachable only by authorized users |
| **Resilience** | Speed of recovery; tracked via MTTR |
| **Cost** | Install + maintain + replace; capex/opex/tax |
| **Responsiveness** | Latency, esp. interactive apps |
| **Scalability** | Elastic growth; monitoring must scale too |
| **Ease of deployment** | Orchestration / automated build-out |
| **Risk transference** | Cyber insurance for residual risk |
| **Ease of recovery** | Image restore vs. full reinstall |
| **Patch availability** | Test then deploy; weak patching = weak posture |
| **Inability to patch** | Embedded/no-connectivity devices; wrap in a firewall |
| **Power** | UPS/generator; capacity planning |
| **Compute** | Scalable processing (cloud compute engine) |

### Key Takeaway
There's no single "most secure" architecture — cloud, on-premises, virtualized, containerized, and embedded each trade off differently. The skill the exam tests is **comparing** them: knowing *where* the security responsibility sits, *how* each model fails, and *which* considerations (availability, cost, recoverability, patchability, power) dominate for a given design.

### Quick Self-Check
1. What does MTTR measure, and what must you determine before you can estimate it?
2. How does cybersecurity insurance fit the idea of risk transference — and what does it *not* do?
3. Why does an image backup improve "ease of recovery" over reinstalling from media?
4. What's the security problem with an "inability to patch," and one way to mitigate it?
5. Name two backup options for the "power" consideration.