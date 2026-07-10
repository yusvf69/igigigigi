# SYSTEM ARCHITECTURE

## Islamic Education Platform (IEP)

---

| Document ID | IEP-ARCH-001 |
|---|---|
| Version | 1.0.0 |
| Status | Approved |
| Classification | Enterprise Internal |
| Effective Date | 2026-07-07 |
| Approving Authority | Office of the Chief Architect |
| Custodian | Architecture Review Board |
| Constitutional Reference | IEP-CONST-001 (Sections 6, 7, 8) |
| Vision Reference | IEP-VISION-001 (Section 17) |

---

## TABLE OF CONTENTS

1. EXECUTIVE OVERVIEW
2. ARCHITECTURAL PHILOSOPHY
3. SYSTEM CONTEXT DIAGRAM
4. HIGH-LEVEL ARCHITECTURE
5. LAYER ARCHITECTURE
6. SERVICE DECOMPOSITION
7. COMMUNICATION ARCHITECTURE
8. DATA ARCHITECTURE
9. DEPLOYMENT ARCHITECTURE
10. SCALABILITY ARCHITECTURE
11. SECURITY ARCHITECTURE
12. OBSERVABILITY ARCHITECTURE
13. RESILIENCE ARCHITECTURE
14. MULTI-TENANCY ARCHITECTURE
15. API ARCHITECTURE
16. EVENT-DRIVEN ARCHITECTURE
17. AI AGENT PLATFORM ARCHITECTURE
18. KNOWLEDGE GRAPH ARCHITECTURE
19. CONTENT DELIVERY ARCHITECTURE
20. SEARCH ARCHITECTURE
21. STORAGE ARCHITECTURE
22. CACHING ARCHITECTURE
23. EDGE COMPUTING ARCHITECTURE
24. OFFLINE ARCHITECTURE
25. DISASTER RECOVERY ARCHITECTURE
26. ARCHITECTURE DECISION RECORDS
27. FUTURE ARCHITECTURAL EVOLUTION
28. APPENDICES

---

## 1. EXECUTIVE OVERVIEW

### 1.1 Purpose

This document defines the complete system architecture for the Islamic Education Platform (IEP). It provides the architectural blueprint that guides all engineering decisions, ensuring that the platform meets its functional and non-functional requirements while adhering to the principles established in the Project Constitution.

### 1.2 Scope

The architecture described herein covers:
- All platform subsystems, services, and components.
- All communication patterns and protocols.
- All data stores, data flows, and data lifecycle management.
- All deployment environments and infrastructure.
- All scalability, performance, security, and resilience mechanisms.
- All integration points with external systems.
- All AI agent infrastructure and orchestration.

### 1.3 Architecture Tenets

This architecture is guided by the following foundational tenets:

1. **Knowledge-Truth Separation**: The single most important architectural principle. Knowledge storage (facts) is strictly separated from content generation (presentation). The Knowledge Graph is the sole source of truth.

2. **Multi-Agent Orchestration**: Complex workflows are decomposed into specialized agents coordinated by a Master Orchestrator. Each agent has a single responsibility.

3. **Event-Driven Communication**: Services communicate primarily through asynchronous events, enabling loose coupling, scalability, and resilience.

4. **Immutable Provenance**: Every fact's complete history is tracked from source to presentation, with cryptographic verifiability.

5. **Isolated Failure Domains**: Each service and agent operates in its own failure domain. No cascade failures.

6. **Evolutionary Architecture**: The architecture is designed to evolve through incremental, reversible changes.

### 1.4 Architecture Overview (One-Paragraph)

The IEP employs a **multi-layer, event-driven, domain-oriented microservices architecture** organized around a central **Knowledge Graph** that serves as the single source of truth for all platform knowledge. **Specialized AI agents** (extraction, verification, content generation, delivery) operate as autonomous services coordinated by a **Master Orchestrator** through asynchronous event streams. The platform is deployed on **Kubernetes** across multiple cloud regions, uses **PostgreSQL** with **Apache AGE** for graph capabilities as its primary database, **Apache Kafka** for event streaming, **Elasticsearch** for search, and **MinIO** for object storage. All AI operations are grounded in the verified Knowledge Graph through **Retrieval-Augmented Generation (RAG)** , ensuring that no generated content deviates from established facts.

---

## 2. ARCHITECTURAL PHILOSOPHY

### 2.1 Design Influences

The IEP architecture draws from:

| Influence | Applied Principle |
|---|---|
| Domain-Driven Design (DDD) | Bounded contexts, ubiquitous language, aggregate roots |
| Event-Driven Architecture | Loose coupling, eventual consistency, event sourcing |
| Microservices Architecture | Service autonomy, decentralized data, polyglot persistence |
| Clean Architecture | Dependency inversion, use case centralization, interface adaptation |
| CQRS (Command Query Responsibility Segregation) | Separate read and write models for knowledge operations |
| Hexagonal Architecture | Ports and adapters, domain isolation, testability |
| Multi-Agent Systems (MAS) | Agent autonomy, coordination, communication, delegation |
| Data Mesh | Domain-owned data, data as product, federated governance |
| Twelve-Factor App | Config, disposability, dev/prod parity, logging, statelessness |

### 2.2 Architectural Style: Hybrid

The IEP uses a **hybrid architectural style** combining:

1. **Layered Architecture** for the overall system decomposition (presentation, API, domain, infrastructure).
2. **Microservices Architecture** within each major domain (knowledge, content, user, learning).
3. **Event-Driven Architecture** for inter-service communication and workflow orchestration.
4. **Multi-Agent Architecture** for AI-powered operations.
5. **Data-Centric Architecture** with the Knowledge Graph as the central data hub.

### 2.3 Key Architectural Decisions

| Decision | Choice | Rationale |
|---|---|---|
| Primary Programming Language | Rust (services), Python (AI/ML), TypeScript (frontend) | Rust for performance/safety, Python for AI ecosystem, TypeScript for web |
| Service Communication | Async: Kafka; Sync: gRPC; External: REST/GraphQL | Async for resilience, gRPC for internal performance, REST for external APIs |
| Primary Database | PostgreSQL with Apache AGE extension | Single database engine for relational + graph workloads |
| Knowledge Graph Storage | PostgreSQL + Apache AGE (primary), Neo4j (analytics) | AGE provides graph capabilities within PostgreSQL ecosystem |
| Event Bus | Apache Kafka | Industry standard for event streaming, excellent durability |
| Container Orchestration | Kubernetes | Industry standard, cloud-agnostic, extensive ecosystem |
| Service Mesh | Istio | Observability, security (mTLS), traffic management |
| AI Agent Platform | Custom framework on LangChain + Ray | Flexibility of LangChain, distributed computing with Ray |
| LLM Integration | Multi-provider abstraction layer | Avoid vendor lock-in, cost optimization |

---

## 3. SYSTEM CONTEXT DIAGRAM

### 3.1 External Systems

```
    +------------------+     +------------------+     +------------------+
    |   Users/Learners |     |  Administrators  |     |   Educators /    |
    |  (Web, Mobile,   |     |  (Dashboard,     |     |   Content Creators|
    |   API Clients)   |     |   Management)    |     |   (Creator Studio)|
    +--------+---------+     +--------+---------+     +--------+---------+
             |                        |                        |
    +--------+-------------------------+------------------------+---------+
    |                                                                     |
    |                     ISLAMIC EDUCATION PLATFORM                      |
    |                                                                     |
    +-----+-----+-----+-----+-----+-----+-----+-----+-----+-----+-----+--+
          |     |     |     |     |     |     |     |     |     |     |
    +-----+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+--+
    | OCR    | LLM   | TTS    | Image  | Cloud  | Payment| CDN   |Auth   |
    | Service| APIs  | Service| Gen API|Infra   | Gateway|       |Service |
    +--------+-------+--------+--------+--------+-------+-------+--------+
```

### 3.2 System Interfaces

| Interface | Type | Protocol | Description |
|---|---|---|---|
| User Web Application | Client-Server | HTTPS, WebSocket | Main browser-based learning interface |
| Mobile Applications | Client-Server | HTTPS, WebSocket | iOS and Android native apps |
| REST API | External | HTTPS, JSON | Public API for third-party integrations |
| GraphQL API | External | HTTPS | Flexible query API for complex data needs |
| Admin Dashboard | Client-Server | HTTPS, WebSocket | Administrative interface |
| Creator Studio | Client-Server | HTTPS, WebSocket | Content creator interface |
| OCR Provider API | Service | HTTP, gRPC | Document digitization |
| LLM Provider API | Service | HTTP, gRPC | AI model inference |
| TTS Service | Service | HTTP, gRPC | Text-to-speech generation |
| Image Generation API | Service | HTTP, gRPC | AI image generation |
| Cloud Infrastructure | Service | API | Cloud provider services |
| Payment Gateway | Service | HTTPS | Payment processing |
| Authentication Provider | Service | OAuth 2.1, OpenID Connect | Identity management |
| SMS/Email Service | Service | HTTPS | Notifications |

### 3.3 Context Diagram Components

| Component | Description |
|---|---|
| Book Ingestion Pipeline | Receives books (PDF, scanned, digital), performs OCR, extracts structure |
| Knowledge Extraction System | Extracts entities, facts, relationships from processed text |
| Verification Pipeline | Verifies extracted knowledge against trusted sources |
| Knowledge Graph | Stores verified knowledge with full provenance |
| Content Generation Engine | Generates learning materials from Knowledge Graph facts |
| Learning Delivery System | Delivers content to users across all modes |
| Master Orchestrator | Coordinates all agents and workflows |
| User System | User accounts, authentication, profiles, preferences |
| Admin System | Platform administration, monitoring, configuration |
| Search System | Full-text and semantic search across all content |
| Analytics System | Usage analytics, learning analytics, business metrics |
| Notification System | Push notifications, emails, in-app notifications |

---

## 4. HIGH-LEVEL ARCHITECTURE

### 4.1 Architecture Overview Diagram

```
+=======================================================================+
|                         PRESENTATION LAYER                             |
|  +-------------+  +-------------+  +-------------+  +-----------+    |
|  | Web App     |  | Mobile App  |  | Admin       |  | Creator   |    |
|  | (React/Next)|  | (Flutter)   |  | Dashboard   |  | Studio    |    |
|  +------+------+  +------+------+  +------+------+  +-----+-----+    |
+=========+===============+===============+===============+============+
          |               |               |               |
+=========|===============|===============|===============|============+
|         v               v               v               v            |
|  +================================================================+  |
|  |                    API GATEWAY LAYER                          |  |
|  |  +-----------+ +-----------+ +-----------+ +--------------+   |  |
|  |  | REST API  | | GraphQL   | | WebSocket | | gRPC Gateway |   |  |
|  |  | Gateway   | | Gateway   | | Gateway   | |              |   |  |
|  |  +-----------+ +-----------+ +-----------+ +--------------+   |  |
|  |  +--------------------------------------------------------+   |  |
|  |  | Authentication & Authorization Layer                   |   |  |
|  |  | (OAuth 2.1, OpenID Connect, RBAC/ABAC, API Keys)      |   |  |
|  |  +--------------------------------------------------------+   |  |
|  +================================================================+  |
|                                                                     |
|  +================================================================+  |
|  |                    SERVICE MESH (Istio)                         |  |
|  |  (Traffic Management, Security (mTLS), Observability, Routing) |  |
|  +================================================================+  |
|                                                                     |
|  +================================================================+  |
|  |                    APPLICATION LAYER                            |  |
|  |                                                                  |  |
|  |  +--------------------+  +------------------+  +-------------+  |  |
|  |  | Knowledge Domain   |  | Content Domain   |  | User Domain |  |  |
|  |  |                    |  |                  |  |             |  |  |
|  |  | - Book Ingestion   |  | - Content Gen    |  | - User Mgmt |  |  |
|  |  | - OCR Service      |  | - Mode Renderers |  | - Auth      |  |  |
|  |  | - Entity Extract   |  | - Media Gen      |  | - Profiles  |  |  |
|  |  | - Fact Extraction  |  | - Assembly       |  | - Progress  |  |  |
|  |  | - Verification     |  | - Publishing     |  | - Settings  |  |  |
|  |  +--------------------+  +------------------+  +-------------+  |  |
|  |                                                                  |  |
|  |  +--------------------+  +------------------+  +-------------+  |  |
|  |  | Learning Domain    |  | Analytics Domain |  | Admin Domain|  |  |
|  |  |                    |  |                  |  |             |  |  |
|  |  | - Learning Path   |  | - Usage Analysis |  | - Config    |  |  |
|  |  | - Assessment      |  | - Learn Analysis |  | - Monitoring|  |  |
|  |  | - Adaptive Engine |  | - Business Intel |  | - Auditing  |  |  |
|  |  | - Recommendation  |  | - Reporting      |  | - Support   |  |  |
|  |  +--------------------+  +------------------+  +-------------+  |  |
|  +================================================================+  |
|                                                                     |
|  +================================================================+  |
|  |                    AGENT PLATFORM LAYER                          |  |
|  |                                                                  |  |
|  |  +--------------------+  +------------------+  +-------------+  |  |
|  |  | Master Orchestrator|  | Extraction Agents|  | Verification|  |  |
|  |  |                    |  |                  |  | Agents      |  |  |
|  |  | - Workflow Mgmt   |  | - OCR Agent      |  | - Cross-Ref |  |  |
|  |  | - Agent Dispatch  |  | - Entity Agent   |  | - LLM Check |  |  |
|  |  | - State Tracking  |  | - Relation Agent |  | - Sch Review|  |  |
|  |  | - Error Handling  |  | - Structure Agent |  | - Contradict|  |  |
|  |  +--------------------+  +------------------+  +-------------+  |  |
|  |                                                                  |  |
|  |  +--------------------+  +------------------+  +-------------+  |  |
|  |  | Content Gen Agents |  | Learning Agents  |  | Quality     |  |  |
|  |  |                    |  |                  |  | Agents      |  |  |
|  |  | - Story Agent     |  | - AI Teacher     |  | - Accuracy  |  |  |
|  |  | - Quiz Agent      |  | - Adaptive Path  |  | - Relevance |  |  |
|  |  | - Flashcard Agent |  | - Assessment     |  | - Completen |  |  |
|  |  | - Podcast Agent   |  | - Recommend      |  | - Style     |  |  |
|  |  | - etc. (25+ modes)|  | - Progress Track |  | - Bias Check|  |  |
|  |  +--------------------+  +------------------+  +-------------+  |  |
|  +================================================================+  |
|                                                                     |
|  +================================================================+  |
|  |                    DATA LAYER                                    |  |
|  |                                                                  |  |
|  |  +------------------+  +----------------+  +----------------+    |  |
|  |  | PostgreSQL + AGE |  | Elasticsearch  |  | Redis          |    |  |
|  |  | (Knowledge Graph)|  | (Search Index)  |  | (Cache/Session)|    |  |
|  |  | (Relational)     |  |                |  |                |    |  |
|  |  +------------------+  +----------------+  +----------------+    |  |
|  |                                                                  |  |
|  |  +------------------+  +----------------+  +----------------+    |  |
|  |  | MinIO (S3)       |  | Apache Kafka   |  | TimescaleDB    |    |  |
|  |  | (Object Store)   |  | (Event Stream)  |  | (Time-Series)  |    |  |
|  |  +------------------+  +----------------+  +----------------+    |  |
|  +================================================================+  |
+========================================================================+
```

