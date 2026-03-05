# Google Cloud Security Podcast EP265 - Unsanctioned AI Agent Risk in Enterprise

**Date**: March 5, 2026
**Guest**: Alastair Paterson, CEO @ Harmonic Security
**Source**: [YouTube](https://www.youtube.com/watch?v=qu51DBOkrNE)

## Three Categories of AI Security (Paterson's Framework)

**1. AI as Threat**: Accelerated phishing, deepfakes, malware generation  
**2. AI for Defense**: SOC automation, autonomous detection, agentic security  
**3. AI in the Enterprise**: ← Harmonic's focus - employees using AI tools in daily workflows

## The Real Problem: Shadow AI

### **Corporate Policy Theater**

Companies rush to implement AI usage policies, but the pattern is familiar:
- Policy documents published
- Nobody reads them
- Even fewer follow them
- Employees continue using AI anyway

**The data backs this up**:
- Dataset: 22 million employee prompts analyzed
- **25% contained sensitive information**
- **16% sent to personal AI accounts** (ChatGPT, Claude, etc.)

### **The Workaround Culture**

Many companies block corporate AI access entirely, so employees adapt:
- Use personal devices
- Use personal accounts
- Bypass corporate networks
- "Backdoor" access to get work done

**The paradox**: Blocking AI makes the problem worse, not better. Drives usage underground where there's zero visibility or control.

## Why Complete Blocking Fails

**Reality check**: AI technology is existential for competitive advantage. Companies that block AI entirely will fall behind competitors who adopt it properly.

**Two bad outcomes from blanket bans**:
1. Employees ignore the ban and use AI anyway (unmonitored)
2. Employees follow the ban and productivity suffers vs competitors

**The only viable path**: Managed, careful adoption with proper controls. Not "if" but "how."

## The 300% Adoption Surge

Corporate AI adoption has increased **300%** - this isn't a trend that's slowing down. The scale of usage makes governance critical now, not later.

**What this means**:
- Shadow AI exposure growing exponentially
- Data exfiltration risk multiplying
- Traditional security controls inadequate
- DLP suddenly became essential (not optional)

## Solving It: Visibility + Enablement

### **Step 1: Understand What Employees Are Actually Doing**

Get visibility into:
- Which AI tools employees use
- What workflows they're trying to enable
- Why they need AI capabilities
- What business problems they're solving

**Key insight**: You can't govern what you can't see. Most companies have no idea how AI is being used across their org.

### **Step 2: Enable, Don't Block**

**Wrong approach**: "We've banned all AI tools"  
**Right approach**: "Here's how to use AI safely for your work"

Security's role shifts from gatekeeper to enabler:
- Provide sanctioned AI tools that meet security requirements
- Give employees legitimate pathways to use AI
- Make the secure option the easy option

### **Step 3: Context-Aware Controls**

Governance has to understand business context:
- Finance team needs different AI access than marketing
- Some data can go to AI, some absolutely cannot
- Controls based on data sensitivity, not blanket rules
- Different risk tolerance by department/role

**Generic policies fail** because they ignore why people need AI in the first place.

## DLP Renaissance

Data Loss Prevention used to be a checkbox compliance thing. AI adoption made it critical infrastructure.

**Why DLP matters now**:
- Employees paste sensitive data into prompts constantly
- 25% of prompts contain sensitive info (per Harmonic's data)
- Personal accounts = data exfiltration by default
- No way to retrieve data once it's in ChatGPT/Claude/etc.

**Modern DLP needs**:
- Real-time prompt scanning before submission
- Context-aware sensitivity detection (not just regex for credit cards)
- Integration with AI tool APIs
- User education at point of risk (not annual training)

## The Enabler Mindset

**Old security**: "How do we stop employees from using AI?"  
**New security**: "How do we let employees use AI safely?"

**Paterson's argument**: Security teams must lean into the technology, not resist it.

Being the enabler means:
- Understanding business value AI provides
- Recognizing productivity gains are real
- Accepting AI adoption is inevitable
- Focusing energy on safe adoption, not prevention

**Cultural shift**: Security as business partner, not department of "no."

## My Takeaways

- **Shadow AI is the new shadow IT**: Same pattern, higher stakes (data vs access)
- **25% sensitive data in prompts is insane**: Most employees have no idea what's sensitive
- **Blocking doesn't work**: Just creates unmonitored risk
- **300% growth means this is urgent**: Not a future problem, it's happening now
- **DLP went from nice-to-have to critical**: AI made data exfiltration frictionless
- **Visibility comes first**: Can't govern what you can't see
- **Context matters more than rules**: Generic policies fail because business needs vary
- **Enablement > prevention**: Security has to help people use AI, not stop them
- **Personal accounts are the gap**: 16% of prompts going to uncontrolled endpoints
- **This is existential technology**: Companies that don't adopt AI lose competitive edge

## Questions to Explore

- How do you detect sensitive data in prompts without inspecting every employee's work?
- What's the false positive rate on AI DLP - how much does it disrupt legitimate work?
- Can you technically prevent copy-paste to personal AI accounts without breaking workflows?
- What percentage of companies actually have visibility into AI tool usage right now?
- How do you measure ROI of secure AI enablement vs blanket blocking?
- Should companies provide sanctioned AI accounts with enterprise agreements?
- What happens when employees use AI on personal devices outside corporate network?
- How do you educate employees on what's "sensitive" when they don't think about it?
