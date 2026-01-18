# Concurrent services.exe Playbook
## Purpose
Analyze alerts indicating concurrent execution of services.exe to determine whether the behavior represents:
* Legitimate restart or maintenance activity, or
* System instability, corruption, or potential malicious abuse of a core Windows process

## When This Playbook Is Used
* Defender XDR alert: Concurrent services.exe execution detected
* Advanced hunting results showing multiple services.exe instances
* Suspicious system integrity or persistence investigations

## Analysis Flow
```mermaid
flowchart TD
    A[Concurrent services.exe Alert Triggered]
        --> B[Collect Alert Details\nProcess Count, Paths, Parents, Users, Timing]

    B --> C{Execution Path = System32?}

    %% PATH VALIDATION
    C -- No --> X1[ESCALATE\nNon-System32 Path Detected]
    C -- Yes --> D{Parent Process = wininit.exe?}

    %% PARENT VALIDATION
    D -- No --> X2[ESCALATE\nUnexpected Parent Process]
    D -- Yes --> E{User Context = SYSTEM?}

    %% USER CONTEXT VALIDATION
    E -- No --> X3[ESCALATE\nNon-SYSTEM Execution Context]
    E -- Yes --> F{Start Times Within ≤ 5 Minutes?}

    %% TIMING CHECK
    F -- No --> G[Likely Sequential Restart\nProceed to Context Review]
    F -- Yes --> H[Concurrent Execution Confirmed]

    %% CONTEXT CHECKS
    G --> I{Recent Reboot, Patch, or Maintenance Activity?}
    I -- Yes --> J[Confirm No Overlapping Execution\nClose as Benign]
    I -- No --> K[Investigate Further\nUnexpected Restart Pattern]

    %% CRASH / PATCH CONTEXT
    H --> L{Evidence of Service Crash or Patch Install?}
    L -- Yes --> M[Document Context\nEscalate if Concurrency Persists]
    L -- No --> N[High-Risk Condition\nEscalate Immediately]

    %% FINAL ESCALATIONS
    M --> X4[ESCALATE\nConcurrent SCM with Operational Context]
    N --> X5[ESCALATE\nPossible Injection or Persistence]

    %% BENIGN RETURN
    J --> R[Return No Residual Risk]
```
## What Analysts Must Validate

### Expected Legitimate State

* Path: C:\Windows\System32\services.exe
* Parent process: wininit.exe
* User: NT AUTHORITY\SYSTEM
* One active Services.exe instance at a time.

### Indicators Requiring Escalation
* Non-System32 execution path
* Unexpected parent process
* Non-SYSTEM execution context
* Overlapping services.exe instances found
* Repeated concurrent execution after patching or reboot

### Common Benign Scenarios
* Sequential service restarts


### High-Risk Scenarios
* Concurrent Services.exe's persisting beyond patch window
* Concurrent Services.exe's combined with suspicious child processes
* Concurrent Services.exe's on high-value servers (DCs, app servers)

## KQL Advanced Hunting Query
```kql
DeviceProcessEvents
| where DeviceName == "<DeviceName>"
| where FileName == "services.exe"
| project
    Timestamp,
    ProcessId,
    ProcessCreationTime,
    FolderPath,
    InitiatingProcessFileName,
    AccountName
| order by ProcessCreationTime asc
```

---
*Author: Michael Baker*  
*Maintained as part of the SecOps Playbooks repository*
(Last updated: 01/2026)
