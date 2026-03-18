# Boggy Serpens (MuddyWater): Iranian Espionage Evolution & AI-Assisted Malware

**Date**: March 18, 2026
**Researcher**: Palo Alto Networks Unit 42
**Source**: [Unit 42 Threat Assessment](https://unit42.paloaltonetworks.com/boggy-serpens-threat-assessment/)
**Attribution**: Iranian Ministry of Intelligence and Security (MOIS)
**Also Known As**: MuddyWater

## Target Profile

**Geography**: Middle East, Caucasus, Central/Western Asia, South America, Europe

**Sectors**: Diplomatic missions, energy, maritime, finance, aviation, telecommunications, government, military

**Recent Victims**: Israel, Hungary, Turkey, Saudi Arabia, UAE, Turkmenistan, Egypt, South America

## Operational Evolution

**Historical Profile** (2017-2024): High-volume, low-sophistication, noisy phishing campaigns

**Current Profile** (2025-2026): Stealthier, long-term persistence focus, advanced evasion techniques

**Key Shift**: From speed-over-stealth to sustained, multi-wave targeting of high-value entities

## Case Study: UAE Marine & Energy Company

**Duration**: August 2025 - February 2026 (6 months)  
**Attack Waves**: Four distinct campaigns  
**Target**: National marine and energy company with ties to Saudi Aramco

### **Wave 1 - Engineering (Aug 16, 2025)**:
- Lure: Blurred subsea pipeline progress report
- Audience: Project engineers
- Tactic: "Enable Content" to trigger macro

### **Wave 2 - Financial (Jan 30, 2026)**:
- Lure: Excel spreadsheet with payment/cash flow projections
- Details: Local currency (AED), "Payroll Payments via WPS", transaction codes
- Audience: Finance and supply chain departments

### **Wave 3 - Travel (Jan 30, 2026)**:
- Lure: Personalized Air Arabia flight reservation (Word document)
- Details: Passenger name, flight route, "Corporate Fare" category
- Indicator: Exfiltrated internal travel itineraries from prior compromise
- Payload: GhostBackDoor malware (newly documented by Group-IB)

### **Wave 4 - Operational Logistics (Feb 11, 2026)**:
- Lure: "Consumption Report" Excel file
- Payload: Nuso (entirely new custom HTTP backdoor family)

**Significance**: Six-month persistence against single target demonstrates specific mandate to infiltrate regional maritime/energy infrastructure.

## Primary Attack Vector: Trusted Relationship Compromise

**Method**: Hijack legitimate internal accounts to bypass email filtering

**Examples**:

**Omani Ministry of Foreign Affairs (Aug 2025)**:
- Compromised mailbox used to distribute documents to foreign ministries
- Lure: "Sustainable Peace" seminar invitation (exploiting regional conflicts)
- Result: Negative spam confidence level (SCL -1) - bypassed filters

**Turkmenistan Telecom (Jan 6, 2026)**:
- Compromised internal account: info@[company].tm
- Lure: "Cybersecurity.doc" appearing as internal IT guidelines
- Result: Authenticated internal sender = bypassed spam filters

**Israeli Organizations (Nov 17, 2025)**:
- Hijacked internal accounts for webinar and HR lures
- Reported by Israeli National Cyber Directorate

**Why This Works**: 
- Email from authenticated internal account
- Recipient assumes credibility based on sender
- Bypasses reputation-based filtering
- Triggers second layer of social engineering (blurred document → "Enable Content")

## Mass Phishing Infrastructure

**Discovery**: Custom Python-based email delivery platform hosted on 157.20.182[.]75:5000

**Features**:
- Upload target email lists
- Customize sender email, subject, body
- Configure SMTP server/port
- Attach malicious files
- Preview and execute mass campaigns

**Purpose**: Automate large-scale spear phishing while maintaining granular control over identities and targets

## Malware Toolset

### **UDPGangster Backdoor**

**Communication**: UDP-based protocol (bypasses traditional TCP-focused defenses)

**Capabilities**: Command execution, data exfiltration, secondary payload deployment

**Delivery**: VBA macros in Office documents

**PDB Paths Reveal Targeting**:
- `gangster` user → Israeli aviation
- `piper` user → Azerbaijan finance
- `surge` user → Israeli telecom

**Live Operator Observation**:
- Activity at ~17:00 Tehran time (end of Iranian business day)
- Manual reconnaissance commands: `nslookup ad`, `ipconfig /all`, `dir C:\users`
- Human operator triaging host in real-time (not automated)

### **LampoRAT (Olalampo)**

**Language**: Rust  
**Disguise**: Masquerades as Kaspersky Anti-Virus (`avp.exe`)  
**C2**: Telegram Bot API (blends with legitimate HTTPS traffic)

**Bot Profile**:
- Display name: "Olalampo"
- Username: `stager_51_bot` ("51" suggests series of bots for different campaigns)
- Hardcoded token: 8398566164:AAEJbk6EOirZ_ybm4PJ-q8mOpr1RkZx1H7Q

**Capabilities**: Shell execution via `cmd.exe /e:ON /v:OFF /d /c <payload>`, captures output, returns via Telegram

**AI Development Indicator**:
- Uses emojis for status: ✅ CD to, ❌ CD error:
- Atypical for malware (usually ASCII: `[+]` or `ERROR:`)
- Characteristic of LLM-generated code for CLIs and Telegram bots
- **Strong evidence of AI-assisted malware development**

### **BlackBeard Backdoor**

**Language**: Rust  
**Purpose**: Custom backdoor for persistence and evasion  
**Significance**: Modern language choice reflects technical maturation

### **Nuso**

**Type**: Custom HTTP backdoor (entirely new family)  
**First Observed**: February 2026 (Wave 4 UAE campaign)

### **GhostBackDoor**

**Type**: Newly documented malware family (identified by Group-IB)  
**First Observed**: January 2026 (Wave 3 UAE campaign)

## VBA Macro Social Engineering

**Two-Tier Deception**:

**Tier 1**: Hijacked internal account (appears credible)  
**Tier 2**: Blurred document claiming "created in older version of Word/Excel"

**Victim Flow**:
1. Open attachment from "trusted" internal sender
2. See blurred content with "Enable Content" prompt
3. Click "Enable Content"
4. Macro deletes overlay → reveals clear, legible document
5. Victim sees legitimate-looking content (reinforces trust)
6. Malware executes silently in background

**Development Structure**: Two parallel tracks sharing infrastructure
- **Phoenix Lineage**: Fully-fledged backdoors
- **UDPGangster Operations**: Lightweight backdoors
- **Shared artifacts**: Identical decryption key, `novaservice.exe` file path

## Historical Context: False Flag Ransomware

**February 2023**: Targeted Technion Israel Institute of Technology

**Tactic**: Masqueraded as "DarkBit ransomware gang"

**Purpose**: 
- Disrupt academic infrastructure
- Mask state-sponsored origins as cybercrime
- Psychological warfare through false flags

**Significance**: Demonstrates willingness to conduct disruptive operations beyond espionage

## Takeaways

- **Six-month single-target persistence** shows operational patience and specific mandates
- **Trusted relationship compromise bypasses traditional defenses** - sender reputation filtering insufficient
- **Four distinct waves against one UAE company** demonstrates determination to infiltrate maritime/energy infrastructure
- **AI-assisted malware development confirmed** - emoji status indicators characteristic of LLM-generated code
- **Rust adoption** signals technical maturation and evasion focus
- **Telegram C2** blends with legitimate traffic, harder to detect than traditional C2
- **Mass phishing platform** enables high-volume operations with customization
- **Human operators manually triage** - not fully automated (live activity at Tehran business hours)
- **Parallel development tracks** ensure redundancy and sustained operational tempo
- **Cross-sector agility** - pivot between aviation, finance, telecom, maritime, government
- **Internal account hijacking** = negative spam scores = bypassed filters

## Questions to Explore

- How many legitimate internal accounts currently compromised for future campaigns?
- What percentage of Iranian state malware now uses AI-assisted development?
- Can Telegram Bot API C2 be detected without breaking legitimate Telegram usage?
- How does "stager_51_bot" numbering correlate to campaign/target assignments?
- What's the relationship between Boggy Serpens and Evasive Serpens (operational overlap in early 2025)?
- Are there more undiscovered backdoor families beyond Nuso and GhostBackDoor?
