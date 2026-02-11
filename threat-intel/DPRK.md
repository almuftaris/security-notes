## DPRK IT Workers: Insider Threat Campaign (UNC5267)

**Date**: February 3, 2026
**Source**: [Mandiant M-Trends 2025](https://cloud.google.com/security/resources/m-trends)
**Corroboration**: CrowdStrike, Palo Alto Unit 42, FBI

### Campaign Overview
Since 2022, North Korea has placed thousands of its citizens in Western companies as remote IT contractors to evade international sanctions and generate revenue for the regime, including weapons programs. Mandiant tracks this as UNC5267. Workers funnel salaries directly back to the DPRK, with one facilitator alone helping generate $6.8M+ across 300+ US companies using 60+ stolen identities.

### Pre-Hiring Tactics
- **Identity Fraud**: Use stolen or wholly fabricated US identities with extensive supporting documentation
- **Resume Recycling**: Borrow heavily from public resumes; reuse uncommon phrases across multiple personas
- **Portfolio Sites**: Create templatized GitHub/portfolio sites often left with default sections unchanged
- **Multiple Applications**: Single operative manages 12+ personas simultaneously applying to different companies
- **Pattern Indicators**: Email addresses/domains frequently contain words like "panda," "dev," "star," "silver," "sun"
- **Reference Scams**: Provide references to other DPRK-controlled false personas
- **Educational Mismatch**: Claim local US address but foreign university degrees that are difficult to verify

### Post-Hiring Operations
- **VPN Masking**: 72% use Astrill VPN to hide true location (primarily Russia/China)
- **Laptop Farms**: US-based facilitators receive corporate laptops, install remote access tools, and host devices so connections appear domestic
- **Physical Facilitators**: In-country accomplices cash paychecks, receive mail, even pass drug tests on behalf of operatives
- **Legitimate Behavior**: Perform actual development work - activity is indistinguishable from legitimate employees, making detection extremely difficult

### Risk Profile
While direct malicious activity is rare (observed in <5 cases in 2024), the access granted creates risk for:
- Data theft and intellectual property espionage
- Infrastructure mapping for future attacks
- Extortion and ransomware deployment
- Supply chain compromise through code injection

### Detection Challenges
- Activity blends seamlessly with legitimate remote work
- Use of common developer tools and VPNs
- Actual job performance meets or exceeds expectations
- No traditional "malicious" indicators to alert on
- Remote work norms make geographic anomalies less suspicious

### Key Indicators
- Resume with local US address but only international education
- Educational background verification doesn't match resume details
- Multiple candidates from same recruiter with similar patterns
- Portfolio sites using common themes/layouts with default sections
- VPN connections (especially Astrill) from same system
- Shipping address discrepancies or reused addresses across candidates

### My Takeaways
- This is a **massive** insider threat campaign hiding within remote work culture
- The sophistication lies in operational security and social engineering, not malware
- Revenue motive is sanctions evasion, but access creates national security risk
- Need better identity verification processes during hiring - current methods clearly insufficient

