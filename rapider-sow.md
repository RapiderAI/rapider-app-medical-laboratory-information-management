<!-- Rapider Official App SOW & Specification | App #157: Medical Laboratory Information Management (medical-laboratory-information-management) | Domain: HLT -->

# #157 Medical Laboratory Information Management (`medical-laboratory-information-management`) — Scope of Work (SOW)

- **App Number & Slug:** `#157` (`157-medical-laboratory-information-management`)
- **App Key:** `medical-laboratory-information-management`
- **Suite / Domain & Model Prefix:** `16. Healthcare & Wellness` (`HLT` — `Hlt*`)
- **Representative Benchmark (`P1`):** LabWare Cloud
- **Combined Feature Catalog:** [../combined-features/157-medical-laboratory-information-management-features.md](../combined-features/157-medical-laboratory-information-management-features.md)

---

## 1. Executive Outcome & Target Audience

`Rapider Medical Laboratory Information Management` (`medical-laboratory-information-management`) unifies Clinical Laboratory Information System (LIS) velocity with Laboratory Information Management System (LIMS) chain-of-custody and quality governance across Physician Office Labs (POLs), hospital core laboratories, specialty molecular/toxicology centers, and anatomic pathology practices. From electronic HL7/FHIR requisition intake and barcode specimen aliquoting through bidirectional analyzer interfacing, Westgard QC lockouts, deterministic delta-check auto-verification, and 15-minute CLIA panic-value read-back enforcement, `medical-laboratory-information-management` eliminates specimen mix-ups, compresses STAT turnaround times, and guarantees inspection-ready CLIA '88, CAP, and ISO 15189 traceability.

- **SME Fast-Path Defaults:** Independent clinic POLs and boutique diagnostic labs launch within hours using pre-seeded LOINC/CPT test & panel templates (`CBC`, `CMP`, `Lipid`, `HbA1c`, `Urinalysis`, `Rapid Strep/Flu`), barcode specimen label generation, clean bench result entry/import queues, stratified age/sex normal intervals, and instant HL7 FHIR `DiagnosticReport` delivery to `#151 EHR`.
- **Enterprise Progressive Disclosure:** High-volume reference laboratories and multi-hospital networks activate bidirectional `ASTM E1394` / `CLSI LIS02-A2` analyzer host-query interfaces, Westgard multi-rule Levey-Jennings QC & expired reagent-lot lockouts, rule-based auto-verification with 72h historical delta checks, reflex/cascade testing, microbiology `MIC`/`S-I-R` antibiograms, anatomic pathology gross -> cassette -> slide barcoding, freezer rack storage coordinates, and public health `ELR` reporting.
- **Measurable Business Outcomes:**
  1. Achieve **>= 75% safe routine result auto-verification** on core chemistry/hematology benches while maintaining **zero release of out-of-control QC or unreviewed critical/delta-check results**.
  2. Enforce **>= 98% on-time (`<= 15 min`) CLIA critical/panic value verbal read-back documentation** via automated escalation timers and Rapider Home `WorkItem`s.
  3. Reduce **pre-analytical specimen rejection and billing claim denials by >= 35%** through draw-time container calculation, positive barcode ID, and medical-necessity ABN checks.
- **Top 6 Global Benchmarks Synthesized (`P1`–`P6`):** `P1` LabWare Cloud (CLIA/CAP LIS+LIMS, Westgard QC & auto-verification), `P2` Clinisys / Orchard Enterprise (multi-bench clinical/microbiology workflows & LCD/ABN medical necessity), `P3` LigoLab (anatomic pathology gross-to-slide chain-of-custody & CPT scrubbing), `P4` LabVantage Medical (biobank freezer hierarchy, PCR plates & reagent/calibration lockouts), `P5` Oracle Health PathNet (enterprise hospital FHIR `ServiceRequest`/`Specimen`/`DiagnosticReport` loop), and `P6` ERPNext Healthcare Laboratory (fast SME/POL test templates & clean financial handoff).

---

## 2. In-Scope, Out-of-Scope & Suite Ownership Boundaries

### In-Scope
- Standardized diagnostic compendium (`HltLabTestDefinition`) with LOINC, SNOMED specimen, CPT codes, CLIA complexity, age/sex/gestational reference bounds, panic bounds, and reflex/cascade rules.
- Electronic order intake (`HltLabOrder`) supporting STAT/Routine priority, ICD-10 medical necessity & ABN consent, send-out reference routing, and clean CPT charge readiness.
- Barcode specimen lifecycle (`HltLabSpecimen`) covering draw calculation, central accessioning, parent-child aliquoting, AP cassette/slide hierarchy, rejection codes, and post-analytical freezer coordinates.
- Bidirectional analyzer run & plate management (`HltLabAnalyzerRun`) with reagent lot (`OpsInventoryItem`) and instrument calibration (`OpsOperationsAsset`) validation.
- Quantitative, qualitative, microbiology (`MIC`/`S-I-R`), and pathology result processing (`HltLabResult`) with Westgard QC evaluation (`HltLabQcRun`), delta-check auto-verification, and mandatory critical read-back logging (`HltLabCriticalCallLog`).
- Immutable signed `PRELIMINARY` / `FINAL` / `CORRECTED` diagnostic reports (`HltLabDiagnosticReport`), public health `ELR` queuing, and Grounded AI contamination/interpretive drafting.

