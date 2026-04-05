# Attacking Multi-Agent AI Systems: Amazon Bedrock Red-Team Research

**Source:** Unit 42 (Palo Alto Networks)  
**Date:** April 5, 2026  
**Link:** https://unit42.paloaltonetworks.com/amazon-bedrock-multiagent-applications/  
**Tags:** AI Security, Prompt Injection, Multi-Agent, LLM, Amazon Bedrock

---

## What This Is

Unit 42 red-teamed Amazon Bedrock Agents' multi-agent collaboration feature to see how far an attacker could get by exploiting prompt injection in an inter-agent environment. They tested against a publicly available AWS demo app (an energy management system) with Bedrock Guardrails intentionally disabled to expose the baseline risk.

No vulnerabilities were found in Bedrock itself. The research is about what's possible when developers leave built-in protections off — and how prompt injection scales in complexity as AI systems become more agentic.

---

## Bedrock Multi-Agent Architecture: Quick Background

Bedrock Agents supports two collaboration patterns:

- **Supervisor Mode** — a supervisor agent receives all requests, breaks them into sub-tasks, delegates to collaborator agents, and consolidates results. Suited for complex, multi-step tasks.
- **Supervisor with Routing Mode** — adds a lightweight router that handles simple requests by forwarding them directly to the appropriate collaborator, bypassing the supervisor. Falls back to full supervisor orchestration for complex requests.

Both modes involve inter-agent communication, which is the core attack surface explored here.

---

## Attack Methodology (Four Stages)

### 1. Operating Mode Detection

The attacker first determines which mode the application is running in, since payload delivery paths differ between modes.

The technique exploits artifacts specific to each mode:
- The `<agent_scenarios>` tag only appears in the router's prompt template, so a payload referencing it and getting routed reveals Supervisor with Routing Mode.
- The `AgentCommunication__sendMessage()` tool is specific to the supervisor, so a response invoking it reveals Supervisor Mode.

By crafting a detection payload that asks the system to behave differently depending on which prompt template is processing it, the attacker gets a clear signal.

### 2. Collaborator Agent Discovery

A crafted payload is sent to the supervisor to enumerate all collaborators. The trick is making the request complex enough that the router (if present) escalates it to the supervisor rather than handling it directly.

The supervisor's prompt template defines accessible collaborators inside an `<agents>` tag, but also contains guardrails telling it not to disclose agent names or tools. The discovery payload uses social engineering — asking for functional descriptions of each agent's capabilities rather than raw prompt contents — which the supervisor reveals even without disclosing exact names or identifiers.

The result is enough context to infer each agent's role and target them specifically.

### 3. Payload Delivery

Delivery varies by mode:

- **Supervisor Mode** — the payload references the target agent using info from the discovery stage, then leverages the supervisor's `AgentCommunication__sendMessage()` tool to forward the exact attacker-controlled instructions, with explicit instructions to the supervisor not to modify them.
- **Supervisor with Routing Mode** — the payload is framed around the target agent's domain, convincing the router to forward it directly rather than escalating.

The goal in both cases is to get attacker-controlled instructions to the target collaborator unmodified.

### 4. Target Agent Exploitation

Three exploit classes were demonstrated:

**Instruction Extraction**  
Payload prompts the agent to describe its role, goals, constraints, and tool usage in general terms. The agent paraphrases its system prompt without directly disclosing it — enough to infer the agent's configuration and capabilities.

**Tool Schema Extraction**  
Variant of the above, targeting tool schemas. The agent reveals each tool's purpose, required input parameters, and expected outputs. In the demo, this was executed against the Peak Load Optimization agent in Routing Mode.

**Tool Invocation with Malicious Inputs**  
The most impactful attack. The payload instructs an agent to invoke a specific tool with attacker-supplied content. In the demo, the Solar Panel Management agent was manipulated into creating a fraudulent support ticket that issued a $20,000 refund and 1,000 free credits. The tool invocation log confirmed the call was executed with the exact attacker-supplied inputs.

---

## Why This Works

LLMs cannot reliably distinguish between developer-defined instructions and adversarial user input. In a multi-agent system, this problem compounds — each additional agent is another LLM that might process untrusted text, and the inter-agent communication layer becomes an additional channel for injection.

The attack chain also demonstrates a clear escalation path: information leakage → schema enumeration → direct tool misuse. Even partial disclosure at stage one creates a foundation for more damaging attacks downstream.

---

## Defenses

**Bedrock-native:**
- **Pre-processing prompt** — runs before orchestration begins. Can be customized to detect suspicious patterns and reject adversarial inputs before they reach any agent.
- **Bedrock Guardrails** — runtime content filtering. Supports prompt injection detection, PII redaction, topic restriction, and response grounding. Can be configured per-agent based on role and sensitivity. Unit 42 confirmed that enabling the default Guardrail blocked all demonstrated attacks.

**General best practices:**
- **Capability scoping** — narrow each agent's task in its prompt template so unrelated requests are rejected outright.
- **Dual-layer input validation** — validate inputs at both the prompt level (acceptable formats) and the tool level (schemas, type checks, allowlists).
- **Tool vulnerability scanning** — tools that invoke APIs or execute code are part of the attack surface. They need SAST, DAST, and SCA like any other software component.
- **Least privilege** — agents should only have access to tools relevant to their role; tools should only have the data and API permissions they need.

---

## Takeaways

- Multi-agent architectures expand the prompt injection attack surface by adding inter-agent communication as a new injection channel.
- Operating mode and collaborator identity can be inferred through behavioral observation, not just direct disclosure.
- The escalation path from information leakage to tool misuse is systematic and doesn't require any vulnerability in the underlying platform.
- Bedrock's built-in protections (pre-processing prompt + Guardrails) are effective when enabled — the risk is largely a misconfiguration problem.
- Security-by-design for agentic systems means treating every LLM in the pipeline as a potential injection point, not just the user-facing interface.