### 4.2 Layer Responsibilities

| Layer | Primary Responsibility | Technologies |
|---|---|---|
| Presentation | User interface, interaction, rendering | React/Next.js, Flutter, Tailwind CSS |
| API Gateway | Request routing, authentication, rate limiting, aggregation | Envoy, GraphQL Federation, Kong |
| Service Mesh | Traffic management, mTLS, observability, fault injection | Istio, Envoy |
| Application | Business logic, domain services, use case orchestration | Rust (Axum), Python (FastAPI), Go |
| Agent Platform | AI agent execution, orchestration, state management | LangChain, Ray, Custom Agent Runtime |
| Data | Data storage, caching, indexing, event streaming | PostgreSQL, AGE, Elasticsearch, Redis, Kafka, MinIO |

---

## 5. LAYER ARCHITECTURE

### 5.1 Presentation Layer

#### 5.1.1 Web Application

| Attribute | Specification |
|---|---|
| Framework | Next.js 14+ (React) with App Router |
| Language | TypeScript 5.x |
| State Management | Zustand for client state, React Query for server state |
| Styling | Tailwind CSS with custom design system |
| Accessibility | WCAG 2.1 AA, aria labels, keyboard navigation |
| Internationalization | next-intl, RTL support |
| PWA | Service workers for offline, push notifications |
| Performance | Server-side rendering, static generation, ISR |
| Testing | Jest, React Testing Library, Playwright |

#### 5.1.2 Mobile Application

| Attribute | Specification |
|---|---|
| Framework | Flutter 3.x |
| Language | Dart |
| State Management | Riverpod |
| Offline | SQLite local storage, background sync |
| Push Notifications | Firebase Cloud Messaging, APNs |
| Performance | Flutter's native compilation, deferred loading |

#### 5.1.3 Admin Dashboard

| Attribute | Specification |
|---|---|
| Framework | Next.js 14+ (separate admin app) |
| Authentication | OAuth 2.1 with MFA requirement |
| Authorization | ABAC (Attribute-Based Access Control) |
| Templating | Customizable dashboard widgets |

#### 5.1.4 Creator Studio

| Attribute | Specification |
|---|---|
| Framework | Next.js 14+ |
| Key Features | Drag-drop content assembly, preview, version management |
| Media Tools | Built-in audio/video recording, image upload, editing |

### 5.2 API Gateway Layer

#### 5.2.1 Gateway Architecture

```
                    +------------------------------------------+
                    |         Client Requests                  |
                    |  (Web, Mobile, External API, Admin)      |
                    +------------------------------------------+
                                         |
                    +------------------------------------------+
                    |         Cloud Load Balancer              |
                    |  (TLS Termination, DDoS Protection)      |
                    +------------------------------------------+
                                         |
                    +------------------------------------------+
                    |         API Gateway (Kong/Envoy)         |
                    |  - Routing                              |
                    |  - Authentication (OAuth 2.1)           |
                    |  - Rate Limiting (token bucket)         |
                    |  - Request/Response Transformation      |
                    |  - Caching                              |
                    |  - Request Logging                      |
                    |  - API Key Validation                   |
                    +------------------------------------------+
                    /                  |                      \
        +------------------+ +------------------+ +------------------+
        | REST API         | | GraphQL          | | WebSocket        |
        | (External,       | | Federation       | | (Real-time       |
        |  Admin)          | | (Complex Queries)| |  Updates)        |
        +------------------+ +------------------+ +------------------+
```

#### 5.2.2 API Versioning Strategy

| Strategy | Detail |
|---|---|
| Version in URL path | `/api/v1/`, `/api/v2/` |
| Version lifecycle | Active (full support), Deprecated (no new features, bug fixes only), Sunset (end-of-life notified 6 months in advance) |
| Minimum overlap | 2 versions supported simultaneously |
| Breaking changes | Require new version; never break backward compatibility |

#### 5.2.3 Rate Limiting

| Tier | Requests/Second | Burst | Applied To |
|---|---|---|---|
| Anonymous | 10 | 20 | Unauthenticated requests |
| Authenticated (Free) | 100 | 200 | Authenticated free users |
| Authenticated (Premium) | 1,000 | 2,000 | Premium subscribers |
| Institutional | 10,000 | 20,000 | Institutional accounts |
| API Keys (Developer) | 100 | 200 | Developer API keys |
| API Keys (Enterprise) | 10,000 | 20,000 | Enterprise API keys |
| Internal Service | Unlimited | Unlimited | Internal service mesh |

### 5.3 Application Layer

#### 5.3.1 Domain Architecture

Each domain follows the **Hexagonal Architecture** (Ports and Adapters) pattern:

```
    +--------------------------------------------------------+
    |                    DOMAIN BOUNDARY                       |
    |                                                         |
    |  +----------------+                                     |
    |  |   Domain Core   |                                     |
    |  |  (Entities,     |                                     |
    |  |   Value Objects,|                                     |
    |  |   Aggregates,   |                                     |
    |  |   Domain Events)|                                     |
    |  +--------+--------+                                     |
    |           |                                              |
    |  +--------+--------+                                     |
    |  |   Use Cases      |  (Application Services)            |
    |  |  (Ports/Interfaces)|                                  |
    |  +--------+--------+                                     |
    |           |                                              |
    |  +--------+--------+     +---------------------+         |
    |  |   Adapters       |     |  Infrastructure     |         |
    |  |  (Inbound:       |     |  (Outbound:         |         |
    |  |   Controllers,   |     |   DB, Message,      |         |
    |  |   Event Handlers,|     |   External APIs)    |         |
    |  |   gRPC Servers)  |     |                     |         |
    |  +--------+--------+     +----------+----------+         |
    +--------------------------------------------------------+
```

#### 5.3.2 Domain Catalog

| Domain | Bounded Context | Services | Data Ownership |
|---|---|---|---|
| Knowledge | Book acquisition, OCR, extraction, verification, storage | `book-ingestion-service`, `ocr-service`, `entity-extraction-service`, `fact-extraction-service`, `verification-service`, `knowledge-graph-service` | Knowledge Database (PostgreSQL + AGE) |
| Content | Content generation, assembly, publishing, rendering | `content-generation-service`, `mode-renderer-service`, `media-generation-service`, `content-publishing-service` | Content Database (PostgreSQL) |
| Learning | Learning paths, assessment, adaptation, recommendation | `learning-path-service`, `assessment-service`, `adaptive-engine-service`, `recommendation-service` | Learning Database (PostgreSQL) |
| User | Authentication, profiles, preferences, progress | `auth-service`, `user-profile-service`, `user-progress-service`, `notification-service` | User Database (PostgreSQL) |
| Analytics | Usage analytics, learning analytics, business intelligence | `usage-analytics-service`, `learning-analytics-service`, `reporting-service` | TimescaleDB, Data Warehouse |
| Admin | Configuration, monitoring, auditing, support | `config-service`, `monitoring-service`, `audit-service`, `support-service` | Admin Database (PostgreSQL) |

#### 5.3.3 Service Design Principles

| Principle | Description |
|---|---|
| Single Responsibility | Each service does one thing well |
| Data Ownership | Each service owns its data exclusively |
| Independent Deployability | Services deploy independently |
| Independent Scalability | Services scale independently |
| Technology Heterogeneity | Services can use different tech stacks |
| Fault Isolation | Service failure does not cascade |
| Team Autonomy | Teams own their services end-to-end |

### 5.4 Agent Platform Layer

#### 5.4.1 Agent Architecture Overview

```
    +===================================================================+
    |                     MASTER ORCHESTRATOR                            |
    |                                                                    |
    |  +------------------+  +-----------------+  +------------------+  |
    |  | Workflow Engine  |  | Agent Registry  |  | State Manager    |  |
    |  | (Temporal/Camunda|  | (Service        |  | (Workflow State,  |  |
    |  |  Workflow)       |  |  Discovery)      |  |  Checkpoints)    |  |
    |  +------------------+  +-----------------+  +------------------+  |
    |                                                                    |
    |  +------------------+  +-----------------+  +------------------+  |
    |  | Event Bus        |  | Task Queue      |  | Result Collector |  |
    |  | (Agent Events)   |  | (Work Assign)   |  | (Output Assembly) |  |
    |  +------------------+  +-----------------+  +------------------+  |
    +===================================================================+
                             |
    +===================================================================+
    |                     AGENT EXECUTION ENVIRONMENT                    |
    |                                                                    |
    |  +------------------+  +-----------------+  +------------------+  |
    |  | Agent Runtime    |  | Model Gateway   |  | Prompt Manager   |  |
    |  | (Ray Cluster)    |  | (LLM Router)    |  | (Template Store,  |  |
    |  |                  |  |                 |  |  Version Control) |  |
    |  +------------------+  +-----------------+  +------------------+  |
    |                                                                    |
    |  +------------------+  +-----------------+  +------------------+  |
    |  | Tool Registry    |  | Memory Manager  |  | RAG Connector    |  |
    |  | (Available Tools, |  | (Agent Memory,  |  | (Knowledge Graph |  |
    |  |  Tool Execution)  |  |  Conversation)  |  |  Integration)    |  |
    |  +------------------+  +-----------------+  +------------------+  |
    +===================================================================+
```

#### 5.4.2 Agent Types

| Agent Category | Example Agents | Execution Mode |
|---|---|---|
| Extraction Agents | OCR Agent, Entity Agent, Relation Agent, Structure Agent | Batch, Pipeline |
| Verification Agents | Cross-Reference Agent, LLM Check Agent, Contradiction Agent, Scholarly Review Agent | Batch, Pipeline |
| Content Generation Agents | Story Agent, Quiz Agent, Flashcard Agent, Podcast Agent, Documentary Agent, Timeline Agent, Maps Agent, Character Agent, Dialogue Agent, Debate Agent, Game Agent, Simulation Agent | On-Demand, Batch |
| Learning Agents | AI Teacher Agent, Adaptive Path Agent, Assessment Agent, Recommendation Agent, Progress Agent | Real-Time, Streaming |
| Quality Agents | Accuracy Check Agent, Relevance Agent, Completeness Agent, Style Agent, Bias Check Agent | Batch, On-Demand |

---

## 6. SERVICE DECOMPOSITION

### 6.1 Service Catalog

#### 6.1.1 Core Infrastructure Services

| Service ID | Service Name | Language | Description |
|---|---|---|---|
| S-INF-001 | api-gateway | Go (Envoy) | API routing, auth, rate limiting |
| S-INF-002 | service-mesh | Go (Istio) | Service communication, mTLS, telemetry |
| S-INF-003 | config-service | Go | Centralized configuration management |
| S-INF-004 | discovery-service | Go | Service discovery and health checking |
| S-INF-005 | secret-service | Go (Vault) | Secrets management and rotation |
| S-INF-006 | event-bus | Java (Kafka) | Event streaming platform |

#### 6.1.2 Knowledge Domain Services

