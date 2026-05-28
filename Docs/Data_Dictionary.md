# Data Dictionary

This data dictionary documents the main process variables used in the digitalised Source-to-Order workflow with supplier selection. The variables are used across Google Forms, Make.com scenarios, Camunda BPMN scripts, Camunda Forms and DMN decision tables.

The purpose of this data dictionary is to improve transparency, maintainability and reproducibility of the implemented workflow.

---

## 1. Customer Request Variables

| Variable | Type | Source | Used in | Description |
| -------- | ---- | ------ | ------- | ----------- |
| `customerName` | String | Google Form: New Material Request | Camunda process, Sales review | Name of the customer or requester submitting the material request |
| `customerEmail` | String | Google Form: New Material Request | Camunda process, Make.com | Email address of the requester for possible communication |
| `requirementDescription` | String | Google Form / Make.com payload | RFQ email, contract draft | Description of the requested material or procurement need |
| `technicalSpecifications` | String | Google Form / Make.com payload | RFQ email, procurement review | Technical requirements or specifications of the requested material |
| `quantity` | Integer | Google Form / Make.com payload | RFQ email, contract draft | Requested quantity of the material |
| `deliveryDate` | Date/String | Google Form / Make.com payload | RFQ email, supplier evaluation context | Requested or expected delivery date |
| `sustainabilityRequirements` | String | Google Form: New Material Request | Sales/procurement review | Sustainability-related requirements, e.g. eco-friendly material or certificates |

---

## 2. Catalogue and Feasibility Check Variables

| Variable | Type | Source | Used in | Description |
| -------- | ---- | ------ | ------- | ----------- |
| `materialInCatalogue` | Boolean/String | Camunda Form: Customer Request Catalogue | BPMN gateway | Indicates whether the requested material already exists in the catalogue |
| `alternativeAvailable` | Boolean/String | Camunda Form: Alternative Procurement Request | BPMN gateway | Indicates whether Sales can propose an alternative product |
| `clientAgrees` | Boolean/String | Camunda Form: Alternative Procurement Request | BPMN gateway | Indicates whether the customer accepts the proposed alternative |
| `feasible` | Boolean/String | Camunda Form: Feasibility / RFQ List | BPMN gateway | Indicates whether the procurement request is feasible |
| `existingSupplierAvailable` | Boolean/String | Camunda Form: Feasibility / RFQ List | BPMN gateway | Indicates whether an existing supplier can fulfil the request |

---

## 3. Supplier Contact Variables

| Variable | Type | Source | Used in | Description |
| -------- | ---- | ------ | ------- | ----------- |
| `supplier1Name` | String | Camunda Form: RFQ Contact List | Make.com RFQ email, supplier selection | Name of the first supplier |
| `supplier1Email` | String | Camunda Form: RFQ Contact List | Make.com RFQ email | Email address of the first supplier |
| `supplier2Name` | String | Camunda Form: RFQ Contact List | Make.com RFQ email, supplier selection | Name of the second supplier |
| `supplier2Email` | String | Camunda Form: RFQ Contact List | Make.com RFQ email | Email address of the second supplier |
| `supplier3Name` | String | Camunda Form: RFQ Contact List | Make.com RFQ email, supplier selection | Name of the third supplier |
| `supplier3Email` | String | Camunda Form: RFQ Contact List | Make.com RFQ email | Email address of the third supplier |
| `expectedResponses` | Integer | Camunda script / RFQ preparation | BPMN gateway | Number of supplier responses expected by the process |
| `receivedResponses` | Integer | Camunda script / supplier response handling | BPMN gateway | Number of supplier responses already received |

---

## 4. Incoming Supplier Response Variables

These variables temporarily store the latest supplier response received from the external RFQ Response Google Form. They are then processed and stored under the corresponding supplier-specific variables.

| Variable | Type | Source | Used in | Description |
| -------- | ---- | ------ | ------- | ----------- |
| `lastSupplierNumber` | Integer | Google Form: RFQ Response / Make.com | Camunda script | Identifies which supplier submitted the latest response |
| `lastSupplierName` | String | Google Form: RFQ Response / Make.com | Camunda script | Name of the supplier submitting the latest response |
| `lastSupplierEmail` | String | Google Form: RFQ Response / Make.com | Camunda script | Email address of the supplier submitting the latest response |
| `lastPrice` | Number | Google Form: RFQ Response / Make.com | DMN: EvaluateSupplierResponse | Quoted price submitted by the supplier |
| `lastDeliveryTime` | Number | Google Form: RFQ Response / Make.com | DMN: EvaluateSupplierResponse | Promised delivery time in days |
| `lastCompliance` | String | Google Form: RFQ Response / Make.com | DMN: EvaluateSupplierResponse | Supplier compliance level, e.g. `FULL`, `PARTIAL`, or `NONE` |
| `lastQualityScore` | Number | Google Form: RFQ Response / Make.com | DMN: EvaluateSupplierResponse | Quality score submitted or assigned for the supplier |
| `lastFinancialStability` | Number | Google Form: RFQ Response / Make.com | DMN: EvaluateSupplierResponse | Financial stability rating submitted or assigned for the supplier |

---

## 5. Stored Supplier Response Variables

After a supplier response is received, the values are stored under supplier-specific variables. This allows the process to compare all suppliers later.

