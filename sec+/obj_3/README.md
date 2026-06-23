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

## Secure Enterprise Infrastructure (Objective 3.2)

Given a scenario, you apply security principles to secure infrastructure — deciding **where devices go**, how they **fail**, how they **monitor**, and how remote users **connect securely**.

### Infrastructure Considerations

- **Device placement & security zones** — A **security zone** logically separates devices by **use or access type** rather than by subnet (e.g., *trusted/untrusted*, *internal/external*, or more granular *inside, internet, servers, databases, screened*). Zones make firewall rules easier to read and maintain ("allow trusted → untrusted," "untrusted → screened"). You place devices into the zone that matches their exposure.
- **Attack surface** — the **sum of all the ways an attacker could get in**: application code, open ports, the authentication process, and human error (one misconfigured firewall rule). The goal is to **minimize** it — audit code, block unnecessary ports, and monitor traffic in real time.
- **Connectivity** — every device connects to others, so build security into the links: protect **network cabling** (physical + logical), assume someone could **tap** the wire, and use **application-level encryption** plus **IPsec tunnels / a VPN concentrator** for remote and site-to-site links.

### Failure Modes — Fail-Open vs. Fail-Closed
An inline security device can crash (power, hardware, software bug). What happens to traffic then?

- **Fail-open** — on failure, **traffic keeps flowing** (no security, but the network stays up).
- **Fail-closed** — on failure, **the connection is severed** (secure, but the network is down).

Most networks prefer fail-open, but not every device defaults that way — **check the documentation**.

### Device Attributes — Active vs. Passive, Inline vs. Tap/Monitor
- **Inline / active monitoring** — the device sits **in the traffic path** and can **block in real time** (the default for an IPS). Risk: an outage or over-aggressive blocking affects live traffic.
- **Passive monitoring / tap** — the device gets a **copy** of traffic via a **port mirror (SPAN)** or a physical **network tap**. It **can't block** (it only alerts), but it **can't cause downtime**. An IPS deployed this way effectively behaves like an **IDS**.

### Network Appliances

- **IPS / IDS** — An **IPS (Intrusion Prevention System)** watches traffic in real time and **blocks** exploits (known vulnerabilities, buffer overflows, SQL injection). An **IDS (Intrusion Detection System)** only **alerts** — it can't block.
- **Jump server** — a hardened, secured device **inside** the network that's **reachable from outside**, used as a stepping stone to manage internal systems (connect to the jump server, then SSH onward). Because it bridges outside-to-inside, it **must be tightly hardened** — compromise it and an attacker reaches everything behind it.
- **Proxy server** — sits in the middle of a conversation and makes requests **on a user's behalf**, inspecting responses for malicious content (and often **caching** and **URL filtering**).
  - **Forward proxy** — controls **outbound** user traffic to the internet.
  - **Reverse proxy** — handles **inbound** traffic to an internal service (e.g., shields a web server, can cache).
  - **Open proxy** — a third-party proxy open to anyone; a **security risk** (unknown operator may inject ads or malware), so it's often **blocked**.
- **Load balancer** — distributes load across multiple servers for **efficiency** and **fault tolerance** (a failed server is dropped automatically). Modes: **active/active** (all servers in use) or **active/passive** (standby servers take over on failure).
- **Sensors & collectors** — **sensors** (built into switches/routers/firewalls/IPS, or standalone) gather statistics and send them to a central **collector**. A **SIEM (Security Information and Event Manager)** is a powerful collector that consolidates, correlates, and reports across diverse devices.

### Port Security — 802.1X & EAP
Security on the **individual switch interface or wireless connection** — authenticate **before** granting network access.

- **EAP (Extensible Authentication Protocol)** — the authentication **framework** that works across wired and wireless gear.
- **802.1X** — the IEEE standard that enforces it (a form of **port-based Network Access Control / NAC**). Plug into a port and you get **no access until you authenticate**.
- **Three roles:** the **supplicant** (client), the **authenticator** (switch/AP), and the **authentication server** (back-end database — often RADIUS, LDAP, TACACS+, or Kerberos).

### Firewall Types

