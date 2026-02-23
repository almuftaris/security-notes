# Anthropic Exposes Industrial-Scale Distillation Attacks by Chinese AI Labs

**Date**: February 23, 2026
**Source**: [Anthropic Security Report](https://www.anthropic.com/news/detecting-and-preventing-distillation-attacks)

## Three Chinese AI Labs Caught Red-Handed

Anthropic identified **industrial-scale distillation campaigns** by three Chinese AI laboratories systematically stealing Claude's capabilities:

### **DeepSeek**
- **Scale**: 150,000+ exchanges
- **Targets**: Reasoning capabilities, reward model training, censorship-safe query generation
- **Technique**: Asked Claude to articulate internal reasoning step-by-step to generate chain-of-thought training data
- **Notable**: Generated censorship-safe alternatives to politically sensitive queries (dissidents, party leaders, authoritarianism) to train their models to avoid censored topics
- **Attribution**: Request metadata traced to specific DeepSeek researchers

### **Moonshot AI (Kimi models)**
- **Scale**: 3.4 million+ exchanges
- **Targets**: Agentic reasoning, tool use, coding, data analysis, computer-use agents, computer vision
- **Technique**: Hundreds of fraudulent accounts across multiple access pathways
- **Attribution**: Request metadata matched public profiles of senior Moonshot staff

### **MiniMax**
- **Scale**: 13 million+ exchanges (largest campaign)
- **Targets**: Agentic coding, tool use, orchestration
- **Caught in real-time**: Anthropic detected campaign before MiniMax released their trained model
- **Adaptive**: When Anthropic released a new Claude model, MiniMax pivoted within 24 hours and redirected ~50% of traffic to the new version
- **Attribution**: Request metadata, infrastructure indicators, timing matched public product roadmap

## Total Campaign Scale

- **Over 16 million exchanges** with Claude
- **~24,000 fraudulent accounts**
- **Violation**: Terms of service and regional access restrictions
- **Access method**: Commercial proxy services reselling Claude API access

## How They Did It

### **"Hydra Cluster" Proxy Architecture**
- Commercial proxy services resell Claude access at scale
- Sprawling networks of fraudulent accounts (one proxy managed 20,000+ accounts simultaneously)
- Distribute traffic across Anthropic's API and third-party cloud platforms
- No single point of failure - when one account banned, another takes its place
- Mix distillation traffic with legitimate customer requests to hide in noise

### **Characteristic Attack Patterns**
- **Volume**: Tens of thousands of similar prompts
- **Coordination**: Synchronized traffic across hundreds of accounts
- **Targeting**: Narrow focus on specific capabilities (reasoning, coding, tool use)
- **Structure**: Highly repetitive prompt structures
- **Load balancing**: Identical patterns, shared payment methods, coordinated timing

### **Example Distillation Prompt Pattern**:
Single prompt seems benign:
> "You are an expert data analyst combining statistical rigor with deep domain knowledge. Your goal is to deliver data-driven insights..."

But variations arriving **tens of thousands of times across hundreds of coordinated accounts** = clear distillation attack.

## Why This Matters

### **National Security Implications**
- **Stripped safeguards**: Distilled models lack safety protections against bioweapon development, malicious cyber operations
- **Military/intelligence use**: Foreign labs feed unprotected capabilities into surveillance systems, offensive cyber ops, disinformation campaigns
- **Open-source risk**: If distilled models are released openly, dangerous capabilities spread beyond any government's control

### **Undermines Export Controls**
- Export controls designed to maintain US AI competitive advantage through chip restrictions
- Distillation allows Chinese labs to **steal** the capabilities export controls are meant to protect
- Chinese labs' "rapid advancements" partially depend on extracted American model capabilities, not just indigenous innovation
- **Reinforces need for export controls**: Restricted chip access limits both direct training AND scale of distillation

## Anthropic's Response

### **Detection Systems Built**:
1. Classifiers to identify distillation attack patterns in API traffic
2. Chain-of-thought elicitation detection for reasoning data extraction
3. Behavioral fingerprinting systems
4. Coordinated activity detection across large account networks

### **Defensive Actions**:
- **Intelligence sharing**: Technical indicators shared with other AI labs, cloud providers, authorities
- **Access controls**: Strengthened verification for educational accounts, security research programs, startup organizations
- **Countermeasures**: Product/API/model-level safeguards to reduce distillation efficacy without degrading legitimate use

### **Industry Call to Action**:
"No company can solve this alone" - requires coordinated response across AI industry, cloud providers, policymakers.

## Key Insights

- **This is happening NOW at massive scale**: 16M+ exchanges is industrial espionage, not research
- **Chinese labs are primary actors**: DeepSeek, Moonshot, MiniMax all Chinese companies
- **Adaptive adversaries**: MiniMax pivoted within 24 hours when new Claude released
- **Proxy services enable attacks**: Commercial resellers of API access are the weak link
- **Detection is possible**: Anthropic successfully identified and attributed all three campaigns
- **Attribution is strong**: IP addresses, metadata, infrastructure, corroboration from other platforms
- **Censorship training**: DeepSeek explicitly training models to avoid politically sensitive topics
- **Real-time visibility**: Caught MiniMax before their model launched

## My Takeaways

- This validates everything in the GTIG report - distillation is active, industrial-scale IP theft
- China is systematically stealing US AI capabilities to bypass export controls
- The 24-hour pivot shows how agile these operations are
- 13 million exchanges from MiniMax alone is staggering scale
- Censorship-safe query generation reveals authoritarian government priorities
- Proxy services are the critical vulnerability enabling this
- Anthropic publishing this is a shot across the bow - public attribution as deterrent
- Export controls ARE working if labs resort to theft rather than independent development

## Questions to Explore

- How many other Chinese labs are doing this but haven't been caught yet?
- What percentage of Chinese AI labs' capabilities come from distillation vs indigenous R&D?
- Can proxy services be regulated or shut down?
- Will other AI labs (OpenAI, Google) publish similar findings?
- What legal recourse exists for this level of IP theft?
- How does this impact US-China AI competition and policy?