| Service ID | Service Name | Language | Description |
|---|---|---|---|
| S-KNW-001 | book-ingestion-service | Rust | Book upload, format detection, initial processing |
| S-KNW-002 | ocr-service | Python (Rust bindings) | OCR processing for scanned documents |
| S-KNW-003 | layout-analysis-service | Python | Document layout analysis and structure extraction |
| S-KNW-004 | entity-extraction-service | Python | Named entity recognition for Islamic text |
| S-KNW-005 | relation-extraction-service | Python | Relationship extraction between entities |
| S-KNW-006 | fact-extraction-service | Python | Triplet extraction (subject-predicate-object) |
| S-KNW-007 | knowledge-graph-service | Rust | Knowledge Graph CRUD, query, traversal |
| S-KNW-008 | provenance-service | Rust | Immutable provenance chain management |
| S-KNW-009 | source-mgmt-service | Rust | Source document management and versioning |

#### 6.1.3 Verification Domain Services

| Service ID | Service Name | Language | Description |
|---|---|---|---|
| S-VRF-001 | verification-orchestrator | Python | Coordinates multi-layer verification workflow |
| S-VRF-002 | cross-reference-service | Python | Cross-references facts against trusted sources |
| S-VRF-003 | llm-check-service | Python | LLM-based consistency and plausibility checking |
| S-VRF-004 | contradiction-service | Python | Detects contradictions across knowledge sources |
| S-VRF-005 | scholarly-review-service | Python | Manages human scholarly review workflow |
| S-VRF-006 | confidence-service | Rust | Confidence scoring across verification layers |
| S-VRF-007 | continuous-verification-service | Python | Scheduled re-verification of stored knowledge |

#### 6.1.4 Content Domain Services

| Service ID | Service Name | Language | Description |
|---|---|---|---|
| S-CNT-001 | content-generation-orchestrator | Python | Coordinates multi-mode content generation |
| S-CNT-002 | story-generation-service | Python | Generates narrative content |
| S-CNT-003 | quiz-generation-service | Python | Generates quiz questions and assessments |
| S-CNT-004 | flashcard-generation-service | Python | Generates flashcards |
| S-CNT-005 | podcast-generation-service | Python | Generates podcast scripts and audio |
| S-CNT-006 | documentary-generation-service | Python | Generates documentary scripts and visuals |
| S-CNT-007 | timeline-generation-service | Python | Generates interactive timelines |
| S-CNT-008 | map-generation-service | Python | Generates interactive maps |
| S-CNT-009 | character-generation-service | Python | Generates character explorations |
| S-CNT-010 | dialogue-generation-service | Python | Generates simulated dialogues |
| S-CNT-011 | debate-generation-service | Python | Generates structured debates |
| S-CNT-012 | game-generation-service | Python | Generates educational games |
| S-CNT-013 | simulation-generation-service | Python | Generates interactive simulations |
| S-CNT-014 | visual-generation-service | Python | Generates visual learning content |
| S-CNT-015 | media-assembly-service | Python | Assembles multi-media content packages |
| S-CNT-016 | translation-service | Python | Content translation between languages |
| S-CNT-017 | tts-service | Python/Go | Text-to-speech generation |
| S-CNT-018 | image-generation-service | Python | AI image and illustration generation |

#### 6.1.5 Learning Domain Services

| Service ID | Service Name | Language | Description |
|---|---|---|---|
| S-LRN-001 | learning-path-service | Rust | Learning path management and navigation |
| S-LRN-002 | assessment-service | Python | Adaptive assessment and scoring |
| S-LRN-003 | adaptive-engine-service | Python | Real-time learning adaptation |
| S-LRN-004 | recommendation-service | Python | Content and path recommendations |
| S-LRN-005 | progress-service | Rust | Learning progress tracking |
| S-LRN-006 | ai-teacher-service | Python | Conversational AI teacher |
| S-LRN-007 | spaced-repetition-service | Rust | Spaced repetition scheduling |
| S-LRN-008 | social-learning-service | Rust | Study groups, discussions, peer learning |

#### 6.1.6 User Domain Services

| Service ID | Service Name | Language | Description |
|---|---|---|---|
| S-USR-001 | auth-service | Rust | Authentication, OAuth 2.1, OpenID Connect |
| S-USR-002 | user-service | Rust | User profiles, preferences, settings |
| S-USR-003 | subscription-service | Rust | Subscription management and billing |
| S-USR-004 | notification-service | Go | Push notifications, emails, in-app alerts |

#### 6.1.7 Analytics Domain Services

| Service ID | Service Name | Language | Description |
|---|---|---|---|
| S-ANL-001 | event-collector | Go | Collects and routes analytics events |
| S-ANL-002 | usage-analytics-service | Python | Usage pattern analysis |
| S-ANL-003 | learning-analytics-service | Python | Learning outcome analysis |
| S-ANL-004 | business-intelligence-service | Python | Business metrics and reporting |
| S-ANL-005 | a-b-testing-service | Python | Experimentation platform |

#### 6.1.8 Admin Domain Services

| Service ID | Service Name | Language | Description |
|---|---|---|---|
| S-ADM-001 | admin-api-service | Rust | Administrative API |
| S-ADM-002 | audit-service | Rust | Audit logging and compliance |
| S-ADM-003 | feature-flag-service | Go | Feature flag management |
| S-ADM-004 | maintenance-service | Rust | System maintenance operations |

### 6.2 Service Dependencies

```
                                +--------------------+
                                |   api-gateway      |
                                +--------+-----------+
                                         |
              +--------------------------+--------------------------+
              |                          |                          |
    +---------v----------+   +----------v----------+   +----------v----------+
    | auth-service       |   | user-service        |   | content-gen-orch    |
    +--------------------+   +---------------------+   +---------------------+
              |                          |                          |
    +---------v----------+   +----------v----------+   +----------v----------+
    | knowledge-graph    |   | progress-service    |   | story-gen-service   |
    +--------------------+   +---------------------+   +---------------------+
              |                                                    |
    +---------v----------+                               +----------v----------+
    | verification-orch  |                               | rag-service         |
    +--------------------+                               +---------------------+
              |                                                    |
    +---------v----------+                               +----------v----------+
    | cross-ref-service  |                               | knowledge-graph     |
    +--------------------+                               +---------------------+
```

### 6.3 Service Templates

Each service follows a standardized template:

```yaml
# Service Template (example: story-generation-service)
service:
  name: story-generation-service
  language: python
  framework: fastapi
  
  # Health
  health:
    liveness: /healthz
    readiness: /readyz
  
  # Resources
  resources:
    cpu: 2 cores
    memory: 4 GB
    gpu: optional (1 GPU for LLM inference)
  
  # Dependencies
  depends_on:
    - knowledge-graph-service (gRPC)
    - rag-service (gRPC)
    - content-generation-orchestrator (Kafka)
    - prompt-manager (gRPC)
    - model-gateway (gRPC)
  
  # Scaling
  scaling:
    min_replicas: 3
    max_replicas: 50
    target_cpu_utilization: 70%
    target_memory_utilization: 80%
    queue_based_scaling: true
  
  # Performance
  performance:
    p50_latency: < 5s
    p95_latency: < 15s
    p99_latency: < 30s
    throughput: 100 generations/minute
  
  # Observability
  observability:
    metrics_port: 9090
    tracing: opentelemetry (jaeger)
    logs: structured JSON to stdout
```

---

## 7. COMMUNICATION ARCHITECTURE

### 7.1 Communication Patterns

| Pattern | Protocol | Use Case | Example |
|---|---|---|---|
| Synchronous Request-Response | gRPC (internal), REST/GraphQL (external) | Real-time queries, CRUD operations | Get user profile, query knowledge graph |
| Asynchronous Event | Kafka | Workflow triggers, state changes, notifications | Book ingestion complete, content generation requested |
| Publish-Subscribe | Kafka | Broadcast events, fan-out | Content published event triggers multiple delivery updates |
| Command Pattern | Kafka (Command Queue) | Directed commands to specific services | Orchestrator commands agent to execute task |
| Saga Pattern | Kafka + Orchestrator | Distributed transactions with compensation | Book ingestion saga with compensation on failure |
| Streaming | gRPC Stream, Kafka Streams | Real-time continuous data | AI Teacher conversation, real-time progress updates |
| WebSocket | WSS | Real-time bidirectional communication | Live collaboration, progress notifications |

### 7.2 Inter-Service Communication

```
+--------------------+          gRPC (sync)          +--------------------+
| Service A          | ===========================> | Service B          |
| (Client)           | <=========================== | (Server)            |
+--------------------+         mTLS encrypted        +--------------------+

+--------------------+          Kafka (async)        +--------------------+
| Service A          | ====== Event/Command =======> | Service B          |
| (Producer)         |                               | (Consumer)         |
+--------------------+                               +--------------------+

+--------------------+          gRPC Stream          +--------------------+
| AI Teacher         | <=========================== | User Session       |
| Service            | ============================> | Manager            |
+--------------------+     bidirectional stream      +--------------------+
```

### 7.3 Event Catalog

#### 7.3.1 Knowledge Domain Events

| Event | Producer | Consumers | Payload |
|---|---|---|---|
| `book.uploaded` | book-ingestion-service | ocr-service, layout-analysis-service | book_id, format, size, checksum |
| `book.ocr_completed` | ocr-service | entity-extraction-service, verification-orchestrator | book_id, page_count, ocr_quality_score |
| `book.layout_analyzed` | layout-analysis-service | entity-extraction-service, fact-extraction-service | book_id, structure (TOC, chapters, sections) |
| `book.entities_extracted` | entity-extraction-service | relation-extraction-service, fact-extraction-service | book_id, entity_count, entities_list |
| `book.facts_extracted` | fact-extraction-service | knowledge-graph-service, verification-orchestrator | book_id, fact_count, facts_list |
| `book.knowledge_stored` | knowledge-graph-service | verification-orchestrator | book_id, graph_node_count, edge_count |
| `book.verification_started` | verification-orchestrator | cross-reference-service, llm-check-service | book_id, verification_id |
| `book.verification_completed` | verification-orchestrator | confidence-service, notification-service | book_id, verification_id, overall_score |
| `knowledge.updated` | knowledge-graph-service | content-generation-orchestrator, search-index-service | entity_id, change_type, affected_modes |
| `knowledge.confidence_changed` | confidence-service | content-generation-orchestrator, monitoring-service | fact_id, old_score, new_score |

#### 7.3.2 Content Domain Events

| Event | Producer | Consumers | Payload |
|---|---|---|---|
| `content.generation_requested` | content-generation-orchestrator | mode-specific-generation-service | content_id, mode_type, knowledge_refs |
| `content.generation_completed` | mode-specific-generation-service | content-generation-orchestrator, media-assembly-service | content_id, mode_type, quality_score |
| `content.published` | content-publishing-service | search-index-service, cdn-invalidation-service | content_id, publish_timestamp, locale |
| `content.updated` | content-publishing-service | search-index-service, cdn-invalidation-service | content_id, version, change_summary |

#### 7.3.3 Learning Domain Events

| Event | Producer | Consumers | Payload |
|---|---|---|---|
| `learning.session_started` | learning-path-service | progress-service, analytics-service | user_id, session_id, start_timestamp |
| `learning.assessment_completed` | assessment-service | adaptive-engine-service, progress-service, recommendation-service | user_id, assessment_id, score, skill_levels |
| `learning.path_updated` | adaptive-engine-service | learning-path-service, recommendation-service | user_id, path_id, adjustments |
| `learning.milestone_reached` | progress-service | notification-service, gamification-service | user_id, milestone, achievement |

#### 7.3.4 User Domain Events

| Event | Producer | Consumers | Payload |
|---|---|---|---|
| `user.registered` | auth-service | user-service, notification-service, analytics-service | user_id, signup_method, locale |
| `user.subscription_changed` | subscription-service | user-service, notification-service, analytics-service | user_id, new_tier, effective_date |
| `user.deleted` | user-service | all services | user_id, anonymization_token |

### 7.4 Message Schema Standards

All Kafka messages follow this standard schema:

```json
{
  "schema": {
    "type": "record",
    "name": "EventEnvelope",
    "namespace": "com.iep.event",
    "fields": [
      { "name": "event_id", "type": "string", "doc": "UUID v7 of the event" },
      { "name": "event_type", "type": "string", "doc": "Fully qualified event type" },
      { "name": "event_version", "type": "int", "doc": "Schema version" },
      { "name": "producer", "type": "string", "doc": "Service that produced the event" },
      { "name": "producer_timestamp", "type": "long", "doc": "Unix timestamp in milliseconds" },
      { "name": "correlation_id", "type": "string", "doc": "For tracing across services" },
      { "name": "causation_id", "type": "string", "doc": "ID of event that caused this event" },
      { "name": "data", "type": "bytes", "doc": "Avro/Protobuf serialized payload" },
      { "name": "data_schema", "type": "string", "doc": "Schema ID for data deserialization" }
    ]
  }
}
```

---

## 8. DATA ARCHITECTURE

### 8.1 Data Architecture Overview

