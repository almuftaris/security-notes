# Iran–Israel Cyber Conflict: Post-March 3 Continuation

**Date Range:** March 4 – March 30, 2026

**Continued from:** *Cyberattacks leading up to and following Operation Epic Fury (March 3, 2026)*

**Sources:** Krebs on Security, BleepingComputer, Unit 42 (Palo Alto Networks), SOCRadar, The Register, Help Net Security, TechCrunch, Axios, Euronews, Wikipedia, DOJ/FBI press releases

---

## Background Recap

As covered in the previous note, the February 28, 2026 joint US-Israeli military strikes under Operation Epic Fury and Operation Roaring Lion triggered an immediate wave of cyber retaliation from Iran-linked actors. By March 3, groups like **Handala**, **Fatimiyoun Electronic Team**, and **Cyber Islamic Resistance** were already active, and DHS/CISA had issued a critical alert warning of imminent attacks on domestic US networks. What followed in the weeks after was a sustained, escalating cyber campaign that grew well beyond the initial hacktivist noise.

---

## March 4–5: OT Claims Escalate and the Front Goes Global

On March 4, APT Iran claimed a month-long intrusion into Jordan's grain storage systems, allegedly manipulating temperatures and underreporting wheat weights.  While the claim was initially treated with skepticism, it would later be partially validated. Z-Pentest Alliance published screenshots of an Israeli water pump HMI, claiming real-time control over valves and alarms. DieNet expanded into Jordanian civilian infrastructure, claiming access to employee payroll and ID data from the electricity distribution company. 

On March 3, pro-Russian hacktivist clusters formally joined the pro-Iran coalition, dividing focus between Europe and the Middle East.  Pro-Russian group NoName057(16) teamed up with Iranian hacktivists to target Israeli defense and municipal organizations, including defense contractor Elbit Systems.  This represented a notable broadening of the coalition — the conflict was no longer just an Iran-Israel affair in cyberspace.

---

## March 6: MuddyWater's Pre-Planted Backdoors Revealed

One of the most significant intelligence disclosures of the conflict came on March 6, when Broadcom's Symantec and Carbon Black published findings showing that **MuddyWater** (also tracked as Seedworm/MANGO SANDSTORM), an APT group operating under Iran's Ministry of Intelligence and Security (MOIS), had already silently compromised multiple organizations *before* Operation Epic Fury began.

The campaign, which began in early February 2026, targeted a small but strategically significant set of organizations: a US financial institution, a US airport, a Canadian non-profit, and the Israeli subsidiary of a US software company that supplied the defense and aerospace sectors. 

Two new malware families were identified:

- **Dindoor** — a previously unknown backdoor notable for using the Deno runtime environment, which allows JavaScript or TypeScript code to run outside a web browser. This design enables attackers to execute commands and maintain control while blending into legitimate software processes. Because the malware had not been publicly documented before, traditional signature-based security tools could struggle to detect it. 

- **Fakeset** — a Python-based backdoor downloaded from servers belonging to Backblaze, an American cloud storage company. The digital certificate used to sign Fakeset had also been used to sign Stagecomp and Darkcomp malware, both previously linked to MuddyWater — providing the attribution chain researchers needed. 

For data exfiltration, researchers observed an attempt to transfer data from the software company using Rclone, a command-line file management tool, to a Wasabi cloud storage bucket.  Using legitimate cloud tools for exfiltration is a classic technique to blend into normal traffic and avoid detection.

The significance here was clear: in May 2025, MuddyWater had compromised a server containing live CCTV streams from Jerusalem, allowing surveillance of the city — and on June 23, Iran bombed Jerusalem. Even if the motive behind this new campaign wasn't disruption originally, analysts warned the group could quickly pivot to destructive attacks on organizations they'd already quietly compromised. 

Jordan's National Cybersecurity Center also officially confirmed it had thwarted an Iranian attack on its wheat silo management system — the first government-confirmed foiled OT (operational technology) intrusion of the conflict, lending credibility to the APT Iran claims from March 4. 

---

## March 11: Handala Wipes Stryker — The First Confirmed Destructive Attack on a US Fortune 500

The single most impactful cyber event of the conflict (to date) occurred on the morning of March 11, 2026.