### Out-of-Scope
- **Longitudinal Patient Chart, Problem List & Medication Prescribing:** Owned canonically by `#151 Electronic Health Records (EHR)` (`HltPatient`, `HltPractitioner`, `HltEncounter`).
- **Physical Reagent Purchasing, Warehouse Stock Ledger & Instrument Preventive Maintenance Work Orders:** Owned by `Rapider Inventory`, `Procurement`, and `Maintenance` (`OpsInventoryItem`, `OpsInventoryPosition`, `OpsProcurementRequest`, `OpsOperationsAsset`, `MfgMaintenanceWorkOrder`).
- **Industrial Non-Clinical Manufacturing Quality Inspection:** Owned by `Rapider Quality` (`MfgQualityInspection`); `medical-laboratory-information-management` owns CLIA/CAP clinical diagnostic Westgard QC (`HltLabQcRun`).
- **Payer Clearinghouse EDI 837P Claims & General Ledger Posting:** `#154 Medical Billing & Claims` adjudicates insurance claims and `FIN` (`FinInvoice`, `FinJournalEntry`) manages client/patient receivables with zero laboratory result PHI exposure.

### Cross-Suite Ownership Contract

| Domain / App | Owns Canonically | Integration Contract with `medical-laboratory-information-management` (`#157`) |
| :--- | :--- | :--- |
| **`medical-laboratory-information-management` (`#157` — `HLT`)** | `HltLabTestDefinition`, `HltLabOrder`, `HltLabSpecimen`, `HltLabAnalyzerRun`, `HltLabResult`, `HltLabQcRun`, `HltLabCriticalCallLog`, `HltLabDiagnosticReport` | Publishes `hlt.lab.specimen_rejected`, `hlt.lab.critical_value_flagged`, `hlt.lab.report_finalized`, `hlt.lab.report_corrected`, `hlt.lab.charge_packet_ready` |
| **`#151 Electronic Health Records` & `#155 Patient Engagement` (`HLT`)** | `HltPatient`, `HltPractitioner`, `HltEncounter`, `HltObservation`, Patient Portal | Submits `ServiceRequest` lab orders; receives `DiagnosticReport` & `Observation` updates, recollection alerts, and Cures Act portal releases |
| **`#154 Medical Billing` -> `FIN` (`billing` / `acc`)** | `HltClaim` (`#154`), `FinInvoice`, `FinJournalEntry` | Consumes `hlt.lab.charge_packet_ready` containing CPT/HCPCS, ICD-10, CLIA ID, ABN modifier, and `Decimal128` fee refs (`\$0` test result or HIV/genetic/tox data in `FIN`) |
| **`Rapider Inventory & Assets` (`OPS` / `MFG`)** | `OpsInventoryItem`, `OpsOperationsAsset`, `MfgMaintenanceWorkOrder` | Supplies active reagent lot expiration dates, analyzer calibration status, and freezer asset references; receives reagent reorder proposals |
| **`Rapider HR & Learning` (`HR` / `LRN`)** | `HrEmployee`, `HrLocation`, `LrnCredential` | Verifies CLIA Moderate/High-complexity technologist & pathologist competency credentials before result verification |

---

## 3. Primary Web Pages & Sidebar Navigation Architecture (12 Pages)

All visible sidebar groups strictly satisfy the **Minimum 3 Items Rule (`count >= 3`)**: `Overview` (3 items), `Bench Operations` (3 items), and `Quality & Compendium` (3 items), while detail and report sign-out views set `displayOnMenu: false`.

