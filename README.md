# 26SS_AQUATIS

# To-Be Process Documentation

## 1. Overview

The To-Be process describes the digital handling of customer requests for materials that are not available in the webshop, including the subsequent procurement and supplier selection process.

The process integrates CRM data management, automated decision-making, and human validation steps to ensure efficiency, transparency, and scalability.

---

## 2. Process Start: Customer Request

The process begins when a customer submits a material request via the **"Submit Request"** form.

- The request is automatically stored in the CRM system.
- A manual check is performed to verify whether the requested product already exists in the material catalogue.

### Decision: Material Available?

- **Yes** → Continue with the standard sales process  
- **No** → Suggest alternative products to the customer  

If the customer accepts an alternative, the standard process continues.  
If the customer rejects the alternatives, the procurement process is initiated.

---

## 3. Procurement Initiation

The request is forwarded to the **Procurement Department**.

Steps include:
- Reviewing and structuring the request in the CRM
- Performing a **feasibility check**

### Decision: Existing Supplier Suitable?

- **Yes** → Proceed directly with procurement using the existing supplier  
- **No** → Start supplier selection process  

---

## 4. Supplier Selection Process

A structured supplier selection process is initiated:

### 4.1 Request for Quotation (RFQ)

- Identification of potential suppliers  
- Sending RFQs to multiple suppliers  
- Collecting and storing quotations in a central repository  

### 4.2 Offer Evaluation

- Automatic creation of a comparison matrix  
- Evaluation of suppliers using predefined criteria (e.g., price, quality, delivery time)  
- Execution via a **Business Rule Task**

### 4.3 Supplier Decision

- A user reviews the recommended supplier:
  - **Accept recommendation** → Select supplier  
  - **Reject recommendation** → Choose alternative supplier (with justification)

This structured evaluation aligns with standard supplier selection practices involving identification, evaluation, and contracting phases :contentReference[oaicite:0]{index=0}.

---

## 5. Contract Management

After selecting a supplier:

- A contract is automatically generated from a template  
- The contract is sent to the supplier for review  

### Negotiation Loop

- Supplier reviews contract  
- Changes may be requested → contract is updated and resent  
- If rejected → next-best supplier is selected automatically  

### Process Support

- Timer-based reminders ensure timely responses  
- Iterative loops support negotiation cycles  

Once agreed, the contract is finalized and archived.

---

## 6. Supplier Onboarding & Order Creation

If the supplier is new:

- A **Vendor Creation Request** is triggered  
- Supplier data is validated and approved  

After onboarding:

- A **Purchase Order (PO)** is generated automatically  
- The PO is reviewed by a user  

### Decision: Order Correct?

- **No** → Adjust order  
- **Yes** → Send order to supplier  

---

## 7. Process Completion

After sending the purchase order:

- All relevant data and documents are archived  
- The procurement case is closed  

---
