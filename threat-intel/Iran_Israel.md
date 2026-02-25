# Iran–Israel Cyber Conflict: Operations & Threat Activity

**Date:** 2025-02-25  
**Topic:** Documented cyber operations from both sides of the Iran–Israel conflict, primarily surrounding the escalation triggered by Operation Rising Lion (June 13, 2025)

---

## Sources

1. **Check Point Research** — *2025: The Untold Stories of Check Point Research*  
   https://research.checkpoint.com/2026/2025-the-untold-stories-of-check-point-research/

2. **Google Threat Intelligence Group (GTIG)** — *Israel and Iran Military Operations Raise Likelihood of Cyber Activity, Targeting from Regional Actors* (June 2025)

3. **Google Threat Intelligence Group (GTIG)** — *Predatory Sparrow (UNC4368) Claims Multiple Operations Targeting Iranian Financial Services Institutions Following Recent Escalations in Israel-Iran Conflict* (June 2025)

---

## Background

On **June 13, 2025**, Israel launched **Operation Rising Lion**, a large-scale counterproliferation air campaign targeting Iranian nuclear facilities (including Natanz) and military leadership. Iran responded with an estimated 450 missiles and 1,000 drones. On **June 21**, the US joined with **Operation Midnight Hammer**, striking three Iranian nuclear sites. A ceasefire was announced but quickly violated. This kinetic escalation directly triggered a significant spike in cyber operations from both sides.

---

## Attacks on Israel (Iranian & Affiliated Actors)

### 1. CCTV Camera Exploitation — *June 2025*
- **Actor:** Iran-nexus (unattributed)
- **Method:** Exploitation of CVE-2023-6895 and CVE-2017-7921 targeting internet-connected cameras across Israel
- **Purpose:** Bombing damage assessment (BDA) — real-time visibility into missile strike impacts to refine targeting
- **Source:** Check Point Research

### 2. MuddyWater — ClickFix Password Spray Campaign — *Late June 2025*
- **Actor:** MuddyWater (Iranian state-affiliated)
- **Method:** Password spray via NordVPN infrastructure → compromised Israeli municipal email account → spear phishing → fake Microsoft Teams invite (ClickFix lure) → PowerShell RAT → C2
- **Target:** Israeli municipal government
- **Source:** Check Point Research

### 3. Void Manticore / Handala Hack — WhiteLock Wiper Campaign
- **Actor:** Void Manticore (aka Handala Hack, IRGC-affiliated) & Cotton Sandstorm
- **Method:** Phishing from compromised Israeli CRM provider → malicious `.msi` installer via MEGA → wiper overwrites files with spaces + political wallpaper hijack
- **Tools:** WezRat infostealer, WhiteLock ransomware
- **Target:** Hundreds of Israeli organizations
- **Source:** Check Point Research

### 4. WIRTE — SameCoin Wiper / Espionage *(2025, ongoing)*
- **Actor:** WIRTE (Hamas-associated)
- **Method:** Malicious archive via Dropbox → DLL side-loading → Havoc payload → C2 exfiltration (DigitalOcean infrastructure)
- **Target:** Israeli organizations; continuation of the *Cyber Toufan Al-Aqsa* campaign (late 2024)
- **Also targeting:** Arabic-speaking political entities in Jordan and Egypt
- **Source:** Check Point Research

### 5. APT42 — TAMECAT Phishing Campaign — *May 25–June 13, 2025*
- **Actor:** APT42 (IRGC-affiliated)
- **Method:** Tailored spear-phishing deploying TAMECAT malware
- **Targets:** Israeli academic, government, and defense sectors; Jordan, Syria, Saudi Arabia, UAE
- **Source:** GTIG

### 6. UNC1549 — PDQ RMM via Microsoft Teams Lure — *June 12, 2025*
- **Actor:** UNC1549 (IRGC-linked)
- **Method:** PDQ remote monitoring & management agent distributed via Teams-themed phishing page — a notable TTP shift for this group
- **Targets:** Suspected Israeli aerospace/aviation/defense sectors
- **Source:** GTIG

### 7. UNC1549 — TWOSTROKE & MAILMUSTER Dataminer — *June 4–12, 2025*
- **Actor:** UNC1549
- **Method:** TWOSTROKE backdoor and MAILMUSTER dataminer deployed
- **Targets:** Israeli diplomatic target in China (June 4); broader Israeli targets (June 12)
- **Source:** GTIG