| # | Route | Page Title | Sidebar Section | Menu Visible | Key Components & UX |
| :---: | :--- | :--- | :--- | :---: | :--- |
| 1 | `dashboard` | Lab Command & TAT Hub | `Overview` | `true` (1) | KPI cards (STAT p90 TAT mins, Auto-Verification %, Active Panic Read-Backs, Specimen Rejection %, Westgard Bench Status), hourly accession throughput, and AI QC/contamination alerts. |
| 2 | `orders-accessioning` | Orders & Accessioning | `Overview` | `true` (2) | Dual view (`<rapider-kanban>` across `ORDERED`, `COLLECTED`, `ACCESSIONED`, `IN_ANALYSIS`, `REPORTED` + paginated table) with `#activeFilterTagsBar`, barcode scan input, aliquot/reject drawers, and clickable `orderNumber` -> `lab-order-detail`. |
| 3 | `critical-readbacks` | Critical Panic Read-Backs | `Overview` | `true` (3) | Real-time 15-min SLA queue of `HltLabCriticalCallLog` items with ordering clinician `<rapider-phone-input>` dialer details, verbatim read-back confirmation modal, and Home `WorkItem` escalation indicator. |
| 4 | `bench-worklists` | Core & Micro Worklists | `Bench Operations` | `true` (1) | High-density bench review workbench filtering by department (`CHEMISTRY`, `HEMATOLOGY`, `MICROBIOLOGY`, `MOLECULAR`, `PATHOLOGY`), delta-check flags, repeat/dilution triggers, and batch technologist verify. |
| 5 | `analyzer-runs` | Analyzer Runs & Plates | `Bench Operations` | `true` (2) | `HltLabAnalyzerRun` monitor showing ASTM/HL7 host queries, 96-well PCR plate control status, active reagent lot badges, and instrument calibration lockout warnings. |
| 6 | `specimen-storage` | Specimens & Freezer Racks | `Bench Operations` | `true` (3) | `HltLabSpecimen` chain-of-custody & biobank rack locator (`Freezer -> Rack -> Box -> Well`), CAP retention countdowns, send-out manifests, and disposal audit actions. |
| 7 | `qc-westgard` | Westgard QC & Charts | `Quality & Compendium` | `true` (1) | `HltLabQcRun` Levey-Jennings control charts, `1_2s`/`1_3s`/`2_2s`/`R_4s` status badges, bench lockout override/corrective-action drawer, and AI drift warnings. |
| 8 | `diagnostic-reports` | Final & Corrected Reports | `Quality & Compendium` | `true` (2) | Paginated `HltLabDiagnosticReport` registry (`PRELIMINARY`, `FINAL`, `CORRECTED`), pathologist sign-out queue, Cures Act portal status, and `ELR` public health transmission badges. |
| 9 | `test-compendium` | Test & Panel Directory | `Quality & Compendium` | `true` (3) | Catalog of `HltLabTestDefinition` assays & panels with LOINC/CPT codes, CLIA complexity chips, age/sex reference intervals, reflex rules, and compendium editor drawer. |
| 10 | `lab-order-detail` | Requisition & Result Workspace | — | `false` | Complete specimen tree (parent tube -> aliquots / cassettes -> slides), analyte delta history, auto-verification audit trace, Grounded AI contamination/interpretation thread (`projectChannelMessage`), and `#btnGoBack`. |
| 11 | `test-definition-detail` | Assay & Reference Range Detail | — | `false` | Stratified age/sex/gestational `Decimal128` intervals, panic bounds, delta-check formulas, reflex cascade builder, and `#btnGoBack` to `test-compendium`. |
| 12 | `report-signout-detail` | Pathologist / Director Sign-Out | — | `false` | Synoptic CAP checklist / interpretive narrative editor, AI draft verification, cryptographic sign-out, and versioned `CORRECTED` amendment workflow. |

**UI & Component Compliance:** Every page uses `findSWR` / `countSWR` without redundant client `isDeleted` filter clauses and filters `item.isDeleted !== true` in memory. Data tables omit standalone `View` buttons in `Actions`, navigate via clickable primary identifiers (`orderNumber`, `accessionBarcode`, `reportNumber`), render icon+text `Edit` and icon-only `Delete`, use `<rapider-phone-input>` for critical call-back numbers and `<rapider-kanban>` for accession workflow tracking, enforce `#ffffff !important` white text on primary/danger buttons, and ban native browser `alert`/`confirm`/`prompt`.

---

## 4. Canonical Data Models (`Hlt*` — 8 Models)

All models omit `projectId` / `workspaceId`, use platform UUIDs (`00000000-xxxx-xxxx-xxxx-xxxxxxxxxxxx` in deterministic seed data), enforce optimistic concurrency `version`, and store analyte measurements, reference bounds, QC statistics, and fee amounts as `Decimal128`.

