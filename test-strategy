# Purpose

**Why this exists**
To protect the end-to-end business flow of **Impetus PLM**—moving a style safely from **Designer → Buyer → TD → PM → Vendor → SAP**—by turning those stage gates into clear, testable rules.

**What we protect**

* Mandatory data at each stage (Main Image, Size Chart, BOM Main Material, HSN/MRP, Vendor, Techpack lock).
* Correct **promote/demote** behavior, role permissions, field locks, and audit comments.
* Clean **SAP handoff** (or approved mock fallback with evidence).

**What this strategy covers**

* Lifecycle **entry/exit criteria** per role and what blocks promotion.
* **Roles & permissions** checks for visibility and actions.
* **UI + API tests** for real workflows and data integrity across Style → Colorway → BOM → Size Chart → Techpack → SAP.
* **Risk-based** priorities (P0/P1/P2/P3) so release decisions are objective.
* **Non-functional basics**: performance spot checks, security-lite, accessibility-lite.
* **Fallback clause**: If SAP sandbox is unavailable, run contract tests against mock API and verify audit logs; mark end-to-end SAP push as **Deferred** with trace tag **@blocked-sap**.

**What success looks like**

* 100% **P0** pass, ≥95% overall pass, <3% UAT leakage.
* No broken promotions, accurate audit trails, valid SAP payloads (or approved deferral).

**Who uses this**
QA, Product, Dev, and UAT—to share one quality contract and ship with confidence.

# Scope

**What’s In (we will test):**

* **Web UI workflows:** Style → Colorway → BOM → Size Chart → Techpack → Vendor/SAP; form rules, blockers (yellow block), field locks, promote/demote, audit comments.
* **APIs & contracts:** CRUD for core entities, lifecycle transition APIs, payload schema/validation, error codes, idempotency, retries.
* **Roles & permissions:** Designer, Buyer, TD, PM, Vendor—menu/tab visibility, allowed actions, read/write locks per stage.
* **Lifecycle transitions:** Entry/exit criteria per stage, promotion/demotion rules, rejection/hold, audit trail integrity.
* **Data integrity:** Cross-module linkage (Style↔Colorway↔BOM↔Size Chart), mandatory fields, UOM/tolerance math, unique IDs, duplicate guards.
* **SAP integration:** Contract tests (schema, required fields), queueing/retry, success/failure states; **E2E SAP push** when sandbox is available.
* **Security (basic):** AuthN/AuthZ flows, session/SSO happy path, role escalation prevention, simple IDOR checks on key endpoints.
* **Accessibility (lite):** Labels, focus order on critical forms, keyboard reachability of promote/demote, contrast on key controls.
* **Performance spot checks:** BOM bulk upload, Techpack render, lifecycle promote under nominal load (baseline timings).
* **Environments/browsers:** QA/UAT/Staging; Chrome (primary) + Edge.

**What’s Out (we will not test now):**

* **Mobile UI** (responsive/mobile apps).
* **Legacy data migration** validation or backfill reconciliation.
* **Production data audits** beyond sanity of test runs.
* **Deep penetration testing** (red teaming, SAST/DAST beyond basic checks).
* **Full accessibility compliance** (WCAG AA end-to-end).
* **Performance/load at scale** (soak, stress, concurrency benchmarks).
* **Non-SAP external systems** beyond declared contracts.

**Assumptions & Boundaries:**

* Test data is synthetic or masked; no PII beyond approved samples.
* SAP sandbox may be intermittently unavailable. **If unavailable:** run contract tests against mock API, capture audit logs/correlation IDs, and mark E2E SAP push **Deferred** with **@blocked-sap** tag.
* Vendor portal access is provided for happy-path flows.
* UAT focuses on golden paths + high-risk negatives defined by business.

**Deliverables within Scope:**

* Test Strategy & Test Plan, role-based test cases (Excel), API contracts & Postman collections, Playwright suites (UI+API), daily/weekly quality reports, UAT entry/exit notes.

# 3) Test Levels & Types — **What we test, why it matters, and where it sits in the PLM flow**

Below is a crisp, stage-mapped catalog of every test type we’ll run for **Impetus PLM**. Each row tells you **why**, **where (stage/role)**, **what we check**, **sample data**, and **pass criteria** so anyone (tech or non-tech) can follow.

---

## A. Unit (Dev-owned)

