# Open University - Mimari Doküman

## İçindekiler

1. [Genel Bakış](#genel-bakış)
2. [Mimari Prensipler](#mimari-prensipler)
3. [Sistem Mimarisi](#sistem-mimarisi)
4. [Teknoloji Stack](#teknoloji-stack)
5. [Servis Katmanları](#servis-katmanları)
6. [Veri Mimarisi](#veri-mimarisi)
7. [Entegrasyon Mimarisi](#entegrasyon-mimarisi)
8. [Güvenlik Mimarisi](#güvenlik-mimarisi)
9. [Deployment Mimarisi](#deployment-mimarisi)
10. [Modül Bağımlılıkları](#modül-bağımlılıkları)

---

## Genel Bakış

Open University, mikroservis tabanlı, modüler bir mimari üzerine inşa edilmiş entegre üniversite yönetim platformudur.

### Temel Özellikler

- **Modüler Yapı**: Her uygulama bağımsız deploy edilebilir
- **API-First**: Tüm iletişim REST/GraphQL API üzerinden
- **Event-Driven**: Servisler arası asenkron iletişim
- **Multi-Tenant Ready**: Çoklu kurum desteği
- **Cloud-Native**: Container tabanlı, ölçeklenebilir

### Yüksek Seviye Mimari

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                              CLIENT LAYER                                    │
├─────────────────┬─────────────────┬─────────────────┬───────────────────────┤
│   Web Portal    │   Mobile App    │   Admin Panel   │   External Systems    │
│   (Next.js)     │   (React Native)│   (Next.js)     │   (API Consumers)     │
└────────┬────────┴────────┬────────┴────────┬────────┴──────────┬────────────┘
         │                 │                 │                   │
         └─────────────────┴────────┬────────┴───────────────────┘
                                    │
┌───────────────────────────────────▼─────────────────────────────────────────┐
│                            API GATEWAY LAYER                                 │
│  ┌─────────────────────────────────────────────────────────────────────┐   │
│  │                        Servis Hub (Kong/Traefik)                     │   │
│  │  • Rate Limiting  • Authentication  • Load Balancing  • Routing     │   │
│  └─────────────────────────────────────────────────────────────────────┘   │
└───────────────────────────────────┬─────────────────────────────────────────┘
                                    │
┌───────────────────────────────────▼─────────────────────────────────────────┐
│                           SERVICE MESH LAYER                                 │
│  ┌──────────────────────────────────────────────────────────────────────┐  │
│  │                    Service Discovery & Communication                  │  │
│  │         (Consul / Kubernetes Service Discovery / Istio)              │  │
│  └──────────────────────────────────────────────────────────────────────┘  │
└───────────────────────────────────┬─────────────────────────────────────────┘
                                    │
┌───────────────────────────────────▼─────────────────────────────────────────┐
│                          MICROSERVICES LAYER                                 │
│                                                                              │
│  ┌─────────────┐ ┌─────────────┐ ┌─────────────┐ ┌─────────────┐           │
│  │     SSO     │ │  Akademik   │ │  Personel   │ │     LMS     │           │
│  │   Service   │ │   Service   │ │   Service   │ │   Service   │   ....    │
│  └─────────────┘ └─────────────┘ └─────────────┘ └─────────────┘           │
│                                                                              │
│  ┌─────────────┐ ┌─────────────┐ ┌─────────────┐ ┌─────────────┐           │
│  │    Email    │ │  Başvuru    │ │   Kalite    │ │   Proje     │           │
│  │     Hub     │ │     Hub     │ │   Service   │ │   Service   │   ....    │
│  └─────────────┘ └─────────────┘ └─────────────┘ └─────────────┘           │
│                                                                              │
└───────────────────────────────────┬─────────────────────────────────────────┘
                                    │
┌───────────────────────────────────▼─────────────────────────────────────────┐
│                          EVENT BUS LAYER                                     │
│  ┌──────────────────────────────────────────────────────────────────────┐  │
│  │                    Message Broker (RabbitMQ / Kafka)                  │  │
│  │         Events: user.created, student.enrolled, grade.updated        │  │
│  └──────────────────────────────────────────────────────────────────────┘  │
└───────────────────────────────────┬─────────────────────────────────────────┘
                                    │
┌───────────────────────────────────▼─────────────────────────────────────────┐
│                            DATA LAYER                                        │
│                                                                              │
│  ┌─────────────┐ ┌─────────────┐ ┌─────────────┐ ┌─────────────┐           │
│  │ PostgreSQL  │ │    Redis    │ │Elasticsearch│ │  MinIO/S3   │           │
│  │  (Primary)  │ │   (Cache)   │ │  (Search)   │ │  (Storage)  │           │
│  └─────────────┘ └─────────────┘ └─────────────┘ └─────────────┘           │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Mimari Prensipler

### 1. Domain-Driven Design (DDD)

Her modül kendi bounded context'ine sahiptir:

```
┌─────────────────────────────────────────────────────────────────┐
│                     BOUNDED CONTEXTS                             │
├─────────────────┬─────────────────┬─────────────────────────────┤
│   ACADEMIC      │    HUMAN        │      RESEARCH               │
│   CONTEXT       │    RESOURCES    │      CONTEXT                │
│                 │    CONTEXT      │                             │
│ • Student       │ • Employee      │ • Project                   │
│ • Course        │ • Department    │ • Publication               │
│ • Enrollment    │ • Leave         │ • Grant                     │
│ • Grade         │ • Payroll       │ • Ethics                    │
│ • Curriculum    │ • Performance   │ • Lab                       │
└─────────────────┴─────────────────┴─────────────────────────────┘
```

### 2. SOLID Prensipleri

- **Single Responsibility**: Her servis tek bir iş yapar
- **Open/Closed**: Genişlemeye açık, değişikliğe kapalı
- **Liskov Substitution**: Alt tipler üst tiplerin yerine geçebilir
- **Interface Segregation**: Küçük, özelleşmiş arayüzler
- **Dependency Inversion**: Soyutlamalara bağımlılık

### 3. 12-Factor App

- Codebase: Tek kod tabanı, çoklu deployment
- Dependencies: Açıkça tanımlanmış bağımlılıklar
- Config: Konfigürasyon ortam değişkenlerinde
- Backing Services: Bağımlı servisler eklenti olarak
- Build/Release/Run: Aşamalar kesin ayrılmış
- Processes: Stateless süreçler
- Port Binding: Servisler port üzerinden erişilebilir
- Concurrency: Yatay ölçekleme
- Disposability: Hızlı başlatma ve zarif kapanış
- Dev/Prod Parity: Ortamlar benzer
- Logs: Event stream olarak loglar
- Admin Processes: Tek seferlik admin görevleri

---

## Sistem Mimarisi

### Katmanlı Mimari

```
┌─────────────────────────────────────────────────────────────────┐
│                    PRESENTATION LAYER                            │
│  ┌───────────────────────────────────────────────────────────┐  │
│  │  • React Components    • Next.js Pages    • Mobile Views  │  │
│  │  • State Management    • Form Handling    • Routing       │  │
│  └───────────────────────────────────────────────────────────┘  │
├─────────────────────────────────────────────────────────────────┤
│                      API LAYER                                   │
│  ┌───────────────────────────────────────────────────────────┐  │
│  │  • REST Controllers    • GraphQL Resolvers                │  │
│  │  • Request Validation  • Response Formatting              │  │
│  │  • Error Handling      • API Documentation (OpenAPI)      │  │
│  └───────────────────────────────────────────────────────────┘  │
├─────────────────────────────────────────────────────────────────┤
│                    SERVICE LAYER                                 │
│  ┌───────────────────────────────────────────────────────────┐  │
│  │  • Business Logic      • Use Cases                        │  │
│  │  • Transaction Mgmt    • Event Publishing                 │  │
│  │  • Authorization       • Validation Rules                 │  │
│  └───────────────────────────────────────────────────────────┘  │
├─────────────────────────────────────────────────────────────────┤
│                   DOMAIN LAYER                                   │
│  ┌───────────────────────────────────────────────────────────┐  │
│  │  • Entities            • Value Objects                    │  │
│  │  • Domain Events       • Domain Services                  │  │
│  │  • Aggregates          • Repository Interfaces            │  │
│  └───────────────────────────────────────────────────────────┘  │
├─────────────────────────────────────────────────────────────────┤
│                 INFRASTRUCTURE LAYER                             │
│  ┌───────────────────────────────────────────────────────────┐  │
│  │  • Database (Prisma)   • Cache (Redis)                    │  │
│  │  • Message Queue       • External APIs                    │  │
│  │  • File Storage        • Email/SMS Providers              │  │
│  └───────────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────────┘
```

### Modül İç Yapısı

Her mikroservis aşağıdaki yapıyı takip eder:

```
module-name/
├── src/
│   ├── api/                    # API Layer
│   │   ├── controllers/        # REST Controllers
│   │   ├── graphql/            # GraphQL Schema & Resolvers
│   │   ├── dto/                # Data Transfer Objects
│   │   ├── validators/         # Request Validators
│   │   └── middlewares/        # API Middlewares
│   │
│   ├── application/            # Application Layer
│   │   ├── use-cases/          # Use Case Implementations
│   │   ├── services/           # Application Services
│   │   ├── commands/           # Command Handlers (CQRS)
│   │   └── queries/            # Query Handlers (CQRS)
│   │
│   ├── domain/                 # Domain Layer
│   │   ├── entities/           # Domain Entities
│   │   ├── value-objects/      # Value Objects
│   │   ├── events/             # Domain Events
│   │   ├── services/           # Domain Services
│   │   └── repositories/       # Repository Interfaces
│   │
│   ├── infrastructure/         # Infrastructure Layer
│   │   ├── database/           # Database Implementation
│   │   │   ├── prisma/         # Prisma Schema & Migrations
│   │   │   └── repositories/   # Repository Implementations
│   │   ├── cache/              # Cache Implementation
│   │   ├── messaging/          # Message Queue
│   │   ├── external/           # External Service Clients
│   │   └── storage/            # File Storage
│   │
│   ├── shared/                 # Shared Utilities
│   │   ├── types/              # TypeScript Types
│   │   ├── constants/          # Constants
│   │   ├── utils/              # Utility Functions
│   │   └── exceptions/         # Custom Exceptions
│   │
│   └── config/                 # Configuration
│       ├── database.ts
│       ├── cache.ts
│       └── index.ts
│
├── tests/
│   ├── unit/                   # Unit Tests
│   ├── integration/            # Integration Tests
│   └── e2e/                    # End-to-End Tests
│
├── docs/                       # Module Documentation
├── Dockerfile
├── docker-compose.yml
├── package.json
└── README.md
```

---

## Teknoloji Stack

### Backend

| Kategori | Teknoloji | Açıklama |
|----------|-----------|----------|
| Runtime | Node.js 20 LTS | JavaScript runtime |
| Framework | NestJS 10 | Enterprise Node.js framework |
| Language | TypeScript 5 | Type-safe JavaScript |
| API | REST + GraphQL | Dual API support |
| ORM | Prisma | Type-safe database client |
| Validation | class-validator, Zod | Request validation |
| Documentation | OpenAPI/Swagger | API documentation |

### Frontend

| Kategori | Teknoloji | Açıklama |
|----------|-----------|----------|
| Framework | Next.js 14 | React framework with SSR |
| Language | TypeScript 5 | Type-safe JavaScript |
| State | Zustand / TanStack Query | State management |
| UI Library | Tailwind CSS + Shadcn/ui | Styling |
| Forms | React Hook Form + Zod | Form handling |
| Charts | Recharts / Apache ECharts | Data visualization |

### Mobile

| Kategori | Teknoloji | Açıklama |
|----------|-----------|----------|
| Framework | React Native / Expo | Cross-platform mobile |
| Navigation | React Navigation | Screen navigation |
| State | Zustand | State management |

### Database & Storage

| Kategori | Teknoloji | Açıklama |
|----------|-----------|----------|
| Primary DB | PostgreSQL 16 | Relational database |
| Cache | Redis 7 | In-memory cache |
| Search | Elasticsearch 8 / Meilisearch | Full-text search |
| File Storage | MinIO / S3 | Object storage |
| Time Series | TimescaleDB (opsiyonel) | Analytics data |

### Message Queue & Events

| Kategori | Teknoloji | Açıklama |
|----------|-----------|----------|
| Message Broker | RabbitMQ / Kafka | Async messaging |
| Event Store | EventStoreDB (opsiyonel) | Event sourcing |

### DevOps & Infrastructure

| Kategori | Teknoloji | Açıklama |
|----------|-----------|----------|
| Containerization | Docker | Container runtime |
| Orchestration | Kubernetes / Docker Swarm | Container orchestration |
| CI/CD | GitHub Actions | Automation |
| API Gateway | Kong / Traefik | API management |
| Service Mesh | Istio (opsiyonel) | Service communication |
| Monitoring | Prometheus + Grafana | Metrics & dashboards |
| Logging | ELK Stack / Loki | Log aggregation |
| Tracing | Jaeger | Distributed tracing |

---

## Servis Katmanları

### Core Services (Çekirdek Servisler)

Bu servisler diğer tüm servislerin bağımlı olduğu temel servislerdir:

```
┌─────────────────────────────────────────────────────────────────┐
│                      CORE SERVICES                               │
│                                                                  │
│  ┌─────────────────────────────────────────────────────────┐    │
│  │                         SSO                              │    │
│  │  • Authentication    • Authorization    • User Mgmt     │    │
│  │  • JWT/OAuth 2.0     • RBAC            • MFA            │    │
│  └─────────────────────────────────────────────────────────┘    │
│                              │                                   │
│  ┌──────────────┬────────────┴────────────┬──────────────┐      │
│  │              │                         │              │      │
│  ▼              ▼                         ▼              ▼      │
│  ┌─────────┐ ┌─────────┐ ┌─────────────┐ ┌─────────────┐       │
│  │ Servis  │ │  LOG    │ │   Email     │ │ Entegrator  │       │
│  │  Hub    │ │  Hub    │ │    Hub      │ │    Hub      │       │
│  └─────────┘ └─────────┘ └─────────────┘ └─────────────┘       │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

### Business Services (İş Servisleri)

İş mantığını içeren ana servisler:

```
┌─────────────────────────────────────────────────────────────────┐
│                    BUSINESS SERVICES                             │
│                                                                  │
│  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐  │
│  │    Akademik     │  │    Personel     │  │      LMS        │  │
│  │    Sistem       │  │    Sistemi      │  │                 │  │
│  │                 │  │                 │  │                 │  │
│  │ • Öğrenci       │  │ • Çalışan       │  │ • Ders          │  │
│  │ • Ders          │  │ • İzin          │  │ • İçerik        │  │
│  │ • Kayıt         │  │ • Bordro        │  │ • Ödev          │  │
│  │ • Not           │  │ • Performans    │  │ • Sınav         │  │
│  └─────────────────┘  └─────────────────┘  └─────────────────┘  │
│                                                                  │
│  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐  │
│  │    Başvuru      │  │    Kalite       │  │     Proje       │  │
│  │      Hub        │  │    Sistemi      │  │    Yönetimi     │  │
│  │                 │  │                 │  │                 │  │
│  │ • Form          │  │ • KPI           │  │ • Başvuru       │  │
│  │ • Değerlendirme │  │ • Değerlendirme │  │ • Bütçe         │  │
│  │ • Karar         │  │ • Akreditasyon  │  │ • Deliverable   │  │
│  └─────────────────┘  └─────────────────┘  └─────────────────┘  │
│                                                                  │
│  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐  │
│  │      CMS        │  │     Mezun       │  │     Finans      │  │
│  │                 │  │    Sistemi      │  │    Servisleri   │  │
│  │                 │  │                 │  │                 │  │
│  │ • Site          │  │ • Profil        │  │ • Sanal POS     │  │
│  │ • İçerik        │  │ • Kariyer       │  │ • Envanter      │  │
│  │ • Medya         │  │ • Etkinlik      │  │ • Satın Alma    │  │
│  └─────────────────┘  └─────────────────┘  └─────────────────┘  │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

### Integration Services (Entegrasyon Servisleri)

Dış sistemlerle entegrasyon sağlayan servisler:

```
┌─────────────────────────────────────────────────────────────────┐
│                  INTEGRATION SERVICES                            │
│                                                                  │
│  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐  │
│  │    E-Devlet     │  │      EBYS       │  │      EWP        │  │
│  │  Entegrasyon    │  │   Entegrator    │  │      Hub        │  │
│  │                 │  │                 │  │                 │  │
│  │ • KPS           │  │ • Belge         │  │ • Erasmus       │  │
│  │ • YÖKSİS        │  │ • İmza          │  │ • Anlaşma       │  │
│  │ • SGK           │  │ • Arşiv         │  │ • LA            │  │
│  └─────────────────┘  └─────────────────┘  └─────────────────┘  │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

---

## Veri Mimarisi

### Database Per Service

Her mikroservis kendi veritabanına sahiptir:

```
┌─────────────────────────────────────────────────────────────────┐
│                    DATABASE PER SERVICE                          │
│                                                                  │
│   ┌─────────┐    ┌─────────┐    ┌─────────┐    ┌─────────┐     │
│   │   SSO   │    │Akademik │    │Personel │    │   LMS   │     │
│   │ Service │    │ Service │    │ Service │    │ Service │     │
│   └────┬────┘    └────┬────┘    └────┬────┘    └────┬────┘     │
│        │              │              │              │           │
│        ▼              ▼              ▼              ▼           │
│   ┌─────────┐    ┌─────────┐    ┌─────────┐    ┌─────────┐     │
│   │ sso_db  │    │academic │    │   hr    │    │ lms_db  │     │
│   │         │    │   _db   │    │   _db   │    │         │     │
│   └─────────┘    └─────────┘    └─────────┘    └─────────┘     │
│                                                                  │
│                     PostgreSQL Cluster                           │
└─────────────────────────────────────────────────────────────────┘
```

### Shared Data Patterns

Servisler arası veri paylaşımı için kullanılan pattern'ler:

#### 1. Event-Driven Data Sync

```
┌──────────────┐         ┌──────────────┐         ┌──────────────┐
│   Akademik   │         │    Event     │         │     LMS      │
│   Service    │────────▶│     Bus      │────────▶│   Service    │
└──────────────┘         └──────────────┘         └──────────────┘
       │                        │                        │
       │  StudentEnrolled       │                        │
       │  Event Published       │   StudentEnrolled      │
       │ ─────────────────────▶ │   Event Consumed       │
       │                        │ ─────────────────────▶ │
       │                        │                        │
       │                        │   Create Course        │
       │                        │   Enrollment           │
```

#### 2. API Composition

```
┌──────────────┐
│   Client     │
└──────┬───────┘
       │
       ▼
┌──────────────┐
│  API Gateway │
│  (Composer)  │
└──────┬───────┘
       │
       ├──────────────────┬──────────────────┐
       ▼                  ▼                  ▼
┌──────────────┐   ┌──────────────┐   ┌──────────────┐
│   Akademik   │   │   Personel   │   │     LMS      │
│   Service    │   │   Service    │   │   Service    │
└──────────────┘   └──────────────┘   └──────────────┘
```

### Ortak Veri Modelleri

Tüm servislerde kullanılan ortak entity'ler:

```typescript
// packages/common/src/types/base.ts

interface BaseEntity {
  id: string;           // UUID
  createdAt: Date;
  updatedAt: Date;
  createdBy?: string;   // User ID
  updatedBy?: string;   // User ID
}

interface AuditableEntity extends BaseEntity {
  version: number;      // Optimistic locking
  deletedAt?: Date;     // Soft delete
}

interface TenantEntity extends AuditableEntity {
  tenantId: string;     // Multi-tenant support
}
```

### Cache Stratejisi

```
┌─────────────────────────────────────────────────────────────────┐
│                      CACHE STRATEGY                              │
│                                                                  │
│  ┌─────────────────────────────────────────────────────────┐    │
│  │                    Redis Cluster                         │    │
│  │                                                          │    │
│  │  ┌───────────────┐  ┌───────────────┐  ┌─────────────┐  │    │
│  │  │   Session     │  │   API Cache   │  │   Query     │  │    │
│  │  │   Store       │  │               │  │   Cache     │  │    │
│  │  │               │  │               │  │             │  │    │
│  │  │ • JWT tokens  │  │ • API resp.   │  │ • DB query  │  │    │
│  │  │ • User sess.  │  │ • Rate limit  │  │ • Agg. data │  │    │
│  │  │ TTL: 24h      │  │ TTL: 5min     │  │ TTL: 1h     │  │    │
│  │  └───────────────┘  └───────────────┘  └─────────────┘  │    │
│  │                                                          │    │
│  └─────────────────────────────────────────────────────────┘    │
│                                                                  │
│  Cache Patterns:                                                 │
│  • Cache-Aside (Lazy Loading)                                   │
│  • Write-Through (Kritik veriler)                               │
│  • Cache Invalidation via Events                                │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

---

## Entegrasyon Mimarisi

### Event-Driven Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                    EVENT-DRIVEN ARCHITECTURE                     │
│                                                                  │
│  Publishers                    Event Bus                         │
│  ──────────                   ─────────────                      │
│                                                                  │
│  ┌──────────┐                 ┌─────────────────────────────┐   │
│  │ Akademik │ ───────────────▶│                             │   │
│  └──────────┘                 │                             │   │
│                               │                             │   │
│  ┌──────────┐                 │      RabbitMQ / Kafka       │   │
│  │ Personel │ ───────────────▶│                             │   │
│  └──────────┘                 │                             │   │
│                               │    Topics / Exchanges:      │   │
│  ┌──────────┐                 │    • student.events         │   │
│  │   LMS    │ ───────────────▶│    • employee.events        │   │
│  └──────────┘                 │    • course.events          │   │
│                               │    • notification.events    │   │
│                               │                             │   │
│                               └──────────────┬──────────────┘   │
│                                              │                   │
│  Subscribers                                 │                   │
│  ───────────                                 │                   │
│                                              │                   │
│  ┌──────────┐                                │                   │
│  │  Email   │ ◀──────────────────────────────┤                   │
│  │   Hub    │                                │                   │
│  └──────────┘                                │                   │
│                                              │                   │
│  ┌──────────┐                                │                   │
│  │   LOG    │ ◀──────────────────────────────┤                   │
│  │   Hub    │                                │                   │
│  └──────────┘                                │                   │
│                                              │                   │
│  ┌──────────┐                                │                   │
│  │    BI    │ ◀──────────────────────────────┘                   │
│  │Dashboard │                                                    │
│  └──────────┘                                                    │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

### Event Naming Convention

```
{domain}.{entity}.{action}

Örnekler:
• academic.student.created
• academic.student.enrolled
• academic.grade.submitted
• hr.employee.hired
• hr.leave.approved
• lms.assignment.submitted
• notification.email.sent
```

### Event Schema

```typescript
// packages/common/src/events/base-event.ts

interface BaseEvent {
  eventId: string;        // UUID
  eventType: string;      // e.g., "academic.student.created"
  timestamp: Date;
  version: string;        // e.g., "1.0"
  source: string;         // Service name
  correlationId: string;  // Request tracing
  tenantId?: string;      // Multi-tenant
  userId?: string;        // Who triggered
  payload: unknown;       // Event-specific data
  metadata?: Record<string, unknown>;
}

// Örnek Event
interface StudentCreatedEvent extends BaseEvent {
  eventType: "academic.student.created";
  payload: {
    studentId: string;
    studentNumber: string;
    firstName: string;
    lastName: string;
    email: string;
    departmentId: string;
    programId: string;
    enrollmentDate: Date;
  };
}
```

### API Entegrasyonu

```
┌─────────────────────────────────────────────────────────────────┐
│                     API INTEGRATION PATTERNS                     │
│                                                                  │
│  Synchronous Communication                                       │
│  ─────────────────────────────                                   │
│                                                                  │
│  ┌──────────┐      REST/GraphQL      ┌──────────┐               │
│  │ Service  │ ◀────────────────────▶ │ Service  │               │
│  │    A     │                        │    B     │               │
│  └──────────┘                        └──────────┘               │
│                                                                  │
│  • Request-Response pattern                                      │
│  • Circuit Breaker ile korumalı                                 │
│  • Retry policy tanımlı                                         │
│  • Timeout belirlenmiş                                          │
│                                                                  │
│  ─────────────────────────────────────────────────────────────  │
│                                                                  │
│  Asynchronous Communication                                      │
│  ──────────────────────────────                                  │
│                                                                  │
│  ┌──────────┐       Event        ┌──────────┐                   │
│  │ Service  │ ──────────────────▶│  Event   │                   │
│  │    A     │                    │   Bus    │                   │
│  └──────────┘                    └────┬─────┘                   │
│                                       │                          │
│                                       ▼                          │
│                                  ┌──────────┐                   │
│                                  │ Service  │                   │
│                                  │    B     │                   │
│                                  └──────────┘                   │
│                                                                  │
│  • Fire-and-forget pattern                                      │
│  • Eventually consistent                                        │
│  • Dead letter queue ile hata yönetimi                         │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

---

## Güvenlik Mimarisi

### Authentication & Authorization Flow

```
┌─────────────────────────────────────────────────────────────────┐
│                   AUTHENTICATION FLOW                            │
│                                                                  │
│  ┌────────┐     ┌─────────┐     ┌─────────┐     ┌─────────┐    │
│  │ Client │────▶│   API   │────▶│   SSO   │────▶│Identity │    │
│  │        │     │ Gateway │     │ Service │     │Provider │    │
│  └────────┘     └─────────┘     └─────────┘     └─────────┘    │
│       │              │               │               │          │
│       │  1. Request  │               │               │          │
│       │─────────────▶│               │               │          │
│       │              │  2. Validate  │               │          │
│       │              │──────────────▶│               │          │
│       │              │               │  3. Verify    │          │
│       │              │               │──────────────▶│          │
│       │              │               │◀──────────────│          │
│       │              │◀──────────────│  4. Token     │          │
│       │◀─────────────│  5. JWT       │               │          │
│       │              │               │               │          │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────┐
│                   AUTHORIZATION FLOW                             │
│                                                                  │
│  ┌────────┐     ┌─────────┐     ┌─────────┐     ┌─────────┐    │
│  │ Client │────▶│   API   │────▶│  Auth   │────▶│ Service │    │
│  │  +JWT  │     │ Gateway │     │Middleware│    │         │    │
│  └────────┘     └─────────┘     └─────────┘     └─────────┘    │
│       │              │               │               │          │
│       │  1. Request  │               │               │          │
│       │  + JWT Token │               │               │          │
│       │─────────────▶│               │               │          │
│       │              │  2. Verify    │               │          │
│       │              │     JWT       │               │          │
│       │              │──────────────▶│               │          │
│       │              │               │  3. Check     │          │
│       │              │               │  Permissions  │          │
│       │              │               │──────────────▶│          │
│       │              │               │◀──────────────│          │
│       │              │◀──────────────│  4. Allow/    │          │
│       │◀─────────────│     Deny      │               │          │
│       │              │               │               │          │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

### Role-Based Access Control (RBAC)

```
┌─────────────────────────────────────────────────────────────────┐
│                         RBAC MODEL                               │
│                                                                  │
│  ┌─────────────────────────────────────────────────────────┐    │
│  │                        USERS                             │    │
│  │   ┌───────┐  ┌───────┐  ┌───────┐  ┌───────┐           │    │
│  │   │ User1 │  │ User2 │  │ User3 │  │ User4 │           │    │
│  │   └───┬───┘  └───┬───┘  └───┬───┘  └───┬───┘           │    │
│  └───────┼──────────┼──────────┼──────────┼────────────────┘    │
│          │          │          │          │                      │
│          ▼          ▼          ▼          ▼                      │
│  ┌─────────────────────────────────────────────────────────┐    │
│  │                        ROLES                             │    │
│  │   ┌─────────┐  ┌─────────┐  ┌─────────┐  ┌─────────┐   │    │
│  │   │ Student │  │ Faculty │  │  Admin  │  │   HR    │   │    │
│  │   └────┬────┘  └────┬────┘  └────┬────┘  └────┬────┘   │    │
│  └────────┼────────────┼────────────┼────────────┼─────────┘    │
│           │            │            │            │               │
│           ▼            ▼            ▼            ▼               │
│  ┌─────────────────────────────────────────────────────────┐    │
│  │                    PERMISSIONS                           │    │
│  │                                                          │    │
│  │   ┌────────────────────────────────────────────────┐    │    │
│  │   │  Resource          Actions                      │    │    │
│  │   ├────────────────────────────────────────────────┤    │    │
│  │   │  students          read, create, update, delete │    │    │
│  │   │  courses           read, create, update         │    │    │
│  │   │  grades            read, create                 │    │    │
│  │   │  employees         read, create, update, delete │    │    │
│  │   │  reports           read, export                 │    │    │
│  │   └────────────────────────────────────────────────┘    │    │
│  │                                                          │    │
│  └─────────────────────────────────────────────────────────┘    │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

### Security Layers

```
┌─────────────────────────────────────────────────────────────────┐
│                      SECURITY LAYERS                             │
│                                                                  │
│  Layer 1: Network Security                                       │
│  ┌─────────────────────────────────────────────────────────┐    │
│  │  • TLS 1.3 everywhere                                    │    │
│  │  • WAF (Web Application Firewall)                        │    │
│  │  • DDoS Protection                                       │    │
│  │  • IP Whitelisting (for admin)                          │    │
│  └─────────────────────────────────────────────────────────┘    │
│                                                                  │
│  Layer 2: API Gateway Security                                   │
│  ┌─────────────────────────────────────────────────────────┐    │
│  │  • Rate Limiting                                         │    │
│  │  • Request Validation                                    │    │
│  │  • JWT Verification                                      │    │
│  │  • API Key Management                                    │    │
│  └─────────────────────────────────────────────────────────┘    │
│                                                                  │
│  Layer 3: Application Security                                   │
│  ┌─────────────────────────────────────────────────────────┐    │
│  │  • Input Validation                                      │    │
│  │  • Output Encoding                                       │    │
│  │  • CSRF Protection                                       │    │
│  │  • SQL Injection Prevention                              │    │
│  │  • XSS Prevention                                        │    │
│  └─────────────────────────────────────────────────────────┘    │
│                                                                  │
│  Layer 4: Data Security                                          │
│  ┌─────────────────────────────────────────────────────────┐    │
│  │  • Encryption at Rest (AES-256)                          │    │
│  │  • Encryption in Transit (TLS)                           │    │
│  │  • Data Masking                                          │    │
│  │  • Audit Logging                                         │    │
│  └─────────────────────────────────────────────────────────┘    │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

---

## Deployment Mimarisi

### Kubernetes Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                   KUBERNETES CLUSTER                             │
│                                                                  │
│  ┌─────────────────────────────────────────────────────────┐    │
│  │                    INGRESS LAYER                         │    │
│  │  ┌─────────────────────────────────────────────────┐    │    │
│  │  │              Ingress Controller                  │    │    │
│  │  │              (NGINX / Traefik)                   │    │    │
│  │  └─────────────────────────────────────────────────┘    │    │
│  └─────────────────────────────────────────────────────────┘    │
│                              │                                   │
│  ┌───────────────────────────▼─────────────────────────────┐    │
│  │                  NAMESPACE: open-university              │    │
│  │                                                          │    │
│  │  Services (Deployments)                                  │    │
│  │  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐   │    │
│  │  │   SSO    │ │ Akademik │ │ Personel │ │   LMS    │   │    │
│  │  │  (3x)    │ │   (3x)   │ │   (2x)   │ │   (3x)   │   │    │
│  │  └──────────┘ └──────────┘ └──────────┘ └──────────┘   │    │
│  │                                                          │    │
│  │  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐   │    │
│  │  │  Email   │ │   LOG    │ │  Servis  │ │   CMS    │   │    │
│  │  │   Hub    │ │   Hub    │ │   Hub    │ │          │   │    │
│  │  └──────────┘ └──────────┘ └──────────┘ └──────────┘   │    │
│  │                                                          │    │
│  └─────────────────────────────────────────────────────────┘    │
│                                                                  │
│  ┌─────────────────────────────────────────────────────────┐    │
│  │                  NAMESPACE: data                         │    │
│  │                                                          │    │
│  │  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐   │    │
│  │  │PostgreSQL│ │  Redis   │ │ RabbitMQ │ │  MinIO   │   │    │
│  │  │ (HA)     │ │ Cluster  │ │ Cluster  │ │          │   │    │
│  │  └──────────┘ └──────────┘ └──────────┘ └──────────┘   │    │
│  │                                                          │    │
│  └─────────────────────────────────────────────────────────┘    │
│                                                                  │
│  ┌─────────────────────────────────────────────────────────┐    │
│  │                  NAMESPACE: monitoring                   │    │
│  │                                                          │    │
│  │  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐   │    │
│  │  │Prometheus│ │ Grafana  │ │  Jaeger  │ │   ELK    │   │    │
│  │  │          │ │          │ │          │ │  Stack   │   │    │
│  │  └──────────┘ └──────────┘ └──────────┘ └──────────┘   │    │
│  │                                                          │    │
│  └─────────────────────────────────────────────────────────┘    │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

### CI/CD Pipeline

```
┌─────────────────────────────────────────────────────────────────┐
│                      CI/CD PIPELINE                              │
│                                                                  │
│  ┌──────┐    ┌──────┐    ┌──────┐    ┌──────┐    ┌──────┐     │
│  │ Code │───▶│Build │───▶│ Test │───▶│ Scan │───▶│Deploy│     │
│  │ Push │    │      │    │      │    │      │    │      │     │
│  └──────┘    └──────┘    └──────┘    └──────┘    └──────┘     │
│                                                                  │
│  ┌─────────────────────────────────────────────────────────┐    │
│  │                                                          │    │
│  │  1. CODE PUSH                                            │    │
│  │     • Git push to branch                                 │    │
│  │     • Pull request created                               │    │
│  │                                                          │    │
│  │  2. BUILD                                                │    │
│  │     • Install dependencies                               │    │
│  │     • TypeScript compilation                             │    │
│  │     • Docker image build                                 │    │
│  │                                                          │    │
│  │  3. TEST                                                 │    │
│  │     • Unit tests                                         │    │
│  │     • Integration tests                                  │    │
│  │     • E2E tests (staging)                               │    │
│  │     • Code coverage check                                │    │
│  │                                                          │    │
│  │  4. SECURITY SCAN                                        │    │
│  │     • SAST (Static Analysis)                            │    │
│  │     • Dependency vulnerability scan                      │    │
│  │     • Container image scan                               │    │
│  │                                                          │    │
│  │  5. DEPLOY                                               │    │
│  │     • Push to container registry                         │    │
│  │     • Deploy to staging                                  │    │
│  │     • Smoke tests                                        │    │
│  │     • Deploy to production (manual approval)             │    │
│  │                                                          │    │
│  └─────────────────────────────────────────────────────────┘    │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

### Environment Strategy

```
┌─────────────────────────────────────────────────────────────────┐
│                    ENVIRONMENTS                                  │
│                                                                  │
│  ┌─────────────────────────────────────────────────────────┐    │
│  │  DEVELOPMENT                                             │    │
│  │  • Local Docker Compose                                  │    │
│  │  • Hot reload enabled                                    │    │
│  │  • Mock external services                                │    │
│  │  • Seed data                                             │    │
│  └─────────────────────────────────────────────────────────┘    │
│                              │                                   │
│                              ▼                                   │
│  ┌─────────────────────────────────────────────────────────┐    │
│  │  STAGING                                                 │    │
│  │  • Kubernetes cluster (scaled down)                      │    │
│  │  • Production-like configuration                         │    │
│  │  • Integration with test external services               │    │
│  │  • Automated testing                                     │    │
│  └─────────────────────────────────────────────────────────┘    │
│                              │                                   │
│                              ▼                                   │
│  ┌─────────────────────────────────────────────────────────┐    │
│  │  PRODUCTION                                              │    │
│  │  • Kubernetes cluster (full scale)                       │    │
│  │  • High availability                                     │    │
│  │  • Real external services                                │    │
│  │  • Monitoring & alerting                                 │    │
│  │  • Backup & disaster recovery                            │    │
│  └─────────────────────────────────────────────────────────┘    │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

---

## Modül Bağımlılıkları

### Bağımlılık Matrisi

```
┌─────────────────────────────────────────────────────────────────┐
│                   MODULE DEPENDENCY MATRIX                       │
│                                                                  │
│              SSO  LOG  EMAIL SERVIS  AKADEMIK  PERSONEL  LMS    │
│            ┌─────┬─────┬─────┬─────┬─────────┬─────────┬─────┐ │
│  SSO       │  -  │  ✓  │  ✓  │  ✓  │         │         │     │ │
│  LOG Hub   │  ✓  │  -  │     │     │         │         │     │ │
│  Email Hub │  ✓  │  ✓  │  -  │     │         │         │     │ │
│  Servis Hub│  ✓  │  ✓  │     │  -  │         │         │     │ │
│  Akademik  │  ✓  │  ✓  │  ✓  │  ✓  │    -    │    ✓    │  ✓  │ │
│  Personel  │  ✓  │  ✓  │  ✓  │  ✓  │         │    -    │     │ │
│  LMS       │  ✓  │  ✓  │  ✓  │  ✓  │    ✓    │    ✓    │  -  │ │
│  CMS       │  ✓  │  ✓  │  ✓  │  ✓  │         │         │     │ │
│  Başvuru   │  ✓  │  ✓  │  ✓  │  ✓  │    ✓    │    ✓    │     │ │
│  Kalite    │  ✓  │  ✓  │  ✓  │  ✓  │    ✓    │    ✓    │     │ │
│  Proje     │  ✓  │  ✓  │  ✓  │  ✓  │    ✓    │    ✓    │     │ │
│            └─────┴─────┴─────┴─────┴─────────┴─────────┴─────┘ │
│                                                                  │
│  ✓ = Bağımlılık var                                             │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

### Başlangıç Sırası (Bootstrap Order)

Sistemin ayağa kalkması için önerilen sıra:

```
Phase 0: Infrastructure
─────────────────────────
1. PostgreSQL
2. Redis
3. RabbitMQ
4. MinIO

Phase 1: Core Services
─────────────────────────
5. SSO (Kimlik Yönetimi)
6. Servis Hub (API Gateway)
7. LOG Hub (Loglama)
8. Email Hub (Bildirimler)

Phase 2: Foundation Services
─────────────────────────────
9. Akademik Sistem
10. Personel Sistemi
11. CMS

Phase 3: Business Services
──────────────────────────
12. LMS
13. Başvuru Hub
14. Kalite Sistemi
15. Proje Yönetimi
... (diğer servisler)

Phase 4: Integration Services
─────────────────────────────
16. E-Devlet Entegrasyon
17. EBYS Entegrator
18. EWP Hub

Phase 5: Analytics & Monitoring
───────────────────────────────
19. BI Dashboard
20. Siber Güvenlik İzleme
```

### Servis İletişim Diyagramı

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                        SERVICE COMMUNICATION DIAGRAM                         │
│                                                                              │
│                                 ┌─────────┐                                  │
│                                 │   SSO   │                                  │
│                                 └────┬────┘                                  │
│                                      │                                       │
│           ┌──────────────────────────┼──────────────────────────┐           │
│           │                          │                          │           │
│           ▼                          ▼                          ▼           │
│     ┌──────────┐              ┌──────────┐              ┌──────────┐       │
│     │ Akademik │◀────────────▶│ Personel │◀────────────▶│   LMS    │       │
│     │  Sistem  │              │  Sistemi │              │          │       │
│     └────┬─────┘              └────┬─────┘              └────┬─────┘       │
│          │                         │                         │              │
│          │    ┌────────────────────┼────────────────────┐   │              │
│          │    │                    │                    │   │              │
│          ▼    ▼                    ▼                    ▼   ▼              │
│     ┌──────────┐              ┌──────────┐              ┌──────────┐       │
│     │ Başvuru  │              │  Kalite  │              │  Proje   │       │
│     │   Hub    │              │  Sistemi │              │ Yönetimi │       │
│     └──────────┘              └──────────┘              └──────────┘       │
│                                                                              │
│     ════════════════════════════════════════════════════════════════        │
│                              EVENT BUS                                       │
│     ════════════════════════════════════════════════════════════════        │
│                                                                              │
│          │                         │                         │              │
│          ▼                         ▼                         ▼              │
│     ┌──────────┐              ┌──────────┐              ┌──────────┐       │
│     │  Email   │              │   LOG    │              │    BI    │       │
│     │   Hub    │              │   Hub    │              │Dashboard │       │
│     └──────────┘              └──────────┘              └──────────┘       │
│                                                                              │
└─────────────────────────────────────────────────────────────────────────────┘

Legend:
───────
  ──────▶  Synchronous (REST/GraphQL)
  ══════   Asynchronous (Event Bus)
```

---

## Ek Dokümanlar

- [API Standartları](./API_STANDARDS.md)
- [Veritabanı Konvansiyonları](./DATABASE_CONVENTIONS.md)
- [Event Kataloğu](./EVENT_CATALOG.md)
- [Güvenlik Rehberi](./SECURITY_GUIDE.md)
- [Deployment Rehberi](./DEPLOYMENT_GUIDE.md)
- [Monitoring Rehberi](./MONITORING_GUIDE.md)

---

## Versiyon Geçmişi

| Versiyon | Tarih | Açıklama |
|----------|-------|----------|
| 1.0.0 | 2025-01 | İlk versiyon |

---

*Bu doküman Open University projesinin teknik mimarisini tanımlar ve geliştirme sürecinde referans olarak kullanılmalıdır.*