```
    +===================================================================+
    |                      DATA ARCHITECTURE                             |
    |                                                                    |
    |  +------------------+  +-----------------+  +------------------+  |
    |  | TRANSACTIONAL     |  | ANALYTICAL      |  | SEARCH           |  |
    |  | DATA              |  | DATA            |  | INDEX            |  |
    |  |                   |  |                 |  |                  |  |
    |  | PostgreSQL        |  | TimescaleDB     |  | Elasticsearch    |  |
    |  | (User data,       |  | (Metrics,       |  | (Full-text       |  |
    |  |  Content,         |  |  Events,        |  |  Search Index)   |  |
    |  |  Config)          |  |  Analytics)     |  |                  |  |
    |  |                   |  |                 |  |                  |  |
    |  | PostgreSQL + AGE  |  | Data Warehouse  |  |                  |  |
    |  | (Knowledge Graph) |  | (ClickHouse)    |  |                  |  |
    |  +------------------+  +-----------------+  +------------------+  |
    |                                                                    |
    |  +------------------+  +-----------------+  +------------------+  |
    |  | OBJECT STORE     |  | CACHE           |  | EVENT STREAM     |  |
    |  |                  |  |                 |  |                  |  |
    |  | MinIO (S3)       |  | Redis           |  | Apache Kafka     |  |
    |  | (Books, Media,   |  | (Session,       |  | (Event Bus,      |  |
    |  |  Assets)         |  |  Cache,         |  |  Message Queue)  |  |
    |  |                  |  |  Rate Limiting)  |  |                  |  |
    |  +------------------+  +-----------------+  +------------------+  |
    +===================================================================+
```

### 8.2 Data Classification and Storage Mapping

| Data Category | Primary Store | Cache | Backup Strategy |
|---|---|---|---|
| Knowledge Graph | PostgreSQL + AGE | Redis (query cache) | Continuous WAL archiving |
| Source Documents | MinIO | CDN | Cross-region replication |
| User Profiles | PostgreSQL | Redis | WAL archiving |
| User Progress | PostgreSQL | Redis | WAL archiving |
| Generated Content | MinIO (media), PostgreSQL (metadata) | CDN | Cross-region replication |
| Content Metadata | PostgreSQL | Redis | WAL archiving |
| Event Logs | Kafka (hot), MinIO (cold) | - | Tiered storage |
| Time-Series Metrics | TimescaleDB | Redis (recent) | Continuous archiving |
| Search Index | Elasticsearch | - | Snapshot and restore |
| Audit Logs | PostgreSQL (hot), MinIO (cold) | - | Immutable storage |
| Configuration | PostgreSQL | Redis | Git + CI/CD |
| Session Data | Redis | - | Redis persistence + replica |
| Vector Embeddings | pgvector (PostgreSQL) | Redis (faiss index) | WAL archiving |

### 8.3 Data Flow: Book to Knowledge to Content

```
BOOK --> [OCR] --> [Entity Extraction] --> [Relation Extraction] --> [Fact Extraction]
                                                                          |
                                                                     [Verification Pipeline]
                                                                          |
                                                                     [Knowledge Graph]
                                                                          |
                                                     +--------------------+--------------------+
                                                     |                    |                    |
                                             [Content Gen]         [AI Teacher]          [Search Index]
                                                     |                    |
                                             [Media Assembly]      [RAG Pipeline]
                                                     |
                                             [CDN Delivery]
                                                     |
                                             [User Learning Session]
```

### 8.4 Data Lifecycle Management

| Stage | Duration | Storage Tier | Actions |
|---|---|---|---|
| Hot (Active) | 90 days | High-performance SSD | Full query capability, all indexes |
| Warm (Recent) | 1 year | Standard SSD | Reduced indexes, slower queries acceptable |
| Cold (Historical) | 3 years | HDD / Archive | Minimal indexes, restore for access |
| Frozen (Archive) | 7 years | Object Store (Glacier) | No direct query, restore to warm for access |
| Deletion | After retention | - | Secure erase, anonymize |

### 8.5 Data Retention Policies

| Data Type | Retention Period | Legal Basis | Deletion Method |
|---|---|---|---|
| User Account Data | Until account deletion + 90 days | GDPR right to deletion | Hard delete with anonymized audit |
| User Learning Data | 3 years after last activity | Educational record keeping | Anonymization |
| User Payment Data | 7 years | Tax/legal requirements | Tokenization (card data not stored) |
| Knowledge Graph | Permanent | Core platform asset | Never deleted, only deprecated |
| Source Documents | Permanent | Provenance requirement | Never deleted |
| Generated Content | 5 years after last access | Platform asset | Archival |
| Audit Logs | 7 years | Compliance | Append-only, never deleted |
| Analytics Events | 2 years | Business intelligence | Aggregation and deletion |
| Session Data | 24 hours | Operational | Automatic expiration |
| Chat/AI Teacher Conversations | 90 days | Learning improvement | Anonymization |

---

## 9. DEPLOYMENT ARCHITECTURE

### 9.1 Deployment Topology

```
                              +-----------------------------+
                              |   Global Load Balancer      |
                              |   (Cloudflare / AWS Global   |
                              |    Accelerator)               |
                              +-------------+---------------+
                                            |
              +-----------------------------+-----------------------------+
              |                             |                             |
    +---------v----------+       +----------v----------+       +---------v----------+
    | Region 1 (Primary) |       | Region 2 (Active)   |       | Region N (Read)    |
    | (e.g., US-East)    |       | (e.g., EU-West)     |       | (e.g., SE Asia)    |
    |                     |       |                     |       |                     |
    |  +---------------+  |       |  +---------------+  |       |  +---------------+  |
    |  | AZ 1 | AZ 2 |  |  |       |  | AZ 1 | AZ 2 |  |       |  | AZ 1 | AZ 2 |  |
    |  |  /    \     |  |  |       |  |  /    \     |  |       |  |  /    \     |  |
    |  +---------------+  |       |  +---------------+  |       |  +---------------+  |
    |                     |       |                     |       |                     |
    |  Knowledge Graph    |       |  Knowledge Graph    |       |  Knowledge Graph    |
    |  (Primary)          |<----->|  (Replica)          |<----->|  (Read Replica)     |
    |  PostgreSQL + AGE   |  async|  PostgreSQL + AGE   |  async|  PostgreSQL + AGE   |
    |                     |       |                     |       |                     |
    |  Kafka Cluster      |<----->|  Kafka Cluster      |       |  Kafka Mirror      |
    |  (Primary)          |  mirro|  (Replica)          |       |  (Read)             |
    |                     |       |                     |       |                     |
    |  MinIO (Primary)    |<----->|  MinIO (Replica)    |       |  MinIO Edge Cache   |
    +---------------------+       +---------------------+       +---------------------+
```

### 9.2 Environment Strategy

| Environment | Purpose | Configuration | Access |
|---|---|---|---|
| `dev` | Individual developer testing | Minimal resources, shared cluster | Developers |
| `staging` | Integration testing, QA | Full stack, reduced scale | Engineering team |
| `perf` | Performance and load testing | Full stack, production-like | Engineering team |
| `pre-prod` | Release candidate validation | Production mirror | QA, Product |
| `prod` | Production | Full HA, auto-scaling | Users |
| `dr` | Disaster recovery | Standby, minimal resources | Operations |

### 9.3 Kubernetes Cluster Architecture

```
    +============================================================+
    |                     KUBERNETES CLUSTER                       |
    |                                                              |
    |  +------------------------------------------------------+  |
    |  |               CONTROL PLANE                            |  |
    |  |  API Server | Scheduler | Controller Manager | etcd    |  |
    |  +------------------------------------------------------+  |
    |                                                              |
    |  +------------------------------------------------------+  |
    |  |               SYSTEM NAMESPACE                        |  |
    |  |  istio-system | kube-system | monitoring | logging    |  |
    |  |  cert-manager | ingress-nginx | external-dns         |  |
    |  +------------------------------------------------------+  |
    |                                                              |
    |  +------------------------------------------------------+  |
    |  |               DATA PLATFORM NAMESPACE                  |  |
    |  |  postgresql | kafka | redis | elasticsearch | minio   |  |
    |  +------------------------------------------------------+  |
    |                                                              |
    |  +------------------------------------------------------+  |
    |  |              AI PLATFORM NAMESPACE                    |  |
    |  |  ray-cluster | model-gateway | prompt-manager         |  |
    |  |  agent-runtime | vector-store                         |  |
    |  +------------------------------------------------------+  |
    |                                                              |
    |  +------------------------------------------------------+  |
    |  |           KNOWLEDGE DOMAIN NAMESPACE                  |  |
    |  |  book-ingestion | ocr | entity-extraction            |  |
    |  |  fact-extraction | knowledge-graph | verification     |  |
    |  +------------------------------------------------------+  |
    |                                                              |
    |  +------------------------------------------------------+  |
    |  |           CONTENT DOMAIN NAMESPACE                    |  |
    |  |  content-gen-orch | story-gen | quiz-gen              |  |
    |  |  flashcard-gen | podcast-gen | documentary-gen        |  |
    |  |  timeline-gen | map-gen | character-gen              |  |
    |  +------------------------------------------------------+  |
    |                                                              |
    |  +------------------------------------------------------+  |
    |  |           LEARNING DOMAIN NAMESPACE                   |  |
    |  |  learning-path | assessment | adaptive-engine         |  |
    |  |  recommendation | ai-teacher | progress               |  |
    |  +------------------------------------------------------+  |
    |                                                              |
    |  +------------------------------------------------------+  |
    |  |           USER / API NAMESPACE                        |  |
    |  |  api-gateway | auth | user | notification             |  |
    |  +------------------------------------------------------+  |
    |                                                              |
    |  +------------------------------------------------------+  |
    |  |           FRONTEND NAMESPACE                         |  |
    |  |  web-app | admin-dashboard | creator-studio           |  |
    |  +------------------------------------------------------+  |
    +============================================================+
```

---

## 10. SCALABILITY ARCHITECTURE

### 10.1 Scaling Dimensions

| Dimension | Strategy | Mechanism |
|---|---|---|
| Horizontal Pod Scaling | Add more replicas | HPA (CPU, memory, custom metrics) |
| Vertical Pod Scaling | Increase pod resources | VPA (for stateful components) |
| Cluster Scaling | Add more nodes | Cluster Autoscaler |
| Database Scaling | Read replicas, sharding | PostgreSQL replicas, Citus sharding |
| Queue Scaling | Partition count | Kafka partition scaling |
| Cache Scaling | Redis cluster | Redis Cluster mode |
| AI Model Scaling | Model parallelism, batching | Ray Serve, vLLM |
| Storage Scaling | Object storage, CDN | S3-compatible storage, CDN |

### 10.2 Auto-Scaling Configuration

#### 10.2.1 Stateless Services

```yaml
# Example: story-generation-service autoscaling
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: story-generation-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: story-generation-service
  minReplicas: 3
  maxReplicas: 50
  metrics:
    - type: Resource
      resource:
        name: cpu
        target:
          type: Utilization
          averageUtilization: 70
    - type: Resource
      resource:
        name: memory
        target:
          type: Utilization
          averageUtilization: 80
    - type: Pods
      pods:
        metric:
          name: kafka_consumer_lag
        target:
          type: AverageValue
          averageValue: 100
```

#### 10.2.2 Stateful Services

```yaml
# Example: knowledge-graph-service
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: knowledge-graph-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: StatefulSet
    name: knowledge-graph-service
  minReplicas: 3
  maxReplicas: 10
  metrics:
    - type: Resource
      resource:
        name: cpu
        target:
          type: Utilization
          averageUtilization: 60
    - type: Resource
      resource:
        name: memory
        target:
          type: Utilization
          averageUtilization: 70
```

### 10.3 Database Scaling Architecture

#### 10.3.1 PostgreSQL Scaling

```
                    +-----------------------------+
                    |   Connection Pooler         |
                    |   (PgBouncer / PgCat)       |
                    +-------------+---------------+
                                  |
              +-------------------+-------------------+
              |                   |                   |
    +---------v----------+ +---------v----------+ +---------v----------+
    | Write Primary      | | Read Replica 1     | | Read Replica N     |
    | (Knowledge Graph   | | (Query Service)    | | (Query Service)    |
    |  Writes,           | |                    | |                    |
    |  Critical Reads)   | |                    | |                    |
    +--------------------+ +--------------------+ +--------------------+
              |                   |                   |
              +-------------------+-------------------+
                                  |
              +-------------------+-------------------+
              |                   |                   |
    +---------v----------+ +---------v----------+ +---------v----------+
    | Shard 1            | | Shard 2            | | Shard N            |
    | (A-D Knowledge)    | | (E-L Knowledge)    | | (All Other)        |
    +--------------------+ +--------------------+ +--------------------+
```

#### 10.3.2 Sharding Strategy

| Shard | Knowledge Domain | Size Estimate | Growth Rate |
|---|---|---|---|
| Shard A | Quran, Tafsir | 500 GB | 50 GB/year |
| Shard B | Hadith (all collections) | 1 TB | 100 GB/year |
| Shard C | Fiqh, Usul al-Fiqh | 750 GB | 75 GB/year |
| Shard D | Aqeedah, Tazkiyah, Tasawwuf | 250 GB | 25 GB/year |
| Shard E | Seerah, Islamic History | 500 GB | 50 GB/year |
| Shard F | Arabic Language | 300 GB | 30 GB/year |
| Shard G | Islamic Finance, Contemporary | 200 GB | 40 GB/year |
| Shard H | Comparative Religion, Philosophy | 100 GB | 10 GB/year |
| Shard I-Z | Other disciplines | 500 GB | 50 GB/year |

