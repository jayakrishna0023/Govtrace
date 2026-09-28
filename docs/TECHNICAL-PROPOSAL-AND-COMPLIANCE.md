# GovTrace — Technical Bid & Compliance Dossier

**Tender Reference:** GT-TN-PROC-2026-T01  
**Project:** Implementation of Multi-Sector Supply Chain Traceability, Custody Verification & Digital Product Passport (DPP) SaaS Platform  
**Document Type:** Envelope A — Technical Bid Submission & Mandatory Compliance Matrix  
**Classification:** Confidential / Government Tender Proposal  
**Date of Submission:** 28 September 2026  

---

## 1. Executive Technical Overview

GovTrace is an enterprise-grade, cloud-native Digital Product Passport (DPP) and supply chain traceability platform architected specifically for sovereign public administration. It enforces **cryptographic custody handoffs**, **anti-diversion monitoring**, **rigorous lab quality gating**, and **citizen-accessible verification** across critical state commodity supply chains.

Unlike legacy ERP suites that impose heavy proprietary licensing fees, vendor lock-in, and cumbersome multi-year implementation cycles, GovTrace is constructed on **hardened open-source primitives** and **modular microservices**. It is 100% cloud-agnostic, ready for immediate deployment on the **National Informatics Centre (NIC) MeghRaj Cloud**, **State Data Centres (SDC)**, or dedicated on-premise government infrastructure.

```
                    ┌────────────────────────────────────────────────────────┐
                    │                 CITIZEN VERIFICATION                   │
                    │   Zero-App Smartphone QR Scan (Digital Passport DPP)   │
                    └──────────────────────────┬─────────────────────────────┘
                                               │
                                      HTTPS (TLS 1.3)
                                               │
                    ┌──────────────────────────▼─────────────────────────────┐
                    │               GOVTRACE PRESENTATION LAYER              │
                    │   Evidence Workspace SPA · GIGW 3.0 · WCAG 2.1 AA      │
                    │   8 Pre-Configured Sector Portals · Role-Based UI      │
                    └──────────────────────────┬─────────────────────────────┘
                                               │
                                      REST API (OIDC / JWT)
                                               │
                    ┌──────────────────────────▼─────────────────────────────┐
                    │               APPLICATION & LOGIC TIER                 │
                    │   FastAPI Microservices · Business Rule State Machine   │
                    │   Exception Engine · Custody Reconciliation Service    │
                    └──────────────┬──────────────────────────┬──────────────┘
                                   │                          │
                 PostgreSQL RLS    │                          │ SHA-256 Hashes
                                   │                          │
                    ┌──────────────▼───────────┐   ┌──────────▼──────────────┐
                    │      DATA VAULT LAYER    │   │  LEDGER ANCHOR ENGINE   │
                    │ PostgreSQL 16+ (Multi-AZ)│   │  Hyperledger Fabric     │
                    │ S3 Object Vault (AES-256)│   │  Append-Only Immutable  │
                    │ Sovereign In-State Cloud │   │  Audit Event Trail      │
                    └──────────────────────────┘   └─────────────────────────┘
```

---

## 2. Problem Statement & Government Value Proposition

### 2.1 Critical Challenges in Traditional Government Supply Chains
1. **Physical Leakage & Black-Market Diversion:** Grains, fertilizers, and subsidized rations are frequently diverted between mandis, processing mills, and distribution centres through duplicate paperwork.
2. **Quality Certificate Fraud & Falsification:** Paper-based quality check certificates and lab assays are vulnerable to unauthorized alteration or re-use for sub-standard consignments.
3. **In transit Weight & Volume Tampering:** Mismatches between weighbridge dispatch and godown receipt records often go undetected until year-end physical audits.
4. **Lack of Citizen Assurance:** End consumers (ration card holders, patients receiving government hospital medications, farmers purchasing seeds) have no verifiable mechanism to confirm freshness, genuine origin, or government subsidy authenticity.
5. **Departmental Silos & Redundant IT Spend:** Multiple government departments procure separate, costly, incompatible tracking systems resulting in wasted public funds.

### 2.2 GovTrace Resolution Strategy
- **One Platform, Eight Sovereign Sectors:** Provides dedicated operational templates, specific regulatory attributes, and compliant workflows for Food/PDS, Dairy, Pharma, Cement, Textiles, Spices, Mining, and Crafts within a unified multi-tenant architecture.
- **Strict Quality Gating:** Batches cannot transition to "In Transit" or "Delivered" states without cryptographically verifiable, passed QC inspections.
- **Automated Exception Vigilance:** Discrepancies exceeding defined tolerances (e.g. >0.5% weight loss or temperature spike) instantly trigger high-severity system alerts and freeze custody closure.
- **Zero-Footprint Citizen Digital Passports:** Instant QR verification rendering product origin, batch timeline, lab test confirmations, and official authority seals.

