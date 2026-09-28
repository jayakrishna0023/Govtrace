# GovTrace — Tender Submission Checklist & Mandatory Declarations

**Tender Reference:** GT-TN-PROC-2026-CHECKLIST  
**Project:** Implementation of Multi-Sector Supply Chain Traceability, Custody Verification & Digital Product Passport (DPP) SaaS Platform  
**Target:** State e-Procurement Portal / GeM Custom Bid Submission  

---

## 1. Tender Submission Document Checklist

Prior to final submission on the government procurement portal (e.g. GeM / Central Public Procurement Portal CPPP / State e-Procurement), ensure all requisite forms and enclosures are signed, stamped, and organized into the designated envelopes:

### Envelope 1: Pre-Qualification & Technical Bid (Unpriced)
- [x] **Form 1.1:** Official Tender Covering Letter / Bid Submission Form (Annexure A below).
- [x] **Form 1.2:** Power of Attorney / Board Resolution authorizing the signatory.
- [x] **Form 1.3:** Complete Technical Proposal Document ([`TECHNICAL-PROPOSAL-AND-COMPLIANCE.md`](file:///c:/Users/Jaya%20Krishna/Desktop/Govtrace/docs/TECHNICAL-PROPOSAL-AND-COMPLIANCE.md)).
- [x] **Form 1.4:** Point-by-Point Functional & Technical Compliance Matrix (Signed & Stamped).
- [x] **Form 1.5:** "Make in India" (Class-I Local Supplier) Certificate (Annexure B below).
- [x] **Form 1.6:** Non-Blacklisting & Clean Vigilance Affidavit (Annexure C below).
- [x] **Form 1.7:** Earnest Money Deposit (EMD) / Bid Security Declaration (Annexure D below).
- [x] **Form 1.8:** Demonstration Test Video / Live URL access link (`http://127.0.0.1:4173/`).

### Envelope 2: Financial / Commercial Bid
- [x] **Form 2.1:** Completed Financial Bid & Bill of Quantities ([`COMMERCIAL-QUOTATION-AND-BOQ.md`](file:///c:/Users/Jaya%20Krishna/Desktop/Govtrace/docs/COMMERCIAL-QUOTATION-AND-BOQ.md)).
- [x] **Form 2.2:** Detailed Cost Breakdown Sheet (Excel / PDF as per GeM/e-Procurement template).
- [x] **Form 2.3:** Price Undertaking confirming no hidden costs or conditionality.

---

## 2. Standard Government Declarations (Official Formats)

### Annexure A: Official Bid Submission Covering Letter
*(To be printed on Bidder's Official Letterhead)*

```text
To,
The Tender Inviting Authority,
Tender Scrutiny & Evaluation Committee,
State Government e-Procurement Cell.

Subject: Submission of Technical & Commercial Bid for "Multi-Sector Supply Chain Traceability,
         Custody Verification & Digital Product Passport (DPP) SaaS Platform"
Tender Ref: GT-TN-PROC-2026-T01

Dear Sir/Madam,

1. Having examined the tender documents, terms and conditions, specifications, and scope of work,
   we, the undersigned, offer to design, deploy, configure, secure, operate, and maintain the
   "GovTrace Multi-Sector Supply Chain Traceability & Digital Product Passport Platform" in strict
   conformity with all stipulations contained in the tender specifications.

2. We confirm that our quotation is firm, fixed, and represents the Lowest Evaluated Cost (L1)
   providing complete turnkey deployment for all 8 state sectors without any hidden fees or
   recurring per-seat license taxes.

3. We hereby declare that all statements, technical architectural specifications, and compliance
   claims submitted in our technical dossier are true and verifiable on our operational prototype.

4. We agree to abide by this bid for a period of 180 (one hundred eighty) days from the bid opening date.

Yours faithfully,

(Authorized Signatory)
Name: __________________________
Designation: ____________________
Company Seal:
Date: 28 September 2026
```

---

### Annexure B: "Make in India" Class-I Local Supplier Certificate
*(In accordance with Public Procurement (Preference to Make in India) Order 2017 - DPIIT / Ministry of Commerce)*

```text
DECLARATION OF LOCAL CONTENT

Tender Ref: GT-TN-PROC-2026-T01

We hereby certify and confirm that the software product "GovTrace" offered against the above tender
is 100% indigenous software developed in India.

1. Local Content Percentage: 100% (One Hundred Percent).
2. Classification: "Class-I Local Supplier" under DPIIT Public Procurement Order.
3. Location of Development & Support Facilities: India.
4. Data Sovereignty: 100% of code, operational data, and cryptographic evidence hashes reside
   strictly within the sovereign territory of the Republic of India.

(Authorized Signatory & Seal)
```

---

### Annexure C: Non-Blacklisting & Integrity Undertaking
*(Format for Affidavit on Non-Judicial Stamp Paper)*

```text
AFFIDAVIT / DECLARATION OF NON-BLACKLISTING

I, ________________________, Director / Authorized Representative of M/s GovTrace Solutions,
do hereby solemnly affirm and state as follows:

1. That the bidder M/s GovTrace Solutions has not been blacklisted, debarred, or banned by any
   Central / State Government Department, Public Sector Undertaking (PSU), Autonomous Body,
   or Court of Law in India as on the date of bid submission.
2. That no vigilance case or criminal proceeding is pending or contemplated against the bidder or its directors.
3. That the firm maintains full integrity and has not paid or engaged in any corrupt or fraudulent
   practices in securing this or any public procurement tender.

DEPONENT
Verified at ____________ on this 28th day of September 2026.
```

---

### Annexure D: Bid Security Declaration (In Lieu of EMD)
*(Applicable for DPIIT Recognized Startups and MSMEs under GFR 2017 Rule 170)*

```text
BID SECURITY DECLARATION

Tender Ref: GT-TN-PROC-2026-T01

We, M/s GovTrace Solutions, declare that:
If we withdraw or modify our bid during the period of validity, or if we are awarded the contract
and fail to sign the contract or submit a performance security before the deadline defined in the
tender documents, we accept that we will be suspended from being eligible to bid in any tender
floated by the State Government for a period of two (2) years.

(Authorized Signatory & Seal)
```

---

## 3. Evaluation Committee Live Demonstration Guide (Winning the 70% Technical Score)

When called for the Technical Presentation / Proof-of-Concept (PoC) evaluation before the Technical Scrutiny Committee, execute this exact **15-minute demonstration script** to showcase operational superiority:

| Time | Demonstration Step | Screen / URL | What to Highlight to the Committee |
| :---: | :--- | :--- | :--- |
| **00:00 - 02:00** | **Multi-Sector Portal** | `#sectors` | Show all 8 sectors (Food/PDS, Dairy, Pharma, Cement, Handloom, Spices, Mining, Crafts). Highlight that **one single system covers all state departments**, saving the government from paying for 8 separate software procurements. |
| **02:00 - 04:00** | **Product Master & Batch Creation** | `#batches` → `+ New Batch` | Show dynamic sector attributes (e.g. Rice Moisture %, Mandi Lot slip vs. Milk Fat % vs. CDSCO license). Demonstrate auto-generation of official batch codes (`TN-RICE-2026-000101`). |
| **04:00 - 06:00** | **Strict Quality Control Gating** | `#quality` → `Record QC` | Demonstrate that unpassed batches **cannot be transferred or dispatched**. Upload a lab test certificate and show immediate SHA-256 cryptographic hash computation. |
| **06:00 - 09:00** | **Anti-Diversion & Custody Variance** | `#transfers` → `Receive Transfer` | **The Climax Test:** Dispatch 5,000 KG. Enter 4,950 KG received (50 KG missing gunny bag). Show the system automatically flagging a `QUANTITY_MISMATCH` alert in the Vigilance Engine (`#exceptions`) and freezing the handoff! |
| **09:00 - 11:00** | **Citizen Digital Product Passport (DPP)** | `#passport` & `#qr` | Generate a dynamic QR code. Scan on a smartphone camera. Show the branded, tamper-proof certificate displaying origin, timeline, and FSSAI/BIS seal with **zero app download**. |
| **11:00 - 13:00** | **Geospatial GIS Tracking Map** | `#map` | Display live Tamil Nadu map with 27 actual facilities (Thanjavur godowns, Salem chilling centers, Chennai ports) and active transit corridors. |
| **13:00 - 15:00** | **Audit Trail & Low-Cost Commercials** | `#audit` & BoQ Presentation | Show the immutable cryptographic event log. Conclude with the **₹32 Lakhs 3-year turnkey pricing**, proving a direct **91% savings** compared to legacy ₹3.5 Crore IT consulting bids. |

---

## 4. Key Procurement FAQ for Tender Committees

**Q1: How does GovTrace ensure data privacy between different government departments?**  
*Answer:* GovTrace implements Row-Level Security (RLS) and schema-isolated multi-tenancy. Civil Supplies officers cannot view confidential Drug Corporation consignments or Mining leases without formal cross-department role authorization.

**Q2: Can GovTrace integrate with existing government weighbridges and mandi portals?**  
*Answer:* Yes. GovTrace includes a lightweight REST API adapter layer that ingests automated electronic weighbridge slips, GPS telematics from transit trucks, and e-Pass records from state portals.

**Q3: What happens if internet connectivity drops at a rural godown?**  
*Answer:* The GovTrace presentation layer operates as a Progressive Web App (PWA) with local caching. Custody entries can be captured offline and synchronized securely once connectivity is restored.

**Q4: Is the government locked into a proprietary vendor?**  
*Answer:* Absolutely not. GovTrace uses standard PostgreSQL, Python/Node, Linux, and open standards. The state receives perpetual deployment rights with zero recurring proprietary software license fees.