| Model Name | Key Fields & Types | Relationships & External Refs | Invariants & Lifecycle |
| :--- | :--- | :--- | :--- |
| **`HltLabTestDefinition`** | `testCode` (String, unique), `name` (String), `isPanel` (Boolean), `department` (`CHEMISTRY\|HEMATOLOGY\|COAGULATION\|URINALYSIS\|MICROBIOLOGY\|MOLECULAR\|ANATOMIC_PATHOLOGY\|SEND_OUT`), `loincCode` (String), `snomedSpecimenCode` (String), `cptCode` (String), `cliaComplexity` (`WAIVED\|MODERATE\|HIGH`), `ucumUnit` (String), `containerType` (String), `minVolumeMl` (`Decimal128`), `statTatMinutes` (Int), `routineTatMinutes` (Int), `referenceRangesJson` (JSON: sex, minAgeDays, maxAgeDays, low, high, critLow, critHigh, linearityMin, linearityMax), `deltaCheckPct` (`Decimal128`), `deltaWindowHours` (Int), `reflexRulesJson` (JSON[]), `is42CfrPart2Sensitive` (Boolean), `status` (`ACTIVE\|INACTIVE`), `version` (Int) | `panelChildTestIds` (UUID[] -> `HltLabTestDefinition`), `referenceVendorId` -> `OpsSupplierProfile` | Non-panel quantitative tests require valid `loincCode` and `ucumUnit`; `critLow <= low < high <= critHigh` in every stratified interval. |
| **`HltLabOrder`** | `orderNumber` (String, unique), `priority` (`STAT\|ASAP\|ROUTINE`), `status` (`ORDERED\|PARTIALLY_COLLECTED\|ACCESSIONED\|IN_ANALYSIS\|PARTIALLY_VERIFIED\|FINALIZED\|CANCELLED`), `icd10Codes` (String[]), `fastingHours` (Int), `abnStatus` (`NOT_REQUIRED\|SIGNED_ON_FILE\|REFUSED`), `clinicalHistoryNote` (String), `is42CfrPart2Segmented` (Boolean), `orderedAt` (DateTime), `chargeHandedOffAt` (DateTime), `version` (Int) | `patientId` -> `HltPatient` (`#151`), `orderingPractitionerId` -> `HltPractitioner` (`#151`), `encounterId` -> `HltEncounter` (`#151`), `orderedTestIds` (UUID[] -> `HltLabTestDefinition`), `drawLocationId` -> `HrLocation` | Orders containing `is42CfrPart2Sensitive == true` tests automatically enforce `is42CfrPart2Segmented = true` ACL isolation. |
| **`HltLabSpecimen`** | `accessionBarcode` (String, unique), `specimenRole` (`PRIMARY_TUBE\|ALIQUOT\|TISSUE_PART\|CASSETTE\|SLIDE`), `snomedSite` (String), `containerType` (String), `volumeMl` (`Decimal128`), `collectedAt` (DateTime), `receivedAt` (DateTime), `status` (`PENDING_DRAW\|COLLECTED\|IN_TRANSIT\|RECEIVED_ACCEPTED\|REJECTED\|ARCHIVED_STORAGE\|DISPOSED`), `rejectionReason` (`NONE\|HEMOLYZED\|CLOTTED\|QNS_INSUFFICIENT_VOLUME\|MISLABELED\|IMPROPER_TEMP\|EXPIRED_TRANSPORT`), `storageCoordinate` (String, e.g., `FRZ1-S2-R4-B1-A08`), `retentionUntil` (DateTime), `version` (Int) | `labOrderId` -> `HltLabOrder`, `parentSpecimenId` -> `HltLabSpecimen`, `collectedByEmployeeId` -> `HrEmployee`, `storageAssetId` -> `OpsOperationsAsset` | `REJECTED` specimens require a non-`NONE` `rejectionReason`, cannot accept verified `HltLabResult`s, and emit `hlt.lab.specimen_rejected`. |
| **`HltLabAnalyzerRun`** | `runNumber` (String, unique), `protocolType` (`ASTM_E1394\|HL7_LIS\|PCR_PLATE_96\|MANUAL_BENCH`), `reagentLotNumber` (String), `reagentExpiresAt` (DateTime), `calibrationValidUntil` (DateTime), `batchControlStatus` (`PASS\|FAIL\|NA`), `plateMapJson` (JSON), `startedAt` (DateTime), `completedAt` (DateTime), `version` (Int) | `analyzerAssetId` -> `OpsOperationsAsset`, `reagentItemId` -> `OpsInventoryItem`, `activeQcRunId` -> `HltLabQcRun`, `operatorEmployeeId` -> `HrEmployee` | **CLIA Reagent & Calibration Gate:** Patient result verification is blocked if `reagentExpiresAt < startedAt`, `calibrationValidUntil < startedAt`, or `batchControlStatus == FAIL`. |
| **`HltLabQcRun`** | `qcRunRef` (String, unique), `controlLevel` (`LEVEL_1_LOW\|LEVEL_2_NORMAL\|LEVEL_3_HIGH`), `lotNumber` (String), `targetMean` (`Decimal128`), `targetSd` (`Decimal128`), `measuredValue` (`Decimal128`), `zScoreSd` (`Decimal128`), `westgardViolation` (`NONE\|WARN_1_2S\|REJECT_1_3S\|REJECT_2_2S\|REJECT_R_4S\|REJECT_4_1S\|REJECT_10_X`), `qcStatus` (`IN_CONTROL\|WARNING_ONLY\|OUT_OF_CONTROL\|CORRECTED_RELEASED`), `correctiveActionNote` (String), `validUntil` (DateTime), `version` (Int) | `analyzerAssetId` -> `OpsOperationsAsset`, `testDefinitionId` -> `HltLabTestDefinition`, `reviewedBySupervisorId` -> `HrEmployee` | Any `REJECT_*` Westgard violation forces `qcStatus = OUT_OF_CONTROL` and halts auto-verification for `(analyzerAssetId, testDefinitionId)` until a fresh `IN_CONTROL` QC run passes. |
| **`HltLabResult`** | `resultRef` (String, unique), `numericValue` (`Decimal128`), `textOrCodedValue` (String), `ucumUnit` (String), `abnormalFlag` (`NORMAL\|LOW\|HIGH\|CRITICAL_LOW\|CRITICAL_HIGH\|ABNORMAL_QUAL`), `deltaFlagTriggered` (Boolean), `instrumentFlags` (String[]), `microSusceptibilityJson` (JSON: organism, antibiotic `MIC`/`S-I-R`), `verificationMode` (`PENDING\|AUTO_VERIFIED\|MANUAL_TECH_VERIFIED\|PATHOLOGIST_VERIFIED`), `status` (`PRELIMINARY\|VERIFIED_FINAL\|AMENDED`), `verifiedAt` (DateTime), `version` (Int) | `labOrderId` -> `HltLabOrder`, `specimenId` -> `HltLabSpecimen`, `testDefinitionId` -> `HltLabTestDefinition`, `analyzerRunId` -> `HltLabAnalyzerRun`, `verifiedByUserId` -> Platform User | **Auto-Verification Safety Invariant:** `AUTO_VERIFIED` is forbidden whenever `abnormalFlag` is `CRITICAL_*`, `deltaFlagTriggered == true`, `instrumentFlags` is non-empty, or linked `HltLabQcRun` is not `IN_CONTROL`. |
| **`HltLabCriticalCallLog`** | `callLogRef` (String, unique), `criticalValueSummary` (String), `flaggedAt` (DateTime), `slaDueAt` (DateTime), `notifiedAt` (DateTime), `recipientName` (String), `recipientCredential` (`MD\|DO\|NP\|PA\|RN`), `recipientPhone` (String), `readBackConfirmedVerbatim` (Boolean), `escalationTier` (Int), `status` (`OPEN_SLA_ACTIVE\|COMPLETED_READBACK\|ESCALATED_BREACH`), `version` (Int) | `labResultId` -> `HltLabResult`, `labOrderId` -> `HltLabOrder`, `patientId` -> `HltPatient`, `orderingPractitionerId` -> `HltPractitioner`, `callerUserId` -> Platform User, `workItemId` -> Home `WorkItem` | **CLIA Read-Back Invariant:** Cannot transition to `COMPLETED_READBACK` unless `recipientName != ""`, `recipientCredential` is populated, and `readBackConfirmedVerbatim == true`. |
| **`HltLabDiagnosticReport`** | `reportNumber` (String, unique), `revisionNumber` (Int), `reportStatus` (`DRAFT_AI_ASSISTED\|PRELIMINARY\|FINAL\|CORRECTED\|AMENDED`), `aiInterpretiveDraft` (String), `signedInterpretation` (String), `synopticChecklistJson` (JSON), `correctionReason` (String), `elrSubmissionStatus` (`NOT_REPORTABLE\|QUEUED\|SUBMITTED`), `portalReleasedAt` (DateTime), `signedAt` (DateTime), `signatureHash` (String), `version` (Int) | `labOrderId` -> `HltLabOrder`, `patientId` -> `HltPatient`, `resultIds` (UUID[] -> `HltLabResult`), `signingPractitionerId` -> `HltPractitioner`, `supersedesReportId` -> `HltLabDiagnosticReport`, `driveFileId` -> Rapider Drive | **CLIA/CAP Final Report Immutability:** `FINAL` reports cannot be mutated in place; post-finalization edits must create a new revision (`revisionNumber + 1`, `reportStatus = CORRECTED`, `supersedesReportId`) with mandatory `correctionReason`. |