### 8. UNC2428 (Agrius) — UGLYIMP Web Shell & Pre-Positioning — *June 2–11, 2025*
- **Actor:** UNC2428 (MOIS-linked, publicly known as Agrius)
- **Method:** UGLYIMP web shell planted on Israeli infrastructure; assessed pre-positioning for follow-on destructive operations
- **Note:** UNC2428 is tied to the BlackShadow hack-and-leak persona and previously deployed MONEYBIRD ransomware against Israeli targets (2023)
- **Source:** GTIG

### 9. Suspected Emennet Pasargad — SPACEHAMMER Wiper — *June 25, 2025*
- **Actor:** Suspected Emennet Pasargad (IRGC/MOIS front company)
- **Method:** New SPACEHAMMER wiper deployed against Israeli targets
- **Source:** GTIG

### 10. SEWERGOO & BLUEWIPE Wipers — *June 19, 2025*
- **Actor:** Unattributed
- **Method:** Two newly identified wiper families deployed against suspected Israeli targets
- **Source:** GTIG

### 11. UNC5203 / Handala Hack — Reinstatement & Intrusions — *June 13, 2025*
- **Actor:** UNC5203 / Handala Hack (MOIS-linked)
- **Method:** Network intrusion with data exfiltration; returned after months of inactivity with rebranded red logo (symbolizing Iranian retaliation)
- **Targets:** Delkol and Delek (petroleum/manufacturing), Aerodreams (flight school alleged to support Israeli drone operations)
- **Source:** GTIG

