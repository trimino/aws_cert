# Chapter 4 — Security Operations (CompTIA Security+ SY0-701)


## Securing Computing Resources (Objective 4.1)

Given a scenario, apply common security techniques — establish **secure baselines**, **harden** every kind of device, lock down **wireless and mobile**, and build security into **applications**.


## Secure Baselines

A **security baseline** is the set of best-practice security settings (firewall config, patch levels, OS and app hardening) that must travel with an application instance everywhere it's deployed. The lifecycle has three stages:

- **Establish** — build the initial baseline. You don't start from scratch: pull foundational baselines from the **application developer**, the **OS vendor** (e.g., Microsoft's **Security Compliance Toolkit / SCT**), or **appliance manufacturers**, then tailor them. (Windows 10 alone has 3,000+ group-policy settings, but only a security subset matters.)
- **Deploy** — push the settings to every component, ideally through an **automated process** so it scales to hundreds or thousands of devices (central console, Active Directory **group policy**, or an **MDM** for mobile).
- **Maintain** — baselines rarely change, but update them for a **new vulnerability**, an **app/OS update**, a **new OS**, or when **two vendors' baselines conflict**. **Test** before deploying, then **audit** to confirm the baseline is still in effect.


## Hardening Targets

Default installs are rarely secure. Use vendor (or trusted third-party) **hardening guides** for each target:

