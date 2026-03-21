# Handala Hack (Void Manticore): Iranian Wiper Operations Expanding to US Targets

**Date**: March 21, 2026
**Researcher**: Check Point Research
**Source**: [Checkpoint Research](https://research.checkpoint.com/2026/handala-hack-unveiling-groups-modus-operandi/)
**Attribution**: Iranian MOIS (Ministry of Intelligence and Security)
**Also Known As**: Void Manticore, Red Sandstorm, Banished Kitten

## Operational Personas

**Three Active Fronts**:
1. **Handala Hack** (dominant since late 2023) - Israel, expanding to US targets
2. **Homeland Justice** (mid-2022+) - Albania-focused operations
3. **Karma** (likely replaced by Handala) - historical overlap with Handala

**Recent Expansion**: Now targeting US enterprises (e.g., Stryker medical technology)

**Leadership**: Operations supervised by Seyed Yahya Hosseini Panjaki (MOIS Counter-Terrorism Division) - **reportedly killed in Israeli strikes March 2026**

## Initial Access: VPN Brute Force & Supply Chain

**Primary Method**: Target IT/service providers for credentials, compromise VPN accounts

**Activity Observed**: Hundreds of VPN logon/brute-force attempts against organizational infrastructure

**Indicators**:
- Default Windows hostnames: `DESKTOP-XXXXXX` or `WIN-XXXXXX`
- Commercial VPN nodes (historically 169.150.227.X for Israeli targets)
- Israeli VPN node: 146.185.219[.]235

**Operational Security Decline**:
- **After Iran internet shutdown (January 2026)**: Attacks originating from **Starlink IP ranges**
- Direct connections from **Iranian IP addresses** (poor OPSEC)
- Previously maintained stronger discipline, always egressing through commercial VPNs

## Attack Preparation

**Pre-positioning**: Access established **months** before destructive phase

**Pre-attack activity**:
- Disable Windows Defender
- LSASS process dumping via `comsvcs.dll` and `rundll32.exe`
- Export registry hives (HKLM)
- Execute ADRecon (`dra.ps1`) for Active Directory enumeration
- Obtain Domain Admin credentials

**C2 Infrastructure**: 107.189.19[.]52

## Lateral Movement: NetBird Mesh Network

**Tool**: NetBird (zero-trust mesh network platform)

**Deployment**:
1. Connect to hosts via RDP
2. Use local browser to download NetBird from official website
3. Install on multiple machines
4. Establish internal connectivity between systems

**Result**: At least **five attacker-controlled machines** operating simultaneously inside network

## Wiping Operations: Four Parallel Techniques

### **1. Handala Wiper (Custom Binary)**

**Distribution**: Group Policy logon scripts executing `handala.bat`

**Technique**: 
- Executable launched remotely from Domain Controller
- Not written to disk on affected machines
- Overwrites file contents
- MBR-based wiping to corrupt/destroy system files

### **2. Handala PowerShell Wiper (AI-Assisted)**

**Distribution**: Group Policy logon scripts

**Function**:
- Enumerate all files in `C:\Users`
- Delete everything recursively
- Place propaganda image `handala.gif` on all logical drives

**Development Indicator**: Detailed code comments suggest **AI-assisted development**

### **3. VeraCrypt Disk Encryption**

**Method**: Manual download from official website via RDP browser session

**Purpose**: Encrypt system drives to complicate recovery (legitimate tool weaponized)

### **4. Manual Deletion**

**Method**: 
- Log in via RDP
- Select all files/VMs
- Delete manually
- Documented in Handala's own leaked videos

**Deployment Strategy**: All four techniques executed in parallel via Group Policy for maximum impact

## Takeaways

- **US targeting expansion** signals broader operational mandate beyond Middle East
- **Starlink usage post-Iran shutdown** shows infrastructure adaptation to censorship
- **OPSEC degradation** (direct Iranian IPs, failed VPN connections) creates detection opportunities
- **NetBird mesh networks** enable efficient multi-machine coordination for destructive operations
- **AI-assisted malware development** (PowerShell wiper comments) confirms trend across Iranian threat groups