### 10.4 Caching Architecture

```
                    +------------------------------------------+
                    |           User Request                   |
                    +------------------------------------------+
                                         |
              +--------------------------+--------------------------+
              |                          |                          |
    +---------v----------+    +---------v----------+    +----------v----------+
    | Browser Cache      |    | CDN Cache          |    | API Gateway Cache   |
    | (Cache-Control,    |    | (CloudFront/       |    | (Response Cache,    |
    |  ETag, Service     |    |  Cloudflare)       |    |  KV Store)          |
    |  Worker)           |    |                    |    |                     |
    +--------------------+    +--------------------+    +--------------------+
                                                                 |
                                                    +------------+------------+
                                                    |                         |
                                          +---------v----------+   +----------v----------+
                                          | Application Cache   |   | Database Cache      |
                                          | (Redis: session,    |   | (PostgreSQL Shared  |
                                          |  rate limiting,     |   |  Buffer, Query Cache,|
                                          |  API response)      |   |  Materialized Views) |
                                          +--------------------+   +--------------------+
                                                                                   |
                                                                          +---------v----------+
                                                                          | Model Cache         |
                                                                          | (LLM response cache |
                                                                          |  for repeated       |
                                                                          |  queries)           |
                                                                          +--------------------+
```

### 10.5 Cache Invalidation Strategy

| Cache Layer | Invalidation Method | TTL |
|---|---|---|
| Browser Cache | ETag, Cache-Control headers | varies by content type |
| CDN Cache | URL-based invalidation, TTL | 1 hour (static), 0 (dynamic) |
| API Gateway Cache | Key-based invalidation | 1 minute |
| Application Cache (Redis) | Event-driven invalidation | varies (minutes to hours) |
| Database Cache | PostgreSQL shared buffers | LRU eviction |
| Model Cache (LLM) | Semantic similarity-based | 24 hours or until related knowledge changes |

---

## 11. SECURITY ARCHITECTURE

### 11.1 Security Architecture Overview

```
    +===================================================================+
    |                       SECURITY ARCHITECTURE                        |
    |                                                                    |
    |  +--------------------------------------------------------------+  |
    |  |                   EDGE SECURITY                               |  |
    |  |  DDoS Protection | WAF | TLS Termination | Bot Detection     |  |
    |  +--------------------------------------------------------------+  |
    |                                                                    |
    |  +--------------------------------------------------------------+  |
    |  |                  API SECURITY                                 |  |
    |  |  Auth (OAuth 2.1) | Rate Limiting | Request Validation       |  |
    |  |  API Key Mgmt | IP Filtering | Payload Encryption            |  |
    |  +--------------------------------------------------------------+  |
    |                                                                    |
    |  +--------------------------------------------------------------+  |
    |  |                  SERVICE MESH SECURITY                        |  |
    |  |  mTLS | Service Auth | Authorization (RBAC/ABAC)             |  |
    |  |  Audit Logging | Traffic Policies | Circuit Breaking         |  |
    |  +--------------------------------------------------------------+  |
    |                                                                    |
    |  +--------------------------------------------------------------+  |
    |  |                  APPLICATION SECURITY                         |  |
    |  |  Input Validation | Output Encoding | CSRF Protection        |  |
    |  |  SQL Injection Prevention | XSS Prevention                   |  |
    |  |  Secure Session Mgmt | Secure File Upload                    |  |
    |  +--------------------------------------------------------------+  |
    |                                                                    |
    |  +--------------------------------------------------------------+  |
    |  |                  DATA SECURITY                                |  |
    |  |  Encryption at Rest (AES-256) | Key Management (Vault)      |  |
    |  |  Data Masking | Data Classification | Backup Encryption      |  |
    |  |  Audit Logging | Access Reviews | DLP                        |  |
    |  +--------------------------------------------------------------+  |
    |                                                                    |
    |  +--------------------------------------------------------------+  |
    |  |                  AI SECURITY                                  |  |
    |  |  Prompt Injection Prevention | Output Filtering              |  |
    |  |  Model Access Control | Training Data Protection             |  |
    |  |  Adversarial Robustness | Bias Detection                     |  |
    |  +--------------------------------------------------------------+  |
    +===================================================================+
```

### 11.2 Authentication Architecture

```
    +------------------+     +------------------+     +------------------+
    | User             |     | API Gateway      |     | Auth Service     |
    | (Browser/App)    |     | (OAuth 2.1       |     | (OpenID Connect  |
    |                  |     |  Client)          |     |  Provider)       |
    +--------+---------+     +--------+---------+     +--------+---------+
             |                        |                        |
             |  1. Login Request      |                        |
             |----------------------->|                        |
             |                        |  2. Redirect to Auth   |
             |                        |----------------------->|
             |                        |                        |
             |  3. Auth Form          |                        |
             |<-----------------------------------------------|
             |                        |                        |
             |  4. Credentials        |                        |
             |----------------------------------------------->|
             |                        |                        |
             |                        |  5. Auth Code          |
             |                        |<-----------------------|
             |  6. Redirect with Code |                        |
             |<-----------------------|                        |
             |                        |                        |
             |  7. Exchange Code      |                        |
             |----------------------->|                        |
             |                        |  8. Token Request      |
             |                        |----------------------->|
             |                        |                        |
             |                        |  9. ID + Access Token  |
             |                        |<-----------------------|
             | 10. Session Cookie     |                        |
             |<-----------------------|                        |
             |  + JWT Token           |                        |
             |                        |                        |
             | 11. API Request + JWT  |                        |
             |----------------------->|                        |
             |                        | 12. Validate Token     |
             |                        |----------------------->|
             |                        |                        |
             |                        | 13. User Info + Claims |
             |                        |<-----------------------|
             | 14. API Response       |                        |
             |<-----------------------|                        |
```

### 11.3 Authorization Model

| Model | Implementation | Used For |
|---|---|---|
| RBAC | Role-Based Access Control | Admin roles, team roles |
| ABAC | Attribute-Based Access Control | Fine-grained content access, data access |
| ReBAC | Relationship-Based Access Control | Content ownership, institutional hierarchy |
| PBAC | Policy-Based Access Control | Complex access policies, regulatory compliance |

### 11.4 Secret Management

```
    +--------------------------------------------------------+
    |                    VAULT (HashiCorp)                     |
    |                                                         |
    |  +------------------+  +------------------+              |
    |  | Static Secrets   |  | Dynamic Secrets  |              |
    |  | - API Keys       |  | - Database       |              |
    |  | - Service Keys   |  |   Credentials    |              |
    |  | - TLS Certs      |  | - Service        |              |
    |  | - Encryption Keys|  |   Tokens         |              |
    |  +------------------+  | - Cloud          |              |
    |                        |   Credentials    |              |
    |  +------------------+  +------------------+              |
    |  | Transit Engine   |                                   |
    |  | - Encryption as  |                                   |
    |  |   a Service      |                                   |
    |  | - Key Rotation   |                                   |
    |  +------------------+                                   |
    +--------------------------------------------------------+
```

### 11.5 Security Compliance by Component

| Component | TLS | Auth | AuthZ | Audit | Encryption |
|---|---|---|---|---|---|
| API Gateway | 1.3 min | OAuth 2.1 | RBAC | All requests | N/A |
| Service Mesh | mTLS | SPIFFE | RBAC | All RPCs | mTLS |
| PostgreSQL | mTLS | SCRAM-SHA-256 | Row-level | All queries | TDE + Column |
| Kafka | mTLS | SASL/SCRAM | ACL | All messages | Topic-level |
| Redis | mTLS | AUTH | ACL | N/A | At-rest |
| Elasticsearch | mTLS | Native | RBAC | All queries | At-rest |
| MinIO | mTLS | IAM | Bucket policy | All ops | SSE-S3 |
| All Services | 1.3 | JWT | ABAC | All API calls | N/A |

---

## 12. OBSERVABILITY ARCHITECTURE

### 12.1 Three Pillars of Observability

```
    +===================================================================+
    |                      OBSERVABILITY PLATFORM                        |
    |                                                                    |
    |  +------------------+  +-----------------+  +------------------+  |
    |  | METRICS          |  | LOGGING         |  | TRACING          |  |
    |  |                  |  |                 |  |                  |  |
    |  | Prometheus       |  | Vector          |  | OpenTelemetry    |  |
    |  | Grafana          |  | Loki            |  | Jaeger           |  |
    |  | Alertmanager     |  | Structured JSON |  | Distributed      |  |
    |  | ServiceMonitor   |  | Fluentd         |  | Tracing          |  |
    |  |                  |  |                 |  |                  |  |
    |  | Metrics Exported:|  | Log Categories: |  | Trace Structure: |  |
    |  | - RED Metrics    |  | - App Logs      |  | - Root Span      |  |
    |  | - USE Metrics    |  | - Access Logs   |  | - Child Spans    |  |
    |  | - Business       |  | - Audit Logs    |  | - Span Tags      |  |
    |  | - AI Agent       |  | - AI Agent Logs |  | - Span Events    |  |
    |  +------------------+  +-----------------+  +------------------+  |
    +===================================================================+
```

### 12.2 Metrics Architecture

#### 12.2.1 RED Metrics (Rate, Errors, Duration)

| Service | Rate Metric | Error Metric | Duration Metric |
|---|---|---|---|
| API Gateway | requests/second | error ratio | latency (P50, P95, P99) |
| All Services | requests/second | error ratio | latency (P50, P95, P99) |
| Knowledge Graph | queries/second | query error ratio | query latency |
| Content Generation | generations/second | generation error ratio | generation time |
| AI Agents | invocations/second | agent error ratio | agent execution time |
| LLM Calls | tokens/second | LLM error ratio | LLM latency |

#### 12.2.2 USE Metrics (Utilization, Saturation, Errors)

| Resource | Utilization | Saturation | Errors |
|---|---|---|---|
| CPU | CPU utilization percentage | CPU load average, run queue | CPU errors |
| Memory | Memory usage percentage | OOM kills, swap usage | Memory errors |
| Disk | Disk usage percentage | I/O wait, queue depth | Disk errors, I/O errors |
| Network | Bandwidth utilization | Connection queue, drops | Packet errors, retransmits |
| Database | Connection utilization | Query queue, lock waits | Query errors, deadlocks |

#### 12.2.3 Business Metrics

| Category | Metrics |
|---|---|
| User Activity | MAU, DAU, new signups, active sessions |
| Learning | Lessons started, completed, quiz attempts, avg score |
| Content | Books ingested, content generated, content delivered |
| Quality | Fact accuracy, error rate, verification coverage |
| Business | Revenue, subscriptions, churn, LTV, CAC |

### 12.3 Alerting Architecture

```
    +--------------------------------------------------------+
    |                    ALERTMANAGER                         |
    |                                                         |
    |  +------------------+  +------------------+              |
    |  | Alert Rules      |  | Silence Manager  |              |
    |  | (PrometheusQL)   |  | (Maintenance)    |              |
    |  +------------------+  +------------------+              |
    |                                                         |
    |  +------------------+  +------------------+              |
    |  | Routing          |  | Inhibition       |              |
    |  | (Severity,       |  | (Suppress         |              |
    |  |  Team, Channel)  |  |  dependent        |              |
    |  +------------------+  |  alerts)          |              |
    |                        +------------------+              |
    +--------------------------------------------------------+
               |               |               |
    +----------v---+   +-------v-------+   +---v-----------+
    | Critical     |   | Warning      |   | Info          |
    | PagerDuty    |   | Slack        |   | Email         |
    | SMS          |   | Email        |   | Dashboard     |
    | Phone Call   |   | Dashboard    |   |               |
    +--------------+   +--------------+   +---------------+
```

### 12.4 Alert Severity Levels

| Severity | Response Time | Channel | Definition |
|---|---|---|---|
| SEV0 | 5 minutes | Phone + SMS + Slack | Complete system outage or data loss |
| SEV1 | 15 minutes | SMS + Slack + PagerDuty | Major feature outage affecting >10% of users |
| SEV2 | 1 hour | Slack + PagerDuty | Partial feature outage, degraded performance |
| SEV3 | 8 hours | Slack + Email | Minor issues, no user impact |
| SEV4 | 24 hours | Email | Technical debt, non-critical issues |

---

## 13. RESILIENCE ARCHITECTURE

### 13.1 Failure Mode Analysis

| Component | Failure Mode | Detection | Mitigation | Recovery |
|---|---|---|---|---|
| Database (Primary) | Hardware failure, data corruption | Monitoring, replicas lag | Automatic failover to replica | Promote replica, repair primary |
| Database (Replica) | Replication lag, crash | Monitoring | Read from other replicas | Automatic restart, resync |
| Kafka Broker | Broker failure | Controller detection | Re-election, partition rebalance | Automatic recovery |
| Service (Crash) | Process crash | Kubernetes liveness probe | Automatic restart, circuit breaker | Pod restart |
| Service (Hung) | Deadlock, infinite loop | Kubernetes readiness probe | Traffic draining, restart | Pod restart |
| Network Partition | Connectivity loss | Mesh health checks | Circuit breaker, fallback | Network recovery |
| LLM API | Rate limiting, downtime | Error rate monitoring | Fallback model, retry with backoff | Provider recovery |
| CDN | Regional outage | Health checks | Failover to alternate CDN | DNS-based failover |
| AI Agent | Hallucination, timeout | Quality checks, timeout | Retry, alternative approach | Human review if repeated |