---

## 5. Core End-to-End User Journeys (6 Journeys)

### Journey 1: SME/POL Fast-Path — EHR Order Intake, Barcode Draw & Auto-Verified Release
1. `#151 EHR` submits a `Routine` `Comprehensive Metabolic Panel` + `Lipid Panel` order (`HltLabOrder`) for `HltPatient`.
2. In `orders-accessioning`, the phlebotomist verifies two patient identifiers, prints a Gold `SST` barcode tube (`ACC-2026-8801`), stamps `collectedAt` and `receivedAt`, and loads the tube onto the chemistry analyzer.
3. The analyzer queries the barcode via `ASTM`, runs the 18 analytes against an active `IN_CONTROL` `HltLabQcRun` and valid reagent lot, and transmits results.
4. All 18 analytes pass linearity, normal/auto-verify bounds, and 72h delta checks -> `HltLabResult` transitions to `AUTO_VERIFIED`, publishing `FINAL` `HltLabDiagnosticReport` to `#151 EHR` and emitting a sanitized CPT `80053` + `80061` charge packet to `#154 Medical Billing`.

### Journey 2: Critical Panic Value (`Potassium 6.9 mmol/L`) -> 15-Minute Read-Back SLA & Home `WorkItem`
1. A `STAT` electrolyte run returns serum Potassium `6.9 mmol/L` (`CRITICAL_HIGH`, normal `3.5–5.1`), blocking auto-verification and routing to the Medical Technologist bench queue.
2. After checking hemolysis index (`0 — clear serum`), the technologist manually verifies the result, immediately creating `HltLabCriticalCallLog` (`slaDueAt = +15m`) and a high-priority Rapider Home `WorkItem` for the ordering physician.
3. From `critical-readbacks`, the technologist calls the physician (`<rapider-phone-input>`), documents `recipientName`, `MD`, and checks `readBackConfirmedVerbatim = true`, closing the CLIA panic loop in `6 minutes`.

