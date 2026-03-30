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

---

## Addendum: The Other Side — US and Israeli Offensive Cyber Activity

One of the defining features of this conflict is the **information asymmetry** in what gets publicly documented. Iran-aligned groups self-report loudly on Telegram. The US and Israel do not. That said, there is meaningful public documentation of the offensive side — it's just mostly concentrated in the opening phase of the conflict, and it operates at a different level of abstraction than individual hacktivist claims.

### US Cyber Command: Confirmed "First Mover" Role

The clearest public acknowledgment came from the Chairman of the Joint Chiefs himself. On March 2, Gen. Dan Caine stated at a Pentagon press briefing that US Cyber Command and US Space Command were the "first movers" in Epic Fury, "layering non-kinetic effects, disrupting and degrading and blinding Iran's ability to see, communicate and respond."  This was a remarkable public admission — US officials have historically kept offensive cyber operations entirely classified, but the Trump-era Joint Staff under Caine has made a deliberate choice to acknowledge cyber as a mainstream domain of military power rather than a covert exception.

Former cyber operators told Breaking Defense that USCYBERCOM's role likely evolved across phases of the operation — starting with first-strike disruption, then shifting into persistent intelligence collection. One former operator noted that for anything beyond a raid, cyber is most valuable for sustained intelligence gathering rather than kinetic-style effects, and that burning a high-value access point on disruption can backfire, as Russia learned when its Viasat attack in Ukraine pushed the Ukrainians to adopt Starlink and accelerated a modernization the Russians inadvertently funded. 

The Pentagon also quietly stood up a new "non-kinetic effects cell" on the Joint Staff ahead of Epic Fury — a team specifically designed to integrate, coordinate, and synchronize cyber and electronic warfare capabilities into the planning and execution of operations globally, not as an afterthought but as a first-order planning element. 

### Israeli SIGINT and Camera Access: Pre-Positioned Intelligence

RUSI's analysis noted that Israel had pre-existing access to Iranian security and traffic camera networks, which — combined with CIA HUMINT about Khamenei's location — allowed US and Israeli planners to adjust the timing of the entire operation to exploit the window when he was present at the compound. The sequencing of HUMINT, SIGINT, and pre-positioned cyber access directly enabled a precision strike that limited collateral damage.  This is a concrete example of cyber as an intelligence enabler rather than a weapon in isolation.

### The February 28 Blackout: Attributed to Israel

Israeli sources described the February 28 internet blackout — which drove Iranian connectivity to 4% of normal — as the culmination of a campaign that began in January, combining electronic warfare that disrupted navigation and communications systems with DDoS attacks and deep intrusions into data systems.  Western intelligence sources confirmed the damage to the IRGC's communications infrastructure was deliberate, designed to prevent counterattack coordination and disrupt the ability to launch drones and ballistic missiles.  Israeli sources described the overall operation as the largest cyberattack in history, though that framing is impossible to independently verify.

### Mossad's Psychological Operations Channel

During the opening hours of Operation Roaring Lion, Mossad launched a Farsi-language Telegram channel offering Iranians an alternative information source, calling on "our Iranian brothers and sisters" and inviting them to share content of their "just struggle against the regime."  This followed a January 2026 operation in which government satellite broadcasts in Iran were reportedly hacked to air regime-change content to millions of households — a campaign attributed to Israeli intelligence operations.

### March 4: IDF Physically Strikes the IRGC's Cyber Warfare Headquarters

In a move that blurred the line between kinetic and cyber operations, the IDF announced on March 4 that it had bombed a large compound in eastern Tehran containing the IRGC's cyber and electronic warfare headquarters and its Intelligence Directorate, among other command centers.  Lt. Gen. Charles Moore, former deputy commander of US Cyber Command, assessed the strikes would have "a significant impact on the regime's ability to continue to execute these types of operations," while cautioning that proxy forces ideologically aligned with Iran could still operate independently.  This prediction turned out to be accurate — as the Stryker attack and subsequent hacktivist operations demonstrated, destroying a physical HQ does not neutralize a geographically distributed proxy network.

### Predatory Sparrow: Active in 2025, Quieter in 2026

During the June 2025 twelve-day conflict, Israeli-linked group Predatory Sparrow (Gonjeshke Darande) targeted Iran's financial infrastructure following US and Israeli strikes on Iranian nuclear facilities.  In those operations, Predatory Sparrow reportedly hacked Bank Sepah and Nobitex, Iran's largest cryptocurrency exchange, stealing and burning approximately $90 million in crypto assets — likely intended to signal to Tehran that Israel could cause domestic economic chaos. 

In the 2026 conflict, Predatory Sparrow is documented as an active allied-side actor, though their public footprint during this conflict has been smaller than in 2025 — consistent with the pattern that when Israel operates at state level, its quieter intelligence and military units tend to absorb the mission that proxy hacktivists handled previously. 

---

### Why the Asymmetry Exists

The information imbalance here is structural, not accidental. Iran-aligned groups post on Telegram because visibility is part of the strategy — hacktivist claims are psychological operations in themselves. The US and Israel operate on the opposite doctrine: acknowledge the role of cyber generally (as Caine did), but never confirm specific operations, targets, or technical methods. That means the offensive side of this conflict will likely remain only partially visible to the public until after-action reports, leaked documents, or journalistic investigations surface years from now — much like Stuxnet, which was only fully understood long after the fact.

This makes the RUSI observation worth keeping in mind: *the intelligence-gathering role of cyber in this conflict is likely to prove as consequential as its disruptive one* — and that part may never be fully public.