### 13.2 Resilience Patterns

| Pattern | Implementation | Applied To |
|---|---|---|
| Circuit Breaker | Istio + Application | All external API calls, LLM calls, DB calls |
| Retry with Backoff | Exponential backoff + jitter | All transient failures |
| Bulkhead | Resource limits, thread pool isolation | Per-service resource allocation |
| Timeout | Per-operation timeouts | All external calls |
| Fallback | Degraded service response | Non-critical features |
| Saga | Orchestrated compensation | Multi-step workflows |
| Dead Letter Queue | Kafka DLQ | Failed message processing |
| Health Checks | Kubernetes probes | All services |
| Graceful Degradation | Feature-level degradation | User-facing features |

### 13.3 Circuit Breaker Configuration

```yaml
# Example: LLM API call circuit breaker
circuit_breaker:
  name: llm-api-inference
  failure_threshold: 5
  success_threshold: 3
  timeout_ms: 30000
  half_open_timeout_ms: 5000
  sliding_window_size: 10
  minimum_calls: 3
  exceptions:
    - NetworkError
    - Timeout
    - RateLimitError
    - ServiceUnavailable
```

### 13.4 Retry Strategy

| Operation Type | Max Retries | Initial Backoff | Backoff Multiplier | Max Backoff |
|---|---|---|---|---|
| Database Query | 2 | 100ms | 2x | 1s |
| LLM API Call | 3 | 1s | 2x | 30s |
| OCR Processing | 2 | 5s | 3x | 60s |
| File Upload/Download | 3 | 500ms | 2x | 10s |
| External API Call | 3 | 1s | 2x | 15s |
| Kafka Publish | 3 | 100ms | 2x | 1s |

---

## 14. MULTI-TENANCY ARCHITECTURE

### 14.1 Tenant Model

| Tenant Type | Isolation Level | Data Separation | Example |
|---|---|---|---|
| Individual User | Application-level | Row-level security | Single learner |
| Family Account | Application-level | Shared account + per-user rows | Family plan |
| Institution | Schema-level | Separate schema per institution | School, university |
| Enterprise | Database-level | Separate database | Large organization |
| Publisher | Service-level | Virtual private cloud | Content publisher |

### 14.2 Row-Level Security (RLS)

All PostgreSQL tables include a `tenant_id` column with RLS policies:

```sql
-- Example RLS Policy
CREATE POLICY tenant_isolation ON knowledge_graph
  USING (tenant_id = current_setting('app.current_tenant_id') OR 
         tenant_id = '00000000-0000-0000-0000-000000000000' /* public */);
```

### 14.3 Tenant-Aware Caching

```
    +--------------------------------------------------------+
    |                    REDIS CACHE                          |
    |                                                         |
    |  Key Format: {tenant_id}:{cache_type}:{entity_id}      |
    |                                                         |
    |  Examples:                                              |
    |  - tenant_abc123:session:user_xyz                      |
    |  - tenant_def456:knowledge:entity_789                  |
    |  - global:config:system_config                          |
    |                                                         |
    |  Invalidation: tenant-specific cache flush             |
    +--------------------------------------------------------+
```

---

## 15. API ARCHITECTURE

### 15.1 API Family

| API | Protocol | Audience | Documentation |
|---|---|---|---|
| Public REST API | HTTPS/JSON | Third-party developers | OpenAPI 3.1, Developer Portal |
| Public GraphQL API | HTTPS | Complex query needs | Apollo Sandbox, Voyager |
| Internal gRPC API | HTTP2/Protobuf | Internal services | Protobuf definitions |
| Admin REST API | HTTPS/JSON | Platform administrators | OpenAPI 3.1 |
| WebSocket API | WSS | Real-time features | AsyncAPI specification |
| Webhook API | HTTPS/JSON | Event notifications | Webhook documentation |

### 15.2 API Design Standards

#### 15.2.1 REST API Standards

| Standard | Specification |
|---|---|
| Base URL | `https://api.islamicedu.com/v1/` |
| Resource Naming | Plural nouns: `/users`, `/books`, `/knowledge` |
| Actions | HTTP verbs: GET, POST, PUT, PATCH, DELETE |
| Pagination | Cursor-based: `?cursor=abc&limit=20` |
| Filtering | Query parameters: `?status=published&category=fiqh` |
| Sorting | Sort parameter: `?sort=-created_at` (negative for DESC) |
| Partial Response | Fields parameter: `?fields=id,title,summary` |
| Error Format | Consistent JSON: `{error: {code, message, details}}` |
| Versioning | URL path: `/v1/` |
| Rate Limiting | Headers: X-RateLimit-* |

#### 15.2.2 gRPC API Standards

| Standard | Specification |
|---|---|
| Proto Version | Proto3 |
| Package Naming | `com.iep.{domain}.{service}` |
| Service Naming | PascalCase |
| Method Naming | VerbNoun: `GetBook`, `ExtractFacts` |
| Message Naming | PascalCase, Request/Response pairs |
| Error Handling | gRPC status codes + details |
| Streaming | Server-streaming for large results |
| Health | Standard gRPC Health Check |

### 15.3 API Versioning Lifecycle

```
    v1 (Active) ======> v2 (Active) ======> v3 (Active)
         |                   |                   |
         | [6 months]        | [6 months]        |
         v                   v                   v
    v1 (Deprecated)     v2 (Deprecated)     v3 (Active)
         |                   |                   
         | [6 months]        |                   
         v                   v                  
    v1 (Sunset)         v2 (Deprecated)      
                            |
                            | [6 months]
                            v
                       v2 (Sunset)
```

---

## 16. EVENT-DRIVEN ARCHITECTURE

### 16.1 Event Bus Topology

```
    +===================================================================+
    |                       KAFKA CLUSTER                                |
    |                                                                    |
    |  +------------------+  +-----------------+  +------------------+  |
    |  | Knowledge Topics |  | Content Topics  |  | Learning Topics  |  |
    |  |                  |  |                 |  |                  |  |
    |  | book.uploaded    |  | content.request |  | session.started  |  |
    |  | ocr.completed    |  | content.generated|  | quiz.completed   |  |
    |  | entities.extracted|  | content.published|  | path.updated    |  |
    |  | facts.extracted  |  | media.assembled |  | milestone.reached|  |
    |  | knowledge.stored |  +-----------------+  +------------------+  |
    |  | verification.done|                                          |
    |  +------------------+  +-----------------+  +------------------+  |
    |  | User Topics      |  | Analytics Topics|  | System Topics    |  |
    |  |                  |  |                 |  |                  |  |
    |  | user.registered  |  | event.collected |  | config.changed   |  |
    |  | user.subscribed  |  | metric.reported |  | alert.triggered  |  |
    |  | user.deleted     |  | report.generated|  | heartbeat        |  |
    |  +------------------+  +-----------------+  +------------------+  |
    |                                                                    |
    |  Topic naming: {domain}.{entity}.{action}                         |
    |  Partitioning: key-based (entity_id)                               |
    |  Retention: 7 days (hot), 30 days (warm), archive to MinIO (cold)|
    +===================================================================+
```

### 16.2 Event Processing Guarantees

| Guarantee | Implementation |
|---|---|
| At-Least-Once Delivery | Producer acks=all, consumer auto-commit after processing |
| Ordering | Same partition = ordered. Key-based partitioning ensures entity-level ordering |
| Exactly-Once Semantics | Idempotent producers + transactional consumers for critical paths |
| Dead Letter Queue | Failed messages routed to DLQ topic with error metadata |
| Poison Message Handling | Automatic DLQ after 3 retries with circuit breaker |

### 16.3 Event Sourcing for Critical Workflows

Workflows that require full auditability use event sourcing:

| Workflow | Event Stream | Rebuild Function |
|---|---|---|
| Book Ingestion | `book-ingestion-{book_id}` | Rebuild book processing state |
| Knowledge Verification | `verification-{fact_id}` | Rebuild verification chain |
| User Learning Journey | `learning-{user_id}` | Rebuild user learning state |

---

## 17. AI AGENT PLATFORM ARCHITECTURE

### 17.1 Agent Platform Overview

```
    +===================================================================+
    |                     AI AGENT PLATFORM                              |
    |                                                                    |
    |  +--------------------------------------------------------------+  |
    |  |                  MASTER ORCHESTRATOR                          |  |
    |  |  (Detailed in IEP-ORCH-001)                                  |  |
    |  +--------------------------------------------------------------+  |
    |                                                                    |
    |  +--------------------------------------------------------------+  |
    |  |                  MODEL GATEWAY                                |  |
    |  |                                                               |  |
    |  |  +------------------+  +-----------------+  +---------------+  |  |
    |  |  | LLM Router       |  | Model Cache     |  | Cost Tracker  |  |  |
    |  |  | (Multi-provider, |  | (Response,      |  | (Token Usage,  |  |  |
    |  |  |  Model Selection,|  |  Embedding)     |  |  Cost, Budget) |  |  |
    |  |  |  Fallback)       |  |                 |  |               |  |  |
    |  |  +------------------+  +-----------------+  +---------------+  |  |
    |  |                                                               |  |
    |  |  Model Providers Supported:                                   |  |
    |  |  - OpenAI (GPT-4o, GPT-4o-mini)                               |  |
    |  |  - Anthropic (Claude 3.5 Sonnet, Opus)                        |  |
    |  |  - Google (Gemini 1.5 Pro, Flash)                             |  |
    |  |  - Open Source (Llama 3, Mistral, Qwen via vLLM/Bedrock)    |  |
    |  |  - Fine-tuned Models (Custom Islamic NER, Classifiers)       |  |
    |  +--------------------------------------------------------------+  |
    |                                                                    |
    |  +--------------------------------------------------------------+  |
    |  |                  PROMPT MANAGEMENT                            |  |
    |  |                                                               |  |
    |  |  +------------------+  +-----------------+  +---------------+  |  |
    |  |  | Prompt Store     |  | Template Engine |  | Version       |  |  |
    |  |  | (Versioned,      |  | (Jinja2/        |  | Control       |  |  |
    |  |  |  Parameterized)   |  |  LangChain Hub) |  | (Git-based)   |  |  |
    |  |  +------------------+  +-----------------+  +---------------+  |  |
    |  +--------------------------------------------------------------+  |  |
    |                                                                    |
    |  +--------------------------------------------------------------+  |
    |  |                  AGENT EXECUTION (Ray Cluster)                |  |
    |  |                                                               |  |
    |  |  +------------------+  +-----------------+  +---------------+  |  |
    |  |  | Agent Worker Pool|  | Task Scheduler  |  | State Store   |  |  |
    |  |  | (Autoscaling     |  | (DAG Execution, |  | (Workflow      |  |  |
    |  |  |  Workers)        |  |  Dependency     |  |  State,        |  |  |
    |  |  |                  |  |  Resolution)    |  |  Checkpoints)  |  |  |
    |  |  +------------------+  +-----------------+  +---------------+  |  |
    |  +--------------------------------------------------------------+  |  |
    |                                                                    |
    |  +--------------------------------------------------------------+  |
    |  |                  RAG INFRASTRUCTURE                           |  |
    |  |                                                               |  |
    |  |  +------------------+  +-----------------+  +---------------+  |  |
    |  |  | Vector Store     |  | Retriever       |  | Context       |  |  |
    |  |  | (pgvector)       |  | (Semantic,      |  | Builder       |  |  |
    |  |  |                  |  |  Keyword,        |  | (Facts,       |  |  |
    |  |  |                  |  |  Hybrid)         |  |  Citations)   |  |  |
    |  |  +------------------+  +-----------------+  +---------------+  |  |
    |  +--------------------------------------------------------------+  |
    +===================================================================+
```

### 17.2 Model Selection Strategy

| Task | Primary Model | Fallback | Cost Tier | Reasoning |
|---|---|---|---|---|
| Content Generation (Story, Documentary) | GPT-4o | Claude 3.5 Sonnet | High | Creative quality, instruction following |
| Quiz/Flashcard Generation | GPT-4o-mini | Claude Haiku | Medium | Good enough for structured output |
| Entity Extraction (NER) | Fine-tuned Llama 3 | GPT-4o-mini | Medium | Domain-specific accuracy |
| Fact Verification | Claude 3.5 Opus | GPT-4o | High | Critical accuracy requirement |
| Translation | Gemini 1.5 Pro | GPT-4o | Medium | Multi-language strength |
| AI Teacher Conversation | GPT-4o-mini (streaming) | Claude Haiku | Pay-per-use | Low latency requirement |
| Contradiction Detection | Fine-tuned RoBERTa | GPT-4o-mini | Low | Specialized NLI model |
| Summarization | Claude 3.5 Sonnet | GPT-4o | Medium | Long context handling |
| Image Generation | DALL-E 3 / Stable Diffusion | Midjourney | Medium | Visual quality |
| Text-to-Speech | ElevenLabs | Amazon Polly | Medium | Natural voice quality |

