# GCP Vertex AI Agent Engine — Privilege Escalation & Data Exfiltration Research (Unit 42, March 2026)

**Source:** [Unit 42 – Double Agents: Exposing Security Blind Spots in GCP Vertex AI](https://unit42.paloaltonetworks.com/)  
**Date:** April 2, 2026

---

## Overview

Unit 42 researchers deployed a malicious AI agent on Google Cloud's Vertex AI Agent Engine and demonstrated a privilege escalation chain that moved from a single compromised agent to unrestricted access across the host GCP project — and into Google's own internal infrastructure. Google updated their documentation following responsible disclosure, and recommended a mitigation path, but the underlying design issues reflect a broader pattern in how agentic AI platforms handle permissions.

---

## The Attack Chain

### Step 1 — Overprivileged Service Agent by Default
When a Vertex AI agent is deployed, GCP automatically assigns it a **Per-Project, Per-Product Service Agent (P4SA)** — a Google-managed service account. By default, this service agent carries excessive permissions far beyond what most agents would need.

### Step 2 — Credential Extraction via Metadata Service
The deployed agent can make requests to GCP's internal metadata endpoint:
```
http://metadata.google.internal/computeMetadata/v1/instance/?recursive=true
```

This returns the service agent's credentials — its identity, project, OAuth scopes, and access token — in plain JSON.

### Step 3 — Pivot to Consumer Project Storage
Using the stolen credentials, researchers gained unrestricted read access to all Google Cloud Storage buckets in the consumer project (i.e., the victim organization's own GCP project). Default permissions included the ability to list and read all bucket objects.

### Step 4 — Access to Google's Internal Infrastructure
The same credentials also unlocked access to **restricted, Google-owned Artifact Registry repositories** — private container image repos that form the core of the Vertex AI Reasoning Engine. These are not publicly accessible, yet the P4SA credentials granted entry.

Researchers were able to download proprietary Google container images, enumerate internal repositories, and inspect deployment files including a `Dockerfile.zip` that contained references to restricted internal GCP buckets.

### Step 5 — Insecure Serialization Risk
Agent code is packaged and deployed as a **Python pickle file** (`code.pkl`). Pickle deserialization is inherently unsafe — a manipulated pickle file can execute arbitrary code. Researchers confirmed the structure of Google's internal source code was visible upon deserialization in a controlled environment. The RCE potential was not tested, but the risk is documented.

### Step 6 — Overly Broad OAuth Scopes
The Agent Engine's default OAuth 2.0 token includes scopes covering Gmail, Google Calendar, Google Drive, and YouTube. While IAM permissions would need to be separately granted before these could be actively abused, the scopes themselves are non-editable by default — violating least privilege at the API access layer before an attack even begins.

---

## Google's Response

- Updated official Vertex AI documentation to explicitly describe how service accounts and agents interact
- Recommended **Bring Your Own Service Account (BYOSA)** as a mitigation: replacing the default P4SA with a custom, scoped service account
- Confirmed that controls prevent the service agent from modifying production Artifact Registry base images (cross-tenant image poisoning is blocked by design)

---

## Key Takeaways

- **Agentic AI platforms are not being built with security-first defaults.** A freshly deployed agent in a major enterprise cloud platform came with credentials capable of pivoting across project boundaries and into the platform vendor's own infrastructure. This is a design failure, not a misconfiguration by the end user.

- **The productivity rush is creating latent risk at scale.** Organizations are deploying AI agents rapidly to automate workflows, and most are not auditing what permissions those agents carry by default. Every agent is a potential insider threat if its service account is over-permissioned — and right now, the defaults make that the common case.

- **Supply chain risk is expanding into the AI agent layer.** A malicious agent packaged as a helpful productivity tool and shared via the open-source ecosystem could activate post-deployment. The agent marketplace model — where pre-built agents are shared and deployed without deep vetting — mirrors the npm/PyPI supply chain problem, now applied to autonomous systems with cloud credentials.

- **The metadata service is a recurring pivot point in cloud attacks.** This is not a new technique — credential theft via cloud metadata endpoints has been documented in AWS, Azure, and GCP environments for years. The difference here is that the attack surface now includes every AI agent an organization deploys, not just misconfigured VMs.

- **The deeper concern is what we don't know yet.** This research was limited in scope. The pickle deserialization vector wasn't fully tested. The OAuth scope exposure wasn't fully exploited. These are known gaps in one vendor's platform, discovered in a controlled setting. The broader agentic AI ecosystem — across cloud providers, enterprise tools, and third-party integrations — has not been systematically audited with this lens.
