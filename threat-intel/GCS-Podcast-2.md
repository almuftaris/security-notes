# Google Cloud Security Podcast EP264 - Measuring Agentic SOC Performance

**Date**: March 3, 2026
**Episode**: Measuring Your (Agentic) SOC: Two Security Leaders Walk into a Podcast
**Source**: [YouTube](https://www.youtube.com/watch?v=ZNyvd__9vuk)

## Core Thesis: Old Metrics Don't Work for AI-Driven SOCs

Traditional SOC metrics like **MTTD (Mean Time To Detect)** and **MTTR (Mean Time To Respond)** create perverse incentives when applied to agentic AI systems. These metrics prioritize speed over quality, encouraging analysts to close tickets as fast as possible rather than conducting thorough investigations.

### **The Problem with Speed Metrics**:
- **Incentivizes button-clicking over thinking**: How fast can you close the ticket vs how well did you investigate
- **Doesn't measure investigation quality**: Fast closure of false positive looks the same as fast closure after deep analysis
- **Misaligned with actual security outcomes**: Speed doesn't equal better security posture
- **Bad for autonomous systems**: AI can close tickets instantly, but was the decision correct?

## New Metrics for Agentic SOC: Accuracy Over Speed

**Google's Detection and Response Director** describes their approach to measuring AI agent performance:

### **Primary Metric: True Positive Accuracy**
- Track how accurately agentic systems identify genuine security events vs false positives
- Measure precision: Of all events flagged as malicious, what percentage are actually threats?
- Monitor false negative rate: What percentage of real threats did the agent miss?

### **Agent Failure Rate Monitoring**
Google actively tracks when AI agents fail during investigation:
- **Failure to investigate**: Agent decides not to investigate an event that should have been examined
- **Investigation errors**: Agent investigates but reaches incorrect conclusion
- **Incomplete analysis**: Agent stops investigation prematurely without gathering sufficient context

**Key insight**: Understanding failure modes is more valuable than measuring speed.

## The Explainability Problem: Non-Deterministic AI in Audit Contexts

### **Regulatory Challenge**:
When an AI agent decides **not** to investigate an event that later turns out to be malicious:
- **How do you explain that decision?** "The AI didn't think it was important" doesn't satisfy auditors
- **Who is accountable?** The AI? The SOC manager who deployed it? The vendor?
- **Can you reproduce the decision?** Non-deterministic AI may make different choices given same inputs
- **How do you prove due diligence?** Audit requirements demand explainable security decisions

### **Compliance Implications**:
Current regulatory frameworks assume human decision-makers with documented reasoning. Agentic SOCs introduce:
- **Black box decisions**: AI reasoning may not be fully transparent
- **Inconsistent decisions**: Same event might be handled differently on different days
- **Accountability gaps**: Who signs off on AI-driven triage decisions?

## Humans Still Required: AI Can't Handle Nuance

**Consensus view from Google security leaders**:

### **What AI Handles Well**:
- Repetitive tasks (log parsing, indicator enrichment, basic correlation)
- High-volume triage (sorting thousands of low-confidence alerts)
- Pattern matching at scale
- Known attack technique detection

### **What Humans Still Own**:
- **Critical thinking**: Understanding "why" behind attacker behavior
- **Nuance and context**: Business-specific knowledge AI lacks
- **Novel threats**: Unknown attack patterns requiring creative analysis
- **Strategic decisions**: Risk acceptance, incident severity judgment, response prioritization
- **The hard parts**: Sophisticated attacks, APT investigations, insider threats

### **AI as Augmentation, Not Replacement**:
The panel agrees AI augments analyst capabilities but doesn't replace the need for skilled security professionals. Automation handles the repetitive work so humans can focus on complex investigations requiring judgment.

## Measuring What Actually Matters

**Proposed shift in SOC metrics**:

### **Old Approach**:
- MTTD: How fast did we detect?
- MTTR: How fast did we respond?
- Tickets closed per day
- Alert volume reduction

### **New Approach**:
- **True positive accuracy**: Are we correctly identifying real threats?
- **False negative rate**: What are we missing?
- **Investigation quality**: How thorough was the analysis?
- **Agent failure analysis**: Where and why do AI systems make mistakes?
- **Explainability score**: Can we articulate why decisions were made?
- **Human analyst satisfaction**: Is AI actually making their jobs better?

## The Accountability Question

When agentic SOC makes a mistake (misses a real attack or flags too many false positives):
- **Technical failure**: Did the AI malfunction or was it trained poorly?
- **Process failure**: Were humans properly supervising AI decisions?
- **Design failure**: Was the AI given appropriate authority for decisions it made?

**Current state**: Industry hasn't settled on accountability frameworks for autonomous security decisions.

## My Takeaways

- **Speed metrics are dead for AI-driven SOCs**: MTTD/MTTR optimized for humans, not autonomous systems
- **Accuracy > Speed**: Better to be slow and right than fast and wrong
- **Explainability is the compliance blocker**: Regulators and auditors need to understand AI decisions
- **Non-determinism creates audit problems**: Can't reproduce AI decisions for compliance review
- **Humans own the nuance**: AI handles volume, humans handle complexity
- **We're in transition period**: Metrics and accountability frameworks haven't caught up to technology
- **Failure analysis is critical**: Understanding where AI fails is more important than celebrating where it succeeds
- **Quality over quantity**: Closing 10,000 tickets fast is useless if 20% were misclassified

## Questions to Explore

- What accuracy threshold makes an agentic SOC "good enough" for production?
- How do you build explainability into non-deterministic AI systems?
- Who gets sued when an AI agent misses a breach - the company, the vendor, the SOC manager?
- Can we create deterministic "audit mode" for AI agents to replay decisions?
- What happens when AI accuracy degrades over time as attackers adapt?
- Should there be regulatory standards for minimum AI agent accuracy in SOC operations?
- How do you measure "investigation quality" quantitatively?
- Will insurance companies require minimum AI accuracy rates for cyber coverage?