| Aspect           | Details                                                                                                   |
| ---------------- | --------------------------------------------------------------------------------------------------------- |
| **Why**          | Catch logic bugs at source; cheapest fix point.                                                           |
| **Stages/Areas** | Designer (style code format), Buyer (MRP bracket validator), TD (POM math, grading), PM (unit cost calc). |
| **Checks**       | Formula math (POM, grading), range checks (MRP), required enums (Confirm Fit), unique Style code.         |
| **Data**         | `MRP_LOW`, `MRP_HIGH`, `POM_EDGE`, `STYLE_DUP`.                                                           |
| **Pass**         | 100% pass on critical validators; ≥80% code coverage on validators/helpers.                               |

---

## B. API (Contracts & Behavior)

| Aspect           | Details                                                                                                                                                                  |
| ---------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Why**          | The UI sits on these contracts; they power automation and integrations.                                                                                                  |
| **Stages/Areas** | CRUD for **Style/Colorway/BOM/Size Chart**, lifecycle endpoints (Promote/Demote/Hold/Reject), SAP payload queue, SSO session APIs.                                       |
| **Checks**       | 200 success paths; 4xx validation (missing Main image, MRP out-of-range); 401/403 authZ; 5xx retry/backoff; schema (required fields, types); idempotency (safe retries). |
| **Data**         | `STYLE_FULL`, `BOM_NO_MAIN`, `BUYER_BAD_MRP`, `SIZECHART_MISS_POM`.                                                                                                      |
| **Pass**         | All required fields enforced; stable schemas; safe re-submission returns same result; errors are meaningful and logged with correlation IDs.                             |

> **SAP fallback clause:** If SAP sandbox is unavailable, run contract tests against mock API and verify audit logs; mark end-to-end SAP push as **Deferred** with trace tag **@blocked-sap**.

---

## C. UI Functional (Role Workflows)

| Aspect           | Details                                                                                                                                                   |
| ---------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Why**          | Proves real user flows, blockers (yellow block), and promote/demote rules.                                                                                |
| **Stages/Areas** | Designer → Buyer → TD → PM → Vendor.                                                                                                                      |
| **Checks**       | Form rules, dropdown dependencies (HIT, HSN), **yellow-block refresh**, **Promote/Demote** visibility, audit comment required, field locks after promote. |
| **Data**         | Images (tagged **Main**), colorways (“Add to BOM”), BOM with **Main Material**, size chart (base + grading).                                              |
| **Pass**         | Each role can complete their mandatory view; blockers appear when data is missing; audit trails record who/when/why.                                      |

---

## D. Integration (PLM ↔ SAP / SSO / Vendor)

| Aspect       | Details                                                                                                                  |
| ------------ | ------------------------------------------------------------------------------------------------------------------------ |
| **Why**      | Handshakes are business-critical (costing, article creation, vendor turnaround).                                         |
| **Positive** | SAP success → status updates to “Article Created / Ready for Bulk”; vendor can download techpack and push sample status. |
| **Negative** | Timeouts, 5xx from SAP, auth errors from SSO, vendor upload failures; retries & queue behavior; correlation IDs in logs. |
| **Pass**     | Success path updates status + audit; failures are queued, retried, and visible; no data loss/duplication.                |

---

## E. Regression (Tagged “stable” flows)

| Aspect    | Details                                                                             |
| --------- | ----------------------------------------------------------------------------------- |
| **Why**   | Guard rails; fast feedback each commit and pre-release.                             |
| **Scope** | Golden path per role; critical negatives (e.g., missing Main image blocks promote). |
| **Run**   | Nightly on QA/UAT; pre-release on Staging.                                          |
| **Pass**  | ≥95% pass; 100% on all **P0** tests.                                                |

---

## F. Smoke (10–15 minutes)

| Aspect    | Details                                                                                                                                      |
| --------- | -------------------------------------------------------------------------------------------------------------------------------------------- |
| **Why**   | Quick confidence after deploy.                                                                                                               |
| **Steps** | Login → Create Style → Tag **Main** image → Load BOM & set **Main** material → Size Chart base/grade → Promote to Buyer → SAP **mock** push. |
| **Pass**  | All steps green; any fail = rollback or fix then re-deploy.                                                                                  |

---

## G. Sanity (Top 10 screens after each deploy)

