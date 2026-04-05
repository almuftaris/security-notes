# AI Security Threats — Week of March 30, 2026

**Primary Source:** [Check Point Research – March 30th Threat Intelligence Report](https://research.checkpoint.com/2026/30th-march-threat-intelligence-report/)  
**Supporting Sources:**  
- [Cyera – LangDrained: 3 Paths to Your Data Through LangChain](https://www.cyera.com/research/langdrained-3-paths-to-your-data-through-the-worlds-most-popular-ai-framework)  
- [The Hacker News – Claude Extension Flaw Enabled Zero-Click Prompt Injection](https://thehackernews.com/2026/03/claude-extension-flaw-enabled-zero.html)  
- [Cybernews – Critical LiteLLM Supply Chain Attack](https://cybernews.com/security/critical-litellm-supply-chain-attack-sends-shockwaves/)

---

## Overview

Three separate AI security disclosures this week share a common theme: the infrastructure underneath AI applications — the frameworks, extensions, and middleware that nobody thinks about — is where attackers are finding their footholds. None of these exploits required breaking the AI model itself. They all went around it.

---

## Story 1 — LiteLLM Supply Chain Compromise

**What is LiteLLM?**  
LiteLLM is a Python library that acts as a universal connector between applications and major AI services (OpenAI, Anthropic, Google, etc.). According to Wiz, it's present in roughly 36% of all cloud environments — making it critical, largely invisible infrastructure.

**What Happened**  
This attack is a downstream consequence of the Trivy supply chain compromise (late February 2026), in which attackers exploited a misconfigured GitHub Actions workflow in Aqua Security's Trivy repository to steal credentials and take over the repo. Aqua rotated some credentials but missed others.

On March 19, a threat group called **TeamPCP** used the remaining stolen tokens to push malicious releases across multiple projects — including LiteLLM on PyPI. The tainted LiteLLM packages contained:
- A credential harvester targeting API keys and cloud credentials
- A Kubernetes lateral movement toolkit
- A persistent backdoor

Because LiteLLM sits between applications and AI providers, any app using the compromised version was potentially exposing every API key it used to call AI services — OpenAI keys, Anthropic keys, cloud credentials — to the attacker.

Mandiant reported 1,000+ SaaS environments already compromised at time of writing, with projections of 10,000+. TeamPCP has since been reported collaborating with LAPSUS$.

**The Broader Chain**  
One misconfigured CI workflow → stolen credentials → Trivy takeover → LiteLLM backdoor → downstream AI applications compromised. Five ecosystems hit in total: GitHub Actions, Docker Hub, npm, Open VSX, and PyPI.

---

## Story 2 — Three Vulnerabilities in LangChain and LangGraph

**What is LangChain?**  
LangChain is the most widely used framework for building AI applications — chatbots, document Q&A systems, autonomous agents. With ~847 million PyPI downloads, it's the plumbing behind a large portion of enterprise AI including Replit, Uber, Cisco, CloudFlare, etc. Many organizations have it as a transitive dependency without knowing it's there.

**The Three Vulnerabilities**  
Cyera researchers found three independent flaws, each exposing a different class of sensitive data:

### Path Traversal — CVE-2026-34070 (CVSS 7.5)
LangChain lets developers load prompt templates from external files. The file path in those configs is passed directly to the file system with zero validation — no restriction on `../` traversal sequences, no base directory check, nothing.

An attacker who can supply a crafted prompt config (e.g., through a shared prompt marketplace or user-facing API) can point the path at any file the server can read — Docker credentials, AWS configs, Kubernetes manifests, CI/CD pipeline definitions. All of these happen to be stored in the exact formats LangChain accepts (JSON, YAML, TXT).

### Serialization Injection — CVE-2025-68664 (CVSS 9.3 — Critical)
LangChain uses a special marker (`"lc": 1`) to identify its own serialized objects. When deserializing, it trusts any dictionary carrying that marker — including ones an attacker smuggled in through an LLM response.

The practical attack: craft a prompt that causes the LLM to include a malicious structure in its response metadata. When LangChain's serialization pipeline processes it, the injected structure resolves to an environment variable — including secrets like `OPENAI_API_KEY`. The secret is extracted silently through normal application operation. With `secrets_from_env` set to `True` by default, no special configuration was required to trigger this.

The parallel to Log4Shell is apt: user-controlled data containing a special marker being interpreted as an executable instruction rather than plain text.

### SQL Injection — CVE-2025-67644 (CVSS 7.3)
LangGraph (LangChain's companion library for stateful agents) stores entire conversation histories in a checkpoint database. Its search function constructs SQL queries by dropping filter keys directly into an f-string with no validation.

An attacker supplying a crafted filter key can bypass access controls and retrieve every conversation in the system — other users' chats, sensitive business data, proprietary prompts, anything the agent has ever processed.

**Patches**  
- `langchain-core` ≥ 1.2.22 patches all three  
- `langgraph-checkpoint-sqlite` ≥ 3.0.1 patches the SQL injection  

**The Uncomfortable Observation**  
These are CWE-22, CWE-89, and CWE-502 — path traversal, SQL injection, and insecure deserialization. The OWASP Top 10 has flagged all three since 2003. They're now living inside the most popular AI framework on the planet.

---

## Story 3 — Zero-Click Prompt Injection in Claude's Chrome Extension

**What Happened**  
Koi Security researcher Oren Yomtov disclosed a vulnerability (dubbed **ShadowPrompt**) in Anthropic's Claude Chrome extension that allowed any website to silently inject prompts into a user's Claude session — with no clicks, no permission dialogs, and nothing visible to the user.

**The Two-Flaw Chain**  
The attack chains two separate issues:

1. **Overly permissive origin allowlist** — The extension trusted any subdomain matching `*.claude.ai` to send it prompts. This is a wider net than necessary.

2. **DOM-based XSS in an Arkose Labs CAPTCHA component** — A third-party CAPTCHA component hosted on `a-cdn.claude.ai` contained a cross-site scripting flaw. This meant an attacker could execute arbitrary JavaScript in the context of a trusted `claude.ai` subdomain.

Combined: an attacker's page embeds the vulnerable CAPTCHA component in a hidden iframe, sends a JavaScript payload through it, and that payload fires a prompt to the Claude extension — which accepts it as legitimate because it comes from an allow-listed domain. The victim sees nothing happen.

**What an Attacker Could Do**  
- Steal session tokens and access conversation history  
- Send emails impersonating the user  
- Request confidential data from Claude on the user's behalf  
- Perform any action the extension is capable of, silently

**Fix**  
Anthropic patched the extension (v1.0.41) to require an exact match to `claude.ai` rather than any subdomain. Arkose Labs fixed the XSS separately on February 19, 2026. Responsible disclosure was filed December 27, 2025.

---

## Key Takeaways

- **The AI attack surface has shifted to the infrastructure layer.** None of this week's exploits touched a model. They went through frameworks, extensions, middleware, and signing keys. Defenders focused on model-level risks are looking in the wrong place.

- **Supply chain compromises cascade faster than organizations can respond.** The Trivy → LiteLLM chain demonstrates how one missed credential rotation can propagate across thousands of environments within days. The AI ecosystem's heavy dependence on shared open-source infrastructure amplifies this risk significantly.

- **Classic application security failures are alive in cutting-edge AI tooling.** Path traversal, SQL injection, and insecure deserialization are not new problems. Their presence in LangChain — software handling the most sensitive data in enterprise environments — reflects an industry that is moving fast and not applying hard-won security lessons to new infrastructure.

- **Browser extensions with agentic capabilities are a high-value target.** An extension that can read your conversations, send emails, and act on your behalf is not just a tool — it's an autonomous agent operating under your identity. Its security boundary is only as strong as the weakest trusted origin in its allowlist.

- **Third-party components inside trust boundaries are a recurring blind spot.** The Arkose Labs CAPTCHA flaw lived on a `claude.ai` subdomain, which made it automatically trusted by the extension. The same pattern appears constantly: a trusted domain hosts a third-party component with weaker security, and that component becomes the entry point.
