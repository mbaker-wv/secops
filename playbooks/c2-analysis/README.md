# 🧨 Ransomware / Malware Playbook (`playbooks/ransomware-malware/README.md`)

# Ransomware / Malware Playbook

## Purpose
Distinguish ransomware, pre-encryption activity,
and non-ransomware malware, and assess residual risk.

## When This Playbook Is Used
- Malware alerts
- Ransomware detections
- Suspicious endpoint activity

## Analysis Flow
```mermaid
flowchart TD
    A[Start C2 Alert or Threat Hunt]
        --> B[Identify Initiating Process\nKQL 1]

    B --> C{Process Category?}

    C -- Browser or User --> D[Human Driven Activity]
    C -- Script Service LOLBin or Unknown --> E[Machine Driven Activity]

    %% HUMAN PATH
    D --> F{Connection Volume?\nKQL 2}
    F -- Weak --> G[Low Confidence Signal]
    F -- Strong --> H[Investigate Timing\nKQL 3]

    %% MACHINE PATH
    E --> I{Connection Volume?\nKQL 2}
    I -- Weak --> H
    I -- Strong --> H

    %% TIMING ANALYSIS
    H --> J{Regular Timing or Low Variance?\nKQL 3}

    J -- No --> K[No Beaconing Detected]
    J -- Yes --> L[Beaconing Detected]

    %% SCOPE CHECK
    L --> N{Multiple Devices?\nKQL 4}

    %% RETURN RESULTS
    G --> R[Return No Residual Risk]
    K --> R

    N -- No --> S[Return Residual Risk Present]
    N -- Yes --> S

```
---
*Author: Michael Baker*  
*Maintained as part of the SecOps Playbooks repository*
(Last updated: 12/2025)