# Chapter 2 — Threats, Vulnerabilities & Mitigations (CompTIA Security+ SY0-701)


## Threat Actors (Objective 2.1)

### What Is a Threat Actor?

A **threat actor** is an entity that causes an event affecting the security of others. They're often called **malicious actors** because their actions negatively impact someone else's security. Knowing *who* the threat actor is helps you understand *why* an attack happened and what the attacker's ultimate goal was.


### Threat Actor Attributes

We describe threat actors using a set of attributes. These are the lenses Security+ wants you to use when characterizing any attacker.

- **Internal vs. External** — The actor may work *inside* your organization or operate from *outside*, trying to gain access through public-facing resources.
- **Resources / Funding** — Ranges from almost none (limited access to tools) to massive (able to fund many simultaneous attacks).
- **Level of Sophistication / Capability** — From an attacker who has no idea what their script even does, to one who builds their own custom tools and capabilities. Many fall somewhere in between.
- **Motivation / Intent** — The *why* behind the attack (see below).

#### Common Motivations

There are many possible reasons behind an attack, depending on the situation, the attacker, and the target:

- Data exfiltration (stealing data out of the organization)
- Espionage (e.g., a competitor wanting to know what you're working on)
- Service disruption (causing problems for the target's customers)
- Philosophical or political reasons
- Financial gain
- Revenge
- Provoking conflict (e.g., pulling a country into war)


### The Six Threat Actor Types

#### 1. Nation State
- **Location:** Usually external
- An entire government, or an arm of it dealing with national security.
- **Motivations:** Data exfiltration, philosophical/political reasons, service disruption, or even drawing someone into war.
- **Resources:** Enormous — can run constant attacks against multiple targets simultaneously and hire the most sophisticated developers.
- **Sophistication:** Very high.
- Often the source of **APTs (Advanced Persistent Threats)** — especially dangerous because they have an entire government behind them. Targets include military control systems, utilities, and a country's finances.
- **Example:** The **Stuxnet worm**, created by the U.S. and Israel, designed to destroy nuclear centrifuges.

#### 2. Unskilled Attacker
- **Location:** Commonly external, but sometimes found inside the organization.
- Runs scripts without understanding what happens under the surface. If the script works, they succeed; if it fails, they can't troubleshoot or modify it.
- **Motivations:** The attack itself — service disruption, data exfiltration, or a philosophical/political reason.
- **Resources:** Generally very limited; no government or large-organization backing.
- **Sophistication:** Low — looks for the easiest way in using readily available scripts.
- *(Older term you may see: "script kiddie.")*

#### 3. Hacktivist
- **Location:** Commonly external, but could get hired and become an internal threat.
- A **hacker + activist** motivated by political or philosophical reasons, or a desire to disrupt/damage an organization.
- **Sophistication:** Often very high — sophisticated technologists who attack in very specific ways.
- **Common tactics:** Denial of service, website defacement (posting their own messages), or stealing private documents to release publicly.
- **Resources:** Usually limited, though some groups use **fundraising** to finance their activities.

#### 4. Insider Threat
- **Location:** Internal
- More than just a sticky note under a keyboard — this is someone acting maliciously, often for **revenge** or **financial gain**.
- **Resources:** All the organization's resources already exist around them; they simply take advantage of existing access.
- **Sophistication:** Medium — but they excel at knowing *exactly* where the data lives and how to circumvent existing security controls.
- **Key defense:** Proper **vetting during the hiring process**.

#### 5. Organized Crime
- **Location:** Usually external
- Professional crime operating in the cybersecurity arena.
- **Motivation:** Almost always **money / profit**.
- **Resources:** Significant — often structured like a corporation:
  - one person hacks,
  - another manages and develops exploits,
  - another sells the data to third parties,
  - and sometimes even **customer support** (common in ransomware operations).
- **Sophistication:** High.

#### 6. Shadow IT
- **Location:** Internal
- A group or department working **around** the official IT department's policies and procedures.
- Builds its own infrastructure, installs its own apps, and uses them without IT's knowledge — avoiding change control and budgeting limits by using their own budget or credit cards (often cloud services accessed via a browser).
- **Resources:** Limited by their budget, but a small budget plus cloud services can build a surprising amount of infrastructure.
- **Sophistication:** Low — many have no IT background, so they overlook backups, change control, and security, creating real risk for the organization.


### Summary Comparison Table

| Threat Actor | Location | Resources | Sophistication | Typical Motivation |
|---|---|---|---|---|
| **Nation State** | External | Extensive | Very High | Disrupt/harm another government; espionage; political |
| **Unskilled Attacker** | Usually External | Very Limited | Low | The attack itself; disruption; philosophical/political |
| **Hacktivist** | External (can go internal) | Limited (may fundraise) | High | Political / philosophical |
| **Insider Threat** | Internal | Already inside the org | Medium | Revenge or financial gain |
| **Organized Crime** | External | Extensive | High | Money / profit |
| **Shadow IT** | Internal | Limited | Low | Working around IT (not malicious by nature) |


### Key Takeaway

If you understand a threat actor's **motivation**, you can adjust your security controls to best prevent that specific type of attacker from gaining access to your systems.

### Quick Self-Check
1. What four attributes do we use to characterize a threat actor?
2. Which two threat actor types are typically *internal*?
3. What does APT stand for, and which actor type is most associated with it?
4. What primarily motivates organized crime?
5. Why is Shadow IT a risk even though its members usually aren't malicious?


## Threat Vectors & Attack Surfaces (Objective 2.2)

### What Is a Threat Vector?

A **threat vector** (also called an **attack vector**) is the *method* an attacker uses to gain access to your systems. Attackers constantly work to **discover or create new vectors** — and they look for both *well-known* vectors and opportunities to exploit *unknown* ones. The **attack surface** is the total of all these possible entry points.


### 1. Message-Based Vectors

Messaging is one of the most common starting points because nearly everyone uses it to communicate.

- **Email** — Malicious links that install malware or lead to a **phishing page** (e.g., a fake bank login that abuses your trust in the message). Malware can be embedded in the message or downloaded from a linked site.
- **SMS (text messages)** — Especially effective on mobile; lures you into clicking links you shouldn't.
- **Instant Messaging / Direct Messages** — Lets the attacker talk to you directly to build trust and entice clicks.
- **Why it works:** Phishing thrives here because the attacker communicates with you directly and can layer on **social engineering** — fake invoices for services never rendered, cryptocurrency scams (stealing wallet access or selling fake crypto), etc.
- **Real example:** A fake "U.S. Postal Service" text (sent from an `onmicrosoft.com` address) claiming a package was suspended due to a bad delivery address, with a link to click. → *Don't click; report as junk.*


### 2. Image-Based Vectors

- **SVG (Scalable Vector Graphics)** is understood by most browsers — but it's more than an image. It's an **XML file** that *describes* the image and can **embed other content**.
- An attacker can inject **HTML** or **JavaScript** inside the XML, which then runs in your browser (e.g., a **cross-site scripting (XSS)** attack).
- **Defenses:** Browsers can enable/disable certain image types, perform **input validation**, and look for/block XSS — but a browser vulnerability or a non-XSS script may still slip through.


### 3. File-Based Vectors

- **Executables** — Software that actively runs in memory; an obvious threat.
- **Adobe PDF** — A "container" that holds text, images, and sometimes scripting — a great hiding place for malware.
- **Compressed files (ZIP, RAR, etc.)** — **Obfuscate** the attack; a single archive can hide one malicious file among hundreds or thousands.
- **Office documents** — **Macros** (common in Microsoft Office) can be written to gather personal info and send it to the attacker.
- **Browser add-ins / extensions** — Installing a malicious extension puts the whole system at risk.


### 4. Voice Call Vectors

- **Vishing (voice phishing)** — Calls designed to get you to give up credit card or personal details.
- **Spam over IP (SPIT)** — VoIP systems used to send automated spam.
- **War dialing** — Finding unpublished phone numbers that grant access to systems (still seen today).
- Calls/messaging can also be used for **denial-of-service**, not just info gathering.


### 5. Removable Devices (USB)

- A **$10 USB drive** can bypass millions of dollars of firewalls, IPS, and filtering.
- **Air-gapped networks** (no direct network connection) can be reached by leaving infected USB drives in a parking lot, hoping an employee plugs one in.
- **USB as a keyboard (HID attack)** — A modified drive appears as a keyboard and **auto-types** commands.
- **Data exfiltration** — Plug in, copy large amounts of data, pocket the drive, and walk out.


### 6. Vulnerable Software

- **Client-based** — An app with an **infected executable** infects the local computer when run. Unknown vulnerabilities give attackers an edge. → Patch monthly (e.g., Microsoft updates) *and* whenever security patches release.
- **Agentless** — Nothing installed locally; you connect to a separate system (e.g., **web-based apps** via browser). If the attacker infects the **central server**, every connecting client can be infected — easy to distribute since each login runs a fresh server-side instance.


### 7. Unsupported Systems & Applications

- Some systems are **no longer patched** by the manufacturer (e.g., **end-of-life operating systems**) — an enormous security risk.
- These systems are often forgotten (the old PC "under someone's desk").
- **Defense:** Maintain an up-to-date **inventory**, and **periodically scan** the network to find and secure unsupported systems.


### 8. Network-Based Vectors

Your network is a **digital highway** that lets attackers move freely once inside.

- **Wireless** — Use the latest protocols; upgrade **WEP / WPA / WPA2 → WPA3**. Scan periodically for **open or rogue access points**.
- **802.1X** — Authentication protocol for **wired and wireless** that blocks access without proper credentials.
- **Bluetooth** — Can be used for reconnaissance, or exploited where the implementation has limited/poor security.


### 9. Open Service Ports

- Every service needs an open port (e.g., a web server uses **TCP 80** and **TCP 443**).
- Open ports give third parties a way in; a vulnerability in the listening software can be exploited.
- **More services = more open ports = larger attack surface.**
- **Misconfiguration** of complex apps can also allow unauthorized access.
- **Defense:** Use **port-based** or **application-aware firewalls** to limit which services are reachable from outside.


### 10. Default Credentials

- Devices ship with defaults like **admin / admin** (routers, access points, cable modems).
- If left unchanged, anyone who knows the default has full access. Defaults are easy to look up (e.g., **routerpasswords.com**).
- **Good news:** Many modern devices **force a password change** on first login.


### 11. Supply Chain Vectors

A trusted third party becomes the way in — the threat walks through the front door.

- **Equipment tampering** — Malicious code added during *or after* the manufacturing process.
- **Managed Service Provider (MSP)** — If an attacker compromises the MSP that manages your systems, they gain access to *your* systems too.
- **Target breach (2013)** — Attackers compromised an **HVAC contractor**, jumped to Target's network, and installed malware on point-of-sale systems to **steal credit card numbers**.
- **Counterfeit hardware** — In 2020, **fake Cisco Catalyst switches** were found (flagged because they couldn't update properly); such gear can carry malicious software.


### 12. Human Vectors / Social Engineering — Phishing

**Phishing** is social engineering that uses various communication methods (mail, text, etc.) to make something fake *look real*, tricking you into giving up private information — a username/password, account details, or other personal data.

#### How to Spot a Phishing Message
- **Check the link destination** — A phishing link points somewhere *other* than the real, trusted site.
- **Something looks "off"** — Bad **spacing**, odd **fonts**, or layout that doesn't match the genuine page (e.g., a fake Rackspace login that's subtly wrong next to the real one).
- **Mismatched sender** — The "from" address doesn't fit (e.g., a "Rackspace" upgrade notice sent from an `icloud.com` address).
- **Manufactured urgency** — Deadlines and threats ("confirm now or you'll be blocked") push you to act before thinking.
- **Best practice:** Don't click links inside email at all — navigate to the site yourself.

#### Why It's Dangerous
We tend to **trust email**, so we click. If an attacker gets your credentials, they can send mail *as you*, mine your inbox for financial info/logins, or abuse **password-reset** features (e.g., trigger a PayPal reset that lands in the inbox they now control). Links may also download **malware**.

#### Key Techniques & Terms
- **Typosquatting** — A lookalike domain that relies on a misread or typo (e.g., `professormessor.com` instead of the real `professormesser.com`). Senders may also **spoof** a real address or use one very close to it.
- **Pretexting** — Outright lying; the attacker invents a story/drama to pull you in and get you to click or log in.
- **Vishing (voice phishing)** — Over-the-phone scam with a **spoofed caller ID** (e.g., "We're from Visa about a failed automatic payment — confirm your card details").
- **Smishing (SMS phishing)** — Phishing via **text message** (SMS), e.g., the fake "USPS package suspended — click to confirm your address" text.
- **Other scams to know:** fake check scam, phone verification code scam, and many more — worth learning so you can help friends/family avoid them.


### 13. Impersonation

**Impersonation** is when an attacker pretends to be someone they're not in order to earn your trust. It's the delivery mechanism behind a lot of **pretexting** (the made-up story) and **vishing** calls — e.g., "Wendy from Microsoft Windows" warning of computer problems, a "US Treasury enforcement action," or a "you qualify for 0% interest" credit-card call.

#### Common Impersonation Angles
- **Authority/peer trust** — Claims to be from your **help desk**, or a **higher-up** (VP of Finance, head of manufacturing) so you comply without thinking.
- **Technical baffling** — Uses big words or technical detail to keep you focused on the content instead of *whether the call is real*.

#### Eliciting Information
The goal is to **elicit** a detail you wouldn't normally hand over (credit card, SSN, bank details). The attacker can't just ask outright — they wrap the request in a **story** (e.g., "there's a problem sending a payment, so I need your bank account number") so you trust them enough to share.

#### Identity Fraud
Using your private info to pretend to *be you*:
- **Credit card fraud** — Opens a card in your name but with *their* address; buys goods with no intent to pay → shows up as nonpayment on **your** credit report.
- **Bank account fraud** — Opens an account in your name to stash illicit funds, then takes out **loans** under your name.
- **Government/tax fraud** — Files your tax return as you and collects **your refund** before you can.

#### Prevention
- **Don't volunteer information** — Real support never needs your password.
- **Don't give personal info** (address, SSN, birthday) over phone/email.
- **Verify independently** — Offer to call back using a **public phone number** you look up yourself.
- Make **verification a normal process** — never take a call at face value, especially when it's after financial or sensitive details.


### 14. Watering Hole Attacks

When employees are **too well-trained** to plug in stray USB drives, click email links, or open attachments, the attacker stops trying to break *in* and instead poisons a site the victims **already visit** — then waits. ("Poison the watering hole and wait for you to come drink.")

- **Requires research** — The attacker finds a third-party site the organization uses regularly (e.g., the local coffee/sandwich shop ordering page) and compromises it, often via a **vulnerability** in that third party or by phishing the third party first.
- **Targeted poisoning** — They may infect the site for everyone, but the connection they actually want is *your* organization's.
- **Real example (Jan 2017):** Attackers compromised the **Polish Financial Supervision Authority**, **Mexico's National Banking and Stock Commission**, and a **state-owned bank in Uruguay**, planting malicious **JavaScript**. It only targeted **specific IP ranges** tied to financial organizations — everyone else saw the normal site. The full results were never made public.
- **Defense: Defense in Depth (layered security)** — No single control stops a watering hole attack. Stack **antivirus + firewall + IPS** and more so that if one layer misses the threat, another catches it. (In the 2017 case, Symantec AV flagged the malicious code for affected visitors.)


### 15. Other Social Engineering Attacks

#### Misinformation & Disinformation
Unlike a difference of opinion, these contain **factually incorrect** details, usually crafted to **divide or confuse** groups of people.
- **Influence campaigns** — Often run on social media around political/social issues; documented cases involve a **nation state** sowing dissension to push a false belief or distract from a damaging truth. Can also arrive through **advertising**.
- **How the misinformation engine works:**
  1. Create many **fake user accounts** (all controlled by the attacker).
  2. Post content from a fake account.
  3. **Amplify** it with likes/shares/follows.
  4. Social media **algorithms** push the popular post to more people.
  5. **Real users** see it and share it further.
  6. **Mass media** notices its popularity and writes stories — legitimizing something that started as misinformation.

#### Brand Impersonation
Attackers exploit instantly recognizable **brand names** (Coca-Cola, McDonald's) by creating hundreds or thousands of lookalike sites. Google indexes them, so a brand search can **redirect** you to an impersonated site — often a pop-up ("You won!" / "special offer" / "download this software") that installs **malware**, serves ads, tracks browsing, or **exfiltrates data**.


### Threat Vector Quick-Reference Table

| Vector Category | Examples | Key Defense |
|---|---|---|
| **Message-based** | Email, SMS, IM/DM phishing | User training; link/sender filtering |
| **Image-based** | Malicious SVG (embedded JS/HTML) | Input validation; XSS protection |
| **File-based** | Executables, PDFs, ZIP/RAR, Office macros, extensions | Disable macros; scan files; vet extensions |
| **Voice calls** | Vishing, SPIT, war dialing | Awareness; verify callers |
| **Removable media** | Malicious USB, HID "keyboard" attack | Disable USB ports; endpoint control |
| **Vulnerable software** | Client executables, agentless/web apps | Patch promptly; secure servers |
| **Unsupported systems** | End-of-life OS/apps | Inventory + network scans |
| **Network** | Weak Wi-Fi, rogue APs, Bluetooth | WPA3, 802.1X, periodic scans |
| **Open ports** | TCP 80/443, extra services | Firewalls; minimize services |
| **Default credentials** | admin/admin on routers | Change defaults immediately |
| **Supply chain** | Tampered/counterfeit gear, MSP compromise | Vendor vetting; verify hardware |
| **Human / social engineering** | Phishing, typosquatting, pretexting, vishing, smishing | User training; verify sender; don't click email links |
| **Impersonation** | Fake help desk/exec calls, eliciting info, identity fraud | Don't volunteer info; verify via public number |
| **Watering hole** | Poisoning a trusted third-party site the org visits | Defense in depth (AV + firewall + IPS) |
| **Misinformation / brand abuse** | Influence campaigns, fake accounts, lookalike brand sites | Verify sources; avoid unknown download prompts |


### Key Takeaway

The more services, devices, messages, and third parties you expose, the larger your **attack surface**. Reducing that surface — patching, minimizing open ports, changing defaults, restricting USB/extensions, vetting your supply chain, and training users — closes off vectors before an attacker can use them.

### Quick Self-Check
1. What's the difference between a threat vector and an attack surface?
2. Why is an SVG file potentially dangerous when an ordinary image isn't?
3. How can a USB drive defeat an air-gapped network?
4. What's the difference between a *client-based* and an *agentless* vulnerable-software vector?
5. In the 2013 Target breach, what type of vector was used to get in?
6. Name three quick wins that shrink your attack surface.
7. What's the difference between typosquatting and pretexting?
8. Define vishing and smishing — what communication method does each use?
9. List three red flags that help you spot a phishing message.
10. What is impersonation, and name two ways an attacker builds false trust with it?
11. Give three forms identity fraud can take.
12. What is a watering hole attack, and why is defense in depth the recommended countermeasure?
13. Walk through the six steps of how a misinformation campaign spreads.

## Zero-Day Vulnerabilities (Objective 2.3)

### What Is a Zero-Day?

Almost every operating system and application has **security vulnerabilities that haven't been discovered yet**. People worldwide hunt for them every day — **researchers** (who share findings with developers so patches can be made) and **attackers** (who want to find them *first*).

A **zero-day attack** happens when attackers exploit a vulnerability for which **no patch exists** because the vendor doesn't even know the vulnerability is there. Until a patch is created, the attacker can keep exploiting it freely — and it's very hard to defend against a problem you don't know exists.

- **CVE (Common Vulnerabilities and Exposures)** — A public catalog of known vulnerabilities, including zero-days, at *cve.mitre.org*. Useful for tracking what's circulating.
- **Real examples (2023):** a Chrome zero-day (memory corruption + sandbox escape); a Microsoft patch for self-signed code running during the UEFI boot process (which Secure Boot should prevent); and three Apple iOS/iPadOS zero-days (sandbox escape, sensitive-information disclosure, arbitrary code execution) — many already exploited in the wild.

### Key Takeaway
A zero-day's danger is the *gap*: the window between an attacker discovering/exploiting the flaw and the vendor shipping a patch. Once disclosed, the security community races to close it.


## Objective 2.4 — Analyze Indicators of Malicious Activity

Objective 2.4 groups attacks into **malware, physical, network, application, cryptographic,** and **password** attacks, plus the **indicators** that reveal them. The sections below follow that structure.


## Malware Attacks

### What Is Malware?

**Malware** is a broad term for any software doing bad things to your system. The objectives call out these types: **virus, worm, ransomware, Trojan, rootkit, keylogger, spyware, bloatware,** and **logic bomb**.

- **Types work together, not alone.** A worm might exploit a known vulnerability to install itself, drop a **remote-access backdoor**, and then let the attacker install *more* malware for additional capabilities.
- **How it runs:** malware needs to execute on your machine — typically a clicked email link or attachment, a malicious pop-up, a **drive-by download** (downloaded just by visiting a site, no click), or a self-propagating **worm**.
- **Why it exists:** your data has value (personal files, financials, planning docs), and attackers monetize it directly or via ransom.
- **Two fundamental defenses:** keep a **known-good (offline) backup**, and **keep the OS and applications patched** with up-to-date **anti-malware signatures**.


### Ransomware
- Encrypts all your data and demands payment (often **cryptocurrency**) for the decryption key.
- The **OS keeps working normally** on purpose — the attacker wants you to see the ransom message.
- **Defense:** offline backups (so ransomware can't reach and encrypt the backup too), patched OS/apps, and current AV signatures. With a good backup you can wipe and restore.


### Virus
- **Replicates from system to system**, but **requires user action** to start (clicking a link or running an executable). Once running, it spreads through the local file system and across the network.
- Antivirus watches executables and matches them against **signature files** — keep those updated.
- **Notable variant — fileless virus:** writes nothing to disk, living entirely in **memory** to evade signature-based AV. A typical chain: user clicks a malicious link → a site exploits a browser/OS vulnerability (e.g., Flash, Java, Windows) → the virus launches **PowerShell** to pull and run further scripts in memory → adds a **registry autostart** so it survives reboots.


### Worm
- **Self-replicates with NO user intervention**, spreading at the **speed of the network** — so infections happen fast and can hit any system at any time.
- **Defense:** network and host-based firewalls plus **IPS** (must have signatures/processes to recognize the worm) to stop propagation.
- **Example — WannaCry:** a worm that *also* delivered ransomware. It found vulnerable systems, used the **EternalBlue / SMBv1** vulnerability to install a backdoor, pulled down ransomware to encrypt files, then continued spreading to other vulnerable hosts.


### Trojan (Trojan Horse)
- Software you *think* performs a legitimate function, but which actually installs malware. A **RAT (Remote Access Trojan)** is a Trojan that gives the attacker remote control.


### Spyware
- Malware that **watches everything** you do — browsing habits, on-screen activity — to serve ads, steal personal info, or commit affiliate fraud. It may also install a **keylogger**.
- **Installed via** peer-to-peer software, fake "security" software, or malicious email links.
- **Defense:** anti-malware, research software before installing, keep good backups, and use dedicated removal tools (e.g., Malwarebytes) that can both detect and remove it.


### Bloatware
- Apps **pre-installed by the hardware manufacturer** (who is usually paid to include them) — beyond the core OS apps you'd expect.
- **Security concerns:** wasted storage, programs auto-running at startup (hurting efficiency), and **vulnerabilities present on a brand-new device**.
- **Removal:** the OS uninstaller, the app's own uninstaller, or — as a last resort — third-party uninstaller software.


### Keylogger
- Captures **every keystroke** (URLs, usernames, passwords, credit-card and financial data) to a file that's periodically sent to the attacker.
- Effective because **keyboard input isn't encrypted**, even when network traffic and stored files are.
- Can also capture the **clipboard, screenshots, instant messages, and search queries**. (Often bundled into a RAT such as DarkComet.)


### Logic Bomb
- Malicious code that **waits for a trigger event**, then detonates — a specific **date/time**, or a condition like **a particular user logging in**. Detonation might reboot, erase data, or wipe the system.
- Usually planted by an **insider** with a specific goal, so it has **no AV signature** and is **hard to detect**.
- **Defense:** monitor critical OS files for unauthorized changes, and enforce **least privilege** (so users can't install one).
- **Example — South Korea, March 2013:** a malicious email attachment installed a Trojan; a day later the logic bomb fired at 2:00 PM local time, deleting storage and the **master boot record**. Infected bank ATMs rebooted to "Boot device not found. Please install an operating system on your hard disk."


### Rootkit
- Name comes from the Unix superuser **"root."** A rootkit typically **hides in the OS kernel**, becoming part of the OS itself — so it's **invisible to traditional AV** and won't appear in the process/task list.
- Some rootkits run as ordinary processes and *can* still be caught by anti-malware; **standalone removal tools** exist for specific variants.
- **Defense:** **Secure Boot** (in UEFI BIOS) checks the OS signature and confirms the kernel hasn't changed before booting — stopping a rootkit from running at startup.


### Malware Comparison Table

| Malware | User action needed? | Defining trait | Key defense |
|---|---|---|---|
| **Virus** | Yes (click/run) | Replicates, needs a host action | AV signatures; patching |
| **Worm** | No | Self-replicates across the network | Firewalls + IPS; patching |
| **Ransomware** | Varies | Encrypts data, demands payment | Offline backups; patching |
| **Trojan** | Yes | Disguised as legitimate software | Vet software; anti-malware |
| **Spyware** | Yes | Watches/steals activity | Anti-malware; trusted sources |
| **Bloatware** | No (pre-installed) | Unwanted vendor apps | Uninstall promptly |
| **Keylogger** | Yes | Captures keystrokes | Anti-malware; MFA |
| **Logic Bomb** | No (insider-planted) | Triggers on an event | File monitoring; least privilege |
| **Rootkit** | Yes | Hides in the kernel | Secure Boot; removal tools |

### Quick Self-Check
1. What's the core difference between a virus and a worm?
2. Why is a *fileless* virus harder for antivirus to catch?
3. Why does ransomware leave the operating system working?
4. What makes a logic bomb so difficult to detect, and which actor usually plants one?
5. Where does a rootkit hide, and what UEFI feature counters it?


## Physical Attacks

Security isn't only digital. **Physical access to a computer means full control** — anyone who can touch the machine can circumvent the OS. ("Door locks only keep out the honest people.")

- **Brute force (physical):** forcing a locked door or window to reach infrastructure. Worth testing against your own data center — how hard is it to physically break in?
- **RFID cloning:** access badges and key fobs commonly use **RFID**. Cloners cost **under \$50** and copy a card in **seconds** — there are documented cases of cloning a badge just by brushing against someone. **Defense:** **multifactor authentication** — a cloned card alone won't supply the PIN or biometrics.
- **Environmental:** attack the systems' surroundings. Examples: cutting **power** (sometimes from outside the building), tampering with **HVAC** to overheat systems until they auto-shut-down, or triggering **fire-suppression** to cause a denial of service.


## Network Attacks

### Denial-of-Service (DoS) and Distributed DoS (DDoS)

A **DoS** forces a service to fail — by **overloading** it or exploiting a **known vulnerability/design flaw**. It can be competitive (knocking a rival offline) or a **smokescreen** while another exploit runs elsewhere. Some DoS conditions are trivial (cutting power, a switching **loop** without spanning tree, saturating a slow link) and some are accidental (a burst water line over the data center).

A **DDoS** uses **many devices worldwide** at once — **botnets** (robot networks of malware-infected machines) the attacker commands with a single instruction. This is an **asymmetric threat**: few attacker resources can overwhelm a much larger target. *(The Zeus botnet peaked at ~3.6 million controlled computers.)*

- **Amplified / Reflected:** send a **small request** that returns a **large response**, multiplying traffic. Common with **NTP, DNS, and ICMP**.
- **DNS amplification example:** a ~15-character query can return ~1,300 characters (~86× amplification). Attackers use **open DNS resolvers** and **spoof the victim's IP** so the large replies flood the victim.


### DNS Attacks

DNS resolves a fully qualified domain name (FQDN) to an IP address. Attackers redirect users to the wrong IP by **poisoning** that resolution. Methods, roughly easiest to hardest:

- **Modify the client host file** — needs access to the machine and elevated rights; the local host file is consulted before querying DNS.
- **On-path interception** — sit in the middle and answer DNS queries in real time with a malicious IP.
- **Modify the DNS server** — DNS servers are well protected, so this is **less common**.
- **Compromise the domain registration** — control *every* DNS record for the domain. **Example — Oct 22, 2016:** attackers changed **36 domains** at a Brazilian bank's registrar for ~6 hours, effectively *becoming* the bank's IP addresses and harvesting credentials and financial data.

**URL hijacking / typosquatting / brandjacking:** registering **misspelled or lookalike domains** (e.g., *professormessor* vs. *professormesser*, a dropped/added letter, or a different TLD like *.org*). Uses include ad revenue, reselling the domain, redirecting to a competitor, fake login pages, and malware. **Don't click email links** — check the domain carefully.


### Wireless Attacks

- **Deauthentication attack:** a DoS that repeatedly disconnects users from Wi-Fi. It exploits **802.11 management frames**, which older versions of the standard sent **in the clear** (no encryption), letting an attacker forge them. **Fix:** **802.11ac and newer** encrypt key management frames (disassociate, deauthenticate, channel-switch). Some frames (beacons, probes, authentication, association) still appear in the clear because they must precede encryption.
- **RF jamming:** a DoS that **interferes with the wireless signal**, lowering the **signal-to-noise ratio** so devices can't hear the access point. Causes range from microwave ovens and fluorescent lights to deliberate jamming (constant, random, legitimate-frame floods, or *reactive* jamming that turns on only when someone transmits). The jammer must be **local**, so it's tracked down by triangulating with a **directional antenna** (and an attenuator to refine direction).


### On-Path Attacks (Man-in-the-Middle)

An **on-path attack** puts the attacker **between two devices**, relaying — and optionally **modifying** — traffic in real time, while remaining **invisible** to both victims.

- **ARP poisoning / spoofing:** works on the **local subnet**, and is easy because **ARP has no authentication or encryption**. The attacker sends spoofed ARP replies claiming the gateway's IP maps to the **attacker's MAC address**; victims overwrite their ARP caches, so traffic flows through the attacker.
- **On-path browser attack (man-in-the-browser):** malware/Trojan **on the victim's own device** acts as a proxy. Because it runs locally, it sees data **in the clear even when the network is encrypted**, then waits for a high-value login (like online banking) and runs hidden sessions.


### Credential Replay (Replay Attacks)

A **replay attack** captures legitimate information and **reuses it** to pose as the victim. Capture methods include a **network tap**, **ARP poisoning**, or **malware** on the victim. (The replay itself isn't an on-path attack, but on-path is often used to *gather* the data.)

- **Pass the hash:** capture a username and **password hash** during authentication, then replay them to the server. **Defense:** encrypt the traffic, and **salt** the password so each authentication uses a different salt/hash; configure the server to reject a repeated hash.
- **Session hijacking / sidejacking:** steal the **session ID** from browser **cookies** and reuse it — the server treats the attacker as the already-logged-in victim. **Defense:** end-to-end encryption (**HTTPS**), browser extensions that force HTTPS, or at minimum a VPN to a concentrator.


### Malicious Code

A catch-all for executables, scripts, macro viruses, Trojans, and more. Because the methods are so varied, you need **defense in depth**: anti-malware, firewalls, continuous patching, and **user training**.

- **WannaCry** — used the **SMBv1** vulnerability for arbitrary code execution, then ran ransomware.
- **British Airways** — attackers placed **22 lines of malicious JavaScript** (**cross-site scripting**) on the checkout page; ~**380,000** customers' cards were potentially exposed.
- **Estonian Central Health Database** — breached with **SQL injection**, exposing the health data of the entire country.


### Network Attack Quick-Reference Table

| Attack | What it does | Key defense |
|---|---|---|
| **DoS / DDoS** | Overloads or crashes a service (DDoS uses botnets) | Patching; upstream filtering; redundancy |
| **Amplified / Reflected** | Small request → large spoofed response floods victim | Secure/close open resolvers; filtering |
| **DNS attacks** | Redirects users to a malicious IP | Protect host files, registrar accounts; verify domains |
| **Typosquatting** | Lookalike/misspelled domains | Don't click email links; inspect URLs |
| **Wireless (deauth / jamming)** | Disconnects or drowns out Wi-Fi | WPA3 / 802.11ac+; locate the source |
| **On-path / ARP poisoning** | Intercepts and modifies traffic in the middle | Encryption; secure switch/ARP controls |
| **Credential replay / pass the hash** | Reuses captured credentials | Encryption; salting; reject repeated hashes |
| **Malicious code** | Code execution via many vectors | Defense in depth; patching; training |

### Quick Self-Check
1. What makes a DDoS an "asymmetric" threat?
2. How does DNS amplification turn a small query into a flood aimed at a victim?
3. Why is ARP poisoning relatively easy to perform?
4. Which 802.11 weakness enables a deauthentication attack, and what fixed it?
5. How does salting defeat a pass-the-hash replay?


## Application Attacks

- **Injection:** inserting malicious code into input the application fails to validate. The most common is **SQL injection (SQLi)**; others include **HTML, XML,** and **LDAP** injection.
- **Buffer overflow:** writing **more data than a variable can hold**, so the excess spills into adjacent memory — enabled by missing input checks. It usually just **crashes** the app; but a **reliable, repeatable** overflow is a powerful attack.
- **Replay:** capturing and re-sending data (a username/hash or a session ID) to gain access; often paired with an on-path attack to gather the data first.
- **Privilege escalation:** exploiting a bug to gain rights you shouldn't have.
  - **Vertical** — moving up to **administrator/SYSTEM** rights.
  - **Horizontal** — moving sideways from one user's access to another user's.
  - **Defenses:** patch the vulnerability, update AV signatures, **Data Execution Prevention (DEP)** (limits where code can run in memory), and **ASLR** (randomizes memory locations each run). *Example: CVE-2023-29336, a Win32k elevation-of-privilege flaw (May 2023) granting SYSTEM privileges.*
- **Forgery — Cross-Site Request Forgery (CSRF / XSRF, "sea surf"):** abuses the fact that a site **already trusts your authenticated browser**, tricking the browser into making requests on the attacker's behalf (e.g., a bank funds transfer triggered by a malicious link you click while logged in). **Defense:** an **anti-forgery cryptographic token** required with each request.
- **Directory traversal:** a **web-server misconfiguration** (or server-software vulnerability) lets an attacker read/write files **outside the web root**. The `../` sequence moves up a directory — seeing `../../windows/system.ini` in a request URL is a classic indicator.

### Quick Self-Check
1. What single root cause enables both injection and buffer-overflow attacks?
2. Distinguish vertical from horizontal privilege escalation.
3. How does an anti-forgery token stop CSRF?
4. What does the `../` pattern in a web log suggest?


## Cryptographic Attacks

Modern algorithms are **public and well-vetted**; if a weakness is found, the algorithm is **abandoned**. So attackers usually target the **implementation**, not the key.

- **Birthday attack → Collision:** based on the **birthday paradox** (in a room of 23 people there's ~50% chance two share a birthday). A **hash collision** is when **two different plaintexts produce the same hash**, typically found by brute force. **Defense:** use a **larger hash output size**. *Example: MD5 (1992) had collisions found by 1996; in Dec 2008 researchers forged a CA-signed certificate via an MD5 collision, and the industry abandoned MD5.*
- **Downgrade:** force two devices to use **weaker or no encryption**. The classic form is **SSL stripping** (combined with an on-path attack): the attacker keeps the victim on plain **HTTP** while talking **HTTPS** to the server, so the attacker reads and can modify everything — including credentials — passing through.

### Quick Self-Check
1. What defines a hash collision, and how does a larger hash help?
2. Why must an attacker also be **on-path** to perform SSL stripping?


## Password Attacks

Never store passwords as plain text ("**in the clear**") — anyone who reaches the file or database instantly has everyone's credentials. Store a **hash** instead.

- **Hashing basics:** a variable-length input becomes a **fixed-length** string (a **message digest / fingerprint**). A different input yields a **very different** hash, and the process is **irreversible** — you can't reconstruct the password from the hash. *(SHA-256 is a common choice.)*
- **Spraying:** try a **few of the most common passwords** across **many accounts** — only a couple of attempts per account, so no lockouts or alarms. Think of it as the **opening move** of a brute force.
- **Brute force:** try **every possible combination** until a hash matches. **Online** brute force is slow and risks lockouts (maybe one or two tries a day), so attackers **steal the password/hash file and brute-force it offline**, where there are no lockouts and unlimited attempts — limited only by **time and compute**.

### Quick Self-Check
1. Why are hashes preferable to plaintext for stored passwords, even if an attacker steals the file?
2. How does a spraying attack avoid triggering account lockouts?
3. Why is offline brute force so much more dangerous than online?


## Indicators of Malicious Activity (Indicators)

An **Indicator of Compromise (IOC)** is evidence giving you **high confidence** that a system has been breached. The objectives list these specific indicators:

- **Account lockout** — locked from failed attempts **you didn't make**, or **administratively disabled** with no authorized reason. May be a setup to social-engineer the **help desk** into resetting the password for the attacker.
- **Concurrent session usage** — the same user logged in from **two places at once**. (Can be legitimate — multiple devices or a **service login** — and is hard to track; an account-access report, like Gmail's, helps confirm.)
- **Blocked content** — you suddenly **can't reach security sites or download patches/signatures**, a sign malware has disabled updates to keep itself resident.
- **Impossible travel** — logins from **far-apart locations in an impossible timeframe** (e.g., Omaha, then Australia minutes later). Spotted by reviewing **authentication logs**.
- **Resource consumption** — an unexpected **spike in traffic** (e.g., large transfers at 3 AM = likely exfiltration). **Firewall logs** show the flow, IPs, and timeframe — often the first sign of an intruder.
- **Resource inaccessibility** — a server **crashed** by a vulnerability probe, **network disruption**, **ransomware-encrypted** files, or a **locked account** after a brute-force attempt.
- **Out-of-cycle logging** — log events at **unexpected times**, such as patches/apps installed **off the normal change schedule**, or off-hours firewall transfers.
- **Missing logs** — attackers **delete logs** to hide their activity; **alert on missing log data**, since each action normally leaves a log.
- **Published / documented** — your **private data suddenly appears publicly online**. Often part of ransomware **double extortion**: attackers **exfiltrate** data *before* encrypting it, then threaten to **leak** it unless paid.


### Indicator Quick-Reference Table

| Indicator | What you'd notice | Likely meaning |
|---|---|---|
| **Account lockout** | Locked without your attempts; admin-disabled | Brute force, or a help-desk reset scam setup |
| **Concurrent session usage** | Same user in two places at once | Stolen credentials (or legit multi-device) |
| **Blocked content** | Can't reach security sites/updates | Malware disabling defenses |
| **Impossible travel** | Logins too far apart, too fast | Compromised credentials |
| **Resource consumption** | Traffic spikes, odd-hour transfers | Data exfiltration in progress |
| **Resource inaccessibility** | Crashes, encrypted files, locked accounts | Active exploitation / ransomware |
| **Out-of-cycle logging** | Events at unexpected times | Unauthorized changes |
| **Missing logs** | Gaps in log data | Attacker covering tracks |
| **Published/documented** | Private data appears online | Successful exfiltration / leak |

### Key Takeaway
No single indicator is proof on its own, but together they tell a story. **Log everything**, **report on it**, and **alert when logs go missing or appear out of cycle** — that's often the only way you'll catch a quiet intrusion.

### Quick Self-Check
1. Why might an attacker *want* your account to be locked out?
2. How do firewall logs reveal data exfiltration?
3. What is "impossible travel," and which logs expose it?
4. Why are *missing* logs themselves an indicator of compromise?
5. How does the "published/documented" indicator tie into ransomware double-extortion?


## Mitigation Techniques (Objective 2.5)

### What Is Mitigation?

**Mitigation** is the process of **reducing the impact** of a security event — or a *potential* one. The techniques below stack together: the goal is to stop attacks before they happen, limit how far they can spread, and shrink how much an attacker can reach if they do get in.


### Segmentation

Breaking the network into **smaller pieces** to limit the scope of any security event.

- **Physical segmentation** — physically separate devices.
- **Logical segmentation** — commonly **VLANs** on network switches.
- **Virtual segmentation** — common in cloud and virtual-machine architectures.

**Why segment?**
- **Performance** — dedicate a subnet to a high-bandwidth application so other traffic doesn't affect its throughput.
- **Security (strategic)** — e.g., users shouldn't talk **directly** to a database server; they go through an **application server**, which talks to the database. A firewall or control list enforces who can reach each tier.
- **Compliance** — a mandate may require it. Under **PCI (Payment Card Industry)** compliance, credit-card data must be kept **completely separate** from the rest of the network.


### Access Control — ACLs & Permissions

An **Access Control List (ACL)** allows or disallows traffic through a network, operating system, or other technology.

- **Match criteria:** source IP, destination IP, port numbers, time of day, or other details.
- **Basis:** by **IP address** (certain addresses reach certain addresses; others are blocked) or by **user** (a regular user is denied, while an **administrator/super user** is permitted).
- **Watch your own access:** account for *all* connections when writing rules — don't build an ACL that **locks you out** of creating future ACLs.
- **Granularity example:** *Bob* can read files on a resource; *Fred* can access the network; *James* can reach **192.168.1.0/24** using **only TCP 80, 443, and 8088**. That last rule shows how specific controls can get.
- **ACLs live in operating systems too** — setting file/folder **permissions**, or creating **groups** and adding users to them, is configuring an ACL.


### Application Allow List / Deny List

A form of segmentation based on the **applications** themselves — ensuring only legitimate apps run and blocking everything else (Trojans, malware, viruses). Two opposite philosophies:

- **Allow list** — **nothing runs unless it's specifically approved** (restrictive; only what's on the list executes).
- **Deny list** — **everything runs except what's specifically blocked.** Your **antivirus/anti-malware** is a deny list: it lets everything run until it spots known-bad code, then blocks it.

**How Windows identifies what to allow/deny:**
- **Application hash** — identifies an app by its **hash**, not its name. If the app changes, the hash no longer matches and the rule won't apply.
- **Digital signature** — allow apps signed by a trusted publisher (Microsoft, Adobe, Google); block anything unsigned.
- **Drive path** — allow an app only from a specific directory; block it if it runs from elsewhere.
- **Network zone** — allow certain apps depending on whether you're on a **private** or **public** network.


### Isolation

**Separating a problem system from everything else** so a threat can't spread or reach resources. When endpoint protection (EDR) detects malicious code, it can **isolate the system** and **quarantine** the threat automatically. Configuration enforcement can also isolate a non-compliant device into a **private VLAN** until it's fixed (see *Configuration Enforcement*).


### Patching

Stopping an attack **before it happens** by closing known vulnerabilities — which often improves **stability** too.

- Vendors like **Microsoft** release patches **monthly**; application developers and device manufacturers ship their own.
- **Home/consumer** systems often patch **automatically** (the OS checks, downloads, and installs without user action).
- **Large organizations** usually **don't** auto-patch — IT **tests patches first**, then deploys them widely.
- **Emergency patches** ship off-schedule when a vulnerability is severe and being **actively exploited**.


### Encryption

Limiting how much usable data an attacker can obtain.

- **File/folder-level encryption** — built into the file system; encrypt just a specific file or folder. In Windows this is **EFS (Encrypting File System)**.
- **Full Disk Encryption (FDE)** — encrypts the **entire volume**, including the OS and user files. Good for devices that **leave the building**. Tools: **BitLocker** (Windows), **FileVault** (macOS).
- **Application-level encryption** — the app encrypts its own data, so it's protected **regardless** of whether file-system or full-disk encryption is in place.


### Monitoring

Logging everything happening on the network so you can identify security events.

- Monitoring can be **built into** switches, routers, and other devices, or done with a **separate sensor**.
- Security devices (firewalls, IPS, authentication systems) generate logs automatically.
- Because logs are **spread across many systems**, consolidate them with a **SIEM (Security Information and Event Manager)** — a central source for reporting and monitoring.


### Least Privilege

Limiting each user's rights and permissions to **exactly what their job role requires** — nothing more.

- **Best case:** no one runs with administrative permissions; rights are **elevated temporarily** when needed, then brought back down.
- **Why it matters:** if malware or an attack hits a user's account, the damage is **limited to what that user can see** — the difference between a breach exposing a *little* data and *all* of it.


### Configuration Enforcement

Checking a system's security state, usually at login, via a **posture assessment** before granting access.

- **What it checks:** latest OS version and patches, up-to-date **antivirus/EDR signatures**, local firewall and EDR enabled, and sometimes a **certificate** proving the device is trusted.
- **If it fails:** the system is **quarantined** or placed in a **private VLAN** where it can be brought up to standard. After fixing, it re-attempts login; once the posture assessment passes, it gets network access.


### Decommissioning

Safely retiring equipment that's past its usable life — the concern is **sensitive data left on storage**.

- **Recycle within the org:** an SSD can be moved to another system (you may need to **format** it first).
- **Sanitize or destroy:** if a drive is no longer needed and may hold sensitive data, **destroying** it guarantees no one can recover the data.


### Core Mitigation Quick-Reference

| Technique | Purpose |
|---|---|
| **Segmentation** | Limit scope; separate tiers/data (physical, VLAN, virtual) |
| **Access control (ACL/permissions)** | Allow/deny traffic and resource access by IP, user, port, time |
| **Application allow/deny list** | Control which apps may run (by hash, signature, path, zone) |
| **Isolation** | Quarantine a compromised/non-compliant system |
| **Patching** | Close known vulnerabilities before exploitation |
| **Encryption** | Protect data at the file, disk, or application level |
| **Monitoring** | Log and consolidate activity (SIEM) to spot events |
| **Least privilege** | Limit user rights to the job role; contain breaches |
| **Configuration enforcement** | Posture-check devices at login; quarantine non-compliant |
| **Decommissioning** | Remove/destroy sensitive data when retiring equipment |

### Quick Self-Check
1. Name the three forms of segmentation and one reason (performance, security, compliance) you'd use it.
2. What four kinds of criteria can an ACL match on?
3. Contrast an allow list with a deny list — which one is your antivirus?
4. Why does least privilege limit the *scope* of an attack?
5. What does a posture assessment check, and what happens to a device that fails it?
6. When decommissioning a drive with sensitive data you no longer need, what's the safest option?


### Hardening Techniques

Hardening means making a device **more secure** by reducing its attack surface. Different platforms (Windows, Linux, macOS, mobile) need their own approach, and there's never one button — you layer many tools for **defense in depth**.

**System hardening best practices (servers & workstations):**
- **Apply security updates** — the single most significant step; OS and patches, released monthly by most vendors.
- **Secure user accounts** — enforce a **password policy** (minimum length and complexity, e.g., 8+ characters with upper/lowercase, numbers, special characters).
- **Limited access** — not every account is an administrator; apply **least privilege**.
- **Restrict remote access** — permit only an approved **IP address range**; deny others.
- **Endpoint protection** — run antivirus/anti-malware/EDR.

The objective calls out these specific hardening techniques:

- **Installation of endpoint protection (EDR)** — *Endpoint Detection and Response*. With ~1 million+ new malware variants daily, signatures alone don't scale, so EDR adds **behavioral analysis**, **machine learning**, and **process monitoring** to catch malware with **no signature**. It can perform **root-cause analysis** and then **act automatically** — isolate the system, quarantine the threat, even **roll back to a previous config** — all driven through an **API** and reported to a central console.
- **Host-based firewall** — a **software** firewall on the OS itself, allowing/blocking **inbound and outbound** traffic per process. Because it sits on the host, it sees data **before/after encryption**, can flag **unknown processes** (a malware sign), and is centrally managed.
- **Host-based intrusion prevention system (HIPS)** — looks for **known attacks** inbound to the device (often built into EDR/anti-malware). Using **signatures** and **heuristics/behavior**, and with visibility into the OS, it can detect a **buffer overflow**, a **registry change**, or modifications to **core OS files**, then alert and block the process.
- **Disabling ports/protocols** — every outward-facing service **opens a port**, and each open port is an opportunity for an attacker. **Close as many as possible**; use a firewall (ideally a **next-generation firewall** that controls by *service*, not just port number). Ports can open **without your knowledge** during OS/app installs — beware software that asks you to open **ports 0–65535** (every port). Use a scanner like **Nmap** to find what's actually open.
- **Default password changes** — routers, switches, firewalls, access points, and app management consoles ship with **default credentials** that attackers can **easily look up**. Some prompt you to change the password on first login, but **not all do** — change them **manually**. Add **multifactor** or **centralized authentication** where possible.
- **Removal of unnecessary software** — every app carries potential bugs/vulnerabilities, and each has its **own update process**, making them hard to keep current. The easy fix: **delete apps you no longer use** — removing the app removes its security risk.
- **Encryption** — also a hardening step: file-level (**EFS**), full-disk (**BitLocker**, **FileVault**), network (**VPN**), and built-in application encryption like **HTTPS** in the browser. *(See the Encryption section above.)*


### Hardening Checklist

| Hardening technique | What it does |
|---|---|
| **Security updates / patching** | Closes known vulnerabilities (most impactful step) |
| **Endpoint protection (EDR)** | Behavioral + ML detection; auto-isolate, quarantine, roll back |
| **Host-based firewall** | Per-process inbound/outbound control on the host |
| **HIPS** | Blocks known attacks; watches OS files, registry, overflows |
| **Disable ports/protocols** | Closes unused open ports; scan with Nmap |
| **Default password changes** | Replaces easily-guessed factory credentials |
| **Remove unnecessary software** | Eliminates unused apps' vulnerabilities and update burden |
| **Encryption** | Protects data at file, disk, network, and app levels |
| **Password policy + limited access** | Enforces length/complexity and least privilege |
| **Restrict remote access** | Permits only approved IP ranges |

### Key Takeaway
No single control hardens a system. You **layer** them — patch, encrypt, run EDR + host firewall + HIPS, close ports, change defaults, remove unused apps, and enforce least privilege — so that if one layer misses a threat, another stops it. That's **defense in depth**.

### Quick Self-Check
1. Why is applying security updates considered the most significant hardening step?
2. How does EDR go beyond traditional signature-based antivirus?
3. Where does a host-based firewall sit, and why does that give it full visibility?
4. What can a HIPS detect that a network IPS might miss?
5. Why is leaving ports 0–65535 open such a bad idea, and how do you find open ports?
6. Name three places encryption can be applied as a hardening measure.