| Firewall | Operates at | What it does |
|---|---|---|
| **Traditional (Layer 4)** | OSI **L4** | Allows/denies by **TCP/UDP port number** |
| **NGFW (Next-Generation)** | OSI **L7** | Decodes the **application layer** — allow/deny by *application* regardless of port (e.g., view Twitter but not post); adds **IPS** and **URL categorization** |
| **UTM (Unified Threat Mgmt)** | mostly **L4** | "All-in-one": URL filtering, malware/spam blocking, VPN, IDS/IPS, bandwidth shaping — but enabling many features can **slow it down** |
| **WAF (Web Application Firewall)** | web input | Analyzes **input to web apps**, blocks **SQL injection / XSS**; often **mandated by PCI DSS** |

### Secure Communication & Access

- **VPN (Virtual Private Network)** — encrypts private data across a public network. A **VPN concentrator** (often built into a next-gen firewall) is the endpoint that decrypts traffic into the corporate network.
- **Tunneling** — the original packet (IP header + data) is **encrypted and wrapped** in new headers so it can cross the public network:
  - **IPsec** — wraps data with an IPsec header/trailer + new IP header; classic for **site-to-site** VPNs.
  - **TLS/SSL** — runs over **TCP 443**, so it passes through firewalls easily; common for **remote-access** VPNs from a single device (client- or browser-based, can be **always-on**).
- **Remote access vs. site-to-site** — SSL/TLS VPNs = **remote-access** (one device → concentrator); IPsec VPNs between firewalls = **site-to-site** (no client software; encryption is automatic between sites).
- **SD-WAN (Software-Defined WAN)** — builds flexible WAN paths so remote sites reach **cloud apps directly** instead of hair-pinning through a central data center.
- **SASE (Secure Access Service Edge)** — the "next-generation VPN": security services live **in the cloud next to the apps**, with a **SASE client** on each device securely connecting users (corporate, home, or mobile) to any cloud service.
- **Selection of effective controls** — organizations often combine these: SSL VPN for users, IPsec site-to-site for offices, SD-WAN for cloud reach, and SASE to secure it all. The right mix depends on the apps and connectivity.

### Key Takeaway
Securing infrastructure is about **placement and posture**: put devices in the right **zone**, shrink the **attack surface**, decide how things **fail** (open vs. closed) and **monitor** (inline vs. tap), pick the right **firewall layer** (L4 vs. L7), and **encrypt the links** (VPN/IPsec/TLS, extended to the cloud with SD-WAN + SASE).

### Quick Self-Check
1. How does a security zone differ from a subnet?
2. A device crashes fail-closed — what happens to the network, and why might you still want that?
3. Inline vs. tap monitoring: which can block, and which can't cause downtime?
4. Why must a jump server be especially well hardened?
5. Forward vs. reverse proxy — which protects an internal web server from inbound traffic?
6. What lets an NGFW allow Twitter viewing but block posting, when both use the same port?
7. Name the three 802.1X roles and one protocol an authentication server might use.
8. Which VPN style is "remote-access" and which is "site-to-site"?
9. What problem does SD-WAN solve, and what does SASE add on top?


## Protecting Data (Objective 3.3)

Comparing concepts and strategies to protect data — **what kind** of data it is, **how sensitive** it is, **what state** it's in, and **which methods** secure it.

### Data Types
- **Regulated** — a **third party sets the protection rules** (e.g., PCI DSS for credit cards; government laws on storage and retention).
- **Trade secret** — secrets and processes known only to the organization; high-value target.
- **Intellectual property** — often visible to others but protected by **copyright/trademark** law.
- **Legal information** — mixes **public** records with **private** details (PII inside proceedings), often stored separately.
- **Financial information** — sensitive whether it's a company's internal numbers or an individual's accounts and payments.
- **Human- vs. non-human-readable** — a document is human-readable; a **barcode or encoded data** is not (sometimes combined, e.g., a barcode with printed digits).

> Related categories you'll see: **proprietary** (unique to the org), **PII (Personally Identifiable Information)** — name, DOB, biometrics, etc., and **PHI (Protected Health Information)** — health records and related payments.

### Data Classifications
Assigning a **sensitivity level** drives access rules:

| Classification | Meaning |
|---|---|
| **Public / unclassified** | Anyone may view it |
| **Sensitive** | Includes IP, PII, PHI — needs care |
| **Confidential** | More sensitive; requires extra access |
| **Private / Restricted** | Extra rights/permissions or an **NDA** to access |
| **Critical** | Must **always be available**; build processes to keep it up |