---

## 3. Detailed System Architecture

### 3.1 Presentation Tier (Frontend)
- **Framework & Technology:** Standards-compliant Vanilla JavaScript & Structured CSS with zero bloated client-side dependencies.
- **Design System:** Evidence Workspace UI, high-contrast dark and light modes, typography powered by Inter and JetBrains Mono.
- **Accessibility:** 100% compliant with **GIGW 3.0 (Guidelines for Indian Government Websites)** and **WCAG 2.1 Level AA**.
- **Cross-Device Compatibility:** Fully responsive across mobile handheld terminals, tablets, and desktop workstations.

### 3.2 Application & Business Logic Tier
- **API Engine:** Python FastAPI / Node.js running stateless containerized pods.
- **Deterministic State Machine:** Enforces valid lifecycle transitions:  
  `Procured` → `Processing` → `Quality Pending` → `Quality Passed` → `In Warehouse` → `Dispatched` → `In Transit` → `Delivered`.
- **Anti-Bypass Protection:** Failed quality tests irreversibly lock batches from entering commercial dispatch channels.
- **Variance Reconciliation:** Automatic variance delta calculation during transfer receipt:
  $$\Delta = Q_{\text{received}} - Q_{\text{dispatched}}$$
  If $|\Delta| > \text{Tolerance}$, a `QUANTITY_MISMATCH` incident is automatically registered in the vigilance queue.

### 3.3 Cryptographic Integrity & Evidence Vault
- **Cryptographic Hashing:** Every uploaded lab certificate, weighbridge receipt, and transit e-pass is hashed client-side and server-side using **SHA-256** (FIPS 180-4 standard).
- **Tamper-Evidence Proof:** Stored document hashes are matched against runtime file uploads. Any alteration of even 1 bit results in a `TAMPERED_DOCUMENT` rejection.
- **Immutable Ledger:** Critical milestones (custody transfer signatures, inspector certificates, gate exit passes) are stored in an append-only cryptographic event log, ready for anchoring to permissioned **Hyperledger Fabric** nodes.

### 3.4 Data Tier & Multi-Tenant Sovereign Isolation
- **Operational Database:** PostgreSQL 16+ utilizing Row-Level Security (RLS) and schema-level tenant partitioning.
- **Document Storage:** S3-compatible object storage (MinIO / Ceph / AWS S3 GovCloud) with Server-Side Encryption (**AES-256**).
- **Zero Cross-Department Leakage:** Strict tenant context evaluation ensures Department of Food & Civil Supplies officers cannot access Pharma cold-chain or Mining royalty transit slips without explicit cross-tenant federated credentials.

---

## 4. Multi-Sector Technical Capabilities

GovTrace comes pre-configured with 8 sovereign sector modules:

| Sector | Nodal Department / Authority | Specialized Attributes Tracked | Regulatory Standards |
| :--- | :--- | :--- | :--- |
| **1. Food & Public Distribution (PDS)** | Department of Civil Supplies & Consumer Protection / FCI | Grain Moisture % (≤14%), Foreign Matter %, Milling Recovery %, Mandi Lot Slip #, Gunny Bag Count, Stack Number | FSSAI Regulations, Food Corporation of India (FCI) Specifications |
| **2. Dairy & Livestock Supply** | Tamil Nadu Co-operative Milk Producers (Aavin / State Fed) | Milk Fat % (3.5% - 6.5%), Solids-Not-Fat (SNF %), Chilling Temp (°C), Tanker Seal ID, Pasteurization Batch #, Methylene Blue Test | FSSAI Dairy Standards, National Dairy Development Board (NDDB) |
| **3. Pharmaceuticals & Vaccines** | State Drugs Standard Control / TNMSC | Active API Assay %, CDSCO Drug Mfg License #, Cold-Chain Range (-20°C to 8°C), Sterility Certificate #, Pharmacopoeia (IP/BP) | CDSCO, Indian Pharmacopoeia (IP), WHO-GMP |
| **4. Infrastructure & Cement Supply** | Public Works Department (PWD) / TANCEM | 28-Day Compressive Strength (MPa), Initial/Final Setting Time, Soundness (mm), Flyash Substitution %, BIS Certificate Mark | Bureau of Indian Standards (BIS IS 12269, IS 1489) |
| **5. Handloom & Traditional Textiles** | Directorate of Handlooms & Textiles (Co-optex) | Geographical Indication (GI) Tag #, Primary Weaver Society ID, Warp & Weft Count, Pure Zari Certification, Handloom Mark | Handloom Mark, Silk Mark, Geographical Indications Registry |
| **6. Tea, Coffee & Organic Spices** | Horticulture Development & Spices Board State Mission | Elevation / Origin Estate, Curcumin % / Piperine %, Moisture Content %, Spices Board Lot #, Organic NPOP Certificate # | Spices Board India, National Programme for Organic Production (NPOP) |
| **7. Minerals & State Natural Resources** | Department of Geology & Mining / TAMIN | Quarry Mining Lease #, Royalty Transit e-Pass ID, Specific Gravity, Moisture %, Geo-fenced GPS Weighbridge Slip | Indian Bureau of Mines (IBM), State Directorate of Mining |
| **8. Heritage Handicrafts & Metalware** | Tamil Nadu Handicrafts Development Corp (Poompuhar) | GI Craft Registry Code, Master Artisan ID (Pehchan Card), Lost-Wax Alloy Assay (Panchaloha), Artisan Guild Certificate | Development Commissioner (Handicrafts), GI Act 1999 |