| Variable | Type | Source | Used in | Description |
| -------- | ---- | ------ | ------- | ----------- |
| `supplier1Price` | Number | Camunda script | DMN / supplier review | Quoted price of supplier 1 |
| `supplier1DeliveryTime` | Number | Camunda script | DMN / supplier review | Delivery time of supplier 1 in days |
| `supplier1Compliance` | String | Camunda script | DMN / supplier review | Compliance level of supplier 1 |
| `supplier1QualityScore` | Number | Camunda script | DMN / supplier review | Quality score of supplier 1 |
| `supplier1FinancialStability` | Number | Camunda script | DMN / supplier review | Financial stability rating of supplier 1 |
| `supplier2Price` | Number | Camunda script | DMN / supplier review | Quoted price of supplier 2 |
| `supplier2DeliveryTime` | Number | Camunda script | DMN / supplier review | Delivery time of supplier 2 in days |
| `supplier2Compliance` | String | Camunda script | DMN / supplier review | Compliance level of supplier 2 |
| `supplier2QualityScore` | Number | Camunda script | DMN / supplier review | Quality score of supplier 2 |
| `supplier2FinancialStability` | Number | Camunda script | DMN / supplier review | Financial stability rating of supplier 2 |
| `supplier3Price` | Number | Camunda script | DMN / supplier review | Quoted price of supplier 3 |
| `supplier3DeliveryTime` | Number | Camunda script | DMN / supplier review | Delivery time of supplier 3 in days |
| `supplier3Compliance` | String | Camunda script | DMN / supplier review | Compliance level of supplier 3 |
| `supplier3QualityScore` | Number | Camunda script | DMN / supplier review | Quality score of supplier 3 |
| `supplier3FinancialStability` | Number | Camunda script | DMN / supplier review | Financial stability rating of supplier 3 |

---

## 6. Supplier Scoring Variables

These variables are used by the DMN decision tables to evaluate supplier responses and select the best supplier.

| Variable | Type | Source | Used in | Description |
| -------- | ---- | ------ | ------- | ----------- |
| `supplierScore` | Number | DMN: EvaluateSupplierResponse | Camunda process | Score calculated for the latest supplier response |
| `supplier1Score` | Number | Camunda script / DMN output | DMN: SelectBestSupplier | Final score of supplier 1 |
| `supplier2Score` | Number | Camunda script / DMN output | DMN: SelectBestSupplier | Final score of supplier 2 |
| `supplier3Score` | Number | Camunda script / DMN output | DMN: SelectBestSupplier | Final score of supplier 3 |
| `bestSupplier` | String | DMN: SelectBestSupplier | Camunda process, review task | Technical identifier of the selected supplier, e.g. `supplier1`, `supplier2`, or `supplier3` |
| `bestSupplierNumber` | Integer | Camunda script | Contract generation, review task | Number of the selected supplier |
| `bestSupplierScore` | Number | Camunda script | Review task | Score of the selected supplier |
| `bestSupplierName` | String | Camunda script | Contract draft, review task | Name of the selected supplier |
| `bestSupplierEmail` | String | Camunda script | Contract draft, communication | Email address of the selected supplier |
| `bestSupplierPrice` | Number | Camunda script | Contract draft | Price offered by the selected supplier |
| `bestSupplierDeliveryTime` | Number | Camunda script | Contract draft | Delivery time offered by the selected supplier |

---

## 7. Contract Draft Variables

| Variable | Type | Source | Used in | Description |
| -------- | ---- | ------ | ------- | ----------- |
| `contractDraftUrl` | String | Make.com / Google Docs | Camunda contract review form | URL of the generated contract draft |
| `contractApproved` | Boolean/String | Camunda Form: Review Contract Draft | BPMN gateway / process closure | Indicates whether the generated contract draft was approved |
| `contractReviewComment` | String | Camunda Form: Review Contract Draft | Documentation / audit trail | Optional comment from Procurement during contract review |

---

## 8. Technical and Integration Variables

| Variable | Type | Source | Used in | Description |
| -------- | ---- | ------ | ------- | ----------- |
| `processInstanceId` | String | Camunda | Make.com, Google Forms prefill links | Unique ID of the running Camunda process instance |
| `messageName` | String | Make.com / Camunda REST | Camunda message correlation | Name of the Camunda message to correlate, e.g. `supplierResponseReceived` or `contractDraftGenerated` |
| `tenantId` | String | Camunda / Make.com configuration | Camunda REST API | Project tenant used in the DigiBP Camunda environment, currently `26DIGIBP34` |
| `processDefinitionKey` | String | Camunda BPMN | Make.com process start | Process key used to start a new Camunda process instance |
| `rfqResponseFormUrl` | String | Make.com | RFQ email | Pre-filled Google Form URL sent to suppliers |
| `makeScenarioName` | String | Make.com documentation | Documentation | Name of the Make.com scenario involved in the integration step |

---

## 9. Data Quality and Naming Conventions

To keep the workflow maintainable, the following naming conventions are applied:

- Process variables use camelCase, e.g. `supplier1Name`, `bestSupplierScore`, `contractDraftUrl`.
- Supplier-specific variables include the supplier number, e.g. `supplier1Price`, `supplier2Price`, `supplier3Price`.
- Temporary variables for the latest incoming response use the prefix `last`, e.g. `lastPrice`, `lastCompliance`.
- Boolean decision variables should be named as clear business questions, e.g. `materialInCatalogue`, `existingSupplierAvailable`.
- Variable names must be used consistently across Camunda Forms, BPMN scripts, DMN tables and Make.com payloads.

---

## 10. Notes and Known Limitations

- The prototype uses up to three suppliers for the RFQ process.
- Supplier response handling currently depends on correct process instance and supplier number mapping.
- Duplicate supplier responses are not fully prevented in the current prototype.
- The process uses polling-based Make.com triggers for Google Form responses.
- A future improvement would be to introduce unique supplier response tokens and webhook-based triggers.
