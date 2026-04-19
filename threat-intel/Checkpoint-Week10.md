# Checkpoint Week 10 — GrafanaGhost, AI Agent Traps & AI Supply Chain Risk

**Date**: April 13, 2026  
**Source**: [Checkpoint Research – April 13th Threat Intelligence Report](https://research.checkpoint.com/2026/13th-april-threat-intelligence-report/)  
**Supporting Sources:**  
- [Noma Security – GrafanaGhost](https://noma.security/blog/grafana-ghost/)

---

## Overview

This week's disclosures share a through-line: AI features added on top of existing enterprise tools are creating attack surfaces that the original security models were never built to handle. GrafanaGhost turns a data visualization platform into a silent exfiltration channel. AI Agent Traps formalizes how web content can hijack autonomous agent behavior. And new research into AI API routers shows that the middleware connecting agents to AI providers is itself an active threat vector. None of these require breaking the underlying AI model. They all go around it.

---

## Story 1 — GrafanaGhost: Silent Data Exfiltration via Prompt Injection

**What is Grafana?**  
Grafana is widely used enterprise software for visualizing and monitoring data — real-time financial metrics, infrastructure health, customer telemetry. It's often one of the most data-rich environments in an organization. Recent versions have added AI-powered features that interact with this data.

**What Happened**  
Noma Security researchers discovered that Grafana's AI components could be turned into a silent exfiltration channel through a chain of three bypasses. No login required. No user interaction required. Data leaves in the background while everything looks normal on screen.

**The Attack Chain**

**Step 1 — Finding the Injection Point**  
Researchers discovered they could inject indirect prompts through nonexistent URL paths in a Grafana instance. The URL structure looked legitimate enough that Grafana processed it, storing the attacker's hidden instructions in the application's data layer where the AI would later retrieve and act on them.

**Step 2 — Bypassing Image Domain Restrictions**  
Grafana had a client-side security check that only allowed images to load from trusted domains (grafana.com, grafana.net, etc.). Researchers bypassed it using a **protocol-relative URL** — a URL starting with `//` instead of `https://`. The security function saw the leading `/` and treated it as a safe relative path. The browser, however, treated it as a full external request to the attacker's server. The check was fooled by a two-character trick.

**Step 3 — Bypassing AI Model Guardrails**  
When researchers tried injecting image markdown directly, the AI model detected it as suspicious and refused. The solution: prepending the word **"INTENT"** to the prompt. This keyword appeared to signal to the model that the instruction was a legitimate internal behavior rather than an attack, causing it to process it without triggering its safety rules.

**The Result**  
With all three bypasses chained, the AI processed the malicious prompt, attempted to render an external image, and in doing so sent an outbound request to the attacker's server — carrying the victim's sensitive Grafana data as a URL parameter. Financial metrics, infrastructure data, customer information — all transferred silently in the background. Grafana has since patched the vulnerability.

---

## Story 2 — AI Agent Traps: Six Ways Web Content Can Hijack Agents

Researchers published a framework describing six distinct web-based attack classes capable of manipulating autonomous AI agents. As agents are increasingly deployed to browse the web, process documents, and execute multi-step tasks, the web itself becomes an attack surface against them.

**The Six Attack Classes**

| Attack Type | What It Does |
|---|---|
| **Hidden Instruction Injection** | Invisible text on a webpage plants commands the agent reads as legitimate instructions |
| **Reasoning Poisoning** | Malicious content corrupts the agent's chain of thought, steering conclusions toward attacker-desired outcomes |
| **Memory Corruption** | Injected content writes false information into the agent's persistent memory, affecting future sessions |
| **Tool Use Manipulation** | Hidden prompts redirect which tools the agent calls and with what parameters |
| **Context Window Flooding** | Attacker-controlled content fills the agent's context, crowding out legitimate instructions |
| **Goal Hijacking** | The agent's top-level objective is overwritten mid-task, redirecting the entire workflow |

**Why This Matters**  
Agents browsing the web or processing user-submitted documents will encounter attacker-controlled content as a matter of routine. Each webpage an agent visits is a potential injection point. The framework makes clear that this is not a niche edge case — it is a systematic property of how web-connected agents operate. Any agent with access to tools, memory, or real-world actions is exposed to these attack classes by design.

---

## Story 3 — AI API Routers: The Middleware Threat

**What Are AI API Routers?**  
Organizations often use third-party middleware — API routers — to manage, load-balance, or optimize calls between their AI agents and model providers like OpenAI or Anthropic. These routers sit in the middle of every tool call an agent makes.

**What Researchers Found**  
In testing several of these routers, researchers found that some were actively malicious or could be weaponized to:

- **Hijack tool calls** — intercepting an agent's intended action and substituting a different one
- **Alter commands mid-flight** — modifying parameters before they reach the model or downstream service
- **Steal intercepted credentials** — harvesting API keys and cloud credentials passing through the router
- **Inject malicious code** — inserting attacker-controlled instructions into agent workflows
- **Trigger wallet theft** — in one case, a router manipulated an agent operating in a researcher's crypto environment to initiate unauthorized transactions

**The Core Problem**  
Agents implicitly trust the infrastructure layer connecting them to AI providers. That trust is not validated. A router that claims to optimize API calls has the same access to agent traffic as a legitimate one — and that access includes every tool call, every credential, and every instruction passing through the pipeline.

---

## Key Takeaways

- **AI features grafted onto existing enterprise tools inherit none of those tools' security assumptions.** Grafana's data security model was built for dashboards, not for an AI component that can be prompted into making outbound HTTP requests. The addition of AI capabilities requires a complete re-evaluation of the threat model, not just an incremental patch.

- **Multi-layer bypasses are the norm, not the exception.** GrafanaGhost required three separate bypasses to achieve full silent exfiltration. Each individual control appeared functional in isolation. The attack only succeeded by chaining them. Defense in depth is necessary but not sufficient if each layer can be defeated independently.

- **The web is now an attack surface against AI agents.** Any agent that reads web content, processes user documents, or interacts with external services will encounter attacker-controlled content. The AI Agent Traps framework shows this isn't hypothetical — it's a systematic property of how agents operate. Agents need to treat external content as untrusted input, the same way applications treat user input.

- **The middleware layer is unaudited and trusted by default.** API routers sit between agents and model providers with full visibility into every tool call and credential. Organizations deploying agents through third-party routers are extending implicit trust to infrastructure they have likely never security-reviewed. This mirrors the LiteLLM supply chain problem from two weeks ago — the AI ecosystem's dependency on shared middleware creates concentrated, largely invisible risk.

- **Prompt injection at the infrastructure level is harder to detect than at the user level.** GrafanaGhost and the router manipulation attacks both operate below the visibility of standard security monitoring. No suspicious user behavior, no anomalous login, no malware signature. Data leaves through legitimate-looking requests. Detection requires monitoring AI-specific behavioral patterns, not traditional IOCs.
