# AI Use in Malware — Current State Analysis (Unit 42, March 2026)

**Source:** [Unit 42 – Analyzing the Current State of AI Use in Malware](https://unit42.paloaltonetworks.com/)  
**Date:** March 30 , 2026

---

## Overview

Unit 42 researchers analyzed malware samples showing signs of LLM integration, sourced through open-source intelligence and internal telemetry. They identify three theoretical use cases for AI in malware:

1. Using AI to **write** malware
2. Using AI for **remote decision making** (AI-controlled C2)
3. Using AI for **local decision making** (on-device agentic execution)

They found real-world examples of categories 1 and 2. No confirmed examples of category 3 exist in the wild yet. This note covers the two analyzed samples, both fitting category 2.

---

## Sample 1 — "AI Theater": .NET Infostealer with Fake LLM Features

### What It Does
A .NET infostealer (obfuscated with ConfuserEx 2) that collects browser cookies, system information, and file listings, then exfiltrates them to a C2 server. It makes four calls to OpenAI's GPT-3.5-Turbo API.

### The Four LLM Calls

| Method | What It Asks the LLM | What It Actually Does With the Response |
|---|---|---|
| `GenerateEvasionTechnique()` | Suggest an evasion technique name | Writes the name to a log file — never implements it |
| `AnalyzeTargetEnvironment()` | Suggest a sleep delay (1–5 seconds) based on OS info | Actually sleeps for that duration — the only functional use |
| `GenerateObfuscatedCommunication()` | Suggest an obfuscation method name | Writes the name to a log file — never implements it |
| `SendToC2ServerWithLLM()` | Generate a legitimate-sounding message | Adds it as a custom HTTP header alongside stolen data |

### Key Finding
Despite verbose console output claiming "LLM INTEGRATION: 100% OPERATIONAL," the AI integration is largely non-functional. The LLM suggestions are logged but not acted upon. Unit 42 calls this **"AI Theater"** — the appearance of sophisticated AI capability with little real impact.

The one exception (`SendToC2ServerWithLLM`) does exfiltrate data, but the LLM-generated header adds no meaningful evasion value. The C2 URL was also set to `localhost`, suggesting the sample was in testing or built by a low-skilled actor.

### What It Suggests
- AI may be lowering the barrier to entry — less-skilled actors can generate functional malware with AI assistance, even if the implementation is poor
- The pattern of logging fake AI activity could be an attempt to confuse analysts or simply reflects an inexperienced developer

---

## Sample 2 — "AI-Gated Execution": Sliver Dropper with LLM Environment Check

### What It Does
A Golang-based malware dropper for **Sliver** (an open-source red team framework). Before dropping its payload, it surveys the victim's system and sends that data to GPT-4, asking the model to decide whether the environment is safe to execute in.

### System Data Collected
- Hostname, username, OS
- Full process list and deviations from baseline
- AV/EDR products detected
- Network info, USB devices, uptime, RDP status, active window

### How the LLM Decision Works
The prompt instructs GPT-4 to act as an "OPSEC AI" and respond with a strict JSON object:
```json
{ "execute": true/false, "confidence": 0.0-1.0, "reason": "..." }
```

If `execute` is `true`, the dropper decrypts and launches the Sliver shellcode. If `false`, it waits and retries. Real log examples show the LLM correctly flagging `sysmon.exe` as a risk and returning `execute: false`.

### Why This Is Significant
Traditional malware uses hard-coded allow/deny lists to check for analysis environments. Using an LLM instead means:
- The logic is harder for defenders to reverse engineer — there's no fixed rule to find
- The model can reason holistically across multiple data points rather than matching against a static list
- Future iterations could move toward a **locally deployed small language model**, removing the dependency on an external API call

---

## Key Takeaways

- Real-world AI-assisted malware exists today, but most current examples are immature — functional infostealers with poorly implemented LLM features
- The more concerning sample is the AI-gated dropper: using an LLM for execution decisions is a genuinely novel approach that could evolve quickly
- The biggest near-term risk is **lowered barrier to entry** — AI helps less-skilled actors produce working malware faster
- The next frontier flagged by Unit 42 is **locally executed AI** embedded directly in malware, enabling real-time adaptation without external API calls

---

## Questions to Explore

- How would a defender detect or disrupt malware that relies on an external LLM API call before executing? (e.g., blocking outbound calls to `api.openai.com`)
- Could defenders poison or manipulate the LLM's response to prevent execution — and is that a realistic defensive strategy?
- What does MITRE ATT&CK say about environment checks pre-execution, and how would AI-gated execution map to existing techniques?