### 17.3 LLM Cost Management

| Strategy | Implementation |
|---|---|
| Model Tiering | Use cheapest capable model for each task |
| Response Caching | Cache identical or semantically similar requests |
| Prompt Optimization | Minimize token usage in prompts |
| Batch Processing | Batch similar requests for efficiency |
| Local Models | Use open-source models for non-critical tasks |
| Context Window Management | Retrieve only necessary context |
| Streaming | Stream responses to reduce perceived latency |
| Budget Tracking | Per-service, per-user, per-model cost tracking |

---

## 18. KNOWLEDGE GRAPH ARCHITECTURE

### 18.1 Knowledge Graph Overview

```
    +===================================================================+
    |                    KNOWLEDGE GRAPH ARCHITECTURE                    |
    |                                                                    |
    |  Storage: PostgreSQL + Apache AGE                                  |
    |  Graph Model: Labeled Property Graph                               |
    |  Query Language: openCypher (via AGE) + SQL                        |
    |                                                                    |
    |  Node Types:                                                       |
    |  - Concept (abstract ideas: Tawheed, Salah, Iman)                  |
    |  - Entity (specific things: a person, place, book)                 |
    |  - Fact (assertion: "Zakat is obligatory")                         |
    |  - Source (document, page, paragraph reference)                    |
    |  - Event (specific occurrence: Battle of Badr)                     |
    |  - Location (geographic place: Mecca, Medina)                      |
    |  - Time (temporal reference: 622 CE, 2 AH)                         |
    |  - ScholarlyPosition (specific scholar's view on topic)            |
    |                                                                    |
    |  Edge Types:                                                       |
    |  - is_a, part_of, relates_to, located_in                           |
    |  - occurs_at, occurs_in, authored_by                               |
    |  - states, supports, contradicts, refines                          |
    |  - derived_from, cites, translates_of                              |
    |  - prerequisite_for, teaches, applies_to                           |
    +===================================================================+
```

### 18.2 Graph Data Model

```
    (Salah) -[:is_a]-> (Ibadah)
    (Salah) -[:has_condition]-> (Wudu)
    (Salah) -[:has_pillar]-> (Ruku)
    (Salah) -[:obligatory_for]-> (Adult_Muslim)
    (Salah) -[:cites]-> (Quran_2_43) -[:in_book]-> (Quran)
    (Salah) -[:cites]-> (Hadith_Bukhari_1) -[:in_book]-> (Sahih_Bukhari)
    (Salah) -[:scholarly_disagreement]-> (Ruling_on_missing_prayer)
    
    (Imam_Abu_Hanifa) -[:states]-> (Position_Hanafi_Salah)
    (Imam_Malik) -[:states]-> (Position_Maliki_Salah)
    (Position_Hanafi_Salah) -[:differs_from]-> (Position_Maliki_Salah) -[:on]-> (Hand_position_in_Salah)
    
    (Battle_of_Badr) -[:occurs_at]-> (Badr)
    (Battle_of_Badr) -[:occurs_in]-> (2_AH)
    (Battle_of_Badr) -[:involved]-> (Prophet_Muhammad)
    (Battle_of_Badr) -[:involved]-> (Abu_Jahl)
```

### 18.3 Graph Partitioning Strategy

| Partition | Domain | Nodes (est.) | Edges (est.) |
|---|---|---|---|
| `graph_quran` | Quran, Tafsir | 5M | 50M |
| `graph_hadith` | Hadith, Mustalah | 10M | 100M |
| `graph_fiqh` | Fiqh, Usul, Maqasid | 3M | 30M |
| `graph_aqeedah` | Aqeedah, Tazkiyah | 2M | 15M |
| `graph_history` | Seerah, Islamic History | 4M | 40M |
| `graph_bio` | Biographies, Scholars | 1M | 10M |
| `graph_language` | Arabic Language | 2M | 5M |
| `graph_contemporary` | Contemporary Issues | 1M | 8M |

---

## 19. CONTENT DELIVERY ARCHITECTURE

### 19.1 Content Delivery Flow

```
    +------------------+     +------------------+     +------------------+
    | Content          |     | Media Assembly   |     | CDN              |
    | Generation       |---->| (Package,        |---->| (CloudFront,     |
    | Service          |     |  Optimize,        |     |  Cloudflare)     |
    +------------------+     |  Format)          |     +------------------+
                             +------------------+           |
                                                             v
    +------------------+     +------------------+     +------------------+
    | User Request     |---->| Delivery Service |<----| Edge Cache       |
    | (Web/Mobile/API) |     | (Session,        |     | (Regional POP)   |
    +------------------+     |  Personalization) |     +------------------+
                             +------------------+
```

### 19.2 Content Format Strategy

| Content Type | Format | Compression | Adaptive Bitrate |
|---|---|---|---|
| Text | JSON, HTML, Markdown | Brotli, Gzip | N/A |
| Audio (Podcast) | MP3, AAC, Opus | Native | 64-320 Kbps |
| Video (Documentary) | HLS, MPEG-DASH | H.265/AV1 | 360p-4K |
| Images | WebP, AVIF, JPEG-XL | Native | Progressive loading |
| Interactive (Timeline, Map) | JSON + WASM | Brotli | Level of detail |
| 3D/Models | glTF, USDZ | Draco compression | LOD |

---

## 20. SEARCH ARCHITECTURE

### 20.1 Search Architecture Overview

```
    +===================================================================+
    |                       SEARCH ARCHITECTURE                          |
    |                                                                    |
    |  +--------------------------------------------------------------+  |
    |  |                   SEARCH INDEXING PIPELINE                    |  |
    |  |                                                               |  |
    |  |  Source --> Text Extraction --> Tokenization --> Indexing     |  |
    |  |           --> Embedding Generation --> Vector Index           |  |
    |  |                                                               |  |
    |  |  Languages: Arabic (stemmed), English, + 28 others           |  |
    |  |  Analyzers: Arabic (custom), Standard, N-Gram                |  |
    |  +--------------------------------------------------------------+  |
    |                                                                    |
    |  +--------------------------------------------------------------+  |
    |  |                   SEARCH QUERY PIPELINE                       |  |
    |  |                                                               |  |
    |  |  Query --> Query Understanding --> Intent Classification      |  |
    |  |        --> Query Expansion --> Hybrid Search                  |  |
    |  |        --> Reranking --> Result Assembly                      |  |
    |  |                                                               |  |
    |  |  Search Types:                                                |  |
    |  |  - Full-Text Search (Elasticsearch BM25)                      |  |
    |  |  - Semantic Search (pgvector cosine similarity)               |  |
    |  |  - Hybrid Search (weighted combination)                       |  |
    |  |  - Knowledge Graph Search (Cypher traversal)                  |  |
    |  |  - Cross-Lingual Search (query in language A, results in B)   |  |
    |  +--------------------------------------------------------------+  |
    +===================================================================+
```

### 20.2 Search Index Structure

| Index Name | Content | Size Estimate | Update Frequency |
|---|---|---|---|
| `books` | Book metadata, TOC | 10 GB (100K books) | Daily |
| `knowledge` | Facts, concepts, entities | 500 GB | Continuous |
| `content` | Generated content | 1 TB | Continuous |
| `users` | User profiles (admin only) | 10 GB | Real-time |
| `verses` | Quran verses, translations | 5 GB | Static |
| `hadith` | Hadith texts, translations | 50 GB | Static |
| `scholars` | Scholar biographies | 1 GB | Weekly |

---

## 21. STORAGE ARCHITECTURE

### 21.1 Storage Tier Architecture

| Tier | Technology | Capacity (Initial) | Capacity (Year 5) | Use Case |
|---|---|---|---|---|
| Hot (SSD) | NVMe SSDs on Kubernetes nodes | 10 TB | 100 TB | PostgreSQL, Redis, Active data |
| Warm (HDD) | Standard cloud volumes | 50 TB | 500 TB | MinIO, Cold PostgreSQL data |
| Cold (Archive) | S3 Glacier / Deep Archive | 100 TB | 1 PB | Backups, logs, rarely accessed content |
| Object (Content) | MinIO / S3 Standard | 50 TB | 10 PB | Books, media, generated content |
| Cache (Memory) | Redis Cluster | 256 GB | 1 TB | Hot cache, sessions, rate limiting |

### 21.2 Storage Sizing Estimates

| Data Category | Year 1 | Year 3 | Year 5 | Growth Factor |
|---|---|---|---|---|
| Knowledge Graph | 500 GB | 5 TB | 25 TB | 5x/year |
| Source Documents | 2 TB | 20 TB | 100 TB | 5x/year |
| Generated Content | 5 TB | 100 TB | 1 PB | 10x/year |
| User Data | 50 GB | 500 GB | 2.5 TB | 2x/year |
| User Learning Data | 100 GB | 2 TB | 10 TB | 5x/year |
| Logs and Events | 10 TB | 100 TB | 500 TB | 5x/year |
| Analytics Data | 5 TB | 50 TB | 250 TB | 5x/year |
| Search Index | 500 GB | 5 TB | 25 TB | 5x/year |
| Backups | 10 TB | 100 TB | 500 TB | 5x/year |

---

## 22. CACHING ARCHITECTURE

### 22.1 Multi-Layer Caching

| Layer | Technology | Cache Type | Hit Rate Target | TTL |
|---|---|---|---|---|
| L1 Browser | Cache-Control, ETag | HTTP cache | 30% | Varies |
| L2 CDN | CloudFront/Cloudflare | Edge cache | 40% | 1-24 hours |
| L3 API Gateway | Redis Cluster | Response cache | 20% | 1-60 minutes |
| L4 Application | Redis Cluster | Object cache | 30% | 5-60 minutes |
| L5 Database | PostgreSQL shared buffers | Page cache | 50% | LRU |
| L6 Model | Redis + FAISS | LLM response cache | 30% | 24 hours |

### 22.2 Cache Key Design

```
    Format: {domain}:{entity}:{id}:{context}:{locale}
    
    Examples:
    knowledge:fact:fact_123:en-US
    content:story:story_456:ar:beginner
    user:session:sess_789
    model:llm:gpt4o:prompt_hash_abc123
```

### 22.3 Cache Invalidation

| Trigger | Invalidation Method | Scope |
|---|---|---|
| Knowledge Update | Event-driven invalidation | All related content caches |
| Content Update | Direct invalidation | Specific content cache |
| User Data Change | Direct invalidation | User-specific caches |
| Scheduled Expiry | TTL-based | All caches |
| Manual (Admin) | Pattern-based invalidation | Bulk invalidation |

---

## 23. EDGE COMPUTING ARCHITECTURE

### 23.1 Edge Capabilities

| Capability | Implementation | Benefit |
|---|---|---|
| Authentication | JWT verification at edge | Reduce auth service load |
| Static Asset Delivery | CDN edge caching | Low-latency asset delivery |
| API Response Caching | CDN worker cache | Reduce origin load |
| Request Throttling | Edge rate limiting | DDoS mitigation |
| Geo-Location Routing | DNS-based routing | Low-latency region selection |
| A/B Testing | Edge worker routing | Zero-latency experimentation |
| Personalization Edge | Edge key-value store | Fast user context retrieval |

### 23.2 Edge Worker (Cloudflare Workers / Service Workers)

```javascript
// Example: Edge worker for API routing and caching
async function handleRequest(request) {
  const url = new URL(request.url);
  const cacheKey = request.url;
  const cache = caches.default;
  
  // Check cache
  let response = await cache.match(cacheKey);
  if (response) return response;
  
  // Authentication check
  const user = await authenticate(request);
  if (!user && requiresAuth(url)) {
    return new Response('Unauthorized', { status: 401 });
  }
  
  // Region-based routing
  const region = request.cf?.region;
  const backend = getBackendForRegion(region);
  
  // Forward request
  response = await fetch(backend + url.pathname + url.search, {
    headers: request.headers,
    cf: { cacheTtl: 3600 }
  });
  
  // Cache if cacheable
  if (response.status === 200 && isCacheable(url)) {
    const clonedResponse = new Response(response.body, response);
    clonedResponse.headers.set('Cache-Control', 'public, max-age=3600');
    await cache.put(cacheKey, clonedResponse);
  }
  
  return response;
}
```

---

## 24. OFFLINE ARCHITECTURE

### 24.1 Offline Capability Overview

| Feature | Online | Offline | Sync Strategy |
|---|---|---|---|
| Book Access | Full library | Downloaded books | Manual download |
| Content Access | All modes | Downloaded content | Manual download |
| Flashcards | Cloud sync | Local execution | Periodic sync |
| AI Teacher | Full | Limited (rule-based) | N/A |
| Progress | Server track | Local track | Sync on reconnection |
| Quizzes | Full | Local questions | Download before offline |
| Podcast/Audio | Streamed | Downloaded | Manual download |
| User Profile | Server | Cached | Pre-cached |

### 24.2 Offline Storage (Mobile/PWA)