| Aspect      | Details                                                                                                                                             |
| ----------- | --------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Why**     | Catch obvious UI/API breakages on critical pages.                                                                                                   |
| **Screens** | Styles list, Properties (Designer view), BOM, Size Chart, Promote dialog, Buyer mandatory view, TD samples, PM costing, Vendor portal, Audit trail. |
| **Pass**    | Loads fast, no console/API errors, key controls clickable.                                                                                          |

---

## H. Load & Performance (Consumer-grade baselines)

| Aspect        | Details                                                                                                                                                                                 |
| ------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Why**       | Ensure day-to-day responsiveness; we’re not stress-testing yet, just “feel fast”.                                                                                                       |
| **Scenarios** | **50 concurrent Designers** creating & promoting (P95 page nav < **3s**; BOM upload 500 rows < **10s**). **200 queued SAP pushes** (≥ **99%** success, mean < **60s**, retries logged). |
| **Pass**      | Targets met in QA/Staging; any regression ≥15% is flagged.                                                                                                                              |

---

## I. Data Integrity (Cross-module consistency)

| Aspect     | Details                                                                                                                                                                                      |
| ---------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Why**    | PLM value = single source of truth; links must never break.                                                                                                                                  |
| **Checks** | Style↔Colorway↔BOM↔Size Chart↔Techpack linkages; unique Style code; immutability after lock; UOM/tolerance math; colorway mapped to BOM placements; image tagged “Main” appears in techpack. |
| **Pass**   | No orphan/duplicate records; locked fields not editable; math consistent across sizes.                                                                                                       |

---

## J. Security-lite

| Aspect     | Details                                                                                                                                                 |
| ---------- | ------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Why**    | Prevent obvious role/URL bypasses without full pen-test scope.                                                                                          |
| **Checks** | SSO happy path; token expiry/refresh; **role isolation** (Designer cannot perform Buyer/PM actions by URL); **IDOR** on style/BOM/size chart endpoints. |
| **Pass**   | Unauthorized actions return 401/403; no cross-role data access; sessions refresh safely.                                                                |

---

## K. Accessibility-lite

| Aspect     | Details                                                                                                         |
| ---------- | --------------------------------------------------------------------------------------------------------------- |
| **Why**    | Usable by keyboard and assistive tech on critical flows.                                                        |
| **Checks** | Form labels, ARIA on Promote/Demote, logical **tab/focus** order, visible focus ring, contrast on key controls. |
| **Pass**   | Keyboard-only can complete mandatory forms & promotions; basic label/contrast present.                          |

---

## L. Exploratory (Chartered, 90 minutes)

| Aspect       | Details                                                                                                                                                                     |
| ------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Why**      | Find edge bugs scripts miss; validate real user behavior.                                                                                                                   |
| **Charters** | By role: Designer (asset/tag anomalies), Buyer (HIT change → demote/promote loops), TD (grading edge sizes), PM (lock/unlock dance), Vendor (late uploads, re-submissions). |
| **Output**   | Session notes + bugs + coverage map; feeds new regression tests.                                                                                                            |

---

### Mapping to Lifecycle (quick view)

| Stage                                               | Key Test Types                                                                                          |
| --------------------------------------------------- | ------------------------------------------------------------------------------------------------------- |
| **Designer – In Development**                       | UI Functional, Data Integrity, Accessibility-lite, Load (page/BOM), Security-lite (role).               |
| **Buyer – Release to Buying**                       | UI Functional, API (MRP/HSN validators), Data Integrity, Security-lite.                                 |
| **TD – Release to TD**                              | UI Functional, Unit (POM math), Data Integrity, Accessibility-lite.                                     |
| **PM – Release to Sourcing / Approved for Costing** | UI Functional, Integration (SAP), API (promote/lock), Data Integrity, Security-lite.                    |
| **Vendor / SAP**                                    | Integration (positive/negative, retries), API contracts, Performance (queue), Exploratory (turnaround). |

---

### Go/No-Go anchors for this section

* **Must pass:** Smoke, all **P0** UI/API/integration checks, data-integrity invariants.
* **Baseline met:** Load/Perf targets (P95 nav <3s; BOM 500 <10s; SAP queue mean <60s, ≥99% success).
* **SAP down?** Run mocks + contracts, tag scenarios **@blocked-sap**, capture audit logs & correlation IDs, and defer E2E SAP push.

This gives everyone—from business to engineering—a clear picture of **what we test, why it protects the release, and exactly where in the PLM lifecycle it applies**.
