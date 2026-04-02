# ChatGPT Data Exfiltration via Hidden Outbound Channel (Check Point Research, March 2026)

**Source:** [Check Point Research – ChatGPT Data Leakage via a Hidden Outbound Channel in the Code Execution Runtime](https://research.checkpoint.com/)  
**Date:** April 2, 2026  
**Patched:** February 20, 2026 (OpenAI confirmed they had already identified the issue internally)

---

## Overview

Check Point Research discovered a hidden outbound communication path inside ChatGPT's code execution environment. A single malicious prompt could silently turn an ordinary conversation into a data exfiltration channel — leaking user messages, uploaded files, and AI-generated summaries to an attacker-controlled server, without any warning to the user.

---

## What Made This Possible

ChatGPT has two legitimate outbound capabilities: web search and GPT Actions. Both are designed with user-visible controls — when data is about to leave the conversation through a GPT Action, the user sees an approval dialog showing what's being sent and where. The code execution environment (where ChatGPT runs Python for data analysis tasks) was explicitly designed to have no outbound internet access.

The vulnerability bypassed both of those safeguards entirely.

---

## How the Attack Worked

### The Initial Hook
A victim pastes a single malicious prompt into a ChatGPT conversation — something easily disguised as a productivity tip, a "best prompts" recommendation, or a claim to unlock premium features for free. From that point on, every subsequent message in the conversation becomes a potential source of leaked data.

### What Gets Stolen
The attacker's prompt instructs the model on what to collect and transmit. This could be raw user messages, content extracted from uploaded files (contracts, lab results, PDFs), or the model's own generated output — summaries, assessments, conclusions. The attacker doesn't need the whole document; they can target the most valuable intelligence the model produces.

### The Malicious GPT Variant
The same attack becomes more dangerous when embedded in a custom GPT. A malicious GPT developer doesn't need the user to copy any prompt — the exfiltration logic is baked into the GPT's instructions from the start. The user just opens what looks like a specialized assistant (a "personal doctor," a contract reviewer, a financial advisor) and interacts normally while their data is quietly sent out.

Check Point demonstrated this with a proof-of-concept medical GPT: a user uploaded lab results, described symptoms, and asked for an interpretation. The GPT produced a normal-looking medical assessment. Meanwhile, the patient's name and the model's assessment were silently transmitted to the attacker's server. When the user directly asked ChatGPT whether their data had been sent anywhere, it confidently said no.

---

## The Covert Channel — DNS Tunneling

The reason this worked despite the execution environment supposedly having no internet access comes down to **DNS**.

DNS is the system that translates domain names (like `google.com`) into IP addresses. Normally it's just infrastructure — you type a URL, DNS resolves it, and your browser connects. But DNS can also be abused as a data transport channel.

Here's how: instead of sending data directly over the internet (which was blocked), the attacker encodes the stolen data into subdomain labels in a DNS request — something like `stolencontent.attacker.com`. When the runtime tries to "resolve" that domain, the DNS query travels outward through normal resolver infrastructure, carrying the encoded data with it. The attacker controls the domain and receives the query on their server, then decodes the data from the subdomain.

Because DNS resolution is a basic networking function that almost no environment blocks, it slipped through the isolation boundary that was supposed to prevent outbound access entirely.

The same channel worked in reverse: the attacker could encode commands into DNS responses, send them back into the container, and effectively run a **remote shell** inside ChatGPT's Linux execution environment — completely outside the normal chat interface and its safety checks.

---

## Key Takeaways

- **Guardrails only work if every exit path is covered.** OpenAI blocked conventional outbound access from the code execution environment, but DNS was left open as a functional necessity. Attackers don't need the front door.

- **The "no data leaves this system" promise is harder to keep than it sounds.** ChatGPT told a user their data hadn't been sent anywhere — while the attacker's server was receiving it in real time. As AI systems grow more capable, the gap between what they claim and what they can verify about their own behavior becomes a real trust problem.

- **Custom GPTs are an underexamined attack surface.** The GPT marketplace model — where anyone can build and publish a specialized assistant — creates a supply chain problem similar to malicious browser extensions or npm packages. Users have limited ability to audit what a GPT is actually doing with their data.

- **AI systems are now execution environments, not just chatbots.** They run code, read files, and process sensitive documents at scale. The security standards applied to those environments need to match that reality — which right now, they don't consistently.

- **This is the second major AI platform data exfiltration disclosure in two weeks.** The Claude.ai prompt injection chain (Oasis Security, March 2026) and this finding both demonstrate the same pattern: AI platforms are expanding in capability faster than their security boundaries are being validated. The attack surface is growing, and researchers are consistently finding ways across it.
