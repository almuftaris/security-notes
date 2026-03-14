# CL-UNK-1068: Chinese Threat Actor Targeting Asia Critical Infrastructure

**Date**: March 14, 2026 (Published March 6)
**Researcher**: Palo Alto Networks Unit 42 (Tom Fakterman)
**Source**: [Unit 42 Research](https://unit42.paloaltonetworks.com/cl-unk-1068-targets-critical-sectors/)
**Active Since**: At least 2020 (6+ years undetected)

## Attribution

**CL-UNK-1068**: Previously undocumented threat cluster  
**Assessment**: Chinese threat actor (high confidence)

**Evidence**:
- Tool origin from Chinese security forums
- Simplified Chinese language artifacts in configuration files
- Longstanding targeting of Asia critical infrastructure
- Use of malware popular with Chinese-speaking actors

**Primary Objective**: Cyberespionage (moderate-to-high confidence), cybercrime not fully ruled out

## Targets

**Geography**: South, Southeast, and East Asia

**Sectors**:
- Aviation
- Energy
- Government
- Law enforcement
- Pharmaceutical
- Technology
- Telecommunications

## Attack Pattern Overview

**Initial Access**: Web shell deployment (GodZilla, modified AntSword - both Chinese-language tools)

**Lateral Movement**: Use web shells to pivot to additional hosts and SQL servers

**Data Theft Focus**:
- Website configuration files (web.config, .aspx, .dll, appsettings.json)
- Browser history and bookmarks
- Sensitive spreadsheets (XLSX, CSV) from user directories
- SQL database backups (.bak files)
- Credentials from various sources

**Exfiltration Method**:
1. Archive files with WinRAR (web.rar, web1.rar, web2.rar)
2. Base64-encode archives using `certutil -encode`
3. Print encoded content to screen via `type` command through web shell
4. Copy-paste exfiltrated data (no file upload needed - bypasses detection)

## Unique Toolset Characteristics

### **DLL Side-Loading via Python**

Technique: Deploy legitimate Python executables (python.exe, pythonw.exe) alongside malicious DLL (python20.dll) and obfuscated shellcode file

**Execution flow**:
1. Legitimate python.exe launched
2. Side-loads malicious python20.dll
3. DLL reads and deobfuscates shellcode from file
4. Executes payload in memory (no malware written to disk)

**Payloads delivered**: FRP tunnels, PrintSpoofer (privilege escalation), ScanPortPlus (custom scanner)

### **Custom FRP (Fast Reverse Proxy) Variant**

**Purpose**: Persistent access and firewall bypass

**Unique identifiers across all samples**:
- **Authentication token**: `frpforzhangwei` ("frp for zhang wei" - common Chinese name)
- **Password**: `f*ckroot123` (same across all versions)
- **Proxy naming convention**:
  - Windows: `10014-win-nic-32-v`
  - Linux: `20012-linux-64-V`, `10013-linux-64-V`

**Cross-platform**: Compiled custom versions for both Windows and Linux

### **ScanPortPlus: Custom Network Scanner**

**Language**: Written in Go  
**Platforms**: Windows and Linux versions  
**Capabilities**: IP address scanning, port scanning, vulnerability scanning

### **Xnote Linux Backdoor**

**History**: First discovered 2015, used by various Chinese threat actors  
**CL-UNK-1068 variant**: Primarily DDoS capabilities plus standard backdoor functions

**Capabilities**:
- File system interaction (upload/download)
- Shell command execution
- Reverse shell
- DDoS attacks (CC, NTP, SYN Flood, UDP Flood)
- Port forwarding
- Reverse proxy/tunneling

## Reconnaissance Operations

### **Evolution**: SuperDump (2020) → Batch Scripts (2021-present)

**SuperDump (.NET tool - used in 2020)**:
Collected comprehensive Windows host information:
- User and system details (IP, processes, drives)
- Desktop and document files
- LSASS memory dump
- Registry data for: Navicat, WinSCP, RDP, FileZilla, PuTTY, SSH configs
- PowerShell history, environment variables

**Batch Scripts (current method)**:
- **Scripts**: `hp.bat`, `hpp.bat`, `a.bat`
- Execute reconnaissance commands, save results to matching .txt files
- Additional scripts (`rar.bat`, `rr.bat`) archive all output: `rar.exe a -df host.rar *.txt`

**Signature**: Consistent naming conventions for scripts and output files across multiple years of attacks

## Credential Theft Arsenal

**Mimikatz**: Dump passwords from memory (standard tool)

**LsaRecorder**: 
- Hooks `LsaApLogonUserEx2` callback to capture login passwords
- Shared on Chinese Kanxue security forum in 2019

**DumpIt + Volatility Framework**:
- DumpIt: Dump victim machine memory
- Volatility modules extract credentials:
  - `windows.hashdump`: NTLM password hashes from SAM
  - `windows.registry.lsadump`: LSA Secrets (service passwords, cached domain creds)
  - `windows.registry.cachedump`: Cached domain credentials
- Executed via batch scripts (`dmp.bat`, `vo.bat`)

**SQL Server Management Studio Password Export**:
- Tool: `ssms.exe` (published on Chinese security blog 2015)
- Extracts saved connection passwords from `sqlstudio.bin` file
- Exfiltration via certutil Base64 encoding → type command

**Database Access**: Deployed `usql` (universal command-line interface for multiple databases) - suggests direct SQL data extraction capability

## Key Tactics & Techniques

**Cross-Platform Operations**: Maintain toolsets for both Windows and Linux

**Living Off the Land**: Heavy use of:
- Legitimate executables for side-loading (Python)
- Windows built-ins (certutil, type, rar.exe)
- Open-source tools (Mimikatz, Volatility, FRP)

**Chinese Security Community Tools**: GodZilla, AntSword, Xnote, LsaRecorder - all popular in Chinese-speaking threat actor circles

**Stealth Focus**:
- Memory-only execution (shellcode loaded in legitimate processes)
- No file uploads for exfiltration (Base64 → copy-paste method)
- Legitimate tools reduce detection signatures

## Takeaways

- **Six years undetected**: Operating since at least 2020, previously undocumented
- **Critical infrastructure focus**: Targeting highest-value sectors across Asia
- **Chinese tool ecosystem**: Entire toolset sourced from Chinese security forums/blogs
- **"Zhang Wei" identifier**: FRP authentication token uses common Chinese name - possible operator reference
- **Profanity in tools**: Password `f*ckroot123` consistent across all FRP samples
- **Evolution over time**: Replaced custom tools (SuperDump) with simpler batch scripts
- **Cross-platform sophistication**: Parallel Windows/Linux toolsets shows advanced capability
- **Exfiltration innovation**: Base64 + copy-paste bypasses file upload monitoring
- **Memory-focused**: Heavy use of in-memory execution and credential dumping
- **Open-source reliance**: Minimal custom malware, mostly community tools and scripts

## Questions to Explore

- How many organizations compromised since 2020 remain undetected?
- Is "Zhang Wei" a real operator name or generic placeholder?
- Why transition from SuperDump to batch scripts - operational security or simplicity?
- Are there other Chinese APT groups using identical FRP authentication tokens?
- What percentage of stolen credentials resulted in deeper network access?
- How much data was exfiltrated using the Base64 copy-paste method over 6 years?
- Are the same operators behind different "UNK" clusters using Chinese tools?
- Will attribution change from CL-UNK-1068 to known Chinese APT group?
