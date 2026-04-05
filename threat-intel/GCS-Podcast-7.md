# EP269 – Reflections on RSA 2026: Beyond AI AI AI AI AI AI AI

**Source:** Google Cloud Security Podcast
**Episode:** 269
**Published:** March 30, 2026
**Reviewed:** April 5, 2026

---

## Overview

A post-RSA 2026 reflections episode. The hosts break down the AI signal vs. noise problem in enterprise security, who's actually using AI vs. performing it, where AI genuinely threatens existing job functions, and how the industry should think about defensive AI products and agent identity.

---

## AI Adoption: Tourists vs. Natives

The hosts segment the market into two types of organizations: those claiming AI adoption without real implementation, and those quietly not using it but not needing to yet. The framing of **AI-native vs. AI-tourist** captures the gap between genuine integration and performative adoption — a theme that was apparently hard to avoid at RSA this year.

---

## Threat to Big Consulting Firms

LLMs pose a structural threat to firms like Gartner whose value is built on analyst-curated knowledge. The counterargument raised: these firms hold proprietary research and institutional knowledge that models aren't trained on. That gap still provides differentiation, at least for now.

---

## Where AI Genuinely Threatens Security Job Functions

The most direct threat identified is **offensive security and static analysis**. The host's view is that LLMs are already capable of distinguishing good code from bad code at a level that puts static analysis as a job function at risk.

Higher-level work like IAM policy writing and documentation is already being offloaded to AI in practice — less controversial, more immediately useful.

On the defensive side, the co-host argues that **AI agents can handle detection rule-building** for specific corporate environments. The workflow: an agent triages the environment, aggregates relevant context, then generates tailored detection rules — potentially eliminating false positives that result from generic, environment-agnostic rules.

---

## Bad Guys With AI vs. Malicious AIs

The hosts distinguish between:
- **Bad actors using AI** — threat actors leveraging LLMs to accelerate operations (recon, social engineering, code generation)
- **Malicious AI systems** — AI built or weaponized specifically to attack

The consensus view among security leaders at RSA was pessimistic. The hosts push back with a fundamentals argument: if your cloud environment has no critical misconfigurations, a malicious AI or AI-assisted actor has nothing obvious to exploit. Same logic applies to codebases — a clean, well-maintained codebase is significantly harder for an AI vuln scanner to find traction in. Security fundamentals remain the most durable countermeasure.

---

## Defensive AI Products

**Promising:**
- AI-powered DLP and data security — highlighted as an area with real near-term value
- **Secure agent identity** — the hosts are optimistic about building proper identity infrastructure for AI agents operating in enterprise environments

**Skeptical:**
- Startups building separate identity systems specifically to support agent workloads. The practical problem: these companies don't own Active Directory. Incumbents with existing identity infrastructure are better positioned.

---

## Incumbents vs. Startups

Counter to the usual narrative of slow-moving enterprises vs. fast-moving startups, the co-host argues that the major security challenges around AI — securing models, supply chains, training data — actually favor incumbents. Large security vendors have the talent, capex, internal AI infrastructure, and existing customer trust to tackle these problems. Google is cited explicitly, though the hosts acknowledge the obvious conflict of interest given where they work.

On the legacy vendor side: the observation that some vendors are effectively the 6th-best firewall or antivirus product and still maintain a customer base reflects how procurement inertia and existing relationships keep older vendors in business well past their competitive prime.
