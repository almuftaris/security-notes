# Cyberattacks leading up to and following the start of operation 'Epic Fury'

**Date**: March 3, 2026
**Context**: Joint US-Israeli military strikes "Operation Epic Fury" and "Operation Roaring Lion" against Iran

## Timeline of Events

**February 28, 2026**:
- **09:52-10:14 local time**: BadeSaba Calendar app compromised, sends surrender notifications to 5M+ Iranian users
- **Afternoon**: Iranian internet connectivity drops to **4% of normal levels**
- **Evening**: State media outlets (IRNA, ISNA, Tasnim) taken offline with defacements

**March 1, 2026**:
- **DHS issues critical alert**: Warning of retaliatory cyberattacks (defacements, DDoS) against US networks
- **Iranian threat groups activated**: Intelligence firms report APT33, APT42, MuddyWater retooling for destructive campaigns
- **Active attacks observed**: Handala Group targeting Israeli ICS and Jordanian fuel infrastructure, Fatimiyoun Electronic Team deploying wipers

---

## UNC5667: Middle East Targeting with New Malware Toolkit

**Attribution**: Iran-nexus threat cluster  
**Active**: January-February 2026 (pre-escalation)  
**Primary Targets**: UAE, Egypt, Israel

### **Attack Vector**: Malicious Macro Documents
VBA macro-enabled Word/Excel files using lures:
- Airline ticket themes
- Invoice documents  
- UAE construction/energy firm materials (logos, reports)
- Recycled lure content from previous campaigns

**Submission Geography**: Majority from UAE, followed by Egypt and Israel

### **New Malware Deployed**:

**RUBBERAXE** (C++ Backdoor):
- Reverse shell access
- File upload/download
- Dynamic C2 server configuration
- Adjustable check-in delays
- HTTP-based communications
- Creates mutex to prevent duplicate instances

**POTATOWALL** (Rust Backdoor):
- Uses Telegram for C2 (hard-coded bot token)
- Leverages Tokio (async runtime) and Teloxide (Telegram library)
- Commands: Change directory, execute cmd.exe, execute PowerShell
- Polls Telegram bot for commands

**LIGHTPHOENIX** (Known Malware - Updated):
- Extensive anti-analysis checks (sandbox, VM, debugger detection)
- Dual-process persistence (primary worker + watchdog backup)
- AES-encrypted C2 communications
- Reflective PE loading

### **Operational Focus**:
Google assesses targeting reflects heightened interest in:
- UAE maritime and energy infrastructure projects
- Regional geopolitical developments affecting Iranian interests
- Defense sector efforts in the Middle East

**Continuous Evolution**: UNC5667 expanding toolkit from traditional C++ to modern Rust-based implants using services like Telegram for C2 - indicates intent for operational longevity and defense evasion.

---

## UNC5667 Follow-Up: Exploit-Driven Campaign with Advanced Tooling

**Timeline**: Continued through early 2026  
**Attribution**: Same Iran-nexus UNC5667 cluster

### **Israeli Targeting Confirmed**:
Decoy webpages disguised as:
- Digital solution providers
- File-sharing services
- Probable phishing infrastructure

### **Vulnerability Exploitation**:
- **FortiOS/FortiProxy**: Critical vulnerabilities exploited
- **React2Shell**: Leveraged for initial access
- **NUCLEI**: Open-source reconnaissance tool usage

### **New Malware Toolkit**:

**DARKFLIP**: Persistence and command-and-control framework  
**DARKDAWN**: Payload delivery mechanism  
**SAINTELLER**: Network tunneling tool  
**SOFTMETAL**: C++ credential stealer (updated variant)

**Objectives**: Persistent access and sensitive data exfiltration

---

## UNC4444: Watering Holes & Credential Phishing

**Attribution**: Iran-nexus cluster (high confidence)  
**Active**: January-February 2026  
**Primary Target**: Israeli entities

### **Watering Hole Campaign**:

**January 2026**: Compromised 5+ legitimate Israeli websites
- **Sectors**: Logistics, legal, business, shipping
- **Technique**: JavaScript injection into main pages
- **Tool**: NOTESTORY fingerprinting script

**NOTESTORY Capabilities**:
- Visitor profiling via Matomo analytics (hosted on actor infrastructure: cdnbootstrap.online)
- Collected data: IP addresses, browser/device info, page navigation, referrer data
- Matomo URI patterns: "?idsite=" with setSiteID values 1, 2, 4, 5, 6, 7
- One instance used obfuscated JavaScript contacting Cloudflare Worker for additional payloads

**Late January/February**: Shifted tactics
- Removed JavaScript from some sites
- Introduced redirects to lionwheel.net (spoofing Israeli delivery company)

### **Credential Phishing Infrastructure**:

**LionWheel Spoofing**:
- lionwheel.net → credential phishing page with embedded NOTESTORY
- login.lionwheel.online → fake Microsoft 365 login with NOTESTORY

**Additional Phishing Sites**:
- **easyuploadhere.com**: Fake file-upload service, validates email addresses, adapts login button based on provider
- **sporttimes.online**: Fake streaming platform with login page and NOTESTORY fingerprinting