### Journey 3: Westgard `1_3s` QC Violation Lockout & Supervisor Corrective Action
1. Morning `LEVEL_3_HIGH` Troponin control measures `+3.4 SD` (`REJECT_1_3S`), transitioning `HltLabQcRun` to `OUT_OF_CONTROL`, rendering a red point on the Levey-Jennings chart (`qc-westgard`), and dispatching a Lab Supervisor Home `WorkItem`.
2. While `OUT_OF_CONTROL`, incoming patient Troponin runs are **hard-blocked** from `AUTO_VERIFIED` or final release.
3. The supervisor recalibrates with a fresh reagent pack, logs `correctiveActionNote`, and runs a passing `IN_CONTROL` QC (`+0.4 SD`), restoring bench verification.

### Journey 4: Reflex Testing, Pathologist AI-Assisted Sign-Out & Corrected Report Versioning
1. An abnormal `TSH` (`9.8 mIU/L`) automatically spawns a child `Free T4` reflex test on parent tube `ACC-2026-8809` (`LIM-003`).
2. For a surgical biopsy case (`Part A` -> `Cassette A1` -> `Slide A1-1`), Grounded AI drafts `aiInterpretiveDraft` (`DRAFT_AI_ASSISTED`) summarizing immunohistochemical markers and citing `HltLabResult` IDs.
3. On `report-signout-detail`, the board-certified Pathologist edits the diagnosis, completes the CAP synoptic checklist, and clicks **Sign Final Report** (`FINAL`). If an addendum/correction is later needed, **Issue Corrected Report** creates `revisionNumber = 2` (`CORRECTED`) preserving `revisionNumber = 1` intact and firing `hlt.lab.report_corrected` to `#151 EHR`.

### Journey 5: Specimen Hemolysis Rejection & Instant Recollection Loop
1. Central receiving inspects an incoming coagulation tube that is grossly clotted (`rejectionReason = CLOTTED`) and marks `HltLabSpecimen` as `REJECTED` via confirmation modal.
2. `hlt.lab.specimen_rejected` immediately notifies the `#151 EHR` nursing station to redraw without billing any CPT charge.

### Journey 6: List -> Detail -> Back Navigation & Active Filter Tags Restoration
1. On `bench-worklists`, a technologist types `"Potassium"` and selects `Department: "CHEMISTRY"` + `Flag: "CRITICAL_HIGH"`; `#activeFilterTagsBar` renders `Search: "Potassium" ✕`, `Department: "CHEMISTRY" ✕`, `Flag: "CRITICAL_HIGH" ✕`, and `[Clear all]`.
2. Clicking `ORD-2026-1104` navigates cleanly to `lab-order-detail`; clicking `#btnGoBack` returns to `bench-worklists` with all active tag chips and filtered rows intact.

---

## 6. Consolidated Feature Priorities & Work Packages

### `P0` Foundation (Core Clinical LIS, Westgard QC, Auto-Verify & CLIA Panic Loop)
- **WP-P0-1 (`LIM-001`, `LIM-002`, `LIM-005`–`LIM-007`):** LOINC/SNOMED/CPT test & panel compendium (`HltLabTestDefinition`), stratified reference/panic bounds, HL7/FHIR `HltLabOrder` intake, barcode `HltLabSpecimen` draw calculation, accessioning, aliquoting, and rejection loop.
- **WP-P0-2 (`LIM-010`, `LIM-011`, `LIM-015`, `LIM-016`):** Bidirectional ASTM/HL7 `HltLabAnalyzerRun` ingestion, STAT bench queues, Westgard Levey-Jennings `HltLabQcRun` lockouts, and reagent lot / calibration expiration gates.
- **WP-P0-3 (`LIM-018`–`LIM-022`):** Delta-check auto-verification (`HltLabResult`), 15-min CLIA `HltLabCriticalCallLog` read-back ledger, immutable `FINAL` / `CORRECTED` `HltLabDiagnosticReport`, sanitized `#154` charge packet handoff, and Lab TAT Command Center.

### `P1` Scale & Specialty Diagnostics (Microbiology, Pathology, Molecular & ELR)
- **WP-P1-1 (`LIM-003`, `LIM-004`, `LIM-008`, `LIM-009`):** Reflex/cascade standing rules, reference lab send-out manifests, Medicare LCD/NCD & signed ABN verification, and freezer/rack archival coordinates (`OpsOperationsAsset`).
- **WP-P1-2 (`LIM-012`–`LIM-014`, `LIM-017`, `LIM-023`, `LIM-024`):** Microbiology culture & CLSI `MIC`/`S-I-R` antibiograms, Anatomic Pathology cassette/slide chain-of-custody & CAP synoptic checklists, 96/384-well PCR batch control validation, CLIA High-complexity personnel credential checks (`LrnCredential`), public health `ELR`, and Cures Act / 42 CFR Part 2 portal release rules.

