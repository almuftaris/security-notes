# The Resolv Hack — $23M DeFi Exploit via Compromised Signing Key (March 2026)

**Source:** [Chainalysis – The Resolv Hack: How One Compromised Key Printed $23 Million](https://www.chainalysis.com/blog/lessons-from-the-resolv-hack/)  
**Date:** March 22, 2026

---

## Overview

Resolv, a DeFi protocol built around a dollar-pegged stablecoin called USR, lost approximately $23 million in a matter of minutes. The smart contract code was never exploited — it worked exactly as designed. The attacker compromised the off-chain signing key that the protocol trusted to authorize token minting, then used it to print $80 million worth of unbacked stablecoins and cash out before anyone could respond.

---

## Background — How USR Minting Worked

Minting USR wasn't fully on-chain. It required two steps:

1. A user deposits USDC and submits a minting request
2. An off-chain service, controlled by a privileged signing key (`SERVICE_ROLE`), reviews the request and calls back to the contract to finalize how much USR to mint

The contract checked that a valid signature existed — but had no on-chain cap on how much could be minted. No maximum mint ratio, no price oracle, no collateral check. Whatever the key holder signed, the contract would execute.

---

## The Attack

**Step 1 — Key Compromise**  
The attacker gained access to Resolv's AWS Key Management Service (KMS) environment, where the protocol's privileged signing key was stored. With that key, they could authorize any minting operation they chose.

**Step 2 — Minting $80M in Unbacked USR**  
Using a deposit of roughly $100K–$200K in USDC, the attacker called `completeSwap` twice with the stolen key, authorizing wildly inflated outputs:
- 50 million USR in the first transaction
- 30 million USR in the second

Total minted: **80 million USR** (~$25M), backed by almost nothing.

**Step 3 — Obscuring the Position**  
Rather than dumping USR directly, the attacker converted it into `wstUSR` (a staked derivative), reducing immediate market impact and making the position harder to trace.

**Step 4 — Cashing Out**  
From `wstUSR`, the attacker rotated through DEX pools and bridges, converting into stablecoins and then ETH. At the time of reporting, the attacker held approximately 11,400 ETH (~$24M).

**Market Impact**  
The flood of unbacked USR into liquidity pools caused the token to de-peg hard — dropping ~80% to $0.20 before a partial recovery to $0.56. Resolv suspended all protocol operations following the attack.

---

## Root Cause

This was not a smart contract vulnerability. The protocol had undergone 18 security audits. The failure was in **off-chain infrastructure trust** — the contract was designed to fully trust whatever the signing key authorized, with no on-chain safeguard to catch an impossible minting ratio. Once the key was compromised, there was nothing left to stop the attack.

---

## Key Takeaways

- **A smart contract that passes every audit can still be exploited through its off-chain dependencies.** DeFi protocols increasingly rely on external services, cloud infrastructure, and privileged keys. Each of those is an attack surface that traditional on-chain audits don't cover.

- **Unlimited trust in a single signing key is a single point of failure.** The contract's design effectively said: "if the key approves it, we'll do it." There was no secondary check, no cap, no sanity validation on-chain. One compromised key was all it took.

- **Speed is the defining factor in DeFi exploits.** The entire attack — from minting to cash-out — happened in minutes. By the time the protocol team could respond, the damage was done. Reactive security doesn't work at blockchain speed.

- **Cloud infrastructure security is now a DeFi security problem.** The attacker didn't break cryptography or find a logic flaw in Solidity — they compromised an AWS KMS environment. Web2 attack vectors are increasingly the path of least resistance into Web3 protocols.

- **Real-time on-chain monitoring is no longer optional.** A monitoring rule flagging any `completeSwap` call where minted output is wildly disproportionate to collateral input would have caught both transactions instantly. The tools exist — the question is whether protocols treat automated response as a core security requirement rather than a nice-to-have.
