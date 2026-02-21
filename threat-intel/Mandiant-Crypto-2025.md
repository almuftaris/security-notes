# Web3, Crypto & DeFi Threats - Mandiant M-Trends 2025

**Date**: February 21, 2026
**Source**: [Mandiant M-Trends 2025](https://cloud.google.com/security/resources/m-trends) - Web3 & Cryptocurrency Section

## Overview: DPRK Dominates Crypto Theft for Sanctions Evasion

North Korean threat actors have stolen over **$500 million USD** in digital assets over the past three years, primarily targeting Web3/blockchain organizations and individuals to bypass international sanctions and fund weapons programs. The immutability of blockchain and decentralized nature of Web3 create unique challenges for defenders - traditional takedown methods often don't work.

## Key DPRK Threat Groups Targeting Web3

### **APT38** - Financial Institution Specialist
Historic focus on attacking financial institutions; responsible for some of the largest cyber heists ever recorded. Primary DPRK actor for large-scale financial theft operations.

### **UNC1069** - Social Engineering via Telegram
- **Active Since**: April 2018
- **TTPs**: Poses as investors from reputable firms on Telegram, sends fake meeting invites (often via compromised accounts)
- **Target**: Web3 and cryptocurrency organizations
- **Objective**: Access to digital assets and cryptocurrency wallets

### **UNC4899** - Job Posting Lures
- **Active Since**: 2022
- **TTPs**: Sophisticated social engineering with fake job postings for prominent firms on social media; supply chain compromises
- **Target**: Cryptocurrency professionals
- **Objective**: Steal digital assets, gain organizational access

### **UNC4736** - Supply Chain Trojanization
- **Notable Operation**: 2022 cascading software supply chain attack - compromised trading software entity, leading to secondary supply chain compromise affecting 9+ organizations
- **TTPs**: Trojanized trading and cryptocurrency software
- **2024 Activity**: Targeted decentralized finance (DeFi) platforms

### **UNC5342** - Package Repository Poisoning
- **Active Since**: January 2024
- **TTPs**: Distributed malicious cryptocurrency-themed NPM and Python packages on GitHub containing BEAVERTAIL downloader → INVISIBLEFERRET backdoor
- **Target**: Software services, biotech, media sectors
- **Impact**: Extensive endpoint control

### **UNC3782** - Large-Scale Phishing & Drainer Operations
- **2023**: Phishing campaign against TRON users - transferred **$137M USD in a single day**
- **2024**: Expanded to target Solana blockchain users with drainer campaigns
- **Scale**: Prolific actor conducting large-scale phishing focused on cryptocurrency industries

## Technical Capabilities

**Custom Tooling:**
- Written in Golang, C++, Rust
- Obfuscated with VMProtect and Garble (anti-analysis tools)
- Multi-platform support: Windows, Linux, macOS (especially important given macOS prevalence among Web3 developers)

**Objectives:**
- Financial gain to fund WMD programs
- Target both individual developers' wallets AND corporate infrastructure
- Exploit trust relationships in developer communities

## Crypto Drainers & Smart Contract Abuse

### **What Are Drainers?**
Malicious smart contracts that execute when users approve wallet transactions. Threat actors use phishing/social engineering to trick users into approving contracts that automatically transfer all wallet contents to attacker-controlled addresses.

### **Drainer-as-a-Service (DaaS)**
- Over **1,200 fake sites** created since January 2024 for drainer operations
- DaaS providers advertise on underground forums and Telegram
- Payment model: DaaS provider takes percentage of drained assets
- Lowers barrier to entry - anyone can run drainer campaigns without technical skills

### **Blockchain Platform Targeting Evolution**
- **Primary Target**: Ethereum network (most drainer activity)
- **2023**: DPRK expanded to TRON blockchain
- **2024**: DPRK expanded to Solana blockchain

### **EtherHiding: Takedown-Resistant Infrastructure**

**UNC5142 Campaign:**
- Compromised vulnerable WordPress sites
- Injected code to retrieve data stored in malicious smart contracts
- Smart contracts contained second-stage code for payload delivery
- Installed info-stealer malware

**Why It Works:**
- **Immutability**: Once on blockchain, can't be removed by traditional takedowns
- **Decentralized**: No single point of control to shut down
- **Updatable**: Smart contracts can be updated before execution, allowing infrastructure pivoting when detected
- **Censorship-Resistant**: Content persists even when surface infrastructure is taken down

## The Web3 Security Challen
