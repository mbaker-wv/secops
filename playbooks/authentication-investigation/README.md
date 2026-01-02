
# Suspicious Login Attempts Playbook  
**Cloud (Entra ID) and On-Prem Active Directory**


## Purpose

This playbook provides a **clear, repeatable investigation workflow** for suspicious login attempts observed in cloud and on-prem environments.

The goal is to:
- Reduce alert fatigue
- Enable fast, defensible decisions
- Escalate only when risk is real
- Treat authentication attempts probabilistically, not emotionally

This playbook is designed to be **checklist-driven** and usable by Tier-1 and Tier-2 SOC analysts.


## Alerts Covered

This playbook applies to alerts related to:

- Suspicious sign-in attempts
- Unusual authentication patterns
- Risky sign-ins (Entra ID)
- Excessive login failures
- Impossible travel
- Legacy authentication usage
- On-prem suspicious logon attempts


## Investigation Philosophy

> **Authentication attempts are common. Compromise is not.**

- Failed attempts alone do not indicate compromise
- Security controls blocking access is a success condition
- Privilege level drives severity
- Successful authentication always requires scrutiny


## Combined Suspicious Login Flow (Cloud + On-Prem)

```mermaid
flowchart TD
    A[Alert Received]

    A --> B{Platform}

    B -->|Cloud Entra ID| C[Review Sign-In Result]
    B -->|On-Prem AD| C

    C --> D{Authentication Successful}

    %% Failed Only
    D -->|No| E[Check Account Privilege]

    E -->|High Privilege| ESC[Escalate Incident]

    E -->|Not Privileged| F[Check Attempt Frequency]

    F -->|Low Frequency| CLOSE[Close and Monitor]
    F -->|High Frequency| ESC

    %% Successful Authentication
    D -->|Yes| G[Check MFA or Context]

    G -->|Expected Context or MFA Used| ESC
    G -->|No MFA or Unexpected Context| H[Contain Account]

    H --> I[Force Password Reset]
    I --> J[Revoke Active Sessions]
    J --> ESC