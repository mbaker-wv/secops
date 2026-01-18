# Suspicious Login Attempts Playbook  
**Microsoft Defender – Cloud (Entra ID) & On-Prem Authentication**

## Purpose

This playbook defines a **repeatable, risk-based investigation process** for suspicious login alerts affecting:

- Cloud identities (Microsoft Entra ID)
- On-prem Active Directory user accounts
- On-prem service accounts

Authentication attempts are evaluated using **frequency, context, controls, and privilege level**, not alert presence alone.


## Alerts Covered

This playbook applies to:

- Suspicious sign-in activity
- Unfamiliar Sign-In Properties
- Unusual authentication patterns
- Risky sign-ins (Entra ID)
- Excessive login failures
- Impossible/Atypical Travel
- Legacy authentication usage
- On-prem suspicious logon attempts


## Investigation Philosophy

> **Authentication attempts are common. Compromise is not.**

- Failed attempts ≠ incident  
- Control enforcement (geo-block, CA) = **success**, not failure  
- Escalation requires **signal**, not just noise  


## Investigation Flow – Cloud and On-Prem

### Unfamiliar Sign-in Attempts Flow

```mermaid
flowchart TD
    A[Alert Received]

    A --> B{Platform}

    %% =====================
    %% CLOUD OR ON-PREM
    %% =====================
    B -->|Cloud Entra ID| C[Review Sign-In Result]
    B -->|On-Prem AD| C

    C --> D{Authentication<br> Successful?}

    %% =====================
    %% FAILED ONLY
    %% =====================
    D -->|No| E[Check Account Privilege]

    E -->|High Privilege| ESC[Escalate Incident]

    E -->|Not Privileged| F[Check Attempt Frequency]

    F -->|Low Frequency| CLOSE[Close and Monitor]
    F -->|High Frequency| ESC

    %% =====================
    %% SUCCESSFUL AUTH
    %% =====================
    D -->|Yes| G[MFA Outcome]

    G -->|Failed| I
    G -->|Passed| H[Isolate Account]

    H --> I[Force Password Reset]
    I --> J[Revoke Active Sessions]
    J --> ESC

```
---
*Author: Michael Baker*  
*Maintained as part of the SecOps Playbooks repository*
(Last updated: 1/2026)