| Target | Key hardening actions |
|---|---|
| **Mobile devices** | Patch promptly; **segment** company vs. personal data; manage via **MDM** |
| **Workstations** (Win/macOS/Linux) | Monthly patch testing/rollout; **remove unused software** |
| **Switches / Routers** | Purpose-built embedded OS, limited access; **change default credentials**, add authentication, watch for rare-but-important vendor patches |
| **Cloud infrastructure** | Secure the **cloud management workstation**; enforce **least privilege**; run **EDR**; back up (ideally to a second provider) |
| **Servers** | Install OS patches/**service packs**; password length/complexity; least privilege; disable unused accounts; restrict which devices can connect; EDR/AV |
| **ICS / SCADA** | Often on an **air-gapped, isolated** network, no internet access |
| **Embedded systems** | Limited OS access, patches rare → **apply security patches ASAP**; put on a **segmented network behind a firewall** |
| **RTOS** | Deterministic, time-critical → **isolate** from the network, run **minimum services**, add host-based security/firewall |
| **IoT devices** | Makers aren't security experts → **prioritize patches**; **segment** so one compromised device can't reach the rest |


## Wireless Devices — Site Surveys & Heat Maps

When installing or troubleshooting wireless, gather data first:

- **Site survey** — inventory existing access points (yours *and* neighbors' you can't control), map the current **spectrum**, and pick the best **channels**. Repeat periodically, since the RF environment changes.
- **Heat map** — a visual signal-strength map (hot colors = strong signal) showing coverage room by room.
- **Survey / analysis tools** — list nearby SSIDs, BSSIDs, channels, and bands (e.g., NetSpot); a **spectrum analyzer** shows *all* signals on a frequency, including non-Wi-Fi interference.


## Mobile Solutions

- **Mobile Device Manager (MDM)** — centrally manages company- and user-owned devices: push apps and policies, **enable/disable features** by location (e.g., disable the camera at HQ), enforce **screen locks / PINs**, and **segment** business data from personal data.
- **Deployment models:**
  - **BYOD (Bring Your Own Device)** — employee's personal phone used for work too; must meet company requirements and be MDM-manageable, keeping **personal and corporate data both protected**. Set policies for when the user trades in the phone (wipe old, enroll new).
  - **COPE (Corporate-Owned, Personally Enabled)** — company buys and assigns the device; usable for work *and* personal, with corporate data **partitioned** off.
  - **CYOD (Choose Your Own Device)** — corporate-owned, but the user picks the model.
- **Connection methods:**
  - **Cellular** (4G/5G, geographic "cells") — limited control over the data path → concerns about **traffic monitoring and location tracking**.
  - **Wi-Fi** — encrypt all networks; on open networks (coffee shop/hotel) use a **VPN**; watch for **on-path attacks** and **interference/DoS**.
  - **Bluetooth** (a **PAN**, short range) — convenient for accessories, but a paired device can reach your data → use the **pairing process** and never auto-connect to unknown devices.


## Wireless Security Settings

Everything is sent over the air, so wireless must provide **confidentiality** (encryption), **integrity** (a **Message Integrity Check / MIC**), and **authentication**.

### WPA3 vs. WPA2
**WPA2's** weakness is the **four-way handshake**: an attacker can capture the pre-shared-key **hash** and **brute-force it offline** (GPU/cloud cracking makes this fast). **WPA3** fixes this:

- **GCMP (Galois Counter Mode Protocol)** — stronger block-cipher encryption with built-in confidentiality + integrity.
- **SAE (Simultaneous Authentication of Equals)** — a Diffie-Hellman-based handshake (the "**dragonfly handshake**") where session keys are **derived on each device** and **never sent across the network**, so there's no hash to brute-force. Adds **mutual authentication**, and every user gets a **different session key** even on the same pre-shared key.

### Authentication Methods
- **WPA3-Personal (WPA-PSK)** — everyone shares one **pre-shared key**; typical at home. ("Open"/"none" = no security at all.)
- **WPA3-Enterprise (WPA3-802.1X)** — prompts each user for credentials, validated against a centralized **AAA server**; standard for the workplace (per-user credentials, easy to revoke).

### AAA, RADIUS, and 802.1X/EAP
- **AAA framework** — after **identification** (username): **Authentication** (prove identity with a secret), **Authorization** (what you may access), **Accounting** (session metrics: login time, data sent, logout).
- **RADIUS (Remote Authentication Dial-In User Service)** — a widely supported AAA/authentication-protocol server used for VPN, device, and network logins (others: **LDAP, TACACS+**).
- **802.1X (NAC)** — blocks network access until credentials are provided (wired *or* wireless), using **EAP (Extensible Authentication Protocol)** as the flexible authentication framework. Three roles: **supplicant** (client) → **authenticator** (switch/AP) → **authentication server** (AAA).


## Application Security

Security must be built into code, since there's always tension between shipping fast and shipping securely. QA testing catches some flaws; researchers/attackers find the rest.

- **Input validation** — confirm every input matches what's expected (e.g., a zip-code field's length/format) and reject anything else. Developers test this with **fuzzing** (automated random/garbage input); unexpected behavior means the validation needs work.
- **Secure cookies** — cookies are data (not malware) but can hold useful info for an attacker, so a **secure attribute** forces them to transmit **only over HTTPS**; developers keep **sensitive data out of cookies** entirely.
- **Static code analysis (SAST)** — analyze source code for vulnerabilities (buffer overflows, injections) *before* release. It's imperfect — it misses things like flawed **crypto implementation** and produces **false positives** a developer must review.
- **Code signing** — the developer **digitally signs** code (asymmetric encryption, a CA-signed key) so the OS can verify it **hasn't changed** and **really came from that developer**, prompting you if validation fails.


## Sandboxing

Run an application with access **only to what it needs**, isolated from everything else.

- **In development** — code is built in an isolated sandbox so it can't affect the production network.
- **At runtime** — a **VM** is isolated from other VMs; **mobile OSes** sandbox apps so a browser can see its bookmarks but **not your camera roll**, limiting an attacker's scope.


## Monitoring

Developers build **monitoring** into applications to watch usage and security: detecting **SQL injection** attempts or exploitation of old vulnerabilities, generating **extensive logs** to hunt for unknown attacks, and surfacing anomalies (unusual file transfers, spikes in client access).

### Key Takeaway
Securing computing resources is a layered routine: **baseline** it, **harden** it (every device type differs), lock down **wireless/mobile** (WPA3 + 802.1X, MDM, segmentation), and bake security **into the app itself** (validate input, sign code, sandbox, monitor).

## Asset Management (Objective 4.2)

Proper management of **hardware, software, and data assets** has direct security implications — from how you **buy** them to how you **destroy** them.

### Acquisition / Procurement
A formal purchasing process: the end user's requirement → **IT and purchasing** analysis → **budget** check → **approvals** (IT, purchasing, management) → **negotiation** with the supplier (price, licensing, contract terms) → purchase → delivery and **invoice** (paid immediately or on net-30/60 terms). Tangible products then enter a central **asset tracking system**.

### Assignment / Accounting
- **Ownership** — every asset is assigned to a person in the tracking system (a laptop is logged under its user), so you know who to visit for updates.
- **Classification** — record device **type** (laptop/desktop/mobile) and whether it's **hardware or software**, which also drives **tax treatment**: hardware is a **capital expense** (depreciates), software is an **operating expense** (doesn't).

### Monitoring / Asset Tracking
- **Inventory** — track where every device, router, and cable is across its whole lifecycle; helps the **help desk** populate tickets with exact make/model. **Asset tags** (barcode labels) add identification and a theft deterrent.
- **Enumeration** — see a device as a single entity *or* drill into its components (CPU, memory, drives, peripherals).

### Disposal / Decommissioning
When retiring or reusing an asset, remove company data so it can't be recovered:

- **Sanitization** — **securely delete** data so it's unrecoverable (lets you safely **reuse** the drive internally); delete only sensitive files if the device stays in the org.
- **Destruction** — to guarantee data is gone forever: **shred/pulverize**, **drill or hammer** the platters, **degauss** (strong magnetic field, also ruins the drive), or **incinerate**.
- **Certification** — a third-party destruction service provides a **certificate of destruction** confirming the drives were destroyed.
- **Data retention** — conversely, some data **must be kept** (e.g., emails or financial records mandated for a set number of years). Cover original data, copies, and backups with written **retention policies**; also useful for recovering accidental deletions and for disaster recovery.

### Asset Lifecycle Quick-Reference

| Stage | Security implication |
|---|---|
| **Acquisition** | Formal approvals/negotiation; controlled entry into tracking |
| **Assignment (ownership/classification)** | Accountability per user; hardware vs. software (capex/opex) |
| **Tracking (inventory/enumeration)** | Know where assets and components are; asset tags deter theft |
| **Sanitization** | Securely wipe for safe reuse |
| **Destruction + certification** | Shred/degauss/incinerate; certificate proves it |
| **Data retention** | Keep mandated data; support recovery |

### Key Takeaway
You can't secure what you can't see. Asset management gives you **accountability** (who owns what), **visibility** (inventory/enumeration), and **safe endings** (sanitize, destroy with certification) — while **retention** rules make sure required data survives.


## Vulnerability Management (Objective 4.3)

Vulnerability management is the ongoing cycle of **finding** weaknesses, **analyzing** how serious they are, **fixing or mitigating** them, **validating** the fix, and **reporting** on it.


### Identification Methods

**Vulnerability scans** check whether a system *could* be susceptible to attack — without actually exploiting it (that would be a penetration test). A simple example is a **port scan** showing open/closed ports. Scan from **outside and inside** (insiders are a real threat), expect a **lot of output**, and review results for **false positives**. Reports rank findings by **severity** (critical/high/medium/low/informational) with details for each (e.g., a weak OpenSSL random-number generator, or an unsupported/unpatchable OS you didn't know was running).

**Application security testing** — three developer-side methods:
- **Static analysis (SAST)** — reviews **source code** for vulnerabilities (buffer overflows, injections). Misses things it can't see in code, like flawed **crypto implementation**, and produces **false positives** to review.
- **Dynamic analysis (fuzzing)** — feeds **random/malformed input** into a running app to see if it crashes or errors, revealing where input validation is needed. Almost always automated (hundreds/thousands of iterations).
- **Package monitoring** — verify that an installed **software package** is trustworthy: did it come **directly from the manufacturer** or a third party that could have inserted malware? Test unknown packages in a **lab** before production.

**Threat feeds / threat intelligence** — staying current on threats *and* threat actors:
- **OSINT (Open-Source Intelligence)** — publicly available info: discussion groups, social media, government reports, public commercial data.
- **Proprietary / third-party** — paid services that analyze threats **across many organizations** at once and alert you to trends before they reach you.
- **Information-sharing organization** — members pool, standardize, validate, and score threat data (e.g., the **Cyber Threat Alliance / CTA**).
- **Dark web** — an overlay network reachable only with special software; hacking-group forums, tools/techniques, and stolen-data markets. Worth monitoring for **your organization's name**.

**Penetration testing** — actually **simulating an attack** to gain access (NIST SP 800-115 is a good reference):
- **Rules of engagement** — a formal document defining **scope** (in/out-of-scope systems, IP ranges), **timing** (e.g., after hours), test types (physical/internal/external), emergency contacts, and how to handle discovered sensitive data. Protects production systems (an exploit can crash a service).
- **Exploitation** — try known vulnerabilities (brute force, social engineering, injection, buffer overflow). Gaining access is just the start: attackers then **pivot / move laterally**, set up a **proxy/relay**, and install a **backdoor** (new account, changed password) to ensure re-entry without reusing the original hole.

**Responsible disclosure & bug bounty** — a researcher reports a vulnerability **privately** to the vendor, who patches and tests *before* it's made public (and added to the **CVE** list) — which is why there's often weeks/months between discovery and disclosure. **Bug bounties** reward researchers (usually vendor-funded) for finding and responsibly reporting bugs.

**System/process audit** — reviewing systems and processes to surface vulnerabilities and confirm controls are in place.


### Analysis

Once vulnerabilities are found, judge how serious each one really is:

- **Confirmation — false positive vs. false negative:**
  - **False positive** — reported but **not actually present**. (Note: low/informational findings are *real* but low-priority — they are **not** false positives.)
  - **False negative** — a **real vulnerability the scanner missed** — worse, because you don't know it's there. **Update scanner signatures** to minimize both.
- **Vulnerability classification** — the severity bucket (critical → informational) that drives what gets fixed first.
- **CVSS (Common Vulnerability Scoring System)** — a **0–10** score (10 = most critical), versioned (CVSS 2.0 vs. 3.x). Found in the **National Vulnerability Database** (nvd.nist.gov).
- **CVE (Common Vulnerabilities and Exposures)** — the public catalog (cve.mitre.org) that scanners reference; cross-check against NVD and the **vendor's** own advisories. Generic findings may have **no CVE** and need extra verification.
- **Exposure factor** — the **percentage** of loss/impact if exploited (e.g., a flaw that can fully disable a public-facing service = ~100%; one causing intermittent downtime = ~50%).
- **Environmental variables** — *where* the vulnerability lives changes priority: a public-cloud system reachable from the internet ranks far above an isolated lab box.
- **Industry / organizational impact** — the same exploit hits different orgs differently (e.g., **Tallahassee Memorial Health**, Feb 2023 ransomware closed it for two weeks and diverted ER patients; **Salt Lake City / LA County** power orgs hit by DDoS, March 2019). Consider user count, internal vs. external exposure, criticality, and revenue impact.
- **Risk tolerance** — how much risk the org will accept by leaving something unpatched while testing. Easy-to-exploit, widespread flaws → **low tolerance** → rush testing and patch fast.
- **Prioritize** — combine CVSS, exposure factor, environment, and impact to order fixes, especially with limited resources.


### Response & Remediation

| Method | What it does |
|---|---|
| **Patching** | The usual fix — scheduled (weekly/monthly) or an **unscheduled** emergency patch for active/zero-day exploits |
| **Insurance** | **Cybersecurity insurance** transfers residual risk (covers post-event losses like recovery, lawsuits; excludes intentional acts) |
| **Segmentation** | Isolate devices onto separate networks/**VLANs** (or an **air gap**) so an attacker can't reach the rest; NGFWs watch inter-segment traffic |
| **Compensating controls** | When you can't patch: disable the vulnerable service, revoke access, set firewall/**ACL** rules, or add a host-based firewall |
| **Exceptions & exemptions** | A security/change-control **committee** formally accepts the risk of not patching (e.g., a flaw only exploitable with local access in a locked data center) |


### Validation of Remediation

After remediating, confirm it actually worked:
- **Rescanning** — re-run the vulnerability scan to confirm the patch took *and* to catch systems you missed.
- **Audit** — check a sample of systems (often via the patch system, sometimes by manual login) to confirm the patch is installed and effective.
- **Verification** — confirm the vulnerability is genuinely gone.


### Reporting

At scale (hundreds/thousands of systems) you need a **reporting system** tracking: total vulnerabilities identified, patched vs. unpatched systems, new threat notifications over time, patch errors, and any **exceptions/exemptions** — so you always know how much is left to close.

### Key Takeaway
Vulnerability management is a **loop**, not a one-time scan: **identify** (scans, app testing, threat feeds, pen tests, disclosures/audits) → **analyze** (confirm real vs. false, score with CVSS/CVE, weigh exposure, environment, impact, and risk tolerance) → **remediate** (patch, or mitigate with segmentation/compensating controls/insurance/exemptions) → **validate** (rescan, audit, verify) → **report**.


## Security Alerting & Monitoring (Objective 4.4)

Because attackers are constantly probing, you must **constantly monitor** — watching the right resources, performing the right **activities** (aggregate, scan, report, archive, alert, respond), and using the right **tools**.


### Monitoring Computing Resources
- **Systems** — authentications and logins (and **where** they come from — logins from a country with no employees is a red flag), running services, completed backups, and installed software versions (to spot what needs patching).
- **Applications** — availability/uptime, and **traffic volume** (an unusual spike can mean **data exfiltration**); keep lines open with developers/vendors about new security issues.
- **Infrastructure** — remote-access/VPN activity (employees vs. vendors vs. guests), and firewall/IPS logs (a spike in attacks is an early warning).

Results are often rolled up into **dashboards** for an at-a-glance view of the security posture.


### Monitoring Activities
- **Log aggregation** — consolidate logs from many diverse devices (firewalls, switches, servers, routers) into one central database — the job of a **SIEM** — so you can report and **correlate** across sources.
- **Scanning** — continuously scan devices (which are constantly moving — laptops, phones, tablets) for OS/driver/app versions and anomalies, storing it all for later reporting.
- **Reporting** — turn that data into **actionable reports**: who's **out of compliance** and what to do about it, which systems are vulnerable to today's new CVE, and **ad hoc / "what-if"** reports (e.g., how many systems will be exposed when an OS reaches end-of-life in six months).
- **Archiving** — retain logs **long-term**. Breaches take a long time to find (an IBM 2022 report cited ~9 months on average to identify and contain one), so long-term logs (sometimes **legally mandated**) support both compliance and **forensics**.
- **Alerting** — send **real-time** notifications (SMS/email, or to a **SOC**) on actionable events like a spike in authentication errors (brute force/spraying) or a large outbound data transfer.
- **Alert response & remediation/validation:**
  - **Quarantine** — isolate a flagged system so the attacker can't gain more access or pivot to other devices.
  - **Alert tuning** — balance accuracy: a **false positive** is an inaccurate alert; a **false negative** is an event that **never alerted at all** (worse). Tuning over time makes alerts trustworthy enough to act on immediately.


### Tools

| Tool | What it does |
|---|---|
| **SCAP (Security Content Automation Protocol)** | NIST-maintained common language so diverse tools (NGFW, IPS, scanner) **name the same vulnerability the same way** — enabling automation of detection and patching |
| **Benchmarks** | Best-practice secure configs for an OS/app/cloud (e.g., **CIS benchmarks**) to lock a system down out of the box |
| **Agents / agentless** | Compliance checks via an **installed agent** (always-on, but needs updates/maintenance) or an **agentless** check that runs at login/VPN connect, in memory, then removes itself (no maintenance, but not 24/7) |
| **SIEM (Security Information and Event Manager)** | Consolidates logs, **correlates** across data types, powerful reporting, and **forensics** over a long history |
| **Antivirus / anti-malware** | Identifies malicious software (worms, Trojans, ransomware, spyware, fileless); the two terms are used interchangeably today |
| **DLP (Data Loss Prevention)** | Detects and **blocks sensitive data** (SSNs, medical, credit-card) leaving the network, in real time; runs on endpoints and in the cloud |
| **SNMP traps** | **SNMP** polls device metrics (stored in a **MIB**, keyed by **OID**, over UDP 161); a **trap** proactively pushes an alert to the management station (UDP 162) when a threshold is crossed |
| **NetFlow** | Standard for monitoring **traffic flows** and app-use statistics: a **probe** (built-in or via SPAN/tap) sends flow data to a **collector** for reports (top talkers, top endpoints) |
| **Vulnerability scanners** | Minimally invasive scans (no exploits) that find active devices and potential vulnerabilities and run a **port scan**; review results for accuracy |

### Key Takeaway
Effective monitoring is a pipeline: **watch** the right resources (systems, apps, infrastructure) → **aggregate and scan** → **report and archive** → **alert** on what matters → **respond** (quarantine) and **tune** to cut false positives/negatives. SCAP plus a SIEM is what makes all those diverse tools — antivirus, DLP, SNMP, NetFlow, scanners — work together.


## Modifying Enterprise Capabilities (Objective 4.5)

Given a scenario, tune the security tools you already have — firewalls, IDS/IPS, web/DNS/email filtering, OS controls, secure protocols, integrity monitoring, DLP, NAC, and EDR/XDR — to enhance protection.


### Firewall
A network-based firewall sits **inline** and allows/denies traffic (by **port** on a traditional firewall, by **application** on an NGFW); it can also act as a VPN endpoint and a layer-3 router (NAT, routing).

- **Rules** — evaluated **top-down**, first match wins, so put **specific rules above general ones**. An **implicit deny** at the bottom drops anything that matched nothing.
- **Access lists (ACLs)** — the rule base itself: source/dest IP, port number, protocol, time of day, application, user, etc.
- **Ports/protocols** — a traditional firewall controls by port (HTTP = TCP 80, HTTPS = 443, SSH = 22, RDP = 3389, DNS = UDP 53, NTP = UDP 123, ICMP = ping); an NGFW recognizes the **application** regardless of port.
- **Screened subnet** — a subnet (formerly "DMZ") for internet-facing services, so external traffic reaches those hosts **without touching the internal network**.


### IDS / IPS
Often built into an NGFW with its own rule base, monitoring traffic in real time.
- **Signatures** — match known malicious patterns (e.g., a worm's traffic); thousands of them, groupable so you can set a broad disposition (e.g., "block all database-injection signatures").
- **Trends / anomalies** — some IPSs block **generic** intrusions (like a SQL injection) by behavior even with **no specific signature**. Tune rules to balance security against **false positives**.


### Web Filter
Controls what content users can reach (a.k.a. content/URL filter; at home, "parental controls"):
- **URL scanning** — allow/block by **fully qualified domain name** (allow list / block list).
- **Content categorization** — group sites into 50+ categories (gambling, adult, education…) and set **block rules** per category (allow, allow-but-log, or block).
- **Reputation** — allow/block by a site's risk score (trustworthy → high risk), scored automatically (or set manually).
- **Centralized proxy** — a proxy sits between users and the internet, makes requests on their behalf, and can filter, cache, and enforce access control (explicit = configured in the app; transparent = invisible to the user).
- **Agent-based** — filtering software on the **endpoint** (centrally managed) so the rules follow **mobile/remote users** regardless of network; agents need regular category updates.


### DNS Filtering
Block at name resolution: the DNS server **won't return the IP** for known-bad domains (lists updated via real-time threat intelligence). Bonus: it also blocks **malware's command-and-control** lookups, not just web pages.


### Operating System Security
- **Group Policy** — on Windows/**Active Directory**, push configuration and security settings to users and devices centrally (login scripts, QoS, security parameters) via the Group Policy Management Editor.
- **SELinux (Security-Enhanced Linux)** — patches that switch Linux from its default **discretionary** access control to **mandatory** access control, letting an admin enforce **least privilege** so a breach has limited scope.


### Implementation of Secure Protocols
Many default protocols send data in the clear (Telnet, FTP, SMTP, IMAP, HTTP, POP3). Three decisions:
- **Protocol selection** — choose the encrypted equivalent: **SSH** over Telnet, **HTTPS** over HTTP, **IMAPS** over IMAP, **SFTP** over FTP. If no secure option exists, don't use the app.
- **Port selection** — the port often hints at security (80 = HTTP clear, 443 = HTTPS encrypted) — but a port alone doesn't *guarantee* encryption; **verify with a packet capture**.
- **Transport method** — when the app can't encrypt, encrypt the **transport**: **WPA3** on wireless, or a **VPN** tunnel to a concentrator.


### Email Security
SMTP has little built-in security, so spoofed "from" addresses are easy. A **mail gateway** (on-prem in a screened subnet, or cloud) decides what's legitimate, using three DNS **TXT records**:

| Record | Purpose |
|---|---|
| **SPF (Sender Policy Framework)** | Lists which mail servers are **authorized to send** for your domain; the receiver checks it against the message headers |
| **DKIM (DomainKeys Identified Mail)** | A **digital signature** added in transit between servers; the receiver validates it with the **public key** in your DNS |
| **DMARC (Domain-based Message Authentication, Reporting & Conformance)** | Tells receivers what to do when SPF/DKIM **fail** (accept / quarantine / reject) and where to send **compliance reports** showing legit vs. spoofed volume |


### File Integrity Monitoring (FIM)
Alerts when files that **should never change** (app executables, core OS files) are modified. Tools: Windows **SFC (System File Checker)** (on-demand, restores good versions), Linux **Tripwire** (real-time), or a **host-based IPS** (which can do FIM since it sits on the OS).


### Data Loss Prevention (DLP)
Detects and blocks **sensitive data** (SSNs, medical, card numbers) in real time, by data state:
- **Data in use** — endpoint DLP watching active memory on the device (can also block **USB** transfers — the 2008 **agent.btz** USB worm led the U.S. DoD to ban USB storage).
- **Data in motion** — network DLP watching packets (standalone appliance or in an NGFW).
- **Data at rest** — software on a server scanning stored files.
- **Cloud DLP** — watches traffic in/out of cloud apps.
- **Email DLP** — the most common leak vector: block inbound spoofs/keywords and outbound sensitive data (e.g., the 2016 **Boeing** email exposing 36,000 employees' data in a spreadsheet's hidden fields — an email DLP would have caught it).


### Network Access Control (NAC)
Enforces a **posture assessment** before granting access — typically at login/connect — checking: trusted-device **certificate**, antivirus installed/updated, apps current, and **full-disk encryption** on remote devices. Fail → **quarantine** to a VLAN with remediation steps, then re-check. Agent types:
- **Persistent agent** — installed permanently; runs anytime; needs updates.
- **Dissolvable agent** — runs at login, then removes itself (no install).
- **Agentless NAC** — integrated with **Active Directory**; runs only at login/logout.


### EDR / XDR
With 1M+ daily malware variants, signatures alone can't keep up.
- **EDR (Endpoint Detection and Response)** — an endpoint agent adding **behavioral analysis, machine learning, and process monitoring**, plus **root-cause analysis** and automated response (isolate, quarantine, **roll back** to known-good — no user action).
- **XDR (Extended Detection and Response)** — correlates data across **many systems and network traffic**, catching detections a single-endpoint agent would miss, reducing false positives, and speeding investigation.


### User Behavior Analytics
Feeds XDR by building a **baseline of normal** (which users, which devices, what traffic, which data they access) so **abnormal** activity stands out — via rules, pattern-matching to known vulnerabilities, or statistical analysis — to catch and stop malicious activity in real time.

### Key Takeaway
4.5 is about **tuning what you already own**: tighten **firewall/IPS** rules (specific-first, implicit deny, signatures + anomaly), filter at the **web/DNS/email** layers (categories, reputation, SPF/DKIM/DMARC), harden the **OS** (Group Policy, SELinux), force **encrypted protocols/transport**, and watch endpoints with **FIM, DLP, NAC, and EDR/XDR** driven by **behavior baselines**.