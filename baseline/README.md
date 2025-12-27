# Baseline SOC Investigation Flow

## Purpose
Provide a consistent, repeatable investigation framework for **all security alerts**.
The baseline determines **what happens next**, while individual playbooks perform
deep analysis for specific threat types.

This flow ensures:
- Consistent analyst behavior
- Clear separation of analysis vs response
- Audit-defensible decisions
- Scalable playbook integration

## What the Baseline Does
- Receives all security alerts
- Performs initial triage and validation
- Identifies the primary impact area
- Routes analysis to the correct playbook
- Makes the **final response decision**

## What the Baseline Does NOT Do
- Deep technical analysis
- Threat-specific investigation
- IOC tuning or blocking decisions
- Declare incidents prematurely

Those actions occur **inside playbooks**.

## Baseline Investigation Flow
Playbooks identify risk. The baseline decides response.

```mermaid
flowchart TD
    A[Security Alert Received]
        --> B[Baseline Investigation]

    B --> C[Identify Primary Impact Area]

    C --> D[Email or Identity]
    C --> E[Endpoint]
    C --> F[Network]
    C --> G[Application or Data]

    D --> H[Invoke Phishing Playbook]
    E --> I[Invoke Malware or Ransomware Playbook]
    F --> J[Invoke C2 Analysis Playbook]
    G --> K[Invoke Application or Data Compromise Playbook]

    H --> L[Playbook Result]
    I --> L
    J --> L
    K --> L

    L --> M{Residual Risk Present?}

    M -- No --> N[Close Investigation]

    M -- Yes --> O{Meets Incident Response Reporting Criteria?}

    O -- No --> P[Targeted Threat Hunt]
    O -- Yes --> Q[Activate Incident Response]

```
---
*Author: Michael Baker*  
*Maintained as part of the SecOps Playbooks repository*
(Last updated: 12/2025)
