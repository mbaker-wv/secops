# Phishing Playbook

## Purpose
Analyze phishing events to determine whether user interaction,
credential compromise, or lateral phishing occurred.

## When This Playbook Is Used
- Phishing alerts
- User-reported emails
- Identity alerts tied to email activity

## Analysis Flow
```mermaid
flowchart TD
    A[Phishing Analysis Invoked]
        --> B[Confirm Malicious Phish\nIOC or Detection]

    B --> C{Confirmed Malicious?}

    %% NOT MALICIOUS
    C -- No --> D[Handle as Suspicious Email]
    D --> R[Return No Residual Risk]

    %% SOURCE TRUST DECISION
    C -- Yes --> E{Source Domain Trusted or Business Related?}

    %% EXTERNAL MALICIOUS SOURCE
    E -- No --> F[Add Malicious Domains or IPs\nTo Endpoint IOC Block List]
    F --> G[Search and Destroy Phish\nTenant-wide Soft Delete]

    %% TRUSTED PARTNER SOURCE
    E -- Yes --> H[Do Not Block Domain Automatically]
    H --> G

    %% USER INTERACTION CHECK
    G --> I[Check for User Interaction\nKQL URL Clicks or Attachments]

    I --> J{Interaction Evidence Found?}

    %% NO INTERACTION
    J -- No --> K[Confirm No Further Exposure]
    K --> R

    %% INTERACTION CONFIRMED
    J -- Yes --> M[Determine Interaction Type]

    %% LINK CLICK PATH
    M --> N[Link Clicked]
    N --> O{Credentials Submitted?}

    %% ATTACHMENT PATH
    M --> P[Attachment Opened]
    P --> Q[Check Endpoint Activity]
    Q --> S[Credential Compromise Suspected]

    %% NO CREDENTIALS
    O -- No --> R

    %% CREDENTIAL COMPROMISE
    O -- Yes --> S
    S --> T[Reset Credentials\nRevoke Sessions]

    %% POST-COMPROMISE CHECKS
    T --> U[Check Mailbox Rules and Forwarding]
    U --> V{Mailbox Rules Found?}

    %% MAILBOX RULE HANDLING
    V -- Yes --> W[Disable Rules and Forwarding]
    V -- No --> X[Check Outbound Email Activity]

    W --> X

    %% OUTBOUND PHISHING CHECK
    X --> Y{Suspicious Emails Sent?}

    %% LATERAL PHISHING LOOP
    Y -- Yes --> Z[Identify Recipients]
    Z --> AA[Invoke Phishing Playbook for Each Recipient]

    %% PARTNER COORDINATION
    E -- Yes --> AB[Coordinate with Partner Security\nNotify Other Modes\nAssess Business Impact]

    %% FINAL RETURNS
    Y -- No --> R
    AA --> R2[Return Residual Risk Present]

```
---
*Author: Michael Baker*  
*Maintained as part of the SecOps Playbooks repository*
(Last updated: 12/2025)


