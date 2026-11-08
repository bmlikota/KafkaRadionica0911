# Zadatak: Streaming logova iz mikroservisa

```mermaid
flowchart LR

    S1[UserService]
    S2[PaymentService]
    S3[InventoryService]

    subgraph Kafka Topic: logs
        P0["Partition 0
            events: e0,e1,e2"]
        P1["Partition 1
            events: e0,e1,e2,e3,e4,e5"]
        P2["Partition 2
            events: e0,e1,e2,e3"]
    end

    S1 -->|"UEvent(e3)"| P0
    S1 -->|"UEvent(e6)"| P1
    S1 -->|"UEvent(e4)"| P2

    S2 -->|"PEvent(e4)"| P0
    S2 -->|"PEvent(e7)"| P1
    S2 -->|"PEvent(e5)"| P2

    S3 -->|"IEvent(e5)"| P0
    S3 -->|"IEvent(e8)"| P1
    S3 -->|"IEvent(e6)"| P2

    subgraph Log Processing Consumer Group
        C1[LogProcessor]
        LogOff["Committed Offsets<br/>P0=1<br/>P1=3<br/>P2=3"]
    end

    P0 -->|"e1"| C1
    P1 -->|"e3"| C1
    P2 -->|"e3"| C1
    C1 --> LogOff
```

## Povećanje broja pristiglih logova

## Slučaj kad jedan consumer padne

```mermaid
flowchart LR

    S1[User Service]
    S2[Payment Service]
    S3[Inventory Service]

    subgraph Kafka Topic: logs
        P0["Partition 0
            events: e0,e1,e2"]
        P1["Partition 1
            events: e0,e1,e2,e3,e4,e5"]
        P2["Partition 2
            events: e0,e1,e2,e3"]
    end

    S1 -->|"UEvent(e3)"| P0
    S1 -->|"UEvent(e6)"| P1
    S1 -->|"UEvent(e4)"| P2

    S2 -->|"PEvent(e4)"| P0
    S2 -->|"PEvent(e7)"| P1
    S2 -->|"PEvent(e5)"| P2

    S3 -->|"IEvent(e5)"| P0
    S3 -->|"IEvent(e8)"| P1
    S3 -->|"IEvent(e6)"| P2

    subgraph Log Processing Consumer Group
        C1[Log Processor1]
        C2[Log Processor2]
        LogOff["Committed Offsets<br/>P0=1<br/>P1=3<br/>P2=3"]
    end

    P0 -->|"e1"| C1
    P1 -->|"e3"| C2
    P2 -->|"e3"| C2
    C1 --> LogOff
    C2 --> LogOff
```
