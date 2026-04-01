# Handala Hack: Operations March 24–30, 2026
**Date:** April 1, 2026
**Source:** Google Threat Intelligence Group (GTIG) Report 26-10016569, published March 31, 2026
**Related notes:** Iran–Israel Cyber Conflict continuation note (March 4–30, 2026)

---

## Overview

This note documents Handala Hack's claimed operations during the final week of March 2026, drawn from GTIG's weekly pro-Iran hacktivist tracker. During this period GTIG tracked 7 claims attributed to Handala, spanning hack-and-leak operations, doxxing, network intrusions, and psychological operations targeting Israeli intelligence figures, US government officials, and US civilian infrastructure.

GTIG attributes Handala Hack with high confidence to **UNC5203** for its inaugural operations, with subsequent infrastructure and tooling overlaps linking it to **UNC4074** — the cluster attributed to Iran's Ministry of Intelligence and Security (MOIS) and also associated with the Homeland Justice and Karmabelow80 personas. The group emerged in December 2023 and has since operated as one of MOIS's most prolific public-facing hacktivist fronts, combining destructive wiper attacks with hack-and-leak operations for maximum psychological impact.

---

## March 24 — $50M Bounty on Trump and Netanyahu

Handala posted a $50 million bounty on US President Donald Trump and Israeli Prime Minister Benjamin Netanyahu, framing it as direct retaliation for the DOJ's $10 million reward placed on Handala members following the Stryker attack and domain seizures. The group characterized Trump and Netanyahu as "the primary architects of global oppression" and signaled continued operations, inviting collaboration through the Session encrypted messaging app. GTIG assessed this as verified, with no credible operational capability behind the bounty claim itself.

---

## March 25 — Tamir Pardo (Former Mossad Director) Hack-and-Leak

Handala claimed to have exfiltrated and published 14 GB of personal documentation belonging to **Tamir Pardo**, former Director of the Mossad. The group alleged the dataset included contents of private email accounts and Google Drive repositories, video recordings, architectural blueprints, and over 300 pages of files the group called "Netanyahu's Legacy." Handala framed this as part of a continuing campaign against Israeli intelligence leadership. GTIG assessed the claim as **plausible** based on data leak and photo evidence, though independent verification of content authenticity was not confirmed.

---

## March 26 — "Operation Lockheed Martin" — Doxxing of US Engineers in Israel

Handala announced what it called a new phase of "Operation Lockheed Martin," claiming to have published the personal data of 28 senior American engineers based in Israel. According to the claim, the dataset included full names, ID numbers, passport details, home addresses, and assigned military bases. Handala stated these individuals support maintenance of F-35, F-22, and THAAD systems in Israel.

The group then issued a 48-hour ultimatum demanding the engineers cease cooperation with the Israeli government and evacuate the region, threatening that their homes would be designated missile targets and that Handala affiliates inside the US would target their families. GTIG assessed this claim with **judgment withheld** — it carries real escalatory implications but the claim's full scope could not be independently verified. The threat to involve physical operatives against family members mirrors earlier Handala behavior documented in the DOJ seizure affidavit.

---

## March 27 — Kash Patel Email Breach

Handala claimed to have compromised FBI Director Kash Patel's personal email, framing it as retaliation for the domain seizures and DOJ bounty. The group published what it claimed were private emails, documents, and classified files. GTIG assessed the broader claim of compromising "FBI internal systems" as **debunked** — a DOJ official confirmed to Reuters that Patel's *personal Gmail* was breached, not FBI infrastructure, that the leaked materials appeared authentic, and that the content was historical in nature (dating to approximately 2019) with no government information involved. TechCrunch independently verified cryptographic signatures on several emails, confirming authenticity of at least a portion of the leak.

This is a useful case study in Handala's information operations pattern: take a real but limited breach, inflate the framing to imply penetration of federal systems, and maximise psychological impact beyond what the technical reality supports.

---

## March 28 — North Country Business Products POS Disruption

Handala claimed a network intrusion against **North Country Business Products**, a US retail technology and POS solutions provider, alleging that 2,680 POS systems across 110 US companies were taken offline. Screenshots of what appeared to be internal dashboards were posted as evidence. GTIG assessed with **judgment withheld** — the dashboard imagery is plausible but claim scale could not be verified.

This operation is notable as an example of supply-chain-style targeting: rather than hitting retailers directly, Handala compromised a technology provider to create downstream disruption across its client base.

Same day, Handala also claimed to have deleted 4 TB of data from a grocery store in Missoula, Montana — a civilian, low-profile target with no obvious strategic value. This is consistent with Handala's pattern of pairing high-profile claims with smaller, harder-to-verify ones to maintain a sense of pervasive reach.

---

## March 29 — Yoav Gallant (Former Israeli Defense Minister) Breach

Handala claimed to have compromised systems belonging to **Yoav Gallant**, Israel's former Minister of Defense. The group alleged it exfiltrated personal contacts, documents, and communications — over 70 pages of contacts — and claimed to have identified the precise coordinates of 11 "ultra-secret" military bases within the files. Handala stated it was withholding the bulk of the data to maintain ongoing access and operational pressure, and framed the claimed base coordinates as imminent missile targets. GTIG assessed the claim as **plausible**.

---

## Pattern Observations (GTIG, Week of March 24–30)

A few consistent patterns are visible across this week's Handala activity:

**Escalating personal targeting.** Three of the seven claims (Pardo, Patel, Gallant) targeted named senior intelligence and government figures by name, with personal data published as proof. This is a shift from targeting organizations to targeting individuals and their personal infrastructure — an approach designed to create a chilling effect on anyone in proximity to Israeli or US security leadership.

**Mixing verified impact with inflated framing.** The Patel breach was real but limited to an old personal Gmail. The FBI "internal systems" framing was false. Handala consistently does this — the actual intrusion is real enough to generate media coverage, but the narrative wrapped around it overstates the damage. Analysts should calibrate accordingly.

**Civilian and supply-chain targeting in the US.** The North Country POS claim and the Montana grocery store hit represent a broadening of Handala's US targeting beyond symbolic or defense-adjacent organizations. The supply-chain vector (hitting a POS vendor to reach 110 downstream clients) reflects more operational sophistication than typical hacktivist DDoS campaigns.

**Threat inflation as a weapon.** The $50M bounty, the 48-hour ultimatum to Lockheed engineers, and the missile-targeting language around Gallant's alleged base coordinates are all designed to generate fear and media coverage rather than reflect genuine operational capacity. The psychological operation component is inseparable from the technical one for Handala.

---

## Attribution Summary

| Common Name | GTIG Cluster | Assessed Affiliation |
|---|---|---|
| Handala Hack | UNC5203 (inaugural ops), overlaps with UNC4074 | Iran MOIS (Void Manticore / Storm-0842) |
| Homeland Justice | UNC4074 | Iran MOIS |
| Karmabelow80 | UNC4074 | Iran MOIS |

The three personas sharing UNC4074 infrastructure is significant — it means what appears to be three separate hacktivist groups is assessed by GTIG as the same underlying MOIS operational unit running multiple public-facing fronts simultaneously, each with a different narrative focus and target set.
