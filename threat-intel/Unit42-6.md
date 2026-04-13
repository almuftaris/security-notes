Amazon Bedrock AgentCore: Sandbox Escape & Identity Abuse (Unit 42, April 2026)
Source: Unit 42 (Palo Alto Networks) — Two-Part Series
Part 1: https://unit42.paloaltonetworks.com/cracks-in-the-bedrock-sandbox/
Part 2: https://unit42.paloaltonetworks.com/cracks-in-the-bedrock-agent-god-mode/
Published: April 7–8, 2026
Tags: AI Security, AWS, Amazon Bedrock, AgentCore, DNS Tunneling, IAM, Privilege Escalation, Sandbox Escape

Overview
Unit 42 published a two-part series on Amazon Bedrock AgentCore, AWS's framework for building and deploying AI agents. The two papers are separate but closely connected — Part 1 is about network isolation (sandbox escape via DNS tunneling), and Part 2 is about identity and permissions (overly broad IAM roles enabling cross-agent compromise). Together they form a complete attack chain: escape the sandbox, steal credentials, escalate, and exfiltrate.
All findings were responsibly disclosed to AWS, who made several remediations.

Part 1 — Escaping the Sandbox via DNS Tunneling
What AgentCore's Code Interpreter Is
AgentCore includes a Code Interpreter — a built-in tool that lets AI agents dynamically execute code. It offers three network modes: public (full internet), VPC (customer-controlled), and sandbox (supposedly fully isolated, "no external network access"). The sandbox mode is what organizations reach for when they want a safe environment to run agent-generated code.
The Problem
Unit 42 started by doing internal reconnaissance inside the sandbox. They found that the microVM running the Code Interpreter had a metadata service (MMDS) — basically the same idea as EC2's IMDS — that was accessible via simple HTTP GET requests with no session token required. This is the older, less secure IMDSv1-style behavior. In modern EC2, IMDSv2 requires a PUT request first to get a session token, specifically to prevent SSRF attacks from trivially leaking credentials. The sandbox was skipping that requirement.
From within the Code Interpreter, they could just call the metadata endpoint and retrieve the IAM role credentials attached to the interpreter. That's a big deal, but also not the end of the world if the network is truly isolated — stolen credentials inside a box with no way out are still contained.
Except the network wasn't truly isolated.
The DNS Tunnel
While poking at the metadata, they found two undocumented endpoints exposing a pre-signed S3 URL and a KMS key ID. The existence of an S3 pre-signed URL implied that the sandbox had some way to reach S3, which in turn implied DNS resolution was functional — because S3 endpoints need to be resolved.
They tested this directly: resolved an internal AWS domain (worked), then resolved google.com (also worked). The sandbox blocks direct TCP/UDP traffic to the internet, but recursive DNS queries to arbitrary public domains go through.
DNS tunneling takes advantage of exactly this. Even without a TCP connection to the outside world, you can exfiltrate data by encoding it into subdomain queries — my-secret-data.attacker-controlled-domain.com. When the sandbox's DNS resolver tries to resolve that domain, the query travels to the attacker's authoritative nameserver, carrying the data with it. You can also receive commands back through DNS responses, making it a full bidirectional C2 channel.
They confirmed this with a PoC: encoded a secret into a subdomain, triggered the DNS resolution, and watched it arrive on their own nameserver instantly. Whois confirmed the source IP was AWS infrastructure.
Why This Is Worse Than It Sounds
Organizations tend to grant more permissive IAM roles to sandbox interpreters precisely because they trust the "no external access" guarantee. They think: even if something goes wrong, the credentials can't leave. That assumption is wrong. An attacker running code inside the sandbox could:

Retrieve IAM credentials from the unprotected MMDS endpoint
Encode them as Base64 subdomains and tunnel them out via DNS
Use those credentials externally to pivot into the broader AWS environment

AWS updated their documentation after disclosure to acknowledge that sandbox mode does permit DNS resolution and S3 access, removing the "no external network access" language. The recommended mitigation for customers needing true isolation is VPC mode with Route 53 Resolver DNS Firewall.

Part 2 — Agent God Mode: Overprivileged IAM by Default
The AgentCore Starter Toolkit
AWS provides a starter toolkit CLI to make deploying agents easier. When you run agentcore launch, it automatically provisions a runtime, ECR image, memory store, and an IAM execution role. The problem is the IAM role it creates by default.
What "God Mode" Means
Instead of scoping permissions to the specific agent being deployed, the auto-generated IAM policy grants wildcard permissions — * — across multiple AgentCore resource types. This means any agent running under that default role can:
Read and poison other agents' memories. AgentCore agents store conversation state and context in memory resources. The default policy grants GetMemory and RetrieveMemoryRecords on arn:aws:bedrock-agentcore:*:memory/* — every memory resource in the account, not just the agent's own. If an attacker compromises one agent, they can read or corrupt the memory of every other agent in the account.
Invoke any other agent. The policy grants InvokeAgentRuntime on arn:aws:bedrock-agentcore:*:runtime/*. A low-privilege agent can trigger execution of any high-privilege agent.
Pull any container image from ECR. Every AgentCore Runtime runs as a Docker container stored in ECR. The default policy grants BatchGetImage and GetAuthorizationToken on all repositories in the account. A compromised agent can authenticate to Docker, pull any container image, and read its full filesystem — including source code, proprietary algorithms, config files, environment variables, and anything else baked into the image.
The Full Kill Chain
The ECR access is what makes the memory attack practical at scale. The barrier to cross-agent memory poisoning is knowing the target's MemoryID — a unique identifier that isn't exposed through normal channels. But that ID is stored in the container's env-output.txt file. Pull the container image, do static analysis, find the MemoryID, then use it to dump or poison the target agent's conversation history.
The complete chain:

Compromise any agent in the account (social engineering, prompt injection, whatever)
Use the default wildcard ECR permissions to pull another agent's container image
Extract the target's MemoryID from the image's config files
Use wildcard memory permissions to read or corrupt that agent's stored context
Optionally invoke the target agent directly using InvokeAgentRuntime

AWS's own security team described the default roles as providing "a flat permission structure that does not align with the principle of least privilege" and said they should never be used in production. The documentation was updated with an explicit warning to that effect following disclosure.

The Combined Picture
Part 1 and Part 2 address different attack surfaces but chain together cleanly:

Part 1 breaks the network boundary — credentials can leave the sandbox via DNS
Part 2 breaks the identity boundary — those credentials (or any agent's role) grant excessive access across the entire account

The full attack: escape sandbox → exfiltrate IAM credentials via DNS → use credentials externally to invoke, read, and pivot across other agents in the account. The "sandbox" guarantee and the "isolated agent" guarantee both fail.

Takeaways

"Sandbox" is a label, not a guarantee. DNS is often left open for legitimate internal reasons, and that's enough to create a covert channel. Network isolation needs to be verified, not assumed.
The IMDSv1 pattern keeps showing up in new contexts. Every time a new service introduces a metadata endpoint without session token enforcement, it recreates the same SSRF-to-credential-theft risk that IMDSv2 was designed to eliminate.
Default IAM roles in developer toolkits are almost always too permissive. AWS's own starter toolkit shipped with wildcard permissions across an entire account. Least privilege should be the starting point, not something you apply before going to production.
Multi-agent systems have a broader blast radius than single-agent ones. Compromising one agent with overpermissioned defaults means compromising all of them. The inter-agent trust model needs to be treated with the same scrutiny as any other lateral movement path.
Abstraction layers aren't air gaps. The managed service model creates an impression of deeper isolation than actually exists — the same cloud primitives (IAM, metadata services, DNS) still apply underneath.
