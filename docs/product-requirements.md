# GovTrace — Phase 0 Product Discovery & Requirements

**Status:** Draft for product-owner approval  
**MVP sector:** Food / Public Distribution System (PDS)  
**Reference workflow:** Rice batch traceability

## 1. Product vision

GovTrace is a government-oriented traceability platform that lets authorised organisations record, verify, and investigate the lifecycle of a product batch—from procurement through distribution—without presenting unverified claims as facts. Its public Digital Product Passport gives citizens a deliberately limited, safe view; its internal workspace gives operators evidence, accountability, and exception handling.

The product is not a blockchain product. A permissioned ledger may later anchor selected lifecycle evidence shared by multiple organisations. The operational system of record remains the platform database and secured document store.

## 2. Problem statement

Government and public-sector supply chains often rely on disconnected records, delayed reconciliation, and documents that are difficult to validate. This makes it costly to answer basic questions: where did a batch originate, who handled it, whether it passed quality checks, and why delivered quantity differs from dispatched quantity. GovTrace creates a consistent evidence trail, controlled access, and an auditable investigation path.

## 3. Target market

Initial customers are government departments, PSUs, cooperatives, procurement agencies, and enterprises operating food/PDS supply chains. The buyer is typically a department or enterprise administrator; daily users span procurement, processing, quality, warehouse, transport, distribution, and audit teams. Future sector configuration may support dairy, pharma, cement, handloom, and handicrafts without separate products.

## 4. Personas

| Persona | Goal | Key need |
|---|---|---|
| Tenant administrator | Configure a tenant safely | Organisations, facilities, users, roles, data export |
| Procurement officer | Register sourced stock | Product/batch creation and source evidence |
| Processor | Record transformation | Parent/child batch lineage and quantity conservation |
| Quality inspector | Approve or fail material | Controlled QC workflow and certificates |
| Warehouse manager | Receive and dispatch stock | Batch-aware inventory and discrepancy reconciliation |
| Transporter | Evidence a handoff | Shipment status and custody acknowledgement |
| Distribution manager | Receive and distribute stock | Receipt, stock availability, and exception resolution |
| Auditor | Reconstruct what happened | Append-only timeline, actor, evidence, and export |
| Public verifier | Check a QR code | Safe public passport without operational disclosure |

## 5. Primary user journey

1. A procurement officer selects a tenant organisation and creates a generic product record.
2. The officer creates a rice batch with origin, quantity, unit, production date, and source facility.
3. The processor records a processing event and any split/merge relationship.
4. A quality inspector submits a quality check; only a passed check permits packaging and warehouse receipt.
5. A warehouse manager receives the batch, creating inventory through an event—not direct stock editing.
6. The sender creates a transfer with dispatched quantity. The receiver inspects and accepts or rejects it.
7. A quantity difference creates an exception while preserving both declared quantities.
8. An authorised user views the internal passport and full evidence timeline. A public user scans a QR code to view only approved public facts.

## 6. Food/PDS rice use case

Synthetic demonstration batch: `TN-RICE-2026-000001`, 10,000 kg, sourced in Thanjavur, processed by a demonstrator rice mill, quality-passed, received in Trichy warehouse, and dispatched as a 5,000 kg shipment. A receiving distribution centre records 4,950 kg. GovTrace raises a 50 kg `QUANTITY_MISMATCH` exception and retains original dispatch and receipt values, evidence, and resolution.

This is synthetic demo data only. The product must not claim a government relationship, approval, deployment, or use of real beneficiary information.

## 7. Complete supply-chain workflow

`Created → Procured → Processing → Quality pending → Quality passed → Packaged → In warehouse → Transfer initiated → In transit → Received → Distributed → Delivered/closed`

Alternative paths: failed quality enters `QUALITY_FAILED` and may only proceed through a controlled retest, quarantine, or recall process; an expired, recalled, or quarantined batch cannot be transferred for delivery. Each permitted transition creates an immutable business event. A transfer is a two-party workflow: sender declaration, transport state, receiver inspection, acceptance/rejection, then reconciliation and custody update.

## 8. Functional requirements

### P0 operational capabilities

- Tenant, organisation, facility, user, and role management.
- Generic product master with configurable sector attributes.
- Batch/lot creation, state transitions, parent-child lineage, and quantity-aware events.
- Append-only supply-chain timeline with actor, organisation, facility, timestamp, quantities, state transition, location, evidence references, and metadata.
- Quality check submission, approval/failure, certificate attachment, and transition gating.
- Warehouse receipt/dispatch and batch-aware inventory movements.
- Custody transfers with acceptance/rejection and automated quantity mismatch exceptions.
- Secure document upload metadata and SHA-256 integrity registration/verification.
- QR creation using an opaque public identifier and a public Digital Product Passport.
- Internal passport, exceptions queue, audit view, and usable dashboard counts calculated from stored records.