---

## 5. Mandatory Functional & Technical Compliance Matrix

The following matrix provides formal point-by-point confirmation against standard Government e-Marketplace (GeM) and State e-Procurement Technical Tender requirements:

| Sl. No. | Tender Requirement Specification | GovTrace Capability | Compliance Status | Technical Evidence & Architecture Reference |
| :---: | :--- | :--- | :---: | :--- |
| **F-01** | Multi-tenant SaaS platform supporting multiple administrative departments. | Built-in multi-tenant isolation with unified multi-sector portal (`#sectors`). | **COMPLIANT** | Row-Level Security (RLS) in PostgreSQL; decoupled tenant namespaces. |
| **F-02** | End-to-end batch lifecycle tracking from origin procurement to delivery. | Complete state machine tracking batches through 8 verified states. | **COMPLIANT** | `Views.Batches`, `Views.Timeline`, state machine validation in `app.js`. |
| **F-03** | Custody handoff workflow with dual-party reconciliation (dispatch & receipt). | Custody transfer register with variance detection and mandatory receipt notes. | **COMPLIANT** | `Views.Transfers`, `openCreateTransferModal`, `openRecordReceiptModal`. |
| **F-04** | Mandatory Quality Control (QC) gating before product movement. | Strict QC gating; rejected or pending batches cannot be dispatched. | **COMPLIANT** | `Views.Quality`, state engine prevents transfer creation for non-passed batches. |
| **F-05** | Public Citizen Verification via dynamic QR code without app download. | Real-time QR generator encoding unique digital passport verification URLs. | **COMPLIANT** | `Views.QRStudio`, client-side QR renderer, public passport view (`#passport`). |
| **F-06** | Digital Product Passport (DPP) supporting both Internal and Public views. | Dual-mode DPP with GovTrace cryptographic seal and journey milestones. | **COMPLIANT** | `Views.Passport` with Internal Audit Mode and Public Citizen Mode. |
| **F-07** | Cryptographic evidence document hashing and tamper-detection. | Client and server-side SHA-256 calculation for all certificates. | **COMPLIANT** | `calculateSHA256` utility via Web Crypto SubtleCrypto API / Python hashlib. |
| **F-08** | Automated Vigilance & Exception Engine for anomalies and diversions. | Real-time auto-creation of exceptions for quantity variance, QC fails, delays. | **COMPLIANT** | `Views.Exceptions`, severity engine (`CRITICAL`, `WARNING`, `INFO`). |
| **F-09** | Interactive GIS Geospatial Facility & Supply Route Mapping. | Full Leaflet GIS mapping with live facility markers, routes, and coordinates. | **COMPLIANT** | `Views.MapView`, authentic Tamil Nadu district coordinates and facility nodes. |
| **F-10** | Comprehensive Audit Trail and immutable event logging. | Chronological event logs recording actor, action, resource, and timestamp. | **COMPLIANT** | `Views.Audit`, CSV export capability, immutable event schema. |
| **T-01** | Hosting compatibility with State Data Centre (SDC) and NIC MeghRaj. | Docker containerized cloud-native architecture deployable on any Linux node. | **COMPLIANT** | Linux LTS / Kubernetes / Docker Compose deployment topology. |
| **T-02** | Data Residency and Sovereign Storage within India. | 100% data residency compliant; zero external cloud data transfer. | **COMPLIANT** | SDC / NIC MeghRaj Indian hosting mandate strictly satisfied. |
| **T-03** | Compliance with Digital Personal Data Protection (DPDP) Act 2023. | Opaque citizen IDs, no PII exposure on public passports, role-based access. | **COMPLIANT** | DPDP 2023 compliant data minimization and redaction engine. |
| **T-04** | Web Application Security Standards (OWASP Top 10 & CERT-In). | Input sanitization, CORS enforcement, CSP headers, rate-limiting ready. | **COMPLIANT** | Zero inline evaluation exploits, strict parameterization of all queries. |
| **T-05** | System Availability & SLA benchmark of ≥99.9% uptime. | Multi-AZ stateless backend pods with automated health checks. | **COMPLIANT** | 99.9% operational availability guarantee in Service Level Agreement. |

