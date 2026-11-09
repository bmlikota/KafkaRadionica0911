# Zadatak: Fraud Detection sustav

```mermaid
flowchart LR

    L[LoginService]
    S[SecurityService]
    P[PaymentService]

    subgraph Kafka Topic: logins
        L0["Partition 0
            events: e0,e1,e2,e3"]
    end

    subgraph Kafka Topic: security-events
        S0["Partition 0
            events: e0,e1,e2"]
    end

    subgraph Kafka Topic: payments
        P0["Partition 0
            events: e0,e1,e2,e3,e4"]
    end

    L -->|"UserLoggedIn(e4)"| L0
    S -->|"PasswordChanged(e3)"| S0
    P -->|"PaymentRequested(e5)"| P0

    subgraph Fraud Detection Group
        FD[FraudDetectionConsumer]
        LOff["Logins Committed Offsets</br>P0=3"]
        SOff["Security Committed Offsets</br>P0=2"]
        POff["Payment Committed Offsets</br>P0=4"]
    end

    L0 -->|"e3"| FD
    S0 -->|"e2"| FD
    P0 -->|"e4"| FD

    FD --> LOff
    FD --> SOff
    FD --> POff
```

## Povećanje broja resursa

Prvi topic - 2 particije

Drugi topic - 3 particije

Treci topic - 2 particije

3 Consumera


```mermaid
flowchart LR

    L[LoginService]
    S[SecurityService]
    P[PaymentService]

    subgraph T1["Kafka Topic: logins"]
        L0["Partition 0<br/>events: e0,e1,e2,e3"]
        L1["Partition 1<br/>events: e0,e1,e2"]
    end

    subgraph T2["Kafka Topic: security-events"]
        S0["Partition 0<br/>events: e0,e1,e2"]
        S1["Partition 1<br/>events: e0,e1,e2,e3"]
        S2["Partition 2<br/>events: e0,e1"]
    end

    subgraph T3["Kafka Topic: payments"]
        P0["Partition 0<br/>events: e0,e1,e2,e3,e4"]
        P1["Partition 1<br/>events: e0,e1,e2"]
    end

    L -->|"UserLoggedIn(e4)"| L0
    L -->|"UserLoggedIn(e3)"| L1

    S -->|"PasswordChanged(e3)"| S0
    S -->|"PasswordChanged(e4)"| S1
    S -->|"PasswordChanged(e2)"| S2

    P -->|"PaymentRequested(e5)"| P0
    P -->|"PaymentRequested(e3)"| P1

    subgraph FDG["Fraud Detection Group"]
        C1["FraudDetectionConsumer1"]
        C2["FraudDetectionConsumer2"]
        C3["FraudDetectionConsumer3"]

        LOff["Logins Committed Offsets
            P0=3
            P1=2"]
        SOff["Security Committed Offsets-events
            P0=2
            P1=3
            P2=1"]
        POff["Payment Committed Offsets
            P0=4
            P1=2"]
    end

    L0 -->|"e3"| C1
    L1 -->|"e2"| C2

    S0 -->|"e2"| C1
    S1 -->|"e3"| C2
    S2 -->|"e1"| C3

    P0 -->|"e4"| C3
    P1 -->|"e2"| C1

    C1 --> LOff
    C1 --> SOff
    C1 --> POff
    C2 --> LOff
    C2 --> SOff
    C3 --> SOff
    C3 --> POff
```