### States of Data
| State | Where it lives | Notes |
|---|---|---|
| **Data at rest** | On a storage device (HDD/SSD/flash) | Protect with **full-disk / file / database encryption** + **permissions** |
| **Data in transit** | Crossing the network | Protect with **TLS**, **IPsec VPNs**, firewalls/IPS |
| **Data in use** | In **RAM / CPU** | Almost always **decrypted** while processed — attackers target it (e.g., **Target 2013**: 110M cards pulled from point-of-sale memory despite encryption at rest and in transit) |

- **Data sovereignty** — data stored in a country is subject to **that country's laws**. E.g., **GDPR** requires data on EU citizens to be **stored in the EU**.
- **Geolocation** — determine a user/device's location (GPS, 802.11, mobile provider) to **grant or restrict** access accordingly.

### Methods to Secure Data
- **Geographic restrictions / geofencing** — allow access only from approved locations (IP subnet, GPS, or 802.11 lookup); e.g., certain data only reachable **inside a corporate facility**.
- **Encryption** — turns **plaintext** into unreadable **ciphertext** (with a large change called *confusion*); reversible only with the **decryption key**.
- **Hashing** — a **one-way** fixed-length **fingerprint / message digest**; can't be reversed to the original. Used for **password storage** and **file-integrity** checks; two different inputs giving the same hash is a **collision** (a reason to retire weak algorithms). *Even a one-character change yields a completely different SHA-256 output.*
- **Obfuscation** — make something understandable **hard to read** while it still works the same (e.g., obfuscated source code); attackers also use it to hide malicious scripts.
- **Masking** — hide part of the data (e.g., a receipt showing only the **last four digits** of a card); the rest is starred out, though the processor can still see the full value behind the scenes.
- **Tokenization** — replace sensitive data with a **token** that has no derivable relationship to the original (e.g., phone/smartwatch payments send a **one-time token** via NFC, not your real card number) — **no encryption math involved**.
- **Segmentation** — split data into **smaller, separate databases** so a single breach doesn't expose everything, and apply **different security per database** by sensitivity.
- **Permission restrictions** — tie **rights/permissions** to the authenticated account (strong authentication first, then groups/file permissions limiting what each user can reach).

### Key Takeaway
Match the protection to the data: classify it by **sensitivity**, know its **state** (at rest / in transit / **in use** is the soft spot), respect **sovereignty**, and layer the right **methods** — encryption and hashing for confidentiality/integrity, masking/tokenization for exposure, segmentation and permissions to limit blast radius.

### Quick Self-Check
1. What makes data "regulated," and give one example.
2. Which classification means the data must always be available?
3. Why is "data in use" attractive to attackers even when encryption is in place?
4. What does GDPR require regarding where EU citizens' data is stored?
5. Hashing vs. encryption — which is reversible, and which produces a non-reversible fingerprint?
6. How does tokenization differ from encryption, and why is a payment token safe if captured?
7. How does segmentation limit the damage of a single breach?


## Resilience & Recovery (Objective 3.4)

Why resilience and recovery matter in architecture — keeping systems **up** despite failures and getting **back up** quickly after disasters.

### High Availability — Clustering vs. Load Balancing
**High availability (HA)** keeps systems **always on**: if one fails, another running beside it takes the load instantly (beyond simple redundancy, which may sit in a box until needed). HA costs more (extra systems, upgraded power, higher-quality parts). Two related approaches:

| | **Server clustering** | **Load balancing** |
|---|---|---|
| **Awareness** | Servers know about each other; act as **one cluster** | Servers **don't know** each other; the load balancer manages them |
| **OS** | Usually identical OSes; **shared storage** keeps them in sync | Can be mixed OSes |
| **Scaling** | Add/remove cluster nodes in real time | Add/remove servers behind the balancer |

### Site Resiliency
A **recovery site** lets you move operations away from a disaster:

| Site | What's there | Recovery speed |
|---|---|---|
| **Hot** | Exact replica — all hardware, apps, **constantly synced data** | Near-immediate |
| **Warm** | Some equipment and data; bring the rest | Medium |
| **Cold** | Empty building with power/lighting; bring everything | Slow |

- **Geographic dispersion** — keep the recovery site **far away** so one regional event (hurricane, flood) can't take out both — though distance complicates moving **equipment, data, and people**.

### Platform Diversity & Multi-Cloud
- **Platform diversity** — use **different operating systems** for different roles so a single OS vulnerability can't affect everything (e.g., Linux + Windows in the data center; macOS + Windows on clients).
- **Multi-cloud systems** — spread services across providers (AWS, Azure, Google Cloud); an outage or security issue at one provider doesn't take you fully down.

