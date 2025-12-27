# Identity Anomalous Sign-In Playbook

## Purpose
Determine whether high-risk or anomalous sign-ins represent
benign user behavior or credential/session compromise.

## When This Playbook Is Used
- High-risk sign-in alerts
- Impossible or unrealistic travel alerts
- Unfamiliar sign-in properties
- Identity protection alerts

## Analysis Flow
```mermaid
flowchart TD
    A[Identity Anomalous Sign-In Playbook Invoked]
        --> B[Review Sign-In Alert and User Context]

    B --> C[Collect Sign-In Details\nIP Location Device App MFA Status]

    C --> D{Impossible Travel or Risky Location?}

    %% NOT ANOMALOUS
    D -- No --> E[Likely Normal Sign-In Pattern]
    E --> Z[Return No Residual Risk]

    %% ANOMALOUS
    D -- Yes --> F{Known Benign Explanation?}

    %% BENIGN CONDITIONS
    F -- VPN Mobile Carrier Travel --> G[Validate User Activity History]
    G --> H{Consistent with Past Behavior?}

    H -- Yes --> I[Benign Identity Behavior Confirmed]
    I --> Z

    %% NOT BENIGN
    H -- No --> J[Proceed with Risk Analysis]
    F -- No --> J

    %% AUTH STRENGTH
    J --> K{MFA Satisfied?}

    %% MFA PASSED
    K -- Yes --> L[Check Session Behavior]
    L --> M{Session Abuse Indicators?}

    M -- No --> N[Strong Auth with No Abuse]
    N --> Z

    %% MFA FAILED OR ABUSED
    K -- No --> O[MFA Bypass or Failure]
    O --> P[Credential or Session Compromise Suspected]

    M -- Yes --> P

    %% POST-COMPROMISE CHECKS
    P --> Q[Review Account Activity\nMailbox Rules Token Usage App Access]

    Q --> R{Suspicious Post Sign-In Actions?}

    %% NO FOLLOW-ON
    R -- No --> S[Contain Identity Risk\nReset Credentials Revoke Sessions]
    S --> Y[Return Residual Risk Present]

    %% CONFIRMED ABUSE
    R -- Yes --> T[Identity Compromise Confirmed]
    T --> Y

```
---
*Author: Michael Baker*  
*Maintained as part of the SecOps Playbooks repository*
(Last updated: 12/2025)