### P1 capabilities

- Shipment route/status tracking, notification provider abstraction, analytics views, configurable retention and exports, and integration API keys.

### P2 capabilities

- Sector workflow builder, mobile offline queue, unit serialisation/NFC, semantic document search, and authorised investigation assistant.

## 9. Non-functional requirements

- Responsive PWA-first interface; keyboard-accessible core flows; WCAG 2.2 AA target.
- Server-side tenant isolation, RBAC, resource-level authorisation, validation, audit logging, and rate limiting.
- UTC timestamps internally; displayed in tenant locale/time zone.
- API pagination, filtering, stable error format, correlation IDs, idempotency keys for transfer/receipt creation.
- Target MVP availability and recovery objectives must be agreed before production. The design will expose health checks, backups, monitoring, and export paths but must not make unsupported SLA claims.
- Sensitive documents encrypted at rest in object storage; only metadata and access-controlled retrieval links are exposed through APIs.

## 10. Business rules

1. Every operational record belongs to exactly one tenant; cross-tenant reads and writes are forbidden.
2. State transitions are validated server-side. A failed or quarantined batch cannot be delivered.
3. Events are append-only. Corrections are compensating events linked to the original, never destructive edits.
4. Quantity movements are explicit and immutable; negative available stock is prohibited unless a separately approved controlled adjustment rule is introduced.
5. Batch split/merge must retain lineage and validate quantity conservation, allowing documented loss/waste only through a reasoned event.
6. Acceptance of a transfer is the only action that updates receiving custody; dispatch alone does not.
7. A document hash match verifies equivalence to the registered file, not truth of the document’s statements.
8. Public passports expose only tenant-approved fields and never internal documents, staff details, exact inventory, routes, or confidential facilities.
9. A ledger status may say `Not configured`, `Pending anchor`, `Anchored`, or `Verification failed`; it must never imply blockchain verification where no live ledger proof exists.

## 11. Core entities

Tenant, Organisation, Facility, User, Role, Permission, Product, Product Category, Product Attribute Definition, Batch, Batch Relationship, Supply Chain Event, Inventory Balance, Inventory Transaction, Shipment, Custody Transfer, Quality Check, Document, Document Hash, QR Identifier, Exception, Audit Log, and Ledger Anchor.

All primary entities use UUIDs. Product/batch business identifiers are human-readable but unique only in the intended tenant scope. Events, balances, and documents retain tenant IDs for defensive access control and indexed queries.

## 12. Roles and permissions

| Role | Minimum authority |
|---|---|
| Super admin | Platform operations; no routine tenant-data access without audited support delegation |
| Tenant admin | Tenant configuration, organisations, facilities, users, roles, exports |
| Procurement officer | Create products/batches and procurement evidence |
| Processor | Record approved processing events and transformations |
| Quality inspector | Submit and approve/fail quality checks within assigned facilities |
| Warehouse manager | Receive, dispatch, and adjust stock with mandatory reason |
| Transporter | View assigned shipments and record transport milestones only |
| Distribution manager | Receive transfers, manage distribution inventory, resolve assigned exceptions |
| Auditor | Read/export authorised records and audit trail; no operational mutation |
| Viewer | Read approved internal records |
| Public verifier | Read only approved public passport fields by opaque QR identifier |

Permissions are action-plus-resource scoped (for example, `batch.read` constrained to a tenant and permitted facility), not merely navigation visibility.

## 13. Data lifecycle

Operational data is created by an authorised actor, validated, recorded as a transaction, made visible to roles with resource access, then retained under a tenant policy. Documents are uploaded to encrypted object storage; their metadata and hash become part of the business evidence trail. Audit logs and lifecycle events are retained as immutable operational history subject to lawful retention requirements. A tenant administrator can request data export and eventual deletion/exit workflows; immutable audit/ledger records require policy-specific treatment rather than a misleading promise of immediate erasure.

## 14. Blockchain requirements

Blockchain is excluded from the first operational MVP release and designed behind a `LedgerAnchorProvider` adapter. The future preferred option is Hyperledger Fabric, with a permissioned organisation model. Only canonical event hashes, document hashes, timestamps, tenant/organisation references, and anchor references may be sent; no documents, personal data, credentials, or secrets go on-chain. Anchoring requires a specific multi-organisation trust reason and a live verification mechanism. The UI must distinguish application audit records from ledger evidence.

## 15. Digital Product Passport requirements

The internal passport presents product identity, batch status, lineage, quantity summary, current custody, approved quality outcome, documents available to the viewer, exceptions, event timeline, and evidence/ledger status. The public passport presents an intentionally smaller approved set: product name/category, batch identifier, broad origin, production/processing date where approved, quality status, current public status, integrity state, and approved journey stages. Both show status provenance and record timestamps. Public passport responses must be rate-limited and resistant to identifier enumeration.