### Continuity of Operations (COOP)
A **non-technical fallback** for when the technology simply isn't available — manual transactions, paper receipts, phoning in card approvals. These processes must be **designed in advance**.

### Capacity Planning
Match **supply to demand** — too little causes slowdowns/outages; too much wastes money. Plan across:
- **People** — humans are hard to ramp up/down (hiring, training take time and money).
- **Technology** — choose tech that **scales** (load balancers + multiple servers, splittable databases, cloud services).
- **Infrastructure** — physical builds need shipping/racking/testing; the cloud lets you **rightsize on demand**.

### Recovery Testing
Test the disaster-recovery plan **regularly**, with a defined scope that won't touch production:

- **Tabletop exercise** — walk through the steps **on paper / around a table** (cheap; surfaces gaps without a real recovery).
- **Failover test** — verify redundant systems **switch over automatically** (multiple ISPs, routers, firewalls, switches, links).
- **Simulation** — test security with realistic scenarios, e.g., a **phishing simulation** to see whether automated systems and users catch it.
- **Parallel processing** — spread transactions across **multiple CPUs/systems** for efficiency *and* resilience (one processor failing doesn't stop the work).

### Backups
Plan the variables: **how much** data, **what media**, **where stored**, **what software**, and **what schedule**.

- **Onsite vs. offsite** — onsite = fast restore, cheaper, no WAN needed; offsite = protected from a local disaster, recoverable from anywhere. Many orgs use **both**.
- **Frequency** — hourly/daily/weekly/monthly, often by how much the data changes; multiple **backup sets** at different intervals.
- **Encryption** — encrypt backups (especially **offsite/cloud**) so stolen media is useless (the classic *stolen backup tapes from a car* scenario) — but **keep the recovery keys**.
- **Snapshots** — one-click point-in-time copy, popular for **VMs/cloud**; behaves like an **incremental** backup (only changes), easy to take before big changes and to roll back.
- **Recovery (testing restores)** — actually **test** that you can restore *and* that apps can use the restored data — restoring is only half the test.
- **Replication** — copies data to one or many locations in **near real time**; great for keeping a **hot site** current.
- **Journaling** — write data to a **journal first**, then to the database, so a **power loss mid-write doesn't corrupt** the database (it replays the journal on restart).

### Power Resiliency
- **UPS (Uninterruptible Power Supply)** — battery backup for **short** outages; also rides through **blackouts, brownouts (low voltage), and surges**. Types range from **offline/standby** → **line-interactive** → **online/double-conversion**. Can signal a **graceful shutdown** as the battery drains.
- **Generator** — **long-term** power for as long as there's fuel; can power a building or marked "generator" outlets.
- **Use both** — the UPS covers the ~minute it takes a generator to **ramp up** after an outage.

### Resilience Quick-Reference

| Mechanism | Purpose |
|---|---|
| **HA (clustering / load balancing)** | Stay up when a component fails |
| **Hot / warm / cold site** | Relocate operations after a disaster |
| **Geographic dispersion** | One event can't hit primary and recovery |
| **Platform diversity / multi-cloud** | Avoid single-OS / single-provider failure |
| **COOP** | Non-technical fallback processes |
| **Capacity planning** | Right-size people, tech, infrastructure |
| **Tabletop / failover / simulation** | Validate the recovery plan |
| **Backups (snapshot/replication/journaling)** | Recover lost or corrupted data |
| **UPS + generator** | Bridge short and long power outages |

### Key Takeaway
Resilience keeps you **running** (HA, diversity, redundant power); recovery gets you **back** (sites, backups, tested plans). The recurring exam themes: **clustering vs. load balancing**, **hot vs. warm vs. cold**, **why you encrypt and *test* backups**, and **UPS for the gap, generator for the long haul**.

### Quick Self-Check
1. In clustering the servers know about each other — what's different about load balancing?
2. Order hot/warm/cold sites by recovery speed and by cost.
3. Why does geographic dispersion matter for a recovery site?
4. How does platform diversity reduce risk from a single vulnerability?
5. What is a tabletop exercise, and why is it cheaper than a full failover test?
6. Why must you *test restores* rather than just trust that backups exist?
7. How does journaling prevent database corruption during a power loss?
8. Why deploy both a UPS and a generator instead of just one?