### `P2` Grounded Laboratory AI & Predictive Operations
- **WP-P2-1 (`LIM-025`–`LIM-028`):** Grounded AI pre-analytical EDTA/IV contamination flags, AI-drafted pathologist/director interpretive comments (gated by mandatory MD/PhD sign-off), Westgard QC systematic drift early-warning alerts, and reagent consumption reorder forecasting to `OPS`.

---

## 7. Roles, Permissions & Field-Level Governance

| Role | Permitted Actions | Restricted / Field-Level Boundaries |
| :--- | :--- | :--- |
| **`Phlebotomist / Accessioning Clerk`** | View draw queues, verify patient ID, print specimen barcodes, log collection/receipt, aliquot tubes, flag pre-analytical specimen rejections | Cannot enter/verify analytical results, view finalized HIV/genetic/42 CFR Part 2 narratives, or alter QC runs. |
| **`Medical Laboratory Technologist (MLS / MT)`** | Operate analyzers, review Westgard QC, perform manual dilutions/reruns, verify `WAIVED`/`MODERATE` (and `HIGH` if `LrnCredential` verified) results, log critical read-backs | Cannot sign out Anatomic Pathology surgical diagnoses or override `OUT_OF_CONTROL` Westgard rules without documented corrective action & fresh QC. |
| **`Laboratory Technical Supervisor`** | Review & release Westgard QC corrective actions, manage reagent lot & analyzer calibration mappings, audit TAT & Proficiency Testing | Cannot mutate `FINAL` reports in place (must follow audited `CORRECTED` workflow); 42 CFR Part 2 substance-use orders restricted to segmented staff. |
| **`Pathologist / Laboratory Director (MD / DO / PhD)`** | Author `HltLabTestDefinition` compendium & auto-verification rules, review/edit AI drafts, cryptographically sign pathology & complex interpretive `HltLabDiagnosticReport`s, issue `CORRECTED` reports | All clinical sign-outs require active NPI & CLIA director/pathologist credential; cannot alter immutable historical audit revisions. |
| **`Lab Billing / Outreach Coordinator`** | Verify ICD-10 medical necessity & signed ABNs, inspect sanitized CPT charge readiness (`80053`, `85025`, etc.), manage B2B clinic outreach accounts | Zero visibility into quantitative analyte values, microbiology organisms, pathology narratives, or 42 CFR Part 2 toxicology findings. |

---

## 8. Technical APIs, Guarded Commands, Events & `requiredWorkflows`

### Guarded Domain Commands
Every mutating command enforces `idempotencyKey`, `correlationId`, and `expectedVersion`:
- `hlt.lab.accessionAndAliquotSpecimen` — Validates container/order state, generates unique parent/aliquot/cassette/slide barcodes, and starts TAT tracking.
- `hlt.lab.ingestAnalyzerRun` — Verifies reagent expiration & calibration windows, evaluates `HltLabQcRun` Westgard rules, ingests `HltLabResult`s, triggers reflex child tests, and executes safe auto-verification.
- `hlt.lab.recordCriticalReadBack` — Validates recipient identity, credential, and `readBackConfirmedVerbatim == true`, closing `HltLabCriticalCallLog` and linked Home `WorkItem`.
- `hlt.lab.finalizeOrCorrectReport` — Verifies CLIA personnel competency / pathologist signature, locks `HltLabDiagnosticReport` (`FINAL` or new `CORRECTED` revision), emits HL7 `ORU^R01` / FHIR R4 `DiagnosticReport`, and dispatches sanitized CPT charges.

### Domain Events & `requiredWorkflows` (`app.manifest.json`)
- **Published Events:** `hlt.lab.order_accessioned`, `hlt.lab.specimen_rejected`, `hlt.lab.qc_westgard_failed`, `hlt.lab.critical_value_flagged`, `hlt.lab.report_finalized`, `hlt.lab.report_corrected`, `hlt.lab.charge_packet_ready`.
- **Consumed Events:** `hlt.ehr.lab_service_requested` (`#151` CPOE order), `ops.asset.calibration_updated` (`OPS`/`MFG` instrument calibration status).
- **`requiredWorkflows`:**
  1. `labCriticalPanicReadBackEscalationWorkflow` — Tracks the 15-minute CLIA critical-value SLA timer, escalating unconfirmed `HltLabCriticalCallLog`s to the on-call covering physician/Medical Director via Home `WorkItem`s and urgent paging.
  2. `labReportDeliveryAndPublicHealthElrWorkflow` — Pushes `FINAL` and `CORRECTED` FHIR `DiagnosticReport`s to `#151 EHR`, schedules Cures Act `#155` portal release, and queues reportable-disease HL7 `ELR` submissions.
  3. `labSanitizedChargeToBillingWorkflow` — Aggregates performed/reflexed CPTs (collapsing component assays into billable CMS panels such as `80053` where required) and delivers PHI-free charge packets to `#154 Medical Billing`.

