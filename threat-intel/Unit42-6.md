# Amazon Bedrock AgentCore: Sandbox Escape & Identity Abuse

**Source:** Unit 42 (Palo Alto Networks) — Two-Part Series  
**Part 1:** https://unit42.paloaltonetworks.com/cracks-in-the-bedrock-sandbox/  
**Part 2:** https://unit42.paloaltonetworks.com/cracks-in-the-bedrock-agent-god-mode/  
**Published:** April 7–8, 2026  
**Date:** April 13

---

## Overview

Unit 42 published a two-part series on Amazon Bedrock AgentCore, AWS's framework for building and deploying AI agents. Part 1 is about network isolation — specifically a sandbox escape via DNS tunneling. Part 2 is about identity and permissions — overly broad IAM roles that enable cross-agent compromise. Together they form a complete attack chain: escape the sandbox, steal credentials, escalate, and exfiltrate.

All findings were responsibly disclosed to AWS, who made several remediations.

---

## Part 1 — Escaping the Sandbox via DNS Tunneling

### What AgentCore's Code Interpreter Is

AgentCore includes a Code Interpreter — a built-in tool that lets AI agents dynamically execute code. It offers three network modes: public (full internet), VPC (customer-controlled), and sandbox (supposedly fully isolated, "no external network access"). Organizations reach for sandbox mode when they want a safe environment to run agent-generated code.

### The Problem

Unit 42 started by doing internal reconnaissance inside the sandbox. They found that the microVM running the Code Interpreter used a metadata service (MMDS) — the same concept as EC2's IMDS — that was accessible via simple HTTP GET requests with no session token required. This is the older IMDSv1-style behavior. Modern EC2 uses IMDSv2, which requires a PUT request first to get a session token specifically to prevent SSRF attacks from trivially leaking credentials. The sandbox skipped that requirement entirely.

From within the Code Interpreter, you could just call the metadata endpoint and retrieve the IAM role credentials attached to the interpreter. That's bad, but not catastrophic if the network is truly isolated — stolen credentials inside a box with no way out are still contained.

Except the network wasn't truly isolated.

### The DNS Tunnel

While poking at the metadata, they found two undocumented endpoints exposing a pre-signed S3 URL and a KMS key ID. The existence of a pre-signed S3 URL implied the sandbox had some way to reach S3, which meant DNS resolution was functional — S3 endpoints need to be resolved.

They tested this directly: resolved an internal AWS domain (worked), then resolved `google.com` (also worked). The sandbox blocks direct TCP/UDP traffic to the internet, but recursive DNS queries to arbitrary public domains go through.

DNS tunneling exploits exactly this. Even without a TCP connection to the outside, you can exfiltrate data by encoding it into subdomain queries — `my-secret-data.attacker-controlled-domain.com`. When the sandbox's DNS resolver tries to resolve that subdomain, the query travels to the attacker's authoritative nameserver, carrying the encoded data with it. Commands can be sent back through DNS responses, making it a full bidirectional C2 channel.

They confirmed this with a PoC: encoded a secret into a subdomain, triggered DNS resolution from inside the sandbox, and watched the query arrive on their own nameserver instantly. Whois confirmed the source IP was AWS infrastructure.

### Why This Is Worse Than It Sounds

Organizations tend to grant more permissive IAM roles to sandbox interpreters precisely *because* they trust the "no external access" guarantee. They assume: even if something goes wrong, credentials can't leave. That assumption is wrong. An attacker running code inside the sandbox could retrieve IAM credentials from the unprotected MMDS endpoint, encode them as Base64 subdomains, and tunnel them out via DNS — then use those credentials externally to pivot into the broader AWS environment.

AWS updated their documentation after disclosure to acknowledge that sandbox mode permits DNS resolution and S3 access. The recommended mitigation for customers needing true isolation is VPC mode with Route 53 Resolver DNS Firewall.

---

## Part 2 — Agent God Mode: Overprivileged IAM by Default

### The AgentCore Starter Toolkit

AWS provides a starter CLI toolkit to make deploying agents easier. When you run `agentcore launch`, it automatically provisions a runtime, ECR image, memory store, and an IAM execution role. The problem is the IAM role it creates by default.

### What "God Mode" Means

Instead of scoping permissions to the specific agent being deployed, the auto-generated IAM policy grants wildcard (`*`) permissions across multiple AgentCore resource types. This means any agent running under the default role can:

**Read and poison other agents' memories.** AgentCore agents store conversation state and context in memory resources. The default policy grants `GetMemory` and `RetrieveMemoryRecords` on every memory resource in the account — not just the agent's own. If an attacker compromises one agent, they can read or corrupt the memory of every other agent in the account.

**Invoke any other agent.** The policy grants `InvokeAgentRuntime` on all runtimes in the account. A low-privilege agent can trigger execution of any high-privilege agent.

**Pull any container image from ECR.** Every AgentCore Runtime runs as a Docker container stored in ECR. The default policy grants `BatchGetImage` and `GetAuthorizationToken` on all repositories in the account. A compromised agent can authenticate to Docker, pull any container image, and read its full filesystem — source code, proprietary algorithms, config files, environment variables, anything baked into the image.

### The Full Kill Chain

The ECR access is what makes the memory attack practical at scale. The barrier to cross-agent memory poisoning is knowing the target's MemoryID — a unique identifier not exposed through normal channels. But that ID is stored in the container's `env-output.txt` file. Pull the container image, do static analysis, find the MemoryID, then use it to dump or poison the target agent's conversation history.

The complete chain:
1. Compromise any agent in the account (prompt injection, social engineering, etc.)
2. Use wildcard ECR permissions to pull another agent's container image
3. Extract the target's MemoryID from the image's config files
4. Use wildcard memory permissions to read or corrupt that agent's stored context
5. Optionally invoke the target agent directly via `InvokeAgentRuntime`

AWS's own security team described the default roles as providing "a flat permission structure that does not align with the principle of least privilege" and confirmed they should never be used in production. Documentation was updated with an explicit warning following disclosure.

---

## The Combined Picture

Part 1 and Part 2 address different attack surfaces but chain together cleanly:

- **Part 1** breaks the network boundary — credentials can leave the sandbox via DNS
- **Part 2** breaks the identity boundary — those credentials (or any agent's role) grant excessive access across the entire account

The full attack: escape sandbox → exfiltrate IAM credentials via DNS → use credentials externally to invoke, read, and pivot across every other agent in the account. The "sandbox" guarantee and the "isolated agent" guarantee both fail.

---

## Takeaways

- **"Sandbox" is a label, not a guarantee.** DNS is often left open for legitimate internal reasons, and that's enough to create a covert channel. Network isolation needs to be verified, not assumed.
- **The IMDSv1 pattern keeps showing up in new contexts.** Every time a new service introduces a metadata endpoint without session token enforcement, it recreates the same SSRF-to-credential-theft risk that IMDSv2 was designed to eliminate.
- **Default IAM roles in developer toolkits are almost always too permissive.** AWS's own starter toolkit shipped with wildcard permissions across an entire account. Least privilege should be the starting point, not something applied before going to production.
- **Multi-agent systems have a broader blast radius than single-agent ones.** Compromising one agent with overpermissioned defaults can mean compromising all of them. Inter-agent trust needs the same scrutiny as any other lateral movement path.
- **Abstraction layers aren't air gaps.** The managed service model creates an impression of deeper isolation than actually exists — the same cloud primitives (IAM, metadata services, DNS) still apply underneath.
