**Impetus PLM QA** is a documentation-driven quality assurance repository built to define and maintain the complete testing lifecycle for the Impetus PLM application. It centralizes three key areas—**Test Strategy**, **Test Plan**, and **Test Cases**—providing a structured, traceable, and auditable QA process for a consumer-centric PLM platform.

### 1) **Test Strategy** (`/strategy/Impetus-PLM_Test-Strategy_v1.0.md`)

* Scope & objectives (PLM lifecycle, roles, environments)
* **Test levels & types** (Unit, API, UI, Integration, Regression, Smoke, Sanity, Exploratory; Non-functional: performance spot checks, security-lite, accessibility-lite; **load test** note for consumer traffic)
* **Risk-based prioritization** (P0/P1/P2/P3 with concrete PLM examples; P0 must pass)
* **Roles & permissions under test** (Designer, Buyer, TD, PM, Vendor — what each can/can’t do)
* **Lifecycle entry/exit rules** (mandatory fields, blockers, who can promote/demote, audit comments)
* **Test data strategy** (masked prod + synthetic, named datasets, reset/teardown)
* **Automation placeholder** (we note future Playwright stack but keep this repo docs-only)
* **Non-functional strategy** (in simple language; what we validate & why)
* **Defect policy + metrics overview**
* **Release gate criteria**
* **Integration fallback** (SAP sandbox down → contract tests + `@blocked-sap`)
* **Exploratory charter model**
* **Flaky SOP placeholder** (docs-only acknowledgment)
* **UAT governance (single QA lead model)**

The **Test Strategy** establishes the long-term vision and QA methodology. It defines test levels such as Unit, API, UI, Integration, Regression, Smoke, Sanity, Load, and Exploratory testing. Risk-based prioritization (P0–P3) ensures critical business workflows are always validated first. The strategy also includes non-functional validation for performance, data integrity, security, and accessibility. Every lifecycle stage—Designer, Buyer, Technical Designer, Product Manager, and Vendor—is covered with defined entry and exit criteria, mandatory fields, promotion rules, and blockers. It introduces an integration fallback policy: *If the SAP sandbox is unavailable, execute contract tests against a mock API, verify audit logs, and tag the scenario @blocked-sap.*

### 2) **Test Plan** (`/plan/Impetus-PLM_Test-Plan_v1.0.md`)

* Milestones & timeline (sprints/UAT window)
* In-scope vs out-of-scope
* Environments (DEV/QA/UAT/Staging) & access
* Test execution schedule (what runs daily vs weekly)
* Roles & RACI (lean, you as QA Lead)
* Entry/exit criteria per phase (QA/UAT/Go-Live)
* Reporting cadence (daily snapshot, weekly deck, UAT sign-off)

The **Test Plan** focuses on release execution, covering sprint timelines, environments (DEV, QA, UAT, STAGE), test schedules, resources, entry and exit criteria, and daily or weekly reporting cadence. It ensures that 100 % of P0 tests must pass, overall test pass rate exceeds 95 %, and no Severity 1 or 2 defects remain open before go-live.


### 3) **Test Cases** (Excel/CSV)

* **Columns** (recommend):
  `TC_ID, Title, UserRole, Area, Pre-reqs, Steps, Expected, Negative?, DataSet, Priority(P0–P3), Severity(S1–S4), Status, Comments, Trace(Story/Req-ID)`
* **Traceability-Matrix.csv**: `Req-ID, Req-Name, Module, TC_IDs, Status`
* **Naming**: `TC_<Module>_<Role>_<ShortName>_<nnn>`
  e.g., `TC_Lifecycle_Designer_PromoteWhenComplete_001`

The **Test Cases** section maintains all manual scenarios in Excel or CSV format, clearly mapped to requirements and datasets. Each case contains ID, module, user role, preconditions, steps, expected results, dataset reference, severity, and risk level.


### 4) **Checklists & Templates** (Strategy/Appendices)

* **Lifecycle Entry/Exit Checklist**
  Designer→Buyer / Buyer→TD / TD→PM / PM→Approved for Costing — with **mandatory fields** and **blockers**
* **Roles/Permissions Checklist**
  Visibility, actions allowed, promote/demote rights by role
* **Exploratory Charter Template**
  90-min sessions, persona-based, notes + debrief fields
* **Risk Register.csv**
  `Risk, Area, Impact, Likelihood, Class(P0..P3), Mitigation, Owner`

### 5) **Governance**

* **Defect Policy**: S1–S4, triage rules, hotfix/rollback & DB snapshot note
* **Metrics & Reporting**: KPI list (pass rate, defect density, automation coverage placeholder, leakage); daily/weekly formats
* **Release Gates**: Go/No-Go (100% P0, ≥95% overall, no S1/S2; No-Go if SAP unstable or critical blockers)

Impetus PLM QA provides a scalable foundation for consistent, repeatable testing. It enables transparent collaboration among QA, Product, and Engineering teams while laying the groundwork for future automation using Playwright and Cursor AI. This repository represents a disciplined, business-driven QA model designed to ensure accuracy, stability, and confidence in every Impetus PLM release.

