# Application / Data Compromise Playbook

## Purpose
Determine whether application or database alerts
represent unauthorized access or data impact.

## When This Playbook Is Used
- WAF alerts
- Application authentication abuse
- API misuse
- Database access anomalies

## Analysis Flow
```mermaid
flowchart TD
    A[Application or Data Playbook Invoked]
        --> B[Review Alert and App Context]

    B --> C[Identify Application or Database]

    C --> D{Authentication or Access Abuse?}

    %% AUTH ABUSE
    D -- Yes --> E[Review Auth Logs Tokens Keys]
    E --> F{Unauthorized Access Confirmed?}

    F -- No --> G[Continue Analysis]
    F -- Yes --> H[Credential or Token Compromise Suspected]

    %% NON-AUTH PATH
    D -- No --> I{Exploit or Injection Indicators?}

    I -- No --> G
    I -- Yes --> J[Review WAF App Logs DB Queries]

    %% DATA IMPACT
    G --> K{Data Access or Modification Observed?}

    K -- No --> L[No Data Impact Identified]
    L --> M[Return No Residual Risk]

    K -- Yes --> N[Assess Scope of Data Impact]

    %% SCOPE
    N --> O{Sensitive or Regulated Data?}

    O -- No --> P[Limited Business Impact]
    O -- Yes --> Q[High Business Impact]

    %% RETURN TO BASELINE
    P --> R[Return Residual Risk Present]
    Q --> R
    H --> R

```
---
*Author: Michael Baker*  
*Maintained as part of the SecOps Playbooks repository*
(Last updated: 12/2025)