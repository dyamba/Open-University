# Open University - Entegrasyon Şeması

Bu doküman sistemler arası entegrasyonların görsel şemasını içerir.

## Genel Sistem Entegrasyonu

```mermaid
flowchart TB
    subgraph Clients["🖥️ Client Layer"]
        WEB[Web Portal]
        MOB[Mobile App]
        ADM[Admin Panel]
    end

    subgraph Gateway["🚪 API Gateway"]
        GW[Servis Hub]
    end

    subgraph Core["⚙️ Core Services"]
        SSO[SSO Service]
        LOG[LOG Hub]
        EMAIL[Email Hub]
        ENT[Entegrator Hub]
    end

    subgraph Academic["📚 Academic Domain"]
        AKD[Akademik Sistem]
        LMS[LMS]
        HAZ[Hazırlık Yönetimi]
        SNV[Sınav Sistemi]
        TEZ[Tez Yönetimi]
    end

    subgraph HR["👥 HR Domain"]
        PER[Personel Sistemi]
        PDKS[PDKS]
        PDRS[PDRS]
    end

    subgraph Student["🎓 Student Services"]
        BAS[Başvuru Hub]
        MEZ[Mezun Sistemi]
        YRT[Yurt Yönetimi]
        BRS[Burs Sistemi]
        KRY[Kariyer Merkezi]
    end

    subgraph Quality["📊 Quality & Strategy"]
        KLT[Kalite Sistemi]
        STR[Stratejik Yönetim]
        AKR[Akreditasyon]
    end

    subgraph Research["🔬 Research"]
        PRJ[Proje Yönetimi]
        ETK[Etik Kurul]
        LAB[Lab Yönetimi]
        KLN[Klinik Araştırma]
    end

    subgraph Content["🌐 Content"]
        CMS[CMS]
        ILT[İletişim Sistemi]
    end

    subgraph Integration["🔗 External Integration"]
        EDV[E-Devlet]
        EBYS[EBYS]
        EWP[EWP Hub]
    end

    subgraph Finance["💰 Finance"]
        POS[Sanal POS]
        ENV[Envanter]
        SAT[Satın Alma]
    end

    subgraph Data["💾 Data Layer"]
        PG[(PostgreSQL)]
        RD[(Redis)]
        MQ[RabbitMQ]
        S3[(MinIO/S3)]
    end

    %% Client connections
    WEB --> GW
    MOB --> GW
    ADM --> GW

    %% Gateway to Core
    GW --> SSO
    GW --> LOG
    GW --> EMAIL
    GW --> ENT

    %% Core to All Services
    SSO -.-> AKD
    SSO -.-> PER
    SSO -.-> LMS
    SSO -.-> CMS
    SSO -.-> BAS
    
    %% Academic Domain
    AKD <--> LMS
    AKD <--> HAZ
    AKD <--> SNV
    AKD <--> TEZ
    AKD --> MEZ

    %% HR Domain
    PER <--> PDKS
    PER <--> PDRS

    %% Cross-domain connections
    AKD <--> PER
    LMS <--> PER
    BAS --> AKD
    BAS --> PER

    %% Quality connections
    KLT --> AKD
    KLT --> PER
    KLT --> PRJ
    STR --> KLT
    AKR --> KLT

    %% Research connections
    PRJ --> PER
    PRJ --> ETK
    PRJ --> LAB
    KLN --> ETK
    KLN --> LAB

    %% Content connections
    CMS <--> ILT
    ILT --> EMAIL

    %% Integration connections
    EDV --> AKD
    EDV --> PER
    EBYS --> ENT
    EWP --> AKD

    %% Finance connections
    POS --> AKD
    POS --> BRS
    ENV --> SAT

    %% Event Bus connections
    AKD --> MQ
    PER --> MQ
    LMS --> MQ
    MQ --> EMAIL
    MQ --> LOG

    %% Data Layer
    AKD --> PG
    PER --> PG
    LMS --> PG
    SSO --> RD
    CMS --> S3
```

## Core Services Detay

```mermaid
flowchart LR
    subgraph SSO["🔐 SSO Service"]
        AUTH[Authentication]
        AUTHZ[Authorization]
        USER[User Management]
        MFA[MFA]
        SESS[Session Mgmt]
    end

    subgraph LOGHUB["📋 LOG Hub"]
        COLL[Log Collector]
        PROC[Log Processor]
        STOR[Log Storage]
        ALRT[Alerting]
    end

    subgraph EMAILHUB["📧 Email Hub"]
        TMPL[Templates]
        QUEUE[Send Queue]
        DLVR[Delivery]
        TRACK[Tracking]
    end

    subgraph SERVISHUB["🚦 Servis Hub"]
        ROUTE[Routing]
        RATE[Rate Limiting]
        CACHE[Response Cache]
        DOC[API Docs]
    end

    AUTH --> AUTHZ
    AUTHZ --> USER
    USER --> MFA
    MFA --> SESS

    COLL --> PROC
    PROC --> STOR
    STOR --> ALRT

    TMPL --> QUEUE
    QUEUE --> DLVR
    DLVR --> TRACK

    ROUTE --> RATE
    RATE --> CACHE
    CACHE --> DOC
```

