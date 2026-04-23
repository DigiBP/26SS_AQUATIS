# 26SS_AQUATIS

# To-Be Process Documentation

## 1. Overview

This project focuses on automating and digitalising the Source‑to‑Order process using a practical, real‑world example. We take an office supplies company with an e‑commerce platform where customers can place orders in self‑service. In our case study, a customer wants to go green and is looking for an eco‑friendly paper product that is not currently available in the catalog and needs to be sourced.

## 1.1 Current Process
In the current setup, sourcing requests are handled manually. The process is triggered when a customer contacts the sales team directly via phone calls or emails. This approach can lead to missed opportunities if calls are not answered, emails are replied to too late, or requests get lost. As a result, response times can be slow and the overall customer experience inconsistent.

## 1.2 Automated Process (Case Study)
In the automated scenario, the customer uses the e‑commerce platform to request the eco‑friendly paper. By clicking a dedicated button, the customer is redirected to a sourcing request form where all necessary information is collected.
Once the form is submitted, the request is automatically created in the CRM system as a new sourcing request and assigned to a sales representative. This automation ensures that requests are captured reliably, handled faster, and seamlessly integrated into the Source‑to‑Order process.

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