On the morning of March 11, employees at Stryker offices across 79 countries turned on their computers and found them blank. Login screens had been replaced with the logo of Handala — a barefoot boy holding a slingshot.  Stryker, a Fortune 500 medical technology company with roughly 56,000 employees and $25 billion in annual revenue, had just been brought offline globally.

**How it happened:**

A source familiar with the attack told BleepingComputer that the threat actor used the wipe command in Microsoft Intune — Microsoft's cloud-based endpoint management service — to erase data from approximately 80,000 devices between 5:00 and 8:00 a.m. UTC. The attacker carried out the action after compromising an administrator account and creating a new Global Administrator account.  No custom malware was needed at all. The attackers weaponized Stryker's own IT management infrastructure against them.

Check Point Research documented that initial access is believed to have been established well before the destructive phase, with network access potentially dating back several months. Handala had maintained reconnaissance through hundreds of brute-force attempts against VPN infrastructure, and since Iran's January 2026 internet restrictions, routed traffic through Starlink IP ranges to blend into legitimate satellite traffic. 

**Attribution and motive:**

Multiple intelligence firms assess Handala Hack Team as an online persona of Void Manticore (Storm-0842 / Banished Kitten), affiliated with Iran's Ministry of Intelligence and Security (MOIS). The group operates using a documented two-actor handoff: Scarred Manticore (Storm-0861) provides initial access via long-dwell operations, then hands off to Void Manticore for destructive wiper deployment — a pattern observed in the 2022 Albania attacks and 2023-2024 Israel campaigns. 

Handala's manifesto referred to Stryker as a "Zionist-rooted corporation," likely referencing the company's 2019 acquisition of Israeli medical tech firm OrthoSpace. The group framed the attack as retaliation for a missile strike on an Iranian school that killed at least 175 people. 

**Aftermath and legal response:**

Stryker confirmed a "global network disruption to its Microsoft environment" in an SEC filing, initially stating no ransomware or malware was detected — consistent with the Intune abuse vector, which required no traditional malware deployment. Stryker entered its 17th day of restoration on March 27, with manufacturing capability "quickly ramping" and most critical lines restored. Four class-action lawsuits were filed in US District Court by current and former employees seeking a total of $20 million in damages. 

Following the attack, Microsoft and CISA released guidance on hardening Windows domains and securing Intune to prevent similar attacks at other organizations. 

---

## March 19–20: FBI Seizes Handala Domains — Group Rebuilds Within Hours

The Justice Department announced the seizure of four domains used by MOIS in furtherance of psychological operations: Justicehomeland[.]org, Handala-Hack[.]to, Karmabelow80[.]org, and Handala-Redwanted[.]to. The FBI's investigation revealed the domains were linked through shared leak sites, Iranian IP ranges, and a common operational playbook — including destructive cyberattacks and "faketivist" psychological operations using stolen data. 

The seized domains had been used to post personally identifiable information of IDF personnel, publish death threats to Iranian dissidents and journalists in the US and abroad, and even coordinate with what Handala described as Mexican cartel "partners" to commit physical acts of violence against targets. 

FBI Director Kash Patel stated: "Iran thought they could hide behind fake websites and keyboard threats to terrorize Americans and silence dissidents." 

The practical effect of the seizures was short-lived. The group launched a replacement site within hours , demonstrating the resilience of state-backed hacktivist infrastructure against domain seizures alone. Handala called the seizures "a desperate attempt to silence our voice" and stated it was building new infrastructure.

---

## March 22–25: Broader Hacktivist Activity and New Targets

The wider coalition of pro-Iran and allied hacktivist groups continued to expand their scope during this period.

On March 23, Handala published nine schematic maps of Israeli power plant and electrical grid infrastructure on its website. NoName057(16) opened a new front in Denmark and Greenland under #OpDenmark, targeting Air Greenland and Nuuk's public transport system. 

On March 24, the Kurdish group Cyb3r Drag0nz formally broke from the Cyber Islamic Resistance coalition, publishing the names and photographs of six Peshmerga fighters killed in Iranian strikes and calling on the Kurdish diaspora to stand against Iran. Iran-aligned Fynix announced attacks on Kurdish governments and organizations in direct response — opening a new sub-front. Handala also posted a $50 million bounty on Trump and Netanyahu in response to the DOJ's own $10 million bounty on Handala members. 