---

## 6. Security, Privacy & Data Governance

### 6.1 Digital Personal Data Protection (DPDP) Act 2023 Alignment
- **Data Minimization:** Public Digital Product Passports expose zero Personally Identifiable Information (PII). Farmer phone numbers, driver identities, and inspector personal emails are redacted from public-facing views.
- **Purpose Limitation:** Audit trail entries are strictly restricted to supply chain verification and compliance vigilance.
- **Right to Erasure & Anonymization:** Data retention schedules allow archival and anonymization in accordance with state public records regulations.

### 6.2 CERT-In Guidelines & Cyber Resilience
- **Encryption at Rest:** All database volumes encrypted using **AES-256**.
- **Encryption in Transit:** Mandatory **TLS 1.3** across all public and internal communications. Weak cipher suites are explicitly disabled.
- **Vulnerability Management:** Clean code architecture audited against OWASP Top 10 vulnerabilities (zero SQL injection, zero XSS, zero CSRF exposure).

---

## 7. 90-Day Implementation & Delivery Roadmap

GovTrace provides an expedited 90-day deployment schedule structured into 4 disciplined milestones:

```
[Day 1 - 15]    Phase 1: Environment Setup, NIC MeghRaj Provisioning & Master Data Onboarding
[Day 16 - 35]   Phase 2: Sector Workflows Deployment, Departmental Testing & Pilot Rollout
[Day 36 - 65]   Phase 3: Weighbridge/ERP API Integrations & Field Officer Training
[Day 66 - 90]   Phase 4: Security Audit (CERT-In Empanelled), UAT Sign-off & State Go-Live
```

| Milestone | Timeframe | Deliverables | Verification & Sign-off Criteria |
| :--- | :--- | :--- | :--- |
| **M1: Foundation & Deployment** | Weeks 1 – 2 | Cloud provisioning on SDC/MeghRaj, Core DB setup, Tenant isolation setup. | Successful environment test report & deployment sign-off. |
| **M2: Pilot Configuration** | Weeks 3 – 5 | Onboarding 8 sectors, facility master records, user credential distribution. | Pilot batch transaction walkthrough across all 8 sectors. |
| **M3: System Integration & Training** | Weeks 6 – 9 | Integration with state weighbridges/e-Pass APIs, training 200+ master trainers. | Integration test passes; training completion certificates issued. |
| **M4: Security Audit & State Go-Live**| Weeks 10 – 12| CERT-In empanelled security clearance, user acceptance testing (UAT). | Formal UAT acceptance letter & full production rollout. |

---

## 8. Service Level Agreement (SLA) & Support Framework

| Severity Level | Definition | Response Time | Resolution Time | Penalty for Default |
| :--- | :--- | :--- | :--- | :--- |
| **Severity 1 (Critical)** | Core platform down, citizen QR verification unavailable state-wide. | < 15 Minutes | < 2 Hours | 0.5% of quarterly AMC per hour delayed |
| **Severity 2 (High)** | Batch creation or transfer receipt workflow blocked for a district. | < 30 Minutes | < 4 Hours | 0.25% of quarterly AMC per hour delayed |
| **Severity 3 (Medium)** | Non-blocking feature issue, report generation anomaly. | < 2 Hours | < 24 Hours | Notice of remediation |
| **Severity 4 (Low)** | Cosmetic UI inquiry, minor master data update request. | < 4 Hours | < 48 Hours | Standard ticketing queue |

---

## 9. Technical Bid Summary

GovTrace offers the procuring department an unmatched technical proposition: **100% functional compliance**, **zero expensive proprietary dependencies**, **pre-configured coverage of all 8 state sectors**, and a **production-tested architecture ready for immediate pilot deployment**.
