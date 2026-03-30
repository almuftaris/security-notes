# Claude.ai Vulnerability Chain — "Claudy Day" (March 2026)

**Source:** [Check Point Research – March 23rd Threat Intelligence Report](https://research.checkpoint.com/2026/23rd-march-threat-intelligence-report/)  
**Original Research:** [Oasis Security](https://www.oasis.security/blog/claude-ai-prompt-injection-data-exfiltration-vulnerability)  
**Date:** March 2026

---

## Overview

Oasis Security researchers disclosed three chained vulnerabilities in Claude.ai, collectively dubbed **"Claudy Day."** When combined, the flaws allow an attacker to silently inject instructions into a victim's Claude session, steal sensitive data from their conversation history, and deliver the attack through a convincing Google search ad — all without requiring any integrations or MCP servers.

Anthropic patched the prompt injection issue and is working on the remaining two.

---

## The Three Vulnerabilities

### 1. Invisible Prompt Injection via URL Parameter
Claude.ai lets users pre-fill a chat prompt through a URL parameter (`claude.ai/new?q=...`). Researchers found that certain HTML tags embedded in this parameter were invisible in the text box but still processed by Claude when the user hit Enter. This allowed attackers to hide arbitrary instructions inside what looked like a normal prompt.

### 2. Data Exfiltration via the Files API
Claude's sandbox blocks most outbound network traffic — but not connections to `api.anthropic.com`. By embedding an attacker-controlled Anthropic API key in the hidden prompt, attackers could instruct Claude to:
- Search the user's conversation history for sensitive information
- Write that data to a file
- Upload it to the attacker's Anthropic account via the Files API

No external tools or integrations needed — only built-in Claude capabilities.

### 3. Open Redirect on claude.com
Any URL in the format `claude.com/redirect/<target>` would redirect visitors to an arbitrary external domain with no validation. Combined with Google Ads (which only checks the hostname, not where it redirects), attackers could serve ads displaying a trusted `claude.com` URL that silently redirected victims to the malicious injection URL. The ad appeared as a legitimate Google search result.

---

## Why This Matters

Even in a default Claude.ai session with no integrations enabled, an attacker could build a detailed profile of a user by extracting their conversation history — health concerns, business strategy, personal matters, financial details.

In environments with MCP servers or enterprise tool integrations, the impact is far worse: injected prompts could read files, send messages, and interact with connected services — all silently.

The delivery mechanism is also notable. This wasn't a phishing email. It was a **Google search result** that looked completely legitimate.

---

## Key Takeaways

- AI assistants that have access to personal history and connected tools are high-value targets for prompt injection attacks
- Prompt injection doesn't require the user to do anything wrong — just clicking a realistic-looking search result is enough
- Sandboxed environments aren't always as isolated as they appear — outbound access to first-party APIs can still be exploited
- Chaining low-severity issues together can result in a critical end-to-end attack

---

## Questions to Explore

- What defenses exist against prompt injection at the model level vs. the application level?
- How does MITRE ATT&CK categorize prompt injection attacks against AI systems?
- Could similar exfiltration paths exist in other AI platforms that use first-party file storage APIs?