On March 25, Handala's most personally targeted operation since the conflict began took shape with a hack-and-leak post naming a former Israeli intelligence chief and claiming 14 gigabytes of documents.  Cybernews reported this was claimed to be former Mossad research director Sima Shine, with over 100,000 emails allegedly exfiltrated.

---

## March 26–27: Handala Claims FBI Director's Email — Lockheed Martin in the Crosshairs

The final days of March produced two of the most headline-grabbing claims of the entire conflict.

Handala claimed to have breached the personal email account of FBI Director Kash Patel, posting materials to its website and Telegram channel. TechCrunch confirmed that at least some of the emails were authentic by verifying cryptographic signatures in the message headers. The files in the leaked cache appeared to date up to about 2019.  The FBI acknowledged it was "aware of malicious actors targeting" Patel's personal email.

Handala also claimed that US aerospace and defense company Lockheed Martin had been compromised. "The manufacturer of the F-35, F-22, THAAD missile defense system and advanced electronic warfare systems could not even protect its own identity," the group posted. Lockheed Martin did not confirm any compromise, stating it was "aware of the reports" and had "policies and procedures in place to mitigate cyber threats." 

---

## Ongoing Phishing and Financial Fraud Campaigns

Running in parallel with the high-profile attacks, Unit 42 documented a sprawling opportunistic fraud wave exploiting the conflict.

Unit 42 identified 7,381 conflict-themed phishing URLs spanning 1,881 unique hostnames. Attackers registered conflict-themed domains numbering in the thousands, used for fake storefronts, donation scams, and phishing portals. Two separate malicious campaigns targeted UAE residents — one exploiting brands with "Emirates" in the name for financial fraud, and another using "Dubai"-branded domains to run crypto and real estate investment scams. 

Threat actors were also running vishing (voice phishing) scams targeting UAE residents, calling victims while impersonating the UAE Ministry of Interior and requesting Emirates ID numbers for "verification." 

---

## Iran's Internet Blackout — A Factor in Cyber Operations

Throughout the conflict, Iran's own internet situation has been a significant variable. As of March 26, 2026, Iran had surpassed its 27th straight day of near-complete internet blackout.  This has had a dual effect: limiting the ability of Iran-based state actors to coordinate sophisticated operations from within the country, while geographically dispersed operators and proxy groups outside Iran — like Handala, which is assessed to operate partially from outside Iranian borders — have remained active and capable.

---

## Key Takeaways

- **Wiper attacks are the weapon of choice.** The Stryker incident demonstrated that destructive wiper operations don't require novel malware — abusing legitimate enterprise tools (Microsoft Intune) is sufficient when privileged access is obtained.
- **Pre-positioning matters.** MuddyWater had access to US networks well before the conflict started. Dwell time is a strategic asset; quiet access established during peacetime becomes leverage during escalation.
- **The hacktivist coalition is broad and loosely coordinated.** Pro-Russian groups (NoName057(16), Z-Pentest Alliance), pro-Iranian groups, and independent actors have all converged around the same targets, though fractures like the Kurdish sub-front show the coalition isn't monolithic.
- **Domain seizures have limited impact against resilient actors.** Handala rebuilt infrastructure within hours of the FBI seizure and kept publishing. Takedowns are disruptive but not decisive against state-backed groups.
- **The conflict has no defined cyber endpoint.** As SOCRadar noted, Iranian APT groups do not stand down when missiles stop flying — they retool and return.

---

## Questions

- How does the Dindoor/Fakeset campaign differ from MuddyWater's previous tooling, and what does the shift to Deno say about how APTs evade detection?
- The Stryker attack required no malware — just admin access to Intune. What does that mean for how organizations should be thinking about privileged access management (PAM) in cloud environments?
- What distinguishes a "hacktivist" group like Handala from a traditional APT, and why does that distinction matter for attribution and legal response?
- How effective are domain seizures as a counter-cyber-operations tool if groups can rebuild infrastructure within hours?
- Iran's internet blackout has limited domestic state actors but hasn't stopped proxy groups abroad. What does this suggest about how Iran structures its cyber proxy relationships?
