### Common Vulnerability Scoring System
## 1️⃣ **Attack Vector (AV)**

👉 _How close does the attacker need to be?_

| Value            | Meaning                   | Example                        |
| ---------------- | ------------------------- | ------------------------------ |
| **Network (N)**  | Over the internet         | SQL injection, RCE via website |
| **Adjacent (A)** | Same network (Wi-Fi, LAN) | ARP spoofing                   |
| **Local (L)**    | On the same machine       | Malicious local script         |
| **Physical (P)** | Physical access required  | Plugging in USB                |

🧠 **Rule of thumb**:  
The farther away the attacker can be, the **more severe** the vulnerability.

---

## 2️⃣ **Attack Complexity (AC)**

👉 _How hard is it to successfully exploit?_

|Value|Meaning|Example|
|---|---|---|
|**Low (L)**|Works every time|Simple buffer overflow|
|**High (H)**|Needs special conditions|Race condition, timing attack|

🧠 **Low complexity = easier exploit = higher severity**

---

## 3️⃣ **Privileges Required (PR)**

👉 _What access does the attacker need beforehand?_

|Value|Meaning|Example|
|---|---|---|
|**None (N)**|No login needed|Public API exploit|
|**Low (L)**|Normal user|Authenticated user bug|
|**High (H)**|Admin/root required|Privilege escalation|

🧠 **Fewer required privileges = worse vulnerability**

---

## 4️⃣ **User Interaction (UI)**

👉 _Does the victim need to do something?_

|Value|Meaning|Example|
|---|---|---|
|**None (N)**|Fully automatic|Wormable exploit|
|**Required (R)**|User must click/open|Phishing attachment|

🧠 If the user must act, severity goes **down**.

---

## 5️⃣ **Scope (S)**

👉 _Can the exploit affect more than the vulnerable component?_

|Value|Meaning|Example|
|---|---|---|
|**Unchanged (U)**|Same system only|App exploit stays in app|
|**Changed (C)**|Breaks isolation|Browser → OS escape|

🧠 **Changed scope is very serious** — it means sandbox escape or privilege crossing.

---

# 💥 Impact Metrics

These describe **what happens after exploitation**.

---

## 6️⃣ **Confidentiality (C)**

👉 _Can data be read or stolen?_

|Value|Meaning|
|---|---|
|**None (N)**|No data access|
|**Low (L)**|Some data exposed|
|**High (H)**|All sensitive data exposed|

📌 Examples:

- Reading password hashes → **High**
    
- Seeing non-sensitive info → **Low**
    

---

## 7️⃣ **Integrity (I)**

👉 _Can data be modified or corrupted?_

|Value|Meaning|
|---|---|
|**None (N)**|No modification|
|**Low (L)**|Limited modification|
|**High (H)**|Full control over data|

📌 Examples:

- Changing config values → **High**
    
- Minor log tampering → **Low**
    

---

## 8️⃣ **Availability (A)**

👉 _Can the system be disrupted or taken down?_

|Value|Meaning|
|---|---|
|**None (N)**|No impact|
|**Low (L)**|Temporary slowdown|
|**High (H)**|Complete DoS/crash|

📌 Examples:

- App crash → **High**
    
- Slight performance degradation → **Low**

### Malware propagation
- **virus**: spreads by attaching to legitimate programs
- **worm**: self replicates and spread independently
- **trojan**: disguises itself as a legitimate program
	- remote access trojan (RAT)

### Malware Payloads
- **spyware**: gather's user info
- **adware**: displays unwanted ads
- **ransomware**: encrypts user data for ransom 

### Logic bombs
![[Pasted image 20260123092523.png]]

### Backdoors
- means to grant future access
- mechanisms
	- hardcoded accounts
	- default passwords
	- unknown access channels

### Advances Malwares
- **polymorphic viruses:** change to avoid signature detection
	- different encryption key for each system 
- **armored viruses** prevent reverse engineering
	- obfuscated assembly language
	- blocking the use of system debuggers
	- preventing the use of sandboxing 

### Rootkits
- gain root access + software techniques designed to hide other software
- rootkit payloads
	- backdoors, botnet agents, spyware, antitheft
	- user mode vs kernel mode 

### Botnets
- collection of zombie computers used for malicious purposes
- steal computing power or storage network connectivity
- indirect and high redundant command and control channels 
![[Pasted image 20260123092908.png]]

### Malware prevention
- **signature detection** 
	- suspicious activity
- **behaviour detection**
	- deviation from normal activity
- **endpoint detection and response** (EDR) solutions
	- installed agents watch for signs of malicious activity
	- automated responses triggers
	- sandboxing executables 

### Attack vectors and attack surface
- vector - the path to obtain initial access
- surface - the entire area of that is susceptible to hacking
- email
	- phishing and malicious attachments/links
- social media
	- malware spread and influence campaigns
- removable media
	- usb drive spreading malware
- magnetic stripe cards
	- skimming attacks at ATM/card readers
- cloud services
	- accessing insecure files and exploiting security flaws
- physical access
	- unsecured network jacks or endpoint devices
- IT supply chain
	- pre-delivery device tampering for backdoor access
- wireless networks
	- exploitation of insecure wireless networks 

### social engineering
- reason for success
	- authority
	- intimidation
	- conesusus
	- scarcity
	- urgency
	- familiarity

### impersonation attacks
- spam
- phishing
	- spear phishing - highly targeted attacks
- prepending
	- adding fake safe tags
- credentials harvesting
	- use compromised credentials
- spoofing 

### zero day vulnerability
- a vulnerability that is discovered but not patched uet

### advanced persistent threats
- characteristics
	- customized code
	- focused objectives
	- multi threat capability
	- human intervention
	- low and slow
