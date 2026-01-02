```mermaid
flowchart TD
    A[Suspicious Login Alert]

    A --> B{Login Location}

    %% On-Prem Path
    B -->|On Prem AD| OP1[Identify Source Device]
    OP1 --> OP2{Expected System or User}
    OP2 -->|Yes| OP3[Review Login Pattern]
    OP3 --> OP4{Anomalous Behavior}
    OP4 -->|No| OP5[Document and Close]
    OP4 -->|Yes| OP6[Investigate Further]

    OP2 -->|No| OP6

    %% Cloud Path
    B -->|Cloud Entra ID| CL1[Identify User and App]
    CL1 --> CL2{Known User and App}
    CL2 -->|Yes| CL3[Review Sign In Risk]
    CL3 --> CL4{Risk Acceptable}
    CL4 -->|Yes| CL5[Document and Close]
    CL4 -->|No| CL6[Investigate Further]

    CL2 -->|No| CL6

    %% Shared Investigation
    OP6 --> X[Check Additional Indicators]
    CL6 --> X

    X --> Y{Credential Abuse Observed}
    Y -->|No| Z[Monitor and Document]
    Y -->|Yes| IR[Escalate Incident]