## 16. QR requirements

QR codes encode a canonical HTTPS verification URL containing a high-entropy opaque identifier, not operational data. The verification endpoint validates identifier format, active/revoked status, tenant public visibility policy, and output fields. Codes support rotation/revocation, scan auditing without unnecessary personal data, trusted-domain messaging, and optional signed payloads later. Duplicate physical copies are not technically preventable by a QR alone; the UI must not claim otherwise.

## 17. Document requirements

Allowed types include purchase order, invoice, QC certificate, lab report, manufacturing certificate, licence, inspection report, transport document, delivery proof, and recall notice. Uploads require allow-listed file types, size limits, malware scanning before release, metadata validation, tenant ownership, access policy, versioning, SHA-256 calculation, and content-disposition-safe download. Verification calculates the current file hash and compares it with the registered version.

## 18. Exception requirements

Deterministic rules launch the MVP: quantity mismatch, failed/missing QC, missing required document, expired batch, unauthorised transfer attempt, and receipt delay (when due date exists). Exceptions have type, severity, linked evidence, owner, status (`OPEN`, `INVESTIGATING`, `RESOLVED`, `DISMISSED`), resolution note, and timestamps. The rules create an alert; they do not automatically infer fraud or overwrite operations.

## 19. Analytics requirements

Dashboard numbers are derived from application records with clearly stated filters and refresh time. P0 includes active batches, batches in transit, pending quality checks, pending receipts, open exceptions, quantity mismatch count, and traceability completeness. P1 adds delivery performance, variance, organisation activity, exception trend, and quality trend. No fabricated metrics, maps, or predictive claims are permitted.

## 20. AI requirements

AI is out of the MVP critical path. A future authorised assistant may summarise records, search approved documents, identify similar past exceptions, and answer evidence-backed questions. It must apply the caller’s tenant/resource permissions before retrieval, cite record identifiers/timeline events in responses, label analysis as analysis, and refuse to invent unavailable facts. It must never train on tenant data without explicit, separate authorisation.

## 21. Security requirements

Use OIDC/OAuth2-compatible identity, strong password hashing where local credentials exist, MFA-ready account model, secure sessions/tokens, CSRF defence for cookie flows, CORS allow-listing, secure headers, audit logging, rate limits, input validation, least privilege, secrets management, encryption in transit/at rest, data backups, monitoring, and regular dependency/vulnerability management. Resource-level checks occur in the backend for every operation. Production security, privacy, retention, hosting-region, and incident requirements need legal/procurement review before any compliance claim.

## 22. Multi-tenant requirements

Each tenant represents a department, PSU, cooperative, or enterprise. All tenant-owned tables carry a tenant identifier; APIs derive tenant context from authenticated claims and never accept unrestricted tenant selection from a browser form. Tenant admins manage only their own users, organisation graph, and data. Platform support access is exceptional, time-bound, auditable, and disabled by default in production. Tenant data exports are scoped and logged.

## 23. Integration requirements

The MVP exposes versioned REST APIs and webhooks only after core workflows are stable. Probable integration boundaries are identity provider (OIDC), object storage, email/SMS provider, WMS/ERP, and later a ledger adapter. Imports must support idempotency, schema validation, source attribution, error reporting, and audit events. GovTrace is a traceability layer; it does not replace statutory customs, tax, food, drug, or beneficiary systems.

## 24. MVP scope and out-of-scope

**In scope:** a single configurable Food/PDS tenant demo; rice product/batch lifecycle; users/roles; facilities; quality gate; warehouse/transfer/receipt; deterministic exceptions; documents + integrity hash; QR public verification; internal/public passports; audit timeline; and computed operational dashboard.

**Out of scope:** live Hyperledger Fabric; real government integrations/data; multi-sector workflow builder; native app; offline sync; GPS tracking; unit serialisation; predictive AI; biometric identity; payments; legal compliance certification; and claims of government approval/deployment.

## 25. Future roadmap

1. Harden the Food/PDS operational MVP and prove the full synthetic demo journey.
2. Add configurable shipment/notification/reporting integrations.
3. Add a live Fabric adapter only after partner/endorsement and governance decisions are real.
4. Add sector configurations and controlled workflow templates.
5. Add field offline capability and unit-level identity based on operational demand.
6. Add permission-aware investigation assistance and semantic search.

## 26. Risks and mitigations

