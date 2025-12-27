# Playbook Routing Logic

## Purpose
Define a consistent, evidence-based method for routing security alerts
from the **Baseline SOC Investigation Flow** to the appropriate **analysis playbook**.

Routing ensures:
- Alerts are analyzed by the correct playbook
- Analysts follow consistent decision logic
- Playbooks are invoked based on evidence, not alert names
- The baseline remains threat-agnostic

## What Routing Does
- Evaluates **primary evidence** from an alert
- Identifies the dominant **impact area**
- Directs analysis to the appropriate playbook

## What Routing Does NOT Do
- Perform deep analysis
- Decide response actions
- Declare incidents
- Close alerts

Routing only answers:
> **“Which playbook should analyze this?”**

Routing only answers:
*“Which playbook should analyze this?”**

## Routing Flow
```mermaid
flowchart TD
    A[Baseline Investigation]
        --> B[Review Alert Evidence]

    B --> C{Primary Evidence Type?}

    %% EMAIL / IDENTITY
    C -- Email or Identity --> D{Phishing or Identity Abuse Indicators?}
    D -- Yes --> E[Route to Phishing Playbook]
    D -- No --> F[Remain in Baseline Analysis]

    %% ENDPOINT
    C -- Endpoint --> G{Malware or Encryption Indicators?}
    G -- Yes --> H[Route to Malware or Ransomware Playbook]
    G -- No --> F

    %% NETWORK
    C -- Network --> I{Automated or Regular External Communication?}
    I -- Yes --> J[Route to C2 Analysis Playbook]
    I -- No --> F

    %% APPLICATION / DATA
    C -- Application or Data --> K{Application or Data Compromise Indicators?}
    K -- Yes --> L[Route to Application or Data Compromise Playbook]
    K -- No --> F

    %% DEFAULT
    F --> M[Continue Baseline Investigation]

```
---
*Author: Michael Baker*  
*Maintained as part of the SecOps Playbooks repository*
(Last updated: 12/2025)
