# GovTrace — Phase 1 Architecture

**Status:** Draft for product-owner approval

## 1. System Architecture Overview

GovTrace follows a **Modular Monolith** architecture. This provides the development speed and operational simplicity needed for an MVP while maintaining strict logical boundaries that allow future extraction into microservices if scaling or team size demands it.

The system is divided into three logical tiers:
1.  **Presentation Tier:** A responsive Single Page Application (SPA) / Progressive Web App (PWA) built for browsers and mobile field usage.
2.  **Application Tier:** A versioned REST API that enforces multi-tenancy, authorization, and business rules.
3.  **Data Tier:** A relational database for operational state, secure object storage for evidence, and an abstract ledger interface for future blockchain anchoring.

## 2. Core Service Boundaries

The backend is logically separated into the following domains:

*   **Identity & Tenant Management:** Manages organizations, facilities, users, RBAC, and cross-tenant isolation.
*   **Product & Batch Engine:** Manages product master data, batch creation, parent-child lineage (splits/merges), and status.
*   **Supply Chain Event Engine:** The core append-only ledger of the application. Processes custody transfers, quality checks, warehouse movements, and transitions state deterministically.
*   **Document Vault:** Manages secure uploads, content hashing (SHA-256), metadata storage, and time-limited access URLs.
*   **Exception Engine:** Evaluates events against business rules (e.g., quantity mismatch, delayed receipt) and creates/assigns exceptions.
*   **Analytics & Reporting:** Computes operational metrics and aggregates data for dashboards and audits without exposing raw logs unnecessarily.
*   **Blockchain Adapter (Post-MVP):** An abstraction layer (`LedgerAnchorProvider`) that queues and anchors critical event/document hashes to a permissioned network.

## 3. Technology Stack

*   **Frontend:** React + TypeScript. Styling via structured Vanilla CSS/CSS Modules to ensure a premium, custom UI matching the "evidence workspace" design language.
*   **Backend:** FastAPI (Python 3.11+) + Pydantic for validation.
*   **Database:** PostgreSQL 16+.
*   **Object Storage:** S3-Compatible Storage (e.g., AWS S3, MinIO for local dev) with Server-Side Encryption (SSE).
*   **Identity Provider (IdP):** OpenID Connect (OIDC) compatible system (e.g., Keycloak) for enterprise federation, MFA, and JWT issuing.
*   **Deployment:** Docker containers orchestrated via Kubernetes (or Docker Compose for local/testing).

## 4. Deployment Model

GovTrace is designed for a cloud-native, scalable deployment suitable for government or enterprise SaaS.

*   **Environment Strategy:**
    *   `Development`: Local docker-compose environment.
    *   `Staging`: Cloud environment matching production topology for QA and UAT.
    *   `Production`: Highly available cloud deployment with multi-AZ redundancy.
*   **Compute:** Stateless backend containers (FastAPI) behind a load balancer/API Gateway (NGINX/Traefik).
*   **Database:** Managed PostgreSQL with automated daily backups, Point-In-Time Recovery (PITR), and Multi-AZ enabled.
*   **Storage:** Private S3 buckets. Public access is strictly denied. The backend generates Pre-Signed URLs for authorized downloads.

## 5. Data Flow

### 5.1. Standard Event (e.g., Quality Check)
1.  **Client:** Authenticated user submits QC form with attached PDF certificate.
2.  **API Gateway:** Routes request to Backend.
3.  **Backend (Auth):** Validates JWT, verifies tenant ID matches the user's tenant, and checks `quality.create` permission for the facility.
4.  **Backend (Document Vault):** Calculates SHA-256 of the PDF, uploads file to S3, and stores metadata + hash in PostgreSQL.
5.  **Backend (Event Engine):** Creates a `QUALITY_SUBMITTED` event, linked to the Batch and Document. Updates the Batch state to `QUALITY_PASSED`.
6.  **Database:** Commits transaction atomically.
7.  **Client:** Receives success response and updated batch state.

### 5.2. Public QR Verification
1.  **Citizen:** Scans QR code, opening `https://verify.govtrace.example/b/<opaque-id>`.
2.  **Frontend (Public):** Requests passport data via `GET /api/v1/public/passport/<opaque-id>`.
3.  **Backend:** Looks up `<opaque-id>`. Validates the batch is not flagged as "internal only".
4.  **Backend:** Returns only a limited, approved subset of data. No documents, staff names, or exact locations are exposed.
5.  **Frontend:** Renders the clean, branded Digital Product Passport.

## 6. Security Architecture

*   **Authentication:** OAuth2/OIDC with JWT.
*   **Authorization:** Enforced at the API level. Every resource query inherently includes a `WHERE tenant_id = ?` clause.
*   **Data at Rest:** Database encrypted at the volume level. Object storage encrypted via AES-256.
*   **Data in Transit:** TLS 1.3 mandated for all communications.
*   **Document Security:** Authenticated users receive short-lived pre-signed S3 URLs.
*   **Auditing:** Every write operation generates an immutable audit log entry (Actor, Action, Resource, Timestamp).

## 7. Blockchain Architecture

*   **Network:** Hyperledger Fabric (Permissioned).
*   **Participants:** Government Departments, Auditors, major Supply Chain partners.
*   **Data Placement:** Only cryptographic hashes and identifiers are stored on-chain.
    *   *Example Chaincode Payload:* `BatchID`, `EventHash`, `DocumentHash`, `Timestamp`, `TenantSignature`.
*   **Execution:** Anchoring is done asynchronously. The operational database is the immediate source of truth, and the blockchain provides historical tamper-evidence.
