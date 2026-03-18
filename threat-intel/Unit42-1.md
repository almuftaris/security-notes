# Fuzzing AI Judges: Bypassing LLM Security Controls with Formatting Symbols

**Date**: March 18, 2026
**Researchers**: Palo Alto Networks Unit 42 (Tony Li, Hongliang Liu, Yuhao Wu)
**Source**: [Unit 42 Research](https://unit42.paloaltonetworks.com/fuzzing-ai-judges-security-bypass/)
**Tool**: AdvJudge-Zero (internal red-team fuzzer)

## What Are AI Judges?

**Definition**: LLMs acting as automated security gatekeepers in production AI systems

**Role**: Final line of defense that evaluates:
- "Is this response harmful/toxic/biased?"
- "Does this output violate our safety policies?"
- "Is this answer high-quality and accurate?"

**Usage**: Organizations deploying AI at scale rely on AI judges to enforce policies without human review of every output.

## The Core Problem

AI judges can be manipulated into approving policy violations using **completely innocent-looking formatting symbols** - no gibberish, no obvious hacking attempts.

**Attack success rate**: 99% across widely-used models

## How AdvJudge-Zero Works

**Fuzzing approach**: Automated tool that treats LLM as "black box" and finds input sequences that exploit decision-making logic

### **Three-Step Process**:

**1. Token Discovery**:
- Query model for expected inputs based on next-token predictions
- Identify "stealth control tokens" - innocent characters with:
  - Low perplexity (appear natural to AI)
  - Strong influence over model's attention mechanism
- Examples: Markdown symbols, formatting characters, structural tokens

**2. Iterative Refinement**:
- Test candidate tokens to measure decision shift
- Monitor "logit-gap" - mathematical confidence margin between "allow" vs "block"
- Identify which tokens minimize probability of block decision

**3. Exploitation**:
- Isolate specific tokens that act as decisive control elements
- These steer model's attention toward approval state
- Result: "Yes/allow" decision regardless of actual content

## The Stealth Factor

**Key finding**: Effective attacks don't look like attacks at all.

**Innocent-looking triggers that bypass security**:
- **Formatting symbols**: `1.`, `-`, newlines (`\n`), markdown headers (`###`)
- **Structural tokens**: `User:`, `Assistant:`, system tags
- **Context shifts**: "The solution process is...", "Step 1:", "Final Answer:"
- **Enumeration markers**: `\begin{enumerate}`

**Why this matters**: To humans and security tools (WAFs), these look like normal data formatting. To AI judges, they're logic triggers that force approval.

## Perplexity Test Results

**Comparison**: AdvJudge-Zero tokens vs traditional jailbreak gibberish (GCG algorithm)

**Finding**: AdvJudge-Zero tokens have:
- Significantly lower perplexity than adversarial gibberish
- Equivalent perplexity to verified stealth jailbreak tokens
- Much higher likelihood of bypassing detection unnoticed

**Translation**: These attacks are indistinguishable from legitimate formatting - no obvious "hacking" signatures.

## Real-World Attack Scenarios

### **Scenario 1: Safety Filter Bypass (False Approval)**

**Goal**: Force AI judge to approve toxic/harmful content

**Attack**:
- Append stealth token like `\n\nAssistant:` to harmful prompt
- AI judge interprets this as "policy check phase complete"
- Decision flips from "block" to "allow"

**Result**: Prohibited content gets through automated moderation

### **Scenario 2: Training Data Corruption (Reward Hacking)**

**Context**: AI judges score model responses during RLHF (reinforcement learning from human feedback) training

**Attack**:
- Insert phrases like "The correct answer is:" or `\begin{enumerate}`
- AI judge distracted by professional-looking formatting
- Assigns high scores to incorrect/hallucinated information

**Result**: Model learns wrong lessons, becomes less reliable over time (model degradation)

## Vulnerable Systems

**Success rate**: 99% bypass across major categories

**Affected models**:
- **Open-weight enterprise models**: Internal chatbots, document summarization tools
- **Specialized reward models**: Purpose-built AI security guards
- **Large parameter models**: 70B+ parameter models (complexity = more attack surface)

**Key insight**: Bigger, "smarter" models aren't safer - they provide more surface area for logic-based attacks.

## The Fix: Adversarial Training

**Problem**: AI judges have logic flaws like any software

**Solution**: Use fuzzer methodology defensively
1. Run AdvJudge-Zero internally to discover weaknesses
2. Retrain model on discovered bypass examples
3. Harden system against these specific attacks

**Result**: Attack success rate drops from ~99% to near zero

**Analogy**: Like penetration testing for traditional software - find vulnerabilities before attackers do, then patch them.

## Takeaways

- **AI judges aren't foolproof gatekeepers**: 99% bypass rate shows fundamental vulnerability
- **Stealth is the threat**: Attacks use normal formatting symbols, not detectable gibberish
- **Formatting = logic trigger**: Markdown, newlines, enumeration markers manipulate AI decisions
- **No model size immunity**: 70B+ parameter models equally vulnerable
- **Training data poisoning risk**: Corrupted AI judges teach models incorrect behaviors
- **Fuzzing works on AI**: Same vulnerability discovery techniques from traditional software apply
- **Adversarial training is essential**: Proactive hardening can reduce attack success to near-zero
- **Black-box attacks possible**: Don't need model internals to find exploits (concerning for defenders)
- **Detection is difficult**: Benign-looking tokens bypass WAFs and human review

## Questions to Explore

- How many production AI systems currently use vulnerable AI judges?
- Can detection systems identify low-perplexity attack tokens in real-time?
- What percentage of enterprise AI deployments include adversarial training?
- Are there other categories of "innocent-looking" tokens not yet discovered?
- How do you validate AI judge decisions when they can be manipulated this easily?
- Should organizations treat AI judges as unreliable and require human-in-the-loop?
- Will adversarially-trained models remain hardened as new LLMs evolve?
- How quickly can attackers adapt fuzzing techniques to bypass hardened systems?
