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