```
    +--------------------------------------------------------+
    |                    OFFLINE STORE                         |
    |                                                         |
    |  +------------------+  +------------------+              |
    |  | Local DB (SQLite)|  | File Store (FS)  |              |
    |  |                  |  |                  |              |
    |  | - User Profile   |  | - Audio Files    |              |
    |  | - Downloaded     |  | - Video Files    |              |
    |  |   Progress       |  | - Images         |              |
    |  | - Queued Actions |  | - Books (PDF)    |              |
    |  | - Flashcard      |  +------------------+              |
    |  |   Scheduled      |                                   |
    |  +------------------+  +------------------+              |
    |  | Sync Queue       |  | Service Worker   |              |
    |  |                  |  | Cache            |              |
    |  | - Pending        |  |                  |              |
    |  |   Submissions    |  | - App Shell      |              |
    |  | - Pending        |  | - API Responses  |              |
    |  |   Downloads      |  | - Static Assets  |              |
    |  +------------------+  +------------------+              |
    +--------------------------------------------------------+
```

### 24.3 Sync Strategy

| Data Type | Sync Direction | Conflict Resolution | Priority |
|---|---|---|---|
| User Progress | Bidirectional | Last-write-wins (server preferred) | High |
| Quiz Results | Local to Server | Server receives all | High |
| Bookmarks | Bidirectional | Merge (no conflict) | Medium |
| Notes | Bidirectional | Last-write-wins | Medium |
| Downloaded Content | Server to Local | Replace on new version | Low |
| Settings | Bidirectional | Last-write-wins (user preferred) | Low |

---

## 25. DISASTER RECOVERY ARCHITECTURE

### 25.1 DR Strategy

| Scenario | RTO | RPO | Strategy |
|---|---|---|---|
| Single AZ Failure | 5 minutes | 0 (synchronous) | Active-active across AZs |
| Single Region Failure | 15 minutes | 1 minute | Active-passive cross-region |
| Database Corruption | 30 minutes | 5 minutes | Point-in-time recovery |
| Full System Failure | 4 hours | 15 minutes | DR region activation |
| Data Center Destruction | 4 hours | 15 minutes | Cross-region failover |
| Ransomware / Cyber Attack | 24 hours | 24 hours (backup) | Air-gapped backup restore |

### 25.2 DR Runbook (Abbreviated)

```
DR ACTIVATION - REGION FAILURE
================================

1. DETECT
   - Monitoring alerts: Region health check failed (3 consecutive)
   - PagerDuty SEV0 notified
   - On-call engineer acknowledges within 5 minutes

2. ASSESS
   - Confirm region failure (not monitoring false positive)
   - Determine impact: services, users, data affected
   - Decision: failover or wait?

3. DECIDE
   - If RTO at risk: initiate failover
   - Authority: Head of Operations or on-call SRE Lead

4. FAILOVER
   a. DNS switch: Global load balancer to DR region
   b. Database promote: Promote DR replica to primary
   c. Kafka: Mirror maker resume, verify offset
   d. MinIO: Verify cross-region replication
   e. Services: Scale up DR region services
   f. Verify: Smoke tests pass

5. COMMUNICATE
   - Internal: Status page update, Slack incident channel
   - External: User-facing status page, social media if widespread

6. RECOVER
   - After primary region restored:
   a. Reverse replication setup
   b. Controlled failback during maintenance window
   c. Verify data consistency
   d. Resume normal operations

7. POST-MORTEM
   - Root cause analysis within 48 hours
   - Action items tracked in incident management system
   - DR drill scheduled within 30 days
```

---

## 26. ARCHITECTURE DECISION RECORDS

### 26.1 ADR Index

| ADR ID | Title | Status | Section Reference |
|---|---|---|---|
| ADR-001 | Knowledge-Truth Separation Principle | Accepted | 2.3 |
| ADR-002 | PostgreSQL + Apache AGE for Knowledge Graph | Accepted | 18.1 |
| ADR-003 | Kafka as Central Event Bus | Accepted | 16.1 |
| ADR-004 | Kubernetes as Container Orchestrator | Accepted | 9.3 |
| ADR-005 | Multi-Provider LLM Abstraction Layer | Accepted | 17.2 |
| ADR-006 | Rust for Performance-Critical Services | Accepted | 6.1.1 |
| ADR-007 | Flutter for Mobile Applications | Accepted | 5.1.2 |
| ADR-008 | Istio Service Mesh | Accepted | 5.2 |
| ADR-009 | MinIO for Object Storage | Accepted | 21.1 |
| ADR-010 | OpenTelemetry for Observability | Accepted | 12.1 |
| ADR-011 | Zero Trust Security Architecture | Accepted | 11.1 |
| ADR-012 | Event Sourcing for Critical Workflows | Accepted | 16.3 |
| ADR-013 | RAG-based Content Generation | Accepted | 17.1 |
| ADR-014 | Row-Level Security for Multi-Tenancy | Accepted | 14.2 |
| ADR-015 | Progressive Web App for Offline | Accepted | 24.1 |

### 26.2 Sample ADR

```markdown
# ADR-002: PostgreSQL + Apache AGE for Knowledge Graph

## Status
Accepted

## Context
The platform requires a knowledge graph that supports:
- Graph traversal queries (complex Islamic knowledge relationships)
- Relational queries (user data, content metadata, configuration)
- ACID transactions (critical for knowledge verification)
- Strong consistency (knowledge accuracy requirements)
- Integration with existing PostgreSQL ecosystem (pgvector, TimescaleDB)
- Mature tooling and operational experience

Options considered:
1. Neo4j (dedicated graph database)
2. PostgreSQL + Apache AGE (graph extension)
3. Amazon Neptune (managed graph)
4. MongoDB + custom graph layer
5. Apache TinkerPop/Gremlin on JanusGraph

## Decision
Use PostgreSQL 16 with Apache AGE extension as the primary knowledge graph storage.

## Rationale
- Single database engine reduces operational complexity
- PostgreSQL provides ACID compliance, strong consistency, mature replication
- AGE provides openCypher graph queries directly in PostgreSQL
- pgvector integration for vector embeddings (RAG pipeline)
- Reuse existing PostgreSQL expertise in the team
- No separate graph database to manage, backup, replicate
- Neo4j reserved for heavy analytical graph queries (optional)

## Consequences
Positive:
- Reduced operational complexity (one primary database system)
- ACID transactions across graph and relational data
- Strong consistency model appropriate for Islamic knowledge
- Integration with existing PostgreSQL tooling

Negative:
- Graph query performance may be lower than dedicated graph databases
- AGE has smaller community than Neo4j
- Some advanced graph algorithms may need external processing

Mitigation:
- Neo4j can be added as supplementary analytics store if needed
- Heavy graph traversal can be cached in Redis
- AGE community growing with active development

## Date
2026-07-07
```

---

## 27. FUTURE ARCHITECTURAL EVOLUTION

### 27.1 Planned Evolutions

| Evolution | Timeline | Description |
|---|---|---|
| Federated Knowledge Graph | Year 3 | Allow third-party knowledge graphs to connect via federation protocol |
| Real-Time Collaboration | Year 3 | Real-time collaborative learning sessions using WebRTC |
| Advanced Analytics Pipeline | Year 2 | Stream processing with Flink for real-time analytics |
| Multi-Modal AI Models | Year 3 | Native multi-modal models (text + image + audio) for richer content |
| Personalization at Edge | Year 3 | Edge-based ML inference for low-latency personalization |
| Quantum-Resistant Crypto | Year 4 | Post-quantum cryptography for provenance verification |
| AR/VR Platform | Year 4 | Augmented and virtual reality learning experiences |
| Autonomous Content Curation | Year 4 | AI-driven content discovery and curation without human intervention |
| Cross-Platform Agent Runtime | Year 4 | Agent portability across different computing environments |

### 27.2 Deprecation Paths

| Component | Timeline | Replacement | Migration Strategy |
|---|---|---|---|
| Legacy REST v1 API | Year 3 deprecation, Year 4 sunset | GraphQL primary API | Header-based redirect with migration guide |
| Python ML Services (monolithic) | Year 2 modularization | Microservices per capability | Strangler pattern |
| Initial Neo4j deployment | Year 2 migration | PostgreSQL + AGE | Dual-write during migration |

---

## 28. APPENDICES

### Appendix A: Technology Stack Summary

| Category | Technology | Version | Purpose |
|---|---|---|---|
| Frontend | Next.js | 14+ | Web application framework |
| Mobile | Flutter | 3.x | Cross-platform mobile apps |
| Backend (Rust) | Axum | Latest | Performance-critical services |
| Backend (Python) | FastAPI | Latest | AI/ML services |
| Backend (Go) | Gin | Latest | Infrastructure services |
| Graph DB | Apache AGE | Latest | Knowledge graph (PostgreSQL extension) |
| Vector DB | pgvector | Latest | Embeddings similarity search |
| Relational DB | PostgreSQL | 16.x | Primary data store |
| Time-Series | TimescaleDB | Latest | Metrics and analytics |
| Search | Elasticsearch | 8.x | Full-text search |
| Cache | Redis | 7.x | Caching, sessions, rate limiting |
| Event Bus | Apache Kafka | 3.x | Event streaming |
| Object Storage | MinIO | Latest | S3-compatible storage |
| Container | Docker | Latest | Containerization |
| Orchestration | Kubernetes | 1.29+ | Container orchestration |
| Service Mesh | Istio | 1.21+ | Service communication |
| CI/CD | ArgoCD | 2.x | GitOps deployment |
| Infrastructure | Terraform | 1.7+ | Infrastructure as code |
| Monitoring | Prometheus + Grafana | Latest | Metrics and dashboards |
| Logging | Loki + Vector | Latest | Log aggregation |
| Tracing | OpenTelemetry + Jaeger | Latest | Distributed tracing |
| Secret Management | HashiCorp Vault | 1.16+ | Secrets and encryption |
| LLM Gateway | Custom | - | Multi-provider LLM routing |
| Agent Runtime | Ray | Latest | Distributed AI execution |
| Workflow | Temporal | Latest | Workflow orchestration |

### Appendix B: Port Mapping

| Service | Port (Internal) | Port (External) | Protocol |
|---|---|---|---|
| API Gateway | 8080 | 443 | HTTPS |
| gRPC Gateway | 9090 | - | HTTP2/gRPC |
| WebSocket Gateway | 8081 | 443 | WSS |
| Prometheus | 9090 | - | HTTP |
| Grafana | 3000 | - | HTTP |
| Jaeger | 16686 | - | HTTP |
| PostgreSQL | 5432 | - | PostgreSQL |
| Redis | 6379 | - | Redis |
| Kafka | 9092 | - | Kafka |
| Elasticsearch | 9200 | - | HTTP |
| MinIO | 9000 | - | S3 API |
| Vault | 8200 | - | HTTP |

### Appendix C: Network Requirements

| Network | CIDR (Example) | Purpose |
|---|---|---|
| Production VPC | 10.0.0.0/16 | Production workloads |
| Staging VPC | 10.1.0.0/16 | Staging workloads |
| Dev VPC | 10.2.0.0/16 | Development workloads |
| Management VPC | 10.3.0.0/16 | Admin and CI/CD |
| Database Subnet | 10.0.1.0/24 | Private, no internet access |
| Application Subnet | 10.0.2.0/24 | Private app tier |
| Public Subnet | 10.0.3.0/24 | Load balancers, gateways |
| Cache Subnet | 10.0.4.0/24 | Redis cluster |
| Queue Subnet | 10.0.5.0/24 | Kafka cluster |

### Appendix D: Service Level Indicators (SLIs)

| SLI | Definition | Collection Method | Target |
|---|---|---|---|
| Availability | % of successful requests | Prometheus up metric | 99.9%+ |
| Latency (P50) | Median response time | Request duration histogram | <100ms |
| Latency (P95) | 95th percentile response time | Request duration histogram | <500ms |
| Latency (P99) | 99th percentile response time | Request duration histogram | <2s |
| Error Rate | % of failed requests | Error counter / total requests | <0.1% |
| Throughput | Requests per second | Request counter | Variable |
| Saturation | Resource utilization | CPU, memory, disk metrics | <80% |
| Knowledge Accuracy | % verified facts | Verification pipeline output | >99.9% |
| Content Freshness | Max age of delivered content | Content publish timestamp | <24 hours |

### Appendix E: Related Documents

| Document ID | Title | Relationship |
|---|---|---|
| IEP-CONST-001 | Project Constitution | Governing principles |
| IEP-VISION-001 | Project Vision | Strategic direction |
| IEP-ARCH-AGENT-001 | Agent Architecture | Agent design details |
| IEP-ARCH-ORCH-001 | Master Orchestrator | Orchestration details |
| IEP-ARCH-DB-001 | Database Architecture | Database design |
| IEP-ARCH-KG-001 | Knowledge Graph Architecture | Knowledge graph details |
| IEP-ARCH-SEC-001 | Security Architecture | Security specifics |
| IEP-DEPLOY-001 | Deployment Architecture | Deployment specifics |

---

**END OF SYSTEM ARCHITECTURE**

---

*This document defines the complete system architecture for the Islamic Education Platform. All implementation must conform to this architecture. Architecture changes must be proposed as ADRs and approved through the Architecture Review Board. Questions regarding this document should be directed to the Office of the Chief Architect.*