### 12. Fariq Fatemiyoun al-Electronic (FFE) — DDoS & Infrastructure Attacks — *June 13, 2025*
- **Actor:** FFE (Iraq-based hacktivist group, tied to Kata'ib Hezbollah)
- **Method:** DDoS and unspecified intrusion attempts
- **Targets:** Israel's Tzofar warning/alert system, Israeli Air Force website (iaf.org.il), unspecified health and response infrastructure
- **Source:** GTIG

### 13. Suspected IRGC — Phishing Campaign — *June 17, 2025*
- **Actor:** Suspected IRGC
- **Method:** Phishing campaign (details unspecified)
- **Targets:** US, UK, Israel
- **Source:** GTIG

---

## Attacks on Iran (Israeli & Pro-Israel Actors)

### 1. Predatory Sparrow — Bank Sepah Destructive Attack — *June 17, 2025*
- **Actor:** Predatory Sparrow / Gonjeshke Darande (UNC4368) — pro-Israel hacktivist persona, widely assessed as Israeli state-linked
- **Method:** Destructive cyberattack resulting in significant data loss
- **Target:** Bank Sepah — Iranian state-owned bank sanctioned by the US for financing Iran's ballistic missile/nuclear programs and IRGC proxy activity
- **Note:** Specific tooling not disclosed; GTIG unable to fully corroborate claimed impact
- **Source:** GTIG

### 2. Predatory Sparrow — Nobitex Cryptocurrency Exchange Hack — *June 18–19, 2025*
- **Actor:** Predatory Sparrow (UNC4368)
- **Method:** Network intrusion → $90 million in crypto stolen → transferred to computationally intensive vanity wallets containing anti-IRGC messaging (funds rendered permanently inaccessible) → full source code, private server keys, passwords, and API keys leaked publicly via Telegram after a 24-hour warning period
- **Target:** Nobitex — Iran's largest cryptocurrency exchange, assessed to have IRGC links
- **Key detail:** The vanity wallet generation across multiple blockchains required significant compute, confirming this was an act of destruction — not financial theft
- **Source:** GTIG

### Historical Operations — Predatory Sparrow (UNC4368)

| Date | Operation | Method | Target | Impact |
|------|-----------|--------|--------|--------|
| Oct/Nov 2023 | Iranian Gas Station Disruption | Cyberattack on fuel distribution systems | Nationwide gas station network | ~70% of stations disabled |
| June 2022 | Steel Company Attacks | METEORLIGHT wiper | Three Iranian steel manufacturers | Claimed physically destructive OT impact; HMI access confirmed |
| July 2021 | Railway System Attack | METEOR wiper | Islamic Republic of Iran Railway (IRIR) + Ministry of Roads | Disrupted rail operations and station display boards nationwide |

---

## Israeli Information Operations (IO)

Israel's social media operations are high-volume and designed to dominate public discourse through both overt **Hasbara** (public diplomacy) and covert influence networks. While its kinetic cyber operations are surgical and low-signature, its cognitive operations are loud, scalable, and AI-assisted.

### 1. Operation PRISONBREAK *(2025)*
- **Actor:** Assessed as an unidentified Israeli government agency or closely supervised subcontractor
- **Method:** Coordinated network of 50+ AI-enabled X (Twitter) profiles, activated in early 2025 and spiking during the June 13 Operation Rising Lion strikes
- **Tactics:** AI-generated deepfake videos depicting Iranian citizens supposedly celebrating the bombing of Evin Prison and chanting anti-regime slogans
- **Targets:** Domestic Iranian public (to incite revolt) and global audiences (to frame the strikes as "liberation")
- **Source:** Citizen Lab — *"We Say You Want a Revolution"* (October 2025)

### 2. Automated Hasbara Commando & AI Bot Networks
- **Tooling:** FactFinderAI, Hasbara Commando, and related AI-driven comment generation tools
- **Scale:** Investigative reporting revealed at least 2 million NIS (~$550,000 USD) allocated by Israel's Ministry of Diaspora Affairs for AI-driven comment campaigns
- **Tactics:**
  - **Comment Flooding:** Automated generation of pro-Israel replies on high-traffic political posts across X, Instagram, and TikTok
  - **Narrative Hijacking:** Mass-reporting of Palestinian or Iranian content to trigger shadowbanning or platform removals
  - **Impersonation:** Fake personas of Western progressives and American students used to influence US domestic political debate
- **Source:** Haaretz / Fake Reporter investigations (2024–2025)

### 3. Lab Dookhtegan — Strategic Leak Persona
- **Actor:** Lab Dookhtegan (widely assessed as linked to Israeli intelligence)
- **Method:** Telegram-based hack-and-leak persona used to publicly release personal photos, home addresses, and payroll data of Iranian cyber officers
- **Purpose:** To convert technical intelligence into a psychological weapon — demoralizing IRGC personnel and signaling deep penetration of Iranian security services

---

## Iranian Information Operations (IO)

### MILAD TOWER *(Post-June 13, 2025)*
- **Type:** Coordinated inauthentic behavior — network of fake news sites + social media accounts (primarily X/Twitter)
- **Reach:** UK, France, Middle East, South & Central Asia; content in multiple languages
- **Narratives:** Condemned Israeli strikes, framed Iranian retaliation as justified, alleged US/Western complicity; leveraged civilian casualty imagery
- **Notable hashtags:** `#IranUnderAttack` (UK), `#Israel_Enemy_of_Humanity` (France/South Asia)

### UTOPIA *(Post-June 13, 2025)*
- **Type:** Long-running Iran-aligned influence network (active since at least 2019)
- **Method:** Social media amplification of pro-Iran narratives, Iranian official statements, and vows of revenge following Israeli strikes

---

## Notable Regional / Third-Party Actors

| Actor | Affiliation | Activity |
|-------|-------------|----------|
| UNC4453 / Plaid Rain | Lebanon (Hezbollah-adjacent) | Coordinated with TEMP.Zagros; abused OneDrive/Dropbox to target Israeli orgs |
| UNC1153 / Lebanese Cedar | Hezbollah | Coordinated with UNC2428 in attack on Ziv Hospital (Nov 2023) |
| UNC5890 | Hamas-linked | Targeting government and media orgs across MENA with IRONWIND downloader |
| Nimbus Manticore | IRGC-affiliated | Phishing campaigns in Northeast Africa; impersonating T-Mobile |
| "Stealth Falcon" | UAE-linked | Exploited CVE-2025-33053 (WebDAV 0-day) against defense entities in Egypt, Qatar, Turkey, Yemen |

---

## Analysis

The Iran–Israel conflict illustrates how modern state-level cyber warfare operates across two distinct but complementary tiers. Iran leans on a broad ecosystem of MOIS and IRGC-linked actors deploying wipers, phishing infrastructure, and pre-positioned access — prioritizing disruption and espionage at scale, with hacktivist personas providing plausible deniability. The June 2025 escalation did not introduce novel Iranian TTPs so much as dramatically increase their tempo, with multiple wiper families, dataminers, and intrusion clusters activating in parallel within days of the kinetic strikes.

Israel, by contrast, employs a **bifurcated cyber doctrine**:

1. **Technical Tier** — High-precision, low-signature sabotage of critical infrastructure and financial systems (exemplified by Predatory Sparrow's OT attacks, the Nobitex hack, and historical operations like the Natanz-era sabotage). These operations are surgical, deniable, and often cause physical or economic damage that extends well beyond the digital domain.

2. **Cognitive Tier** — High-volume, AI-assisted influence operations utilizing bot networks, comment commandos, deepfake video, and strategic leak personas to flood social media, delegitimize the Iranian regime, demoralize IRGC personnel, and actively shape global and domestic Iranian sentiment.

The convergence of these tiers — precision sabotage paired with coordinated narrative warfare — reflects a mature, whole-of-domain approach to conflict that goes well beyond traditional cyber operations.