| Risk | Mitigation |
|---|---|
| Treating ledger anchoring as business proof | Separate fact, integrity verification, and analysis in UX and documentation |
| Weak source data | Require actor, source, evidence, and approvals; integrity cannot establish truth |
| Tenant data leakage | Backend scoping, automated authorization/IDOR tests, audited support access |
| Overbuilt MVP | Deliver one complete rice journey before sector expansion |
| QR cloning/misinterpretation | Opaque IDs, revocation, clear public guidance, no impossible anti-copy claims |
| Quantity reconciliation disputes | Preserve both figures, evidence, and explicit resolution events |
| Regulatory mismatch | Early legal/procurement review; avoid compliance claims before evidence |

## 27. Architecture decisions

| Decision | Recommendation | Trade-off / migration |
|---|---|---|
| Product architecture | Modular monolith with domain modules and a versioned REST API | Faster, safer MVP; extract services when measured scale/ownership needs justify it |
| Frontend | React + TypeScript with a component system and responsive PWA shell | Strong app ergonomics; native mobile remains later |
| Backend | FastAPI + Python with PostgreSQL | Product/domain clarity and strong validation; a TypeScript backend remains viable if team preference changes before build |
| Data | PostgreSQL for operations; S3-compatible encrypted object storage for files | Operational simplicity; analytics warehouse only when required |
| Identity | OIDC-compatible provider boundary | Initial local/dev identity may be simpler; preserve provider abstraction |
| Ledger | Deferred Fabric adapter, no fake ledger claims | No immutable shared ledger in MVP; protects credibility and allows a real partner-led rollout |
| Design language | Original “evidence workspace”: disciplined type, warm off-white canvas, deep ink, indigo status accents, ledger-like timelines, and concise data cards | Avoids generic neon/AI-dashboard styling; must validate contrast and density with users |

## 28. Acceptance criteria

Phase 0 is accepted when the product owner confirms the MVP sector/use case, role model, public/private data boundary, state/quantity rules, ledger deferral, and architecture direction; outstanding policy decisions are recorded; and the backlog has a prioritised first build sequence. Phase 0 does not deliver a UI, database, API, or a blockchain network.

## 29. Definition of done for future product increments

An increment is done only when its acceptance tests pass, tenant/authorisation enforcement is verified, audit behavior is documented, failure behavior is clear, accessibility is checked for the changed flow, analytics use real records, tests run in CI, documentation matches the code, and no mock/adapter is represented as a production integration.

## 30. Initial product backlog

| Epic | Feature / user story | Priority | Dependencies | Acceptance criteria |
|---|---|---:|---|---|
| Foundation | As a tenant admin, I can sign in and work only in my tenant | P0 | Identity, tenant model | Backend rejects cross-tenant reads/writes; tests cover IDOR |
| Foundation | As a tenant admin, I manage organisations, facilities, users, and roles | P0 | Tenant isolation | Scoped CRUD is audited and role-limited |
| Product master | As a procurement officer, I create a reusable food product | P0 | Foundation | Generic product fields and configurable attributes validate correctly |
| Batch lifecycle | As an officer, I create a batch and see permitted next states | P0 | Product master | Invalid transitions are rejected; creation records an event |
| Event engine | As an auditor, I can reconstruct a batch timeline | P0 | Batch lifecycle | Timeline is ordered, actor-attributed, and append-only |
| Quality | As an inspector, I submit a result that gates onward movement | P0 | Event engine, documents | Failed QC blocks delivery transition; certificate reference is shown |
| Inventory | As a warehouse manager, I receive/dispatch batch stock | P0 | Batch lifecycle | Balance is calculated from immutable transactions |
| Custody | As a receiver, I accept/reject a transfer and record actual quantity | P0 | Inventory, events | Custody changes only on acceptance; mismatch creates exception |
| Documents | As an authorised user, I upload and verify an evidence document | P0 | Storage, security | File scan/allow-list/hash/access controls are enforced |
| Passport | As an internal user, I view full authorised batch evidence | P0 | Events, QC, docs | Passport is permission-filtered and distinguishes evidence state |
| Public verify | As a citizen, I scan a QR and see approved facts | P0 | Passport | Opaque identifier, revocation, rate limit, no internal disclosure |
| Exceptions | As a manager, I investigate a quantity mismatch | P0 | Transfers | Original values/evidence persist through resolution |
| Audit | As an auditor, I export action history | P0 | Foundation | Audit actions cannot be edited through normal product APIs |
| Analytics | As an operator, I see real operational counts | P1 | P0 domain data | Counts reconcile with filtered source records |
| Integrations | As a tenant, I import/export through stable APIs | P1 | Domain model | Versioning, idempotency, logs, and validation are documented |
| Ledger | As a partner organisation, I verify a real anchor | P1 | Partner governance, Fabric | Live proof is verifiable; offline/mock status is explicit |
| Intelligence | As an auditor, I ask an evidence-backed question | P2 | Authorised retrieval | Answer cites permitted records and labels analysis |