### **Operational Assessment**:
Google assesses UNC4444 uses credential phishing and watering holes for **victim profiling** to enable follow-on operations. Given evolving Middle East geopolitical situation, infrastructure may adapt to changing priorities and mission-specific needs.

---

## PANICPOACH: Android Malware Targeting Israeli Civilians

**Date**: March 1, 2026 (day after hostilities began)  
**Attribution**: UNC6729 (suspected Iran-nexus)  
**Delivery**: SMS phishing to Israeli citizens

### **Attack Overview**:

**Lure**: SMS messages claiming technical issues with RedAlert app (legitimate airstrike warning application)
- "A problem with receiving notifications has been resolved. Please update to the new version as soon as possible"
- Sent during active hostilities when civilians desperately need emergency alerts
- Links to bit.ly URLs → redirects to compromised WordPress site

**Trojanized App**: RedAlert.apk
- Package name: com.red.alertx (extra letter mimics legitimate com.red.alert)
- Functional RedAlert app with hidden malware embedded
- Two-stage loader: Initial APK reflectively loads PANICPOACH malware from assets

### **Data Harvesting Capabilities**:

Five separate collection modules activate immediately after permission verification:

1. **SMS Data**: Message content, timestamps, sender/recipient numbers, read status
2. **Contacts**: Names, phone numbers, last contact timestamps
3. **Precise Location**: GPS coordinates, altitude, accuracy, timestamp, mock location detection
4. **Account Information**: Usernames, emails, phone numbers, service provider identifiers
5. **Installed Applications**: Complete app inventory

**Exfiltration**: Data staged in hidden files on device before C2 transmission

---

## Massive Cyber Offensive During Military Strikes

**Date**: February 28, 2026  
**Military Operations**: "Operation Epic Fury" (US) and "Operation Roaring Lion" (Israel)

### **Iranian Infrastructure Disrupted**:

**Internet Blackout**: 
- Connectivity dropped to **4% of normal levels**
- Near-total communications disruption

**State Media Offline**:
- IRNA (Islamic Republic News Agency)
- ISNA
- Tasnim News
- Widespread defacements observed

### **Psychological Operation - BadeSaba Calendar Compromise**:

**Target**: BadeSaba Calendar mobile app (popular in Iran)  
**Impact**: 5+ million users  
**Time**: 09:52 - 10:14 local time (26-minute window)

**Push Notifications Sent**:
- Urging Iranian military personnel to surrender
- Encouraging regime defection
- Messages about defending brothers/regime criticism

**Significance**: Direct messaging to Iranian population and military during active combat operations - psychological warfare at massive scale.

---

## Iranian Retaliation: Threat Group Activation

### **Established APT Groups Retooling** (Anomali Intelligence):
- **APT33**: Activated for destructive campaigns
- **APT42**: Retooling for retaliation
- **MuddyWater**: Preparing offensive operations

### **Active Attacks Observed**:

**Handala Group**:
- Disrupted Israeli industrial control systems (ICS)
- Targeted Jordanian fuel station infrastructure
- Leaked data from Israel's Clalit healthcare network

**Fatimiyoun Electronic Team**:
- Deploying wiper malware against Western financial and energy firms
- Targeting US, Israel, allied nations

**Cyber Islamic Resistance**:
- Denial-of-service attacks
- Targeting: Financial, energy, military logistics sectors

### **US Government Response**:

**March 1, 2026 - DHS/CISA Critical Alert**:
- Warning of imminent cyberattacks against domestic networks
- Expected tactics: Website defacements, distributed denial-of-service (DDoS)
- Elevated threat posture for critical infrastructure

---

## My Takeaways

- **Cyber as warfare component**: Not separate from kinetic operations - integrated, synchronized attacks
- **Massive psychological ops**: BadeSaba compromise shows offensive cyber targeting civilian population at scale
- **Infrastructure pre-positioning**: UNC5667/UNC4444 campaigns in Jan-Feb were groundwork for Feb 28 escalation
- **Retaliation is guaranteed**: Iranian APT activation within hours demonstrates pre-planned response capability
- **Multi-vector approach**: Espionage (UNC5667), credential harvesting (UNC4444), destructive attacks (Handala, Fatimiyoun)
- **Targeting diversity**: From ICS to healthcare to fuel infrastructure - broad attack surface
- **Geopolitical cyber**: Middle East conflict driving rapid evolution of threat actor tactics and targets
- **Critical infrastructure risk**: Energy, financial, healthcare, logistics all actively targeted
- **Telegram C2 adoption**: POTATOWALL using Telegram reflects broader trend of legitimate platform abuse
- **Rust malware emergence**: Modern programming languages (Rust) becoming preferred for evasion

## Questions to Explore

- How many BadeSaba users actually saw the surrender notifications vs how many were blocked/filtered?
- What percentage of Iranian APT infrastructure was disrupted by the 4% internet blackout?
- Are Western critical infrastructure operators actually implementing DHS/CISA defensive guidance?
- How coordinated were the offensive cyber operations with kinetic strikes?
- What's the attribution for the BadeSaba compromise - US, Israel, or proxy?
- Will this escalation pattern (kinetic + cyber) become the new normal for Middle East conflicts?
- How sustainable are Iranian retaliatory cyber operations under internet blackout conditions?
