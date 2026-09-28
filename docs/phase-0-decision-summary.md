# GovTrace — Phase 0 Decision Summary

## What we are building

A focused Food/PDS traceability MVP centred on one complete rice-batch journey: product and batch creation, quality gating, warehouse movements, custody transfer, receipt reconciliation, document integrity, QR verification, internal/public Digital Product Passports, audit trail, and deterministic exceptions.

The visual direction is original: an evidence workspace rather than a generic dashboard—quiet surfaces, high-legibility data tables, lifecycle timelines that read like a ledger, and restrained status colour. It will use open-source implementation primitives where useful, but not copy another product’s identity or interface.

## What we are not building yet

No live blockchain/Fabric network, government-system connection, production AI, native mobile app, offline sync, unit-level serialisation, multi-sector configuration studio, or government approval/deployment claims. We will not fabricate operational analytics or call a demo ledger transaction a blockchain verification.

## Recommended architecture direction

Build a modular monolith: React + TypeScript frontend, FastAPI + Python backend, PostgreSQL for operational records, encrypted S3-compatible object storage for documents, and an OIDC-compatible identity boundary. Use a deferred ledger-adapter interface, so a real permissioned Fabric integration can be added after a genuine multi-organisation governance decision.

## Recommended MVP

Build the internal operating flow and the public QR verifier together, using entirely synthetic Food/PDS data. The tender/demo story should lead from procurement to a 50 kg receipt discrepancy, then show the exception, evidence timeline, and safe public passport.

## Major assumptions

- The first tenant can be modelled as a single Food/PDS demonstration organisation with multiple facilities.
- Product data is business-sensitive, while a small approved subset can be public.
- A quality-passed state is required before dispatch/delivery, with detailed transition policy finalised before the schema.
- Tenant context is resolved from identity claims, not client-controlled input.
- The product will be deployed later in an agreed jurisdiction; hosting/retention policy is not yet fixed.

## Decisions required from the product owner

1. Which identity approach should the first deployable version support: an existing government/enterprise OIDC provider, or local accounts first with OIDC compatibility?
2. Is QR verification intended for the open public internet from MVP day one, or only authenticated/internal pilot users initially?
3. Which state/department and data-hosting jurisdiction will govern the initial pilot? This determines retention, privacy, and support requirements.
4. Does a live multi-organisation blockchain deployment have a named partner and governance owner, or should it remain explicitly post-MVP as recommended?
5. For the initial rice workflow, which transitions require a human approval beyond QC and receiving acceptance?