## Veri Akış Şeması

```mermaid
flowchart TB
    subgraph Input["📥 Data Input"]
        UI[User Interface]
        API[External API]
        FILE[File Upload]
        EVT[System Events]
    end

    subgraph Processing["⚙️ Processing"]
        VAL[Validation]
        TRANS[Transformation]
        BUS[Business Logic]
        EVTP[Event Publishing]
    end

    subgraph Storage["💾 Storage"]
        DB[(Database)]
        CACHE[(Cache)]
        SEARCH[(Search Index)]
        FILES[(File Storage)]
    end

    subgraph Output["📤 Output"]
        RESP[API Response]
        NOTIF[Notifications]
        REPORT[Reports]
        EXPORT[Data Export]
    end

    UI --> VAL
    API --> VAL
    FILE --> VAL
    EVT --> VAL

    VAL --> TRANS
    TRANS --> BUS
    BUS --> EVTP

    BUS --> DB
    BUS --> CACHE
    BUS --> SEARCH
    FILE --> FILES

    DB --> RESP
    CACHE --> RESP
    EVTP --> NOTIF
    DB --> REPORT
    DB --> EXPORT
```

## Deployment Topolojisi

```mermaid
flowchart TB
    subgraph Internet["🌐 Internet"]
        USERS[Users]
        EXT[External Systems]
    end

    subgraph Edge["🛡️ Edge Layer"]
        CDN[CDN]
        WAF[WAF]
        LB[Load Balancer]
    end

    subgraph K8S["☸️ Kubernetes Cluster"]
        subgraph Ingress["Ingress"]
            ING[Ingress Controller]
        end

        subgraph Apps["Application Pods"]
            SSO_POD[SSO x3]
            AKD_POD[Akademik x3]
            LMS_POD[LMS x3]
            OTHER[Other Services]
        end

        subgraph Data["Data Pods"]
            PG_POD[PostgreSQL HA]
            RD_POD[Redis Cluster]
            MQ_POD[RabbitMQ Cluster]
        end

        subgraph Monitor["Monitoring"]
            PROM[Prometheus]
            GRAF[Grafana]
            ALERT[AlertManager]
        end
    end

    subgraph Storage["💾 Persistent Storage"]
        PV[Persistent Volumes]
        BACKUP[Backup Storage]
    end

    USERS --> CDN
    EXT --> WAF
    CDN --> WAF
    WAF --> LB
    LB --> ING
    ING --> Apps
    Apps --> Data
    Data --> PV
    Monitor --> Apps
    Monitor --> Data
    PV --> BACKUP
```

## Event Flow

```mermaid
sequenceDiagram
    participant C as Client
    participant G as API Gateway
    participant S as Service A
    participant E as Event Bus
    participant S2 as Service B
    participant N as Notification

    C->>G: HTTP Request
    G->>G: Auth & Rate Limit
    G->>S: Forward Request
    S->>S: Process Business Logic
    S->>S: Save to Database
    S-->>E: Publish Event
    S->>G: Response
    G->>C: HTTP Response
    
    E-->>S2: Deliver Event
    S2->>S2: Handle Event
    S2-->>N: Trigger Notification
    N->>N: Send Email/SMS
```

## Kimlik Doğrulama Akışı

```mermaid
sequenceDiagram
    participant U as User
    participant C as Client App
    participant G as API Gateway
    participant SSO as SSO Service
    participant S as Protected Service
    participant DB as Database

    U->>C: Login Request
    C->>G: POST /auth/login
    G->>SSO: Forward to SSO
    SSO->>DB: Verify Credentials
    DB->>SSO: User Data
    SSO->>SSO: Generate JWT
    SSO->>G: JWT Token
    G->>C: Token Response
    C->>C: Store Token

    U->>C: Access Protected Resource
    C->>G: GET /api/resource + JWT
    G->>G: Validate JWT
    G->>S: Forward + User Context
    S->>S: Check Permissions
    S->>DB: Get Data
    DB->>S: Data
    S->>G: Response
    G->>C: Response
    C->>U: Display Data
```

---

## Nasıl Kullanılır

Bu diyagramlar [Mermaid](https://mermaid.js.org/) formatındadır. Görüntülemek için:

1. **GitHub**: GitHub Markdown dosyalarında otomatik render edilir
2. **VS Code**: "Markdown Preview Mermaid Support" extension
3. **Online**: [Mermaid Live Editor](https://mermaid.live/)
4. **Docusaurus/GitBook**: Native Mermaid desteği

---

*Diyagramlar Open University v1.0.0 mimarisini yansıtmaktadır.*
