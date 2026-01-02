# Reconnaissance & Enumeration Alert Playbook  
**Microsoft Defender – SAMR, Account Enumeration, Identity Discovery**

## Purpose

This playbook defines a **repeatable investigation and process-of-elimination workflow** for Microsoft Defender alerts related to **identity, account, and directory reconnaissance** activity.

These alerts commonly indicate **early-stage discovery behavior**, but in enterprise environments they are frequently triggered by **legitimate systems and administrative activity**.

This playbook ensures:
- Consistent SOC investigations
- Reduced false-positive escalations
- Clear audit and management justification
- Escalation only when follow-on malicious behavior is observed

## Alerts Covered

This playbook applies to the following Microsoft Defender alert types (and similar):

- User and group membership reconnaissance (SAMR)
- Account enumeration detected
- Sensitive group discovery
- Directory service enumeration
- Suspicious RPC / LDAP / SMB discovery
- NTLM or legacy authentication reconnaissance
- Identity discovery without exploitation
- Entra ID / Azure AD directory enumeration alerts


## Investigation Philosophy

> **Reconnaissance without exploitation is not an incident.**

Discovery activity alone does not indicate compromise.  
Escalation occurs **only when reconnaissance is paired with additional malicious indicators**.


## Investigation Flow

```mermaid
flowchart TD
    A[Defender SAMR Recon Alert]

    A --> B{Source Type}

    %% Domain Controller
    B -->|Domain Controller| DC1[Expected DC Behavior]
    DC1 --> DC2[Document and Close]

    %% Server Path
    B -->|Server| S1[Identify Server Role]
    S1 --> S2{Known AD Integrated Software}
    S2 -->|Yes| S3[Service Account or SYSTEM]
    S3 -->|Yes| S4[Expected Behavior]
    S4 --> S5[Document and Close]

    S3 -->|No| S6[Review Account Permissions]
    S6 --> S7{Permissions Justified}
    S7 -->|Yes| S5
    S7 -->|No| S8[Escalate Incident]

    S2 -->|No| S9[Investigate Installed Software]
    S9 --> S10{Business Justification Found}
    S10 -->|Yes| S5
    S10 -->|No| S8

    %% Workstation Path
    B -->|Workstation| W1[Identify User Role]
    W1 --> W2{IT Admin}
    W2 -->|Yes| W3[Recent Admin Activity]
    W3 -->|Yes| W4[Document and Close]
    W3 -->|No| W5[Escalate Incident]

    W2 -->|No| W6[Check for Additional Indicators]
    W6 --> W7{Suspicious Activity}
    W7 -->|No| W4
    W7 -->|Yes| W5


```
---
*Author: Michael Baker*  
*Maintained as part of the SecOps Playbooks repository*
(Last updated: 1/2026)