### Grounded Clinical AI Rules
- AI contamination flags and interpretive drafts **MUST** cite exact `HltLabResult` and `HltLabQcRun` UUIDs, obey HIPAA and **42 CFR Part 2** segmentation ACLs, never auto-release `CRITICAL_*` or pathology diagnoses, and require explicit human Pathologist/Director e-signature before transitioning out of `DRAFT_AI_ASSISTED`.

---

## 9. Security, Privacy, Compliance & Non-Functional Requirements

- **CLIA '88, CAP, ISO 15189 & 21 CFR Part 11 Traceability:** Every analyzer raw value, manual edit, technologist credential check, QC run, and report revision is recorded in an append-only tamper-evident audit log; laboratory test records retained `>= 2 years`, immunohematology/pathology reports `>= 10 years` per CLIA/CAP regulations.
- **HIPAA & 42 CFR Part 2 Substance-Use Disorder Segmentation:** Drug/toxicology requisitions originating from Part 2 programs (`is42CfrPart2Segmented = true`) enforce granular re-disclosure consent checks and are blocked from unauthorized cross-department browsing; general `FIN` ledgers (`FinInvoice`, `FinJournalEntry`) receive `\$0` diagnostic result content (only sanitized CPT/fee codes via `#154`).
- **Performance, Accessibility & Responsive UX:** Analyzer host-query response p95 `< 150ms`; high-density bench keyboard navigation & WCAG 2.2 AA compliance (distinguishable abnormal/panic symbols `L`, `H`, `LL!`, `HH!` alongside color); responsive across `390px` (mobile phlebotomy rover), `768px` (bench tablet), and `1440px` (multi-monitor core lab).

---

## 10. Deterministic Acceptance Criteria

1. **Westgard `OUT_OF_CONTROL` Auto-Verify Hard Block:** Recording a `REJECT_1_3S` or `REJECT_2_2S` `HltLabQcRun` sets `qcStatus = OUT_OF_CONTROL` and deterministically prevents any patient `HltLabResult` on that `(analyzerAssetId, testDefinitionId)` from reaching `AUTO_VERIFIED` or `VERIFIED_FINAL` until a subsequent `IN_CONTROL` QC run is recorded.
2. **Expired Reagent & Overdue Calibration Gate:** Submitting `hlt.lab.ingestAnalyzerRun` with `reagentExpiresAt < startedAt` or `calibrationValidUntil < startedAt` rejects verification deterministically.
3. **Delta-Check & Panic Exception Routing:** An analyte breaching `deltaCheckPct` within `deltaWindowHours` or hitting `CRITICAL_LOW`/`CRITICAL_HIGH` is never `AUTO_VERIFIED` and routes immediately to the manual technologist queue.
4. **CLIA Critical Read-Back Enforcement:** Attempting to close `HltLabCriticalCallLog` with `readBackConfirmedVerbatim == false` or blank `recipientName` fails validation; overdue open logs past `slaDueAt` escalate via Home `WorkItem`.
5. **Final Report Immutability & Corrected Versioning:** Attempting to overwrite a `FINAL` `HltLabDiagnosticReport` in place is rejected; invoking correction creates `revisionNumber + 1` (`CORRECTED`) referencing `supersedesReportId` with non-empty `correctionReason`.
6. **Specimen Rejection Block:** Marking an `HltLabSpecimen` as `REJECTED` (`HEMOLYZED`/`CLOTTED`) blocks result verification on that tube, suppresses CPT billing, and emits `hlt.lab.specimen_rejected`.
7. **Mandatory Pathologist Sign-Off on AI Drafts:** AI interpretive drafts remain `DRAFT_AI_ASSISTED` and cannot publish to `#151 EHR` or `#155` portal until a credentialed Pathologist/Lab Director signs off.
8. **42 CFR Part 2 & Zero Financial PHI Leakage:** `is42CfrPart2Segmented` toxicology results are hidden from non-authorized staff, and `hlt.lab.charge_packet_ready` payloads (e.g., CPT `80053` at `\$42.00`) contain zero analyte values, organism names, or pathology narratives.
9. **Sidebar Minimum 3 Items Rule:** `app.manifest.json` defines 3 visible items each under `Overview`, `Bench Operations`, and `Quality & Compendium`, with all 3 detail views hidden (`displayOnMenu: false`).
10. **SWR Clean Queries & Optimistic Updates:** All views use `findSWR`/`countSWR` without redundant client `isDeleted` clauses, update in-memory arrays and clear SWR cache optimistically on save/delete, and auto-close drawers/modals with `finally` button reset.
11. **Active Filter Tags & Clear All:** Filtering `orders-accessioning` or `bench-worklists` renders HTML-escaped chips in `#activeFilterTagsBar` with individual `✕` dismissal and `[Clear all]` reset.
12. **Table Action, Contrast & Real Click-Through E2E Journey:** Tables omit `View` buttons in `Actions`, render icon+text `Edit` and icon-only `Delete`, enforce `#ffffff !important` on primary/danger buttons with zero native browser dialogs, and pass real click-through navigation (`orders-accessioning` -> `lab-order-detail` -> `#btnGoBack`) across light and dark themes.
