# Case Study: AI-Assisted AWS Attack - 0 to Admin in 10 Minutes

**Date**: November 28, 2025 (Investigated by Sysdig TRT)
**Source**: [Sysdig Threat Research Team Analysis ](https://www.sysdig.com/blog/ai-assisted-cloud-intrusion-achieves-admin-access-in-8-minutes)
**Timeline**: Initial access to administrative privileges in **under 10 minutes**

## Attack Overview

Threat actor compromised an AWS environment with remarkable speed, demonstrating multiple indicators of LLM-assisted offensive operations. The attack progressed through credential theft, privilege escalation, lateral movement across 19 AWS principals, LLMjacking via Bedrock, and GPU instance provisioning for resource abuse.

## Attack Chain Summary

### 1. **Initial Access - Credential Theft from Public S3 Buckets**
- Discovered valid IAM credentials in public S3 buckets containing RAG data for AI models
- Buckets named using common AI tool naming conventions (likely targeted during reconnaissance)
- Compromised IAM user had Lambda read/write permissions and restricted Bedrock access
- **AI Indicator**: Active searching for AI-related bucket naming patterns

### 2. **Extensive Reconnaissance Across AWS Services**
Enumerated resources across:
- Core Services: Secrets Manager, SSM, S3, Lambda, EC2, ECS, RDS, CloudWatch, KMS
- AI Services: Bedrock (agents, knowledge bases, foundation models, custom models, inference profiles), OpenSearch Serverless, SageMaker
- **AI Indicator**: Comprehensive, systematic enumeration pattern typical of automated reconnaissance

### 3. **Privilege Escalation via Lambda Code Injection**
- Attempted to assume admin roles (sysadmin, netadmin) - failed
- Pivoted to Lambda function code injection using UpdateFunctionCode permissions
- Replaced existing Lambda function code **three times**, iterating on target users
- Successfully compromised admin user "frick" and created access keys
- **AI Indicators**:
  - Code comments written in **Serbian** (suggesting threat actor origin)
  - Comprehensive exception handling
  - Speed of script development (entire sequence in 8 minutes)
  - Clean, well-structured code with proper error handling

### 4. **Lateral Movement Across 19 Unique AWS Principals**
Compromised:
- 6 different IAM roles across 14 sessions
- 5 IAM users (4 existing + 1 newly created "backdoor-admin")
- Total: **19 unique AWS principals**

Attempted to assume OrganizationAccountAccessRole across multiple accounts including:
- **AI Hallucination Evidence**: Attempted roles in non-existent accounts with ascending/descending digit patterns (123456789012, 210987654321)
- Used session names reflecting intent: "explore", "test", "pwned", "escalation", **"claude-session"**

### 5. **Data Collection and Exfiltration**
Collected sensitive data from:
- Secrets Manager secrets
- SSM parameters
- CloudWatch logs
- Lambda function source code
- Internal S3 bucket data
- CloudTrail events
- IAM Access Analyzer findings (to identify additional attack paths)

### 6. **LLMjacking via Amazon Bedrock**
- Checked if model invocation logging was enabled (it wasn't)
- Invoked multiple AI models:
  - Claude Sonnet 4, Opus 4, 3.5 Sonnet, 3 Haiku
  - DeepSeek R1
  - Llama 4 Scout
  - Amazon Nova Premier
  - Amazon Titan Image Generator
  - Cohere Embed v3
- Used cross-region inference for increased throughput
- Uploaded Terraform module to S3 for backdoor Lambda deployment with public URL (not executed before access revoked)

### 7. **GPU Instance Provisioning for Resource Abuse**
- Enumerated EC2 instances and deep learning AMIs (1,300+ images)
- Created key pair "stevan-gpu-key" and open security group
- Attempted to launch p5.48xlarge instance 5 times (failed - insufficient capacity)
- Successfully launched **p4d.24xlarge instance** ($32.77/hour = ~$23,600/month)
  - 8x NVIDIA A100 GPUs (40GB each = 320GB total)
  - 96 vCPUs, 1152GB RAM, 400 Gbps network
  - 2TB shared EBS volume
- Terminated instance after 5 minutes (unclear why)
- **AI Indicators**:
  - User data script references **non-existent GitHub repository** (https://github.com/anthropic/training-scripts.git) - **hallucination**
  - Script designed to launch publicly accessible JupyterLab on port 8888 for backdoor access

### 8. **Defense Evasion Techniques**
- **IP rotator tool**: Changed source IP for each request to bypass correlation-based detection
- **Principal distribution**: Spread operations across 19 identities to complicate tracking and maintain persistence
- **Role chaining**: Assumed roles only to assume other roles, obscuring privilege escalation paths

## Evidence of AI-Assisted Operations

### Strong AI Indicators:
1. **Serbian comments in code**: Suggests non-native English speaker using LLM for code generation
2. **Hallucinated resources**:
   - AWS account IDs with sequential patterns (123456789012, 210987654321)
   - Non-existent GitHub repository (anthropic/training-scripts.git)
3. **Session name "claude-session"**: Direct reference to Claude AI
4. **Code quality**: Professional exception handling, clean structure, rapid development
5. **Speed**: 0 to admin in 8 minutes including reconnaissance
6. **Systematic enumeration**: Pattern consistent with automated LLM reconnaissance
7. **Iterative approach**: Modified Lambda code three times, learning from failures

### Behavioral Patterns:
- Comprehensive resource enumeration beyond typical human scope
- Rapid code development and deployment
- Systematic trial-and-error with role assumptions
- Well-structured Terraform modules and scripts appearing suddenly in S3

## Key Vulnerabilities Exploited

1. **Public S3 buckets** with long-term IAM credentials (instead of temporary IAM roles)
2. **Overly permissive Lambda execution role** enabled privilege escalation
3. **No Bedrock model invocation logging** allowed undetected LLMjacking
4. **Lack of Service Control Policies** to restrict model invocations and instance types
5. **ReadOnlyAccess policy** on compromised user enabled extensive reconnaissance
6. **No monitoring** for IAM Access Analyzer enumeration

## Threat Actor Objectives (Unclear)

Possible motivations:
- **Model training**: GPU instance setup with deep learning libraries
- **Compute reselling**: JupyterLab backdoor for selling access
- **Credential harvesting**: Creating persistent backdoors for future access
- **Espionage**: Data exfiltration from Secrets Manager, SSM, CloudWatch
- **Testing/demonstration**: Rapid termination of GPU instance suggests possible proof-of-concept

## My Takeaways

- **AI is accelerating offensive operations**: What would take hours/days now takes minutes
- **LLM hallucinations in attacks**: Non-existent repos and fake account IDs reveal AI assistance
- **Speed is the new threat vector**: 10 minutes is insufficient time for human SOC response
- **AI-vs-AI defense necessary**: Only autonomous SOC systems can respond at this speed
- **"Living off the cloud"**: Abusing legitimate AWS features (Lambda, Bedrock, IAM) leaves minimal malicious footprint
- **Credential security is critical**: Public S3 bucket exposure led to complete environment compromise
- **IAM least privilege is non-negotiable**: Overly permissive Lambda execution role was the pivot point
- **Detection must be behavioral**: Traditional signature-based detection useless against AI-generated code

## Connection to Other Trends

- **Sean Heelan's Exploit Industrialization**: AI trading tokens for offensive capabilities (now applied to cloud attacks)
- **Autonomous SOC**: Reinforces need for AI-driven defense - humans can't respond in 10 minutes
- **LLMjacking**: First identified by Sysdig in May 2024, now mainstream attack technique
- **DPRK Operations**: Similar pattern of abusing cloud AI services for unauthorized compute/model access
- **AI-Generated Malware**: KONNI, Amaranth-Dragon also using LLMs - now extended to cloud attacks

## Questions to Explore

- Can we fingerprint LLM-generated scripts by provider (GPT vs Claude vs others)?
- What behavioral patterns differentiate AI-assisted attacks from human attacks?
- How do organizations balance AI service accessibility with security controls?
- Should cloud providers implement mandatory rate limiting on administrative API calls?
- What's the threshold for "suspicious enumeration" that doesn't create false positives?
- How can we detect role chaining patterns automatically?

## Detection Recommendations (Sysdig)

**Critical Events to Monitor:**
- UpdateFunctionCode on Lambda
- CreateAccessKey for users
- AttachAdministratorPolicy
- Bedrock model reconnaissance activity
- Security group rule allowing public ingress
- Lateral movement using role chaining
- High number of Bedrock model invocations
- Access key/IAM/Lambda/Organization enumeration patterns

**Defense Priorities:**
1. Remove long-term credentials from public S3 buckets (use IAM roles instead)
2. Apply least privilege to Lambda execution roles
3. Restrict UpdateFunctionCode, UpdateFunctionConfiguration, PassRole permissions
4. Enable Lambda function versioning and use aliases
5. Enable Bedrock model invocation logging
6. Implement Service Control Policies for instance types and model invocations
7. Monitor IAM Access Analyzer enumeration
8. Deploy runtime detection with behavioral analytics

This specific incident is one of many highlighting the severity of cloud misconfigurations.
