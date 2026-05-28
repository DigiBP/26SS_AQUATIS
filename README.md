# Digitalised Source-to-Order with Supplier Selection

### **Team: Aquatis**

> **Module:** Digitalisation of Business Processes (DigiBP) — SS 2026
> **Programme:** FHNW · MSc Business Information Systems
> **Supervisors:** Andreas Martin · Charuta Pande · Devid Montecchiari

---

## Table of Contents

- [Project Members](#project-members)
- [Abstract & Project Overview](#abstract--project-overview)
- [Repository Structure](#repository-structure)
- [AS-IS Process](#as-is-process)
  - [Process Description](#process-description)
  - [Key Limitations](#key-limitations-of-the-as-is-process)
  - [AS-IS BPMN Diagram](#as-is-bpmn-diagram)
  - [Project Goal](#project-goal)
- [TO-BE Process](#to-be-process)
  - [Process Overview](#process-overview)
  - [TO-BE BPMN Diagram](#to-be-bpmn-diagram)
  - [Step-by-Step Walkthrough](#step-by-step-walkthrough)
  - [Challenges Addressed](#challenges-addressed-by-the-to-be-process)
  - [Users and Stakeholders](#users-and-stakeholders)
- [Decision Automation (DMN)](#decision-automation-dmn)
  - [Evaluate Supplier Response](#1-evaluate-supplier-response)
  - [Select Best Supplier](#2-select-best-supplier)
- [Technologies Used](#technologies-used)
- [Workflow Orchestration](#workflow-orchestration)
  - [Camunda BPMN Engine](#camunda-bpmn-engine)
  - [Make.com Integration](#makecom-integration)
  - [End-to-End Flow](#end-to-end-flow)
- [Digital User Interfaces (Forms)](#digital-user-interfaces-forms)
- [Limitations & Future Improvements](#limitations--future-improvements)
- [Project Workflow Agenda](#our-project-workflow-agenda)

---

# Project Members

## Project Team / Authors

| Name                         | Role                                  | Email                                     |
| ---------------------------- | ------------------------------------- | ----------------------------------------- |
| *Talip Ates*                 | *Make Scenarios & Documentation*      | `talip.ates@students.fhnw.ch`             |
| *Joanne Chimuti-Lobsiger*    | *As-IS BPMN & To-Be Documentation*    | `joanne.chimutilobsiger@students.fhnw.ch` |
| *Harpreet Kaur*              | *Forms, Presenation & Documentation*  | `harpreet.kaur@students.fhnw.ch`          |
| *Alexis Marquet*             | *Repository, Documentation*           | `alexis.marquet@students.fhnw.ch`     |
| *Lukas Uske*                 | *To-be BPMN, Make & Documentation*    | `lukas.uske@students.fhnw.ch`             |

## Supervisors

| Name               | Email                      |
| ------------------ | -------------------------- |
| Andreas Martin     | andreas.martin@fhnw.ch     |
| Charuta Pande      | charuta.pande@fhnw.ch      |
| Devid Montecchiari | devid.montecchiari@fhnw.ch |

---

## Abstract & Project Overview

This project digitalises and automates the **Source-to-Order (S2O) process** of a fictional office-supplies company that sells via an e-commerce platform. The driving scenario: a customer requests an **eco-friendly paper product** that is not in the existing catalogue and must be sourced from a new supplier.

In the **AS-IS** process, such sourcing requests are handled manually — triggered by phone calls or emails to the sales team. This leads to missed opportunities, slow response times, and an inconsistent customer experience.

The **TO-BE** process replaces manual handovers with a structured digital workflow. The customer submits a sourcing request via a web form; the system automatically records it, routes it through procurement, runs an automated supplier evaluation, generates a contract, and produces a purchase order — all orchestrated by **Camunda 7** (BPMN/DMN) and **Make.com**.

The core innovation lies in the **automated supplier selection** step: incoming supplier responses are scored using a DMN decision table (price × delivery time × compliance × quality × financial stability), and a second DMN compares the resulting scores to recommend the best candidate. A human reviewer retains the final say.

[⬆️ Back to Top](#table-of-contents)

---

## How to Run the Process

The implemented process combines Camunda, Make.com and Google Forms to demonstrate a digital source-to-order workflow with supplier selection. The process can be tested end-to-end by submitting a new material request and then following the created process instance in Camunda.

### 1. Start the Process

The process starts with the external Google Form **New Material Request**.

[Open New Material Request Form](https://docs.google.com/forms/d/e/1FAIpQLSeTFIsw-KNPZBVw3_ue3LixSkw15_3P3KyKSNXANNTvKQDmOw/viewform?usp=header)

After submitting the form, Make.com transfers the submitted request data to Camunda and starts a new process instance under the project tenant `26DIGIBP34`.

### 2. Follow the Process in Camunda

After the process has been started, it can be followed in the DigiBP Camunda Tasklist.

[Open DigiBP Camunda Tasklist](https://digibp.engine.martinlab.science/camunda/app/tasklist/default/#/login)

After logging in, select the project tenant `26DIGIBP34` and complete the generated user tasks in sequence. The main user tasks are:

1. **Review Customer Request and Catalogue Check**  
   Sales reviews the request and checks whether the requested material is available in the catalogue.

2. **Alternative Procurement Request**  
   If the requested material is not available or not feasible, Sales can propose an alternative product.

3. **Feasibility Check and RFQ Contact List**  
   Procurement checks feasibility and enters the supplier contacts for the RFQ.

4. **Review Supplier Response**  
   Procurement reviews the submitted supplier quotations.

5. **Review Contract Draft**  
   Procurement reviews the generated contract draft before finalisation.

### 3. Submit Supplier Responses

Supplier quotations are submitted through the external Google Form **RFQ Response**.

[Open RFQ Response Form](https://docs.google.com/forms/d/e/1FAIpQLSco4foSfT7dFTarkni2qyGCryNh_46HVuw1-V4ZWXt5Oj3vYg/viewform?usp=header)

In the actual process, the RFQ links are sent to suppliers with pre-filled fields such as the process instance ID and supplier information. This allows each submitted supplier response to be matched to the correct Camunda process instance in tenant `26DIGIBP34`.

#### Limitation of Make.com
The Make.com scenarios use polling-based triggers for Google Form responses. This means that Make.com checks regularly whether new responses are available. If no new data is found, the scenario does not continue (intervall every 15 minutes). 

---

## Repository Structure

```
/
├── BPMN/                          BPMN process models
│   ├── AS-IS_S2O.bpmn             Current manual Source-to-Order process
│   ├── TO-BE_S2O.bpmn             Redesigned digital process (full S2O)
│   └── Weiterentwicklung-Automation_2.bpmn   Executable Camunda 7 sub-process
├── DMN/                           Decision models
│   ├── Evaluate-supplier-response_1.dmn      Scores a single supplier (5 inputs → score)
│   └── DMN-Best-Supplier_1.dmn               Selects best supplier from scores
├── Forms/                         Camunda user task forms (.form)
│   ├── customer-request-catalogue_1.form     Customer request + catalogue check
│   ├── alternative_procurement-request_1.form Alternative product proposal
│   ├── feasbible_RFQ-list.form             Feasibility check + RFQ contact list
│   ├── Review-Supplier_1.form                Review supplier responses
│   └── Review_Contract_Draft_1.form          Review contract draft
├── Make/                          Make.com integration screenshots
│   ├── 1-Trigger_Integration_Google-Forms_HTTP.png
│   ├── 2-Send_RFQ.png
│   ├── 3-Supplier_Response.png
│   └── 4-Generate_contract-draft.png
├── Process_Automation_Methodology.md   Our 8-step methodology
└── README.md                      This file
```

[⬆️ Back to Top](#table-of-contents)

---

# AS-IS Process

## Process Description

In the current setup, a sourcing request begins when a customer contacts the **Sales team** directly via phone or email. The Sales representative manually logs the request, checks the product catalogue, and — if the item is not available — informs Procurement via email. Procurement then identifies suppliers, sends RFQs (often as Word documents attached to emails), compiles incoming quotations in **Excel**, manually computes a comparison matrix, and selects a supplier. Contracts are prepared as Word documents, sent for signature, and archived in shared folders. New suppliers are created manually in the ERP system by Master Data.

## Key Limitations of the AS-IS Process

The AS-IS analysis surfaced the following structural weaknesses:

### Manual, Channel-Dependent Trigger
- Requests arrive via phone or email; missed calls and lost emails mean missed opportunities
- No standardised data capture — Sales reps must chase customers for missing information

### Fragmented Tooling
- CRM, Excel, email, and the ERP system are not connected
- RFQs and quotations move through inboxes, not a structured repository
- Supplier comparison is built by hand in Excel — error-prone and not auditable

### Slow, Opaque Hand-Offs
- Each transition between Sales, Procurement, and Master Data is a manual notification
- No visibility on where a request currently sits
- Contract negotiation loops rely on email threads with no SLA enforcement

### No Decision Support
- Supplier evaluation depends on the procurement officer's intuition
- No consistent weighting of price, delivery time, compliance, quality, or financial stability
- Hard to justify why a particular supplier was chosen

Together, these issues result in **long cycle times, lost requests, inconsistent supplier choices, and poor traceability**.

## AS-IS BPMN Diagram

The full AS-IS model is in [`BPMN/AS-IS_S2O.bpmn`](BPMN/AS-IS_S2O.bpmn). It uses three swimlanes — **Customer · Sales · Procurement · Master Data** — and shows manual hand-offs, email-based RFQs, and Excel-based supplier comparison.

> *Open the file in [Camunda Modeler](https://camunda.com/download/modeler/) to inspect the model.*

## Project Goal

The goal is to **digitalise and partially automate the Source-to-Order process**, with a particular focus on **structured supplier selection**. The TO-BE system should:

- Replace the manual trigger with a **digital self-service request form**
- Standardise data capture across all stakeholders (Sales, Procurement, Master Data)
- **Automate supplier scoring and selection** through DMN decision tables
- Orchestrate the entire flow via **Camunda 7** with clear, traceable task assignments
- Use **Make.com** to bridge between Camunda and external services (Google Forms, email, Google Docs)
- Provide a **complete audit trail** — every decision, every supplier score, every contract version is logged

[⬆️ Back to Top](#table-of-contents)

---

# TO-BE Process

## Process Overview

The TO-BE process replaces the email-driven workflow with a digital end-to-end pipeline. A customer submits a structured request, which becomes a Camunda process instance. Procurement performs a feasibility check, an RFQ is sent to multiple suppliers via Make.com, supplier responses are stored back into Camunda, each response is scored by a DMN, the best supplier is selected by a second DMN, and the process continues to contract generation and purchase order.

The TO-BE model is split across two BPMN files:

| File | Purpose |
| ---- | ------- |
| [`BPMN/TO-BE_S2O.bpmn`](BPMN/TO-BE_S2O.bpmn) | **Conceptual** end-to-end TO-BE model covering the full Source-to-Order flow |
| [`BPMN/Weiterentwicklung-Automation_2.bpmn`](BPMN/Weiterentwicklung-Automation_2.bpmn) | **Executable** Camunda 7 sub-process implementing the automated supplier-selection block (RFQ → score → best supplier → contract draft) |

## TO-BE BPMN Diagram

*Open both BPMN files in Camunda Modeler. Together they describe the conceptual flow and the deployable automation core.*

## Step-by-Step Walkthrough

### 1. Customer Request Submission
A customer fills out the **"Submit Request"** form on the e-commerce site. The form is implemented as a **Google Form** that captures: customer details, requested product, quantity, sustainability requirements, and deadline.

> 📄 Form: [`Forms/customer-request-catalogue_1.form`](Forms/customer-request-catalogue_1.form) — used by Sales to review and check the catalogue.

### 2. Trigger Camunda via Make.com
When the form is submitted, **Make.com** picks it up via the Google Forms module and sends an HTTP `POST` to Camunda's REST API:
```
POST https://digibp.engine.martinlab.science/engine-rest/process-definition/key/supplier_request_process/tenant-id/26DIGIBP34/start
```
This starts a new process instance with the form data as process variables.

> 🖼️ See [`Make/1-Trigger_Integration_Google-Forms_HTTP.png`](Make/1-Trigger_Integration_Google-Forms_HTTP.png).

### 3. Catalogue Check (Sales)
A user task is created in the **Camunda Tasklist** for Sales: "*View new request and Check Material Catalogue*". The Sales representative confirms whether the requested product exists in the catalogue.

- **Yes** → Continue the standard sales process (out of scope).
- **No** → Procurement is engaged.

### 4. Propose Alternative (Optional)
If a close alternative exists, Sales proposes it to the customer via [`Forms/alternative_procurement-request_1.form`](Forms/alternative_procurement-request_1.form). If the customer accepts, the standard process resumes; otherwise the procurement flow is triggered.

### 5. Procurement: Feasibility Check & RFQ List
Procurement opens [`Forms/feasbible_RFQ-list_1.form`](Forms/feasbible_RFQ-list_1.form) to:
- Decide whether an **existing supplier** can fulfil the request (if so, source directly).
- Otherwise, compile a list of **3–5 candidate suppliers** to receive an RFQ.

### 6. Send RFQs via Make.com
A Camunda **Send Task** posts the RFQ payload to a Make.com webhook. Make.com routes the request to each of the 3 supplier email addresses, generating individual RFQ emails.

> 🖼️ See [`Make/2-Send_RFQ.png`](Make/2-Send_RFQ.png) — webhook → router → 3 supplier email branches.

### 7. Wait for Supplier Responses (Message Catch Event)
The process pauses on **`supplierResponseReceived`** message catch events. Suppliers submit their offers via another Google Form (price, delivery time, compliance level, quality score, financial stability rating). Make.com correlates each response back to the running process instance via Camunda's `/engine-rest/message` API.

> 🖼️ See [`Make/3-Supplier_Response.png`](Make/3-Supplier_Response.png).

### 8. Score Each Supplier Response (DMN — `Evaluate supplier response`)
Each incoming supplier response triggers a **Business Rule Task** that invokes the [`DMN/Evaluate-supplier-response_1.dmn`](DMN/Evaluate-supplier-response_1.dmn) decision. See [Decision Automation](#decision-automation-dmn) below for details. The resulting `supplierScore` is stored back as a process variable.

### 9. Select Best Supplier (DMN — `Select Best Supplier`)
Once all expected responses are scored, a second Business Rule Task runs [`DMN/DMN-Best-Supplier_1.dmn`](DMN/DMN-Best-Supplier_1.dmn), which compares `supplier1Score`, `supplier2Score`, and `supplier3Score` and returns the winner.

### 10. Human Review (User Task)
The recommended supplier is presented to a procurement manager via [`Forms/Review-Supplier_1.form`](Forms/Review-Supplier_1.form). The reviewer can:
- **Accept** the recommendation → continue to contract.
- **Override** with justification → next-ranked supplier is selected.

### 11. Generate Contract Draft via Make.com
Camunda fires a webhook to Make.com, which uses **Google Docs** to render a contract from a template, populating the supplier name, terms, and price.

> 🖼️ See [`Make/4-Generate_contract-draft.png`](Make/4-Generate_contract-draft.png) — Webhook → Google Docs (create from template) → HTTP back to Camunda.

### 12. Contract Review 
The generated contract draft is reviewed via [`Forms/Review_Contract_Draft 1.form`](Forms/Review_Contract_Draft%201.form). Procurement can check the generated contract draft and decide whether it is acceptable. In the current Process, this step represents the manual review before finalisation.

### 13. Process Closure
After the contract draft has been reviewed, the Process reaches its final review stage. Due to our company's internal policies, signatures must currently still be provided manually. Therefore, the pre-filled contract can be downloaded and reused.

## Challenges Addressed by the TO-BE Process

| Challenge (AS-IS)                                          | Solution (TO-BE)                                                |
| ---------------------------------------------------------- | --------------------------------------------------------------- |
| Manual phone/email trigger; lost requests                  | Digital self-service form → automatic process start             |
| Excel-based supplier comparison; inconsistent decisions    | DMN decision tables produce auditable, reproducible scores      |
| Fragmented tooling (CRM, Excel, email, ERP)                | Camunda orchestrates; Make.com integrates external services    |
| No visibility on request status                            | Camunda Cockpit provides real-time process instance tracking    |
| Email-based RFQ and contract loops                         | Automated RFQ dispatch, message catch events, timer escalations |
| No audit trail                                             | Every decision, score, and form submission stored as variables  |

## Users and Stakeholders

| Stakeholder              | AS-IS Role                                  | TO-BE Role                                                 |
| ------------------------ | ------------------------------------------- | ---------------------------------------------------------- |
| **Customer**             | Calls/emails Sales                          | Submits structured request via web form                    |
| **Sales**                | Manually logs request, contacts Procurement | Reviews catalogue match, proposes alternatives in Tasklist |
| **Procurement**          | Manages RFQs by email, builds Excel matrix  | Runs feasibility check, reviews DMN-recommended supplier   |
| **Master Data**          | Manually creates supplier in ERP            | Reviews vendor data, approves automated creation request   |
| **Supplier (external)**  | Replies via email                           | Submits quotation via form; responds to contract via form  |

[⬆️ Back to Top](#table-of-contents)

---

# Decision Automation (DMN)

Two DMN decision tables drive the automated supplier-selection logic. They are invoked from BPMN **Business Rule Tasks** in the executable `Weiterentwicklung-Automation_2.bpmn`.

## 1. Evaluate Supplier Response

> **File:** [`DMN/Evaluate-supplier-response_1.dmn`](DMN/Evaluate-supplier-response_1.dmn)
> **Decision ID:** `EvaluateSupplierResponse`
> **Hit Policy:** `FIRST`

This decision scores **one** supplier response on a multi-criteria basis.

### Inputs
| Variable                 | Type    | Description                                              |
| ------------------------ | ------- | -------------------------------------------------------- |
| `lastPrice`              | number  | Quoted unit price                                        |
| `lastDeliveryTime`       | number  | Promised delivery time in days                           |
| `lastCompliance`         | string  | `"FULL"`, `"PARTIAL"`, or `"NONE"` — compliance with T&C |
| `lastQualityScore`       | number  | Quality score (provided alongside the response)          |
| `lastFinancialStability` | number  | Financial-stability rating                               |

### Output
| Variable        | Type   | Description                                                  |
| --------------- | ------ | ------------------------------------------------------------ |
| `supplierScore` | number | Composite score: `pricePts + deliveryPts + qualityScore + financialStability + compliancePts` |

### Scoring Logic (summary)

- **Compliance gate:**
  - `NONE` → score is `0` (instant disqualifier — supplier is dropped)
  - `PARTIAL` → compliance contribution = `2`
  - `FULL` → compliance contribution = `5`
- **Price band:** `≤1000 → 5`, `1000–2000 → 4`, `2000–3000 → 3`, `>3000 → 2`
- **Delivery band:** `≤5 days → 5`, `5–10 → 4`, `10–15 → 3`, `>15 → 2`
- **Quality & financial stability** are added directly from inputs.

The full table contains 33 rules (1 `NONE` + 16 `FULL` × 16 `PARTIAL`).

## 2. Select Best Supplier

> **File:** [`DMN/DMN-Best-Supplier_1.dmn`](DMN/DMN-Best-Supplier_1.dmn)
> **Decision ID:** `SelectBestSupplier`
> **Hit Policy:** `FIRST`

This decision takes the scores of up to three suppliers and returns the winning supplier name.

### Inputs
| Variable             | Type    | Description                                |
| -------------------- | ------- | ------------------------------------------ |
| `expectedResponses`  | integer | Number of supplier responses received (1–3) |
| `supplier1Score`     | number  | Score from DMN #1 for supplier 1            |
| `supplier2Score`     | number  | Score from DMN #1 for supplier 2            |
| `supplier3Score`     | number  | Score from DMN #1 for supplier 3            |

### Output
| Variable        | Type   | Description                              |
| --------------- | ------ | ---------------------------------------- |
| `bestSupplier`  | string | `"supplier1"`, `"supplier2"`, or `"supplier3"` |

### Decision Logic
The table compares scores pairwise across the responses received. Ties resolve in favour of the lower-indexed supplier (consistent with the `FIRST` hit policy).

[⬆️ Back to Top](#table-of-contents)

---

# Technologies Used 🛠

| Component                  | Tool / Technology                          | Purpose                                              |
| -------------------------- | ------------------------------------------ | ---------------------------------------------------- |
| Workflow engine            | **Camunda Platform 7** (BPMN + DMN)        | Orchestrates the entire process; runs DMN decisions  |
| Process modelling          | **Camunda Modeler**                        | Designing BPMN and DMN files                         |
| Integration / iSaaS        | **Make.com**                               | Connects Google Forms, Gmail, Google Docs to Camunda |
| Customer-facing entry      | **Google Forms**                           | Customer request and supplier response submission    |
| Document generation        | **Google Docs** (via Make)                 | Contract drafts from templates                       |
| Notifications              | **Gmail** (via Make)                       | RFQ emails to suppliers                              |
| Simulated CRM/ERP          | **Google Sheets**                          | Lightweight data store for materials, suppliers, requests |
| API testing                | **Postman**                                | Validating Camunda REST endpoints                    |
| User interfaces            | **Camunda Forms** (`.form`)                | Human-task interfaces in the Camunda Tasklist        |

[⬆️ Back to Top](#table-of-contents)

---

# Workflow Orchestration

## Camunda BPMN Engine

The executable BPMN model ([`Weiterentwicklung-Automation_2.bpmn`](BPMN/Weiterentwicklung-Automation_2.bpmn)) runs on the FHNW Camunda instance.

### Configuration
- **Engine REST base URL:** `https://digibp.engine.martinlab.science/engine-rest`
- **Camunda Tasklist:** [`Open DigiBP Camunda Tasklist`](https://digibp.engine.martinlab.science/camunda/app/tasklist/default/#/login)
- **Tenant ID:** `26DIGIBP34`
- **Process Key:** `supplier_request_process`
- **Authentication:** FHNW DigiBP Camunda credentials are required

### Key BPMN constructs used

| Construct                  | Where it is used                                                      |
| -------------------------- | --------------------------------------------------------------------- |
| **User Task**              | `view new request and Check Material Catalogue`, `Review supplier response`, `Review Selected Supplier and contract draft` |
| **Service Task (Send)**    | `Send RFQs to all suppliers` (→ Make webhook); `Generate contract draft` (→ Make webhook) |
| **Business Rule Task**     | `Evaluate supplier response` (DMN #1); `Select best supplier` (DMN #2) |
| **Message Catch Event**    | `supplierResponseReceived`, `contractDraftGenerated`                  |
| **Exclusive Gateway (XOR)**| `material in catalogue?`, `client agrees?`, `Can an existing supplier fulfill the request?`, `All responses received?` |
| **Timer Boundary Event**   | Reminder timers on supplier-response and contract-review tasks        |

### Starting a Process Instance

Process instances are started by Make.com (Google Forms trigger) via:
```http
POST https://digibp.engine.martinlab.science/engine-rest/process-definition/key/supplier_request_process/tenant-id/26DIGIBP34/start

Content-Type: application/json

{
  "variables": {
    "customerName":    { "value": "...", "type": "String"  },
    "requestedItem":   { "value": "...", "type": "String"  },
    "quantity":        { "value": 100,   "type": "Integer" },
    "sustainability":  { "value": "...", "type": "String"  }
  }
}
```

## Make.com Integration

Make.com scenarios bridge Camunda and external services. There are **four** scenarios:
> Note: The Make.com scenario links are shared for documentation and review purposes. They provide a read-only view of the automation logic. The executable scenarios remain in the project Make.com workspace.

### Scenario 1 — Trigger Process from Google Form
> 🖼️ [`Make/1-Trigger_Integration_Google-Forms_HTTP.png`](Make/1-Trigger_Integration_Google-Forms_HTTP.png)
> 🔍 [View read-only Make scenario](https://eu1.make.com/public/shared-scenario/WcpIaP5PdpB/1-trigger-integration-google-forms-htt)

`Google Forms (Watch Responses)` → `HTTP POST /engine-rest/process-definition/key/{key}/start`

### Scenario 2 — Send RFQs to Suppliers
> 🖼️ [`Make/2-Send_RFQ.png`](Make/2-Send_RFQ.png)
> 🔍 [View read-only Make scenario: Send RFQ](https://eu1.make.com/public/shared-scenario/ZWe6cXPJurT/2-send-rfq)

`Custom Webhook (from Camunda)` → `Router` → `Gmail × 3` (one branch per supplier email).

### Scenario 3 — Receive Supplier Responses
> 🖼️ [`Make/3-Supplier_Response.png`](Make/3-Supplier_Response.png)
> 🔍 [View read-only Make scenario: Supplier Response](https://eu1.make.com/public/shared-scenario/cFjOgpsfDM8/3-supplier-response)

`Google Forms (Watch Responses)` → `HTTP POST /engine-rest/message` to correlate `supplierResponseReceived` to the running instance.

### Scenario 4 — Generate Contract Draft
> 🖼️ [`Make/4-Generate_contract-draft.png`](Make/4-Generate_contract-draft.png)
> 🔍 [View read-only Make scenario: Generate Contract Draft](https://eu1.make.com/public/shared-scenario/0BUzqPCd6Al/4-generate-contract-draft)

`Custom Webhook` → `Google Docs (Create from Template)` → `HTTP POST /engine-rest/message` to correlate `contractDraftGenerated`.

## End-to-End Flow of the Executable Prototype

```text
1. Google Form submission: New Material Request
   ↓ Make scenario 1
2. Camunda process instance starts under tenant 26DIGIBP34
   ↓
3. Sales user task: Review Customer Request and Catalogue Check
   ├─ material in catalogue → standard sales process / end of prototype path
   └─ material not in catalogue → continue with procurement
   ↓
4. Optional Sales user task: Alternative Procurement Request
   ├─ alternative accepted → standard sales process / end of prototype path
   └─ no suitable alternative or procurement required → continue
   ↓
5. Procurement user task: Feasibility Check and RFQ Contact List
   ├─ existing supplier can fulfil request → source through existing supplier / end of prototype path
   └─ external RFQ required → continue
   ↓
6. Camunda sends RFQ payload to Make scenario 2
   ↓
7. Make.com sends RFQ emails to the entered supplier contacts
   ↓
8. Suppliers submit quotations through the RFQ Response Google Form
   ↓ Make scenario 3
9. Make.com correlates supplierResponseReceived messages to the running Camunda instance
   ↓
10. Business Rule Task: Evaluate each supplier response with DMN "EvaluateSupplierResponse"
   ↓
11. Gateway: all expected supplier responses received?
   ├─ no → wait for further supplier responses
   └─ yes → continue
   ↓
12. Business Rule Task: Select best supplier with DMN "SelectBestSupplier"
   ↓
13. Procurement user task: Review Supplier Recommendation
   ↓
14. Camunda sends contract-generation request to Make scenario 4
   ↓
15. Make.com generates a contract draft in Google Docs
   ↓
16. Make.com correlates contractDraftGenerated message to Camunda
   ↓
17. Procurement user task: Review Contract Draft
   ↓
18. End of executable prototype. The User can download the contract draft and continue manually with the process
```

[⬆️ Back to Top](#table-of-contents)

---

# Digital User Interfaces (Forms)

All human interactions occur through Camunda Forms (`.form`) rendered in the **Camunda Tasklist**:

| File | Screenshot | Used by | Purpose |
| ---- | ---------- | ------- | ------- |
| [`Forms/customer-request-catalogue 1.form`](Forms/customer-request-catalogue%201.form) | [Screenshot](Forms/Form-Customer-Request-Catalogue.png) | Sales | Review customer request + record catalogue check |
| [`Forms/alternative_procurement-request 1.form`](Forms/alternative_procurement-request%201.form) | [Screenshot](Forms/Form-Alternative-proc-Request.png) | Sales | Propose an alternative product; prepare procurement |
| [`Forms/feasible_RFQ-list.form`](Forms/feasible_RFQ-list%201.form) | [Screenshot](Forms/Form-Feasible-RFQ-List.png) | Procurement | Feasibility check + capture RFQ contact list (3 suppliers) |
| [`Forms/Review-Supplier 1.form`](Forms/Review-Supplier%201.form) | [Screenshot](Forms/Form-Review-Supplier.png) | Procurement | Review individual supplier responses |
| [`Forms/Review_Contract_Draft 1.form`](Forms/Review_Contract_Draft%201.form) | [Screenshot](Forms/HTTP-Form-Review-Contract-Draft.png) | Procurement | Review the generated contract draft |

The customer-facing **request** and the supplier-facing **quotation submission** are both implemented as **Google Forms** outside Camunda, integrated via Make.com.

The following Google Forms are used for interactions outside the Camunda Tasklist. They are integrated into the workflow through Make.com.

| Google Form | Link | Screenshot | Used by | Purpose |
| ----------- | ---- | ---------- | ------- | ------- |
| New Material Request | [Open form](https://docs.google.com/forms/d/e/1FAIpQLSeTFIsw-KNPZBVw3_ue3LixSkw15_3P3KyKSNXANNTvKQDmOw/viewform?usp=header) | [Screenshot](Forms/Google-Form-New-Material-Request.pdf) | Customer / Requester | Starting form for submitting a new material procurement request |
| RFQ Response | [Open form](https://docs.google.com/forms/d/e/1FAIpQLSco4foSfT7dFTarkni2qyGCryNh_46HVuw1-V4ZWXt5Oj3vYg/viewform?usp=header) | [Screenshot](Forms/Google-Form-RFQ-Response.pdf) | Supplier | Supplier-facing quotation submission form with pre-filled fields for process instance and supplier data |

[⬆️ Back to Top](#table-of-contents)

---

# Limitations & Future Improvements

This section captures the **current limitations** of the prototype and the **future improvements** we would prioritise for a production-grade Source-to-Order solution. The two are presented together because each limitation directly motivates a specific next step.

## 1. Supplier database is still manually maintained

One limitation of the current workflow is that supplier identification and RFQ contact-list creation are still performed manually. While this keeps the demonstrator understandable and controllable, it limits scalability and prevents the process from automatically leveraging supplier master data, historical performance, certifications, or external supplier databases.

**Improvement.** Connect the workflow to a supplier master-data system or supplier database. This would allow the process to automatically suggest suitable suppliers based on product category, previous orders, delivery performance, certifications, location, compliance status, or historical quality ratings — making the workflow more data-driven and less dependent on manual input.

## 2. Supplier evaluation uses a simplified scoring model

The current supplier evaluation is based on a simplified scoring model. The DMN decision evaluates price, delivery time, quality score, financial stability, and compliance. This is useful for demonstrating decision automation because the logic is transparent and easy to understand. However, the model is relatively static and does not yet reflect the full complexity of real supplier selection. In practice, supplier selection is a multi-criteria decision-making problem and different procurement cases may require different weightings.

**Improvement.** Move from a static rule-based scoring to **case-specific weighting profiles** (e.g. "cost-driven", "quality-driven", "sustainability-driven"). The DMN could be split into a weighting decision plus a scoring decision, and the weights could be parameterised at process start.

### 3. Webhook-Based Make.com Triggers

The current Make.com scenarios are based on polling triggers. This means that Make.com checks regularly whether new Google Form responses or data entries are available. Although this works for the demonstrator, it is not fully event-driven and may create unnecessary scenario executions.

**Improvement.** A future improvement would be to replace polling-based triggers with webhook-based triggers. With webhooks, Make.com would only start when a new event actually occurs, for example when a new material request is submitted or when a supplier sends an RFQ response.

## 4. Missing timeout and escalation for supplier responses

The current workflow assumes that all contacted suppliers submit a response. The process counts received responses and compares them with the expected number. However, if one supplier does not answer, the process may remain open and wait indefinitely. In practice, suppliers may answer late, forget to respond, or decline to participate.

**Improvement.** Add **timer events and escalation paths** to the BPMN model. The workflow could send an automatic reminder after a defined number of days. If no response is received after the final deadline, the process could continue with the available responses or escalate to a procurement employee — making the workflow robust against stuck process instances.

## 5. Risk of duplicate or incorrect supplier responses

The current response handling counts incoming supplier responses and stores values based on supplier number. This works for the demonstrator but does not fully prevent duplicate responses: if a supplier submits the form twice, the response counter increases twice. Incorrect supplier numbers or process instance IDs could also lead to wrong mappings.

**Improvement.** Introduce **unique response tokens per supplier**. Each RFQ email would contain a supplier-specific link with a unique token. When a supplier submits a response, the workflow checks whether this supplier has already responded — and either rejects the duplicate or updates the existing record without re-incrementing the counter. This improves data consistency and reduces process errors.

## 6. Limited error handling for external service integration

The workflow depends on external services such as Make.com for sending RFQ emails and generating contract drafts. This demonstrates service integration well, but the current BPMN has limited error handling for technical failures — a Make.com webhook could be unavailable, a payload malformed, or a response incomplete.

**Improvement.** Model **technical error handling directly in BPMN**. Service tasks would include retry mechanisms, boundary error events, and fallback paths. If an external service fails after several retries, the workflow could create a manual correction task for procurement — making the process production-ready.

## 7. No negotiation loop included

The current workflow evaluates supplier responses and selects the best supplier. It does not include a structured negotiation loop. In real procurement, the first offer is rarely the final offer — teams typically negotiate price, delivery time, payment terms, warranty, or technical specifications before deciding.

**Improvement.** Add a **negotiation sub-process** before the final supplier selection. After reviewing responses, procurement could decide whether negotiation is required. If yes, the workflow would send negotiation requests to selected suppliers, wait for updated offers, and re-evaluate them through the DMN — aligning the process with realistic procurement practice.

## 8. Implicit tie-breaking in best-supplier selection

The current best-supplier selection may include implicit tie-breaking. If two or more suppliers achieve the same score, the `FIRST` hit policy of the DMN automatically picks the first matching supplier. This works technically but is not transparent: a procurement employee may not understand why one supplier was preferred when both scored identically.

**Improvement.** Define **explicit tie-breaking rules** in the DMN. For example: if scores are equal, compare compliance level first, then delivery time, then price. If there is still no clear winner, route to a manual review task — making the selection logic transparent and easy to justify.

## 9. Limited process monitoring and reporting

The current workflow focuses on executing the procurement process. It does not yet provide advanced monitoring or reporting. Procurement managers cannot easily see how many RFQs are open, which suppliers have not responded, which cases are delayed, or how long each step takes.

**Improvement.** Add **monitoring dashboards** showing open process instances, average response times, supplier participation rates, decision outcomes, and bottlenecks. This helps procurement managers control the process and identify opportunities for continuous improvement.

## 10. Digital signature integration is missing

The contract signing step is not yet fully digitalised. The workflow generates and reviews a contract draft, but the final signature is handled manually outside the process. This creates a media break — the contract may need to be downloaded, printed, signed, scanned, and exchanged by email.

**Improvement.** Integrate a **digital signature solution** such as DocuSign or Adobe Acrobat Sign. After the draft is approved, the workflow would automatically send the contract for digital signature, store the signed contract, and continue or close the case without leaving the system.

[⬆️ Back to Top](#table-of-contents)
