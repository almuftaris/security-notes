# Malicious AI Assistant Browser Extensions Harvest Chat Histories

**Date**: March 13, 2026
**Source**: [Microsoft Security Blog](https://www.microsoft.com/en-us/security/blog/2026/03/05/malicious-ai-assistant-extensions-harvest-llm-chat-histories/)
**Impact**: ~900,000 installs, 20,000+ enterprise tenants affected

## The Attack: Fake AI Sidebars Stealing Everything

Malicious browser extensions disguised as AI assistant tools (ChatGPT sidebars, DeepSeek helpers) are silently collecting every conversation users have with AI platforms and every website they visit.

### **What They Look Like**:
- Named to sound legitimate: "Chat GPT for Chrome with GPT-5, Claude Sonnet & DeepSeek"
- Published on official Chrome Web Store (trusted distribution channel)
- Work on both Chrome and Edge browsers
- Mimic real extensions like AITOPIA to appear familiar

### **What They Steal**:
- **Full AI chat histories**: Every ChatGPT/DeepSeek/Claude conversation - prompts and responses
- **Complete browsing history**: All URLs visited, including internal company sites
- **Navigation context**: What page you came from, where you went next
- **Persistent tracking**: UUID assigned to follow you across sessions

### **Where It Goes**:
Data uploaded to attacker domains via HTTPS (looks like normal web traffic):
- deepaichats[.]com
- chatsaigpt[.]com  
- chataigpt[.]pro
- chatgptsidebar[.]pro

## Why This Matters for Enterprises

**The risk**: Employees routinely share sensitive information with AI tools:
- Proprietary code
- Internal workflows and processes
- Strategic discussions and planning
- Customer data, financial information
- Unreleased product details

**One extension install** = continuous leak of everything that employee discusses with AI or views in their browser.

## The Deceptive Tactics

### **1. Trusted Distribution**:
Extensions published on Chrome Web Store - users assume Google vetted them. Single listing works for both Chrome and Edge.

### **2. Misleading Consent**:
- Initial install asks permission to collect "analytics" (sounds harmless)
- Users can disable data collection... initially
- **Automatic updates silently re-enable collection** without notifying user
- Users think they opted out, but data still flowing

### **3. Agentic Browser Auto-Install**:
Some AI-powered browsers automatically downloaded these extensions without user approval because the names/descriptions seemed legitimate. User never explicitly chose to install it.

### **4. Minimal Suspicion**:
- Presents as normal productivity tool
- Data collection looks like standard analytics
- Periodic uploads blend into routine browser traffic
- No elevated permissions or suspicious behavior to trigger alerts

## How It Works (Simplified)

**Installation** → Extension granted broad permissions  
**Background collection** → Logs URLs and AI chats continuously  
**Local storage** → Data staged in Base64-encoded JSON  
**Periodic upload** → HTTPS POST to attacker servers every few minutes  
**Clear evidence** → Local data deleted after upload

**Persistence**: Extension auto-loads every time browser starts - no additional user action needed.

## The Scale Problem

**900,000 total installs** across consumer and enterprise users  
**20,000+ enterprise tenants** where employees interact with AI using sensitive data

**Why it spread so fast**:
- AI assistant extensions are wildly popular right now
- Users install them constantly for productivity
- Permissive enterprise extension policies (many companies don't restrict)
- Familiar AI branding (ChatGPT, Claude, DeepSeek names)

## What Data Was Exposed

**For individual users**: Personal conversations, browsing habits, accounts accessed

**For enterprises**:
- Code being debugged/written with AI assistance
- Business strategy discussed in AI chats
- Internal application URLs and access patterns
- Customer information queried through AI
- Financial data analyzed via AI tools
- Competitive intelligence shared with AI for analysis

**Duration of exposure**: From initial install until removal - could be weeks or months of accumulated data.

## Microsoft's Response

**Detection**: Microsoft Defender now flags these specific extension IDs as malicious

**Network protection**: Blocks connections to known attacker domains

**Vulnerability management**: Tool to inventory and audit all browser extensions in organization

**Microsoft Purview**: AI data security controls for browser-based AI chat applications

## Takeaways

- Browser extensions are the new malware delivery vector - trusted by users, rarely scrutinized
- AI assistant extensions have huge install bases because everyone wants AI productivity tools
- Misleading consent + automatic re-enabling = users have no idea data is being collected
- Chrome Web Store publication doesn't mean safe - attackers can publish malicious extensions
- Sensitive data leakage is continuous and comprehensive (not a one-time breach)
- 20,000 enterprises affected shows how widespread unsanctioned AI tool usage is
- Automatic updates can weaponize previously "safe" extensions
- Agentic browsers auto-installing extensions creates new attack vector

## Questions to Explore

- How long were these extensions available before detection?
- What percentage of the 900k installs were in enterprise vs consumer contexts?
- How much data was actually exfiltrated before takedown?
- Are the attackers nation-state, criminal, or something else?
- Why didn't Chrome Web Store review catch this?
- How many similar extensions are still active and undetected?
- Should enterprises block all AI assistant extensions by default?
- Can browser extension permissions be more granular to limit this risk?
