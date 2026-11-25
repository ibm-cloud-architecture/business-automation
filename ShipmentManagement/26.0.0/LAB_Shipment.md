## Shipment Management Automation
**an IBM Cloud Pak for Business Automation use case**
***

**Choose an option to access the Demo:**

* **IBM Technology Zone environment:** [Getting Started Lab](https://techzone.ibm.com/collection/apollo-business-automation/journey-cp4ba-demos).

  To retrieve the Shipment Management demo URLs after the environment is provisioned:
  1. Sign in to the **Red Hat OpenShift Container Platform (OCP) Console** for the reserved environment.
  2. From the left navigation menu, select **Workloads**.
  3. Select **ConfigMaps**.
  4. Confirm that the correct project or namespace is selected.
  5. Open the **Shipment info** ConfigMap.
  6. Review the ConfigMap data and use the listed Shipment Management application URLs to open the demo.

* **Manual installation on an existing CP4BA environment:** [Deployment Instructions](./README.md#6-deployment-instructions).

  Use this option to deploy the Shipment Management solution manually on an existing IBM Cloud Pak for Business Automation environment. The deployment instructions cover application import, Business Automation Workflow configuration, Decision Intelligence setup, IBM Navigator configuration, IBM Business Automation Insights setup, AI Agent setup, and application customization.

**Demo Walkthrough:**

* **YouTube Demo:** [Demo](https://www.youtube.com/watch?v=nX3UKtNRIkw).
* **End-to-End Interactive Walkthrough:** [Navattic Walkthrough](https://demo-now.techzone.ibm.com/psl/01s087v?g=cmshpob6k000000iqkg69lzvz&s=0).


# Introduction

**Use Case:** End-to-end shipment, warehouse, carrier, decision, and content automation

**Use Case Overview:** Shipment Management Automation coordinates a shipment from initial request through warehouse processing, carrier delivery, and proof of delivery. The solution uses **IBM Business Automation Workflow (BAW)** for process orchestration, **IBM Business Automation Application (BAA)** for the user experience, **Decision Intelligence (DI)** for transport mode, delivery date, and approval decisions, and **IBM FileNet Content Manager** for generated and uploaded documents.

**AI Capabilities:** watsonx AI agents provide inventory-shortage analysis, conversational inventory support, sales forecasting, carrier-confidence recommendations, and weather assessment, while Decision Intelligence provides predictive delivery-risk insights.

**GenAI Capability:** GenAI creates business content for the original-order PO Receipt, inventory-restock PO Receipt, Shipping Label, and Proof of Delivery, which are managed through IBM Content Services and stored in the order-specific IBM FileNet folder.

In this lab, you will work with three connected business applications:

1. **Shipment Application**: create and submit a shipment, validate shipment information, review approval results, and access shipment documents.
2. **Warehouse Application**: review inventory, confirm warehouse readiness, generate shipment documents, assign pickup information, and hand the shipment to the carrier stage.
3. **Carrier Application**: review assigned deliveries, update delivery status, provide tracking information, and upload proof of delivery.

The scenario demonstrates role-based access, required-field validation, automated decisions, reusable navigation, process orchestration, document generation, FileNet storage, notifications, and exception handling.

![Shipment Management Automation](./Images/000-shipment-management-overview.png)

***

<a id="getting-started-lab"></a>

# Getting Started Lab

<a id="section-0-general-information"></a>
<details>
<summary><strong>0. General Information</strong></summary>


### Lab applications and roles

| Application | Primary role | Main responsibility |
| :---------- | :----------- | :------------------ |
| Shipment Application | Shipment Requester / Shipment Manager | Create, validate, submit, review, approve, generate purchase order receipt and track shipment requests |
| Warehouse Application | Warehouse Staff / Warehouse Manager | Confirm inventory, prepare the shipment, generate shipping label, and arrange pickup |
| Carrier Application | Carrier Staff / Delivery Driver | Update tracking and delivery status and provide proof of delivery |

### Main automation services

| Service | Use in this solution |
| :------ | :------------------- |
| IBM Business Automation Workflow | Orchestrates the Shipment, Warehouse, Carrier, and Carrier Delivery Update processes |
| IBM Business Automation Application | Provides the Shipment, Warehouse, Carrier, and navigation user interfaces |
| IBM Decision Intelligence (DI) | Determines transport mode, estimated delivery date, and approval-plan outcomes |
| IBM FileNet Content Manager | Stores PO receipts, shipping labels, proof-of-delivery documents, and shipment folders |
| Email and notification services | Notifies staff, managers, and customers about approvals, exceptions, and delivery events |

[Go to Getting Started Lab](#getting-started-lab)

</details>

<a id="section-1-scenario-introduction-shipment-management-automation"></a>
<details>
<summary><strong>1. Scenario Introduction - Shipment Management Automation</strong></summary>

**Use Case Overview**

Managing a shipment requires shipment, warehouse, and carrier teams to work together. Disconnected processes can create inventory shortages, fulfillment delays, missing documents, and limited delivery visibility. This solution connects the teams through one unified Shipment Management ecosystem.

The solution validates inventory, responds to shortages through AI-assisted procurement, provides AI sales forecasts, evaluates carrier confidence and delivery risk, checks weather conditions, generates shipment documents with GenAI, sends automated email notifications, stores lifecycle documents in IBM FileNet Content Manager, and presents live process metrics in IBM Business Automation Insights.

The demonstration follows the **Not Enough Inventory** path. A watsonx AI inventory agent compares the requested quantity with real-time inventory, explains the exact shortage, and recommends procurement. A supervisor is assigned for governance, and IBM Business Automation Workflow creates an auditable procurement task. GenAI creates the PO Receipt content, IBM Content Services stores the document, and the integrated email service sends stakeholders a clickable document link.

After stock is replenished, the shipment moves to warehouse fulfillment. Warehouse staff retrieve the complete order from database services, run a watsonx AI sales forecast, review carrier confidence, use a predictive decision model in Decision Intelligence (DI) for delivery-risk assessment, and review an AI weather analysis. Finalizing the carrier assignment generates the Tracking ID and GenAI-assisted Shipping Label.

The Carrier Application retrieves the same order context, records delivery status and notes, and completes the delivery. GenAI creates the Proof of Delivery, IBM FileNet stores it in the order folder, and IBM Business Automation Insights displays shipment and process-performance metrics.

**End-to-End Flow**

1. Select the **Not Enough Inventory** demonstration path.
2. Enter the Order Number, Product, and Order Count.
3. Check inventory and create an order-specific FileNet folder.
4. Use the watsonx AI inventory agent to identify and explain the exact shortage.
5. Assign a supervisor and submit an auditable procurement request.
6. Generate and store the GenAI-assisted restock PO Receipt.
7. Notify warehouse and shipment stakeholders through integrated email.
8. Mark the order ready after stock replenishment and create the shipment.
9. Use DI to derive transport mode and expected delivery date.
10. Retrieve the order in the Warehouse Application.
11. Run the watsonx AI sales forecast and conversational sales assistant.
12. Evaluate carrier confidence, predictive delivery risk, and AI weather confidence.
13. Finalize the carrier, generate the Tracking ID and GenAI-assisted Shipping Label, and send notifications.
14. Retrieve the order in the Carrier Application and record successful delivery.
15. Generate, email, and store the GenAI-assisted Proof of Delivery.
16. Review live shipment and execution metrics in IBM Business Automation Insights.


[Go to top](#shipment-management-automation) | [Go to Getting Started Lab](#getting-started-lab)

</details>


<a id="section-3-review-the-shipment-management-navigation-application"></a>
<details>
<summary><strong>3. Review the Shipment Management Navigation Application</strong></summary>

The navigation application provides a single starting point for the three Shipment Management applications.

1. Open the **Shipment Management Navigation** application.

2. Review the available application tiles.


3. Confirm that each tile opens the expected application:

| Tile | Expected application |
| :--- | :------------------- |
| Shipment | Shipment Application |
| Warehouse | Warehouse Application |
| Carrier | Carrier Application |

4. Return to the navigation page and select **Shipment Application**.


[Go to top](#shipment-management-automation) | [Go to Getting Started Lab](#getting-started-lab)

</details>

<a id="section-4-run-the-inventory-shortage-and-shipment-creation-scenario"></a>
<details>
<summary><strong>4. Run the Inventory Shortage and Shipment Creation Scenario</strong></summary>

### 4.1 Start the Not Enough Inventory path

1. Open the **Shipment Application**.
2. Open **Select Path for Demo**.
3. Select **Not Enough Inventory**.
4. Confirm the selected scenario and click **Start Demo**.


The Business Automation Application initializes the shipment workflow. Background processing loads order data and prepares the inventory and watsonx AI decision points.

### 4.2 Enter the order and check inventory

1. Enter the required order values:

| Field | Lab action |
| :--- | :--- |
| Order Number | Enter the test Order ID |
| Product | Select the product used for the demonstration |
| Order Count | Enter a quantity greater than available inventory |

2. Click **Check Inventory**.

This action starts an IBM Business Automation Workflow process, creates an order-specific folder in IBM FileNet Navigator, compares requested units with available stock, and invokes the watsonx AI inventory agent.


### 4.3 Review the watsonx AI inventory explanation

1. Wait for the **AI Warehouse Procurement** window to open automatically.
2. Expand **Show Inventory AI Explanation**.
3. Review the insufficient-stock warning, exact shortage quantity, and recommended procurement action.

The response is produced by a watsonx AI agent using current warehouse inventory data. This explanation gives the user an immediate, traceable reason for the procurement recommendation.


### 4.4 Submit the procurement request

1. Open **Supervisor Name**.
2. Select the supervisor responsible for approving the procurement.
3. Click **Submit Procurement Request**.

IBM Business Automation Workflow records the shortage quantity and supervisor assignment and creates an auditable procurement task. IBM Content Services creates and stores the restock PO Receipt, while GenAI generates its business content. The integrated email service sends the procurement details and a clickable **Open PO Receipt** link to the appropriate stakeholders.

![Submit Procurement Request](./Images/020-submit-procurement-request.png)


### 4.5 Review procurement email and PO Receipt

1. Open the automated inventory-restock email.
2. Verify the Order ID, product, required quantity, price per item, requester, order date, and order status.
3. Click **Open PO Receipt**.
4. Review the GenAI-assisted PO Receipt.


### 4.6 Complete shipment creation

1. Return to **Build your shipment**.
2. Confirm **Order is ready for shipment?** remains **No** while procurement is incomplete.
3. After stock is replenished, change **Order is ready for shipment?** to **Yes**.
4. Optionally open the AI chatbot and ask about current warehouse inventory. The chatbot connects the BAA interface to a watsonx AI inventory agent for conversational, real-time responses.
5. Click **Create Shipment**.

IBM Business Automation Workflow consolidates order, delivery, and procurement data. The process integrates with Decision Intelligence (DI), database services, IBM Content Services, and integrated email. DI uses delivery distance to determine transport mode and expected delivery date. The system stores entered and derived values in the database and emails the approved shipment and delivery details.

![Create Shipment](./Images/021-create-shipment.png)


### 4.7 Verify Shipment Application documents

Open the Order ID folder in IBM FileNet Navigator. At this point, it should contain:

1. The PO Receipt for the original customer order.
2. The PO Receipt for the inventory-shortage restock order approved by the supervisor.

Both documents are generated automatically and retained in the same order folder for centralized access and auditability.

[Go to top](#shipment-management-automation) | [Go to Getting Started Lab](#getting-started-lab)

</details>

<a id="section-5-review-intelligent-decisions-ai-explanations-and-notifications"></a>
<details>
<summary><strong>5. Review Intelligent Decisions, AI Explanations, and Notifications</strong></summary>

The Shipment and Warehouse applications combine watsonx AI agents, GenAI, and Decision Intelligence (DI). Each capability has a distinct responsibility.

| Capability | Function in the solution |
| :--- | :--- |
| watsonx AI inventory agent | Compares requested and available quantities, identifies the shortage, explains the result, and recommends procurement |
| watsonx AI inventory chatbot | Answers conversational questions about available warehouse inventory |
| GenAI | Produces business content for PO Receipts, Shipping Labels, and Proof of Delivery documents |
| Decision Intelligence | Determines transport mode and expected delivery date and runs the predictive delivery-risk model |
| Integrated email service | Sends procurement, approval, warehouse, tracking, and delivery notifications with document links |
| Database services | Persists shipment data and derived values and supplies complete order context to downstream applications |

### Verify the shipment decision email

1. Open the shipment confirmation or delivery approval email.
2. Confirm that it includes the Order ID, delivery address, delivery distance, DI-derived transport mode, and expected delivery date.
3. Select **Open PO Receipt** and verify that the document opens.


[Go to top](#shipment-management-automation) | [Go to Getting Started Lab](#getting-started-lab)

</details>

<a id="section-6-process-the-order-in-the-warehouse-application"></a>
<details>
<summary><strong>6. Process the Order in the Warehouse Application</strong></summary>

### 6.1 Retrieve the complete order

1. Open the **Warehouse Application**.
2. Enter or confirm the Order ID.
3. Click **Load Order Details**.

Database services retrieve and populate the order, procurement, shipment, delivery, transport-mode, and expected-delivery-date fields. This avoids duplicate data entry and gives warehouse staff the complete shipment context.


### 6.2 Run the watsonx AI sales forecast

1. Click **Check Sales Forecast**.
2. In the AI Sales Forecast Assessment window, select the product.
3. Select the forecast month and year.
4. Click **Proceed**.
5. Review the forecast returned by the sales forecast AI agent, such as the demonstrated result of **256 units**.

The watsonx AI agent uses the selected product, month, year, and historical sales data to generate a data-driven forecast. Warehouse and procurement teams can use the result to reduce stockouts, excess picks, expedited procurement, and working-capital pressure.

![AI Sales Forecast](./Images/030-ai-sales-forecast.png)


### 6.3 Use the sales forecast chatbot

Open the AI chatbot to ask questions about historical sales figures or future forecasts. The conversational interface connects to the watsonx AI sales forecast agent.


### 6.4 Save warehouse readiness

1. Review the automatically populated transport mode and expected delivery date.
2. After inventory and forecast checks are complete, select **Delivery Ready for Warehouse**.

Database services save the updated values so the latest order information is available to carrier validation and downstream fulfillment.

[Go to top](#shipment-management-automation) | [Go to Getting Started Lab](#getting-started-lab)

</details>

<a id="section-7-evaluate-carrier-confidence-delivery-risk-and-weather"></a>
<details>
<summary><strong>7. Evaluate Carrier Confidence, Delivery Risk, and Weather</strong></summary>

### 7.1 Check carrier confidence

1. Click **Check Carrier Confidence**.
2. Review the carrier recommendation produced from historical carrier-performance data.

The Carrier Confidence AI agent recommends a reliable option to reduce late deliveries, rerouting, and operational cost.

![Carrier Confidence](./Images/040-carrier-confidence.png)


### 7.2 Run the DI predictive delivery-risk assessment

1. Open the **AI Delivery Risk Assessment**.
2. Verify the carrier and delivery dates populated by the system.
3. Run the analysis.
4. Review the returned **Risk Score**, **Risk Level**, **Recommended Action**, and **Top Reasons**.

The predictive decision model in IBM Decision Intelligence analyzes the delivery information and explains the factors influencing the recommended action. This gives warehouse users transparency into the automated carrier decision.

![DI Delivery Risk](./Images/041-di-delivery-risk.png)


### 7.3 Review AI weather confidence

1. Expand **Show AI Weather Analysis**.
2. Review the weather retrieved for each scheduled delivery date.
3. Verify the number of safe and adverse-weather days and the overall weather-confidence rating.


[Go to top](#shipment-management-automation) | [Go to Getting Started Lab](#getting-started-lab)

</details>

<a id="section-8-finalize-carrier-assignment-and-generate-the-shipping-label"></a>
<details>
<summary><strong>8. Finalize Carrier Assignment and Generate the Shipping Label</strong></summary>

1. Review the selected carrier, pickup details, delivery-risk recommendation, and weather confidence.
2. Click **Finalize Carrier Assignment**.

This action starts an IBM Business Automation Workflow process that:

* Generates the Tracking ID.
* Uses IBM Content Services and GenAI to create the Shipping Label.
* Stores the Shipping Label in the existing Order ID folder in IBM FileNet Navigator.
* Updates the database for the Carrier Application.
* Emails the customer with the Tracking ID, expected delivery date, and delivery address.
* Emails the shipment team with warehouse instructions and an **Open Shipping Label** link.


### Verify the document folder

The Order ID folder should now contain:

1. Original-order PO Receipt.
2. Inventory-restock PO Receipt.
3. GenAI-assisted Shipping Label.


[Go to top](#shipment-management-automation) | [Go to Getting Started Lab](#getting-started-lab)

</details>

<a id="section-9-complete-delivery-in-the-carrier-application"></a>
<details>
<summary><strong>9. Complete Delivery in the Carrier Application</strong></summary>

### 9.1 Retrieve carrier and shipment records

1. Open the **Carrier Application**.
2. Enter or confirm the Order Number.
3. Click **Load Order Details**.

Database services populate the shipment, carrier, Tracking ID, pickup, and delivery information, reducing duplicate entry and accelerating verification.


### 9.2 Record successful delivery

1. In **Update Carrier Delivery Status**, enter the delivery timestamp.
2. Set the Delivery Status to **Delivered**.
3. Enter brief delivery notes.
4. Submit the update.
5. Confirm the message **Your shipment status is delivered**.
6. Click **Finish**.


### 9.3 Review the Proof of Delivery

After completion, the integrated email service notifies the customer that the order was delivered. The customer can select **Open Proof of Delivery** to view the document generated from delivery information and GenAI-created content.

IBM Content Services stores the Proof of Delivery in the same Order ID folder in IBM FileNet Navigator.

![Final Delivery Email](<./Images/051-final-delivery-email.png>)


[Go to top](#shipment-management-automation) | [Go to Getting Started Lab](#getting-started-lab)

</details>

<a id="section-10-verify-documents-data-and-business-automation-insights"></a>
<details>
<summary><strong>10. Verify Documents, Data, and Business Automation Insights</strong></summary>

### 10.1 Verify the complete FileNet record

Open the Order ID folder and confirm that it contains:

| Document | Purpose |
| :--- | :--- |
| Original-order PO Receipt | Records the customer's original order |
| Inventory-restock PO Receipt | Records the supervisor-approved procurement created for the shortage |
| Shipping Label | Supports carrier pickup and shipment routing |
| Proof of Delivery | Records the completed delivery |

All documents are generated automatically and stored in one order-specific folder for centralized access, traceability, and audit review.

![Complete FileNet Order Folder](./Images/050-complete-filenet-order-folder.png)


### 10.2 Review IBM Business Automation Insights

1. Select **Go to BAI Dashboard**.
2. Review live shipment metrics by product, delivery status, carrier, and date.
3. Review average execution times for transport and delivery-date decisions, inventory checks, database updates, Shipping Label generation, customer notifications, and Proof of Delivery creation.

IBM Business Automation Insights captures process events from the Shipment, Warehouse, and Carrier processes and makes operational performance and shipment trends visible to business teams.

![BAI Shipment Dashboard](./Images/060-bai-shipment-dashboard.png)


[Go to top](#shipment-management-automation) | [Go to Getting Started Lab](#getting-started-lab)

</details>

<a id="section-11-explore-additional-exception-paths"></a>
<details>
<summary><strong>11. Explore Additional Exception Paths </strong></summary>


### 11.1 Inventory shortage and procurement

The demonstrated shortage path includes the complete exception response:

1. Requested quantity exceeds available stock.
2. The watsonx AI inventory agent identifies and explains the exact shortage.
3. The application prevents shipment creation by keeping **Order is ready for shipment?** set to **No**.
4. A supervisor is assigned for governance and traceability.
5. BAW creates an auditable procurement task.
6. GenAI creates the restock PO Receipt content.
7. IBM Content Services stores the receipt.
8. Integrated email notifies the warehouse team and supplies the document link.
9. Shipment creation resumes only after replenishment.

### 11.2 Tracking exception

Process a separate order without enabling tracking or without generating a Tracking ID. Confirm that the solution raises the configured tracking follow-up instead of silently advancing the shipment.

### 11.3 Delivery failed

In the Carrier Application, set the status to **Delivery Failed**, provide the failure reason, and submit. Confirm that the configured process notifies the manager and customer and preserves the failure information for reporting.

[Go to top](#shipment-management-automation) | [Go to Getting Started Lab](#getting-started-lab)

</details>

<a id="section-12-summary"></a>
<details>
<summary><strong>12. Summary</strong></summary>

The Shipment Management solution connects Shipment, Warehouse, and Carrier teams across one order-to-delivery lifecycle.

* **IBM Business Automation Application** provides the Shipment, Warehouse, Carrier, and navigation user interfaces.
* **IBM Business Automation Workflow** orchestrates inventory checks, procurement, shipment creation, warehouse fulfillment, carrier assignment, delivery updates, notifications, and document operations.
* **IBM Decision Intelligence** determines transport mode and expected delivery date and provides the predictive delivery-risk model with a risk score, risk level, recommended action, and explanatory reasons.
* **watsonx AI agents** support inventory-shortage analysis, conversational inventory questions, sales forecasting, conversational sales insights, carrier confidence, and weather analysis.
* **GenAI** produces business content for PO Receipts, Shipping Labels, and Proof of Delivery documents.
* **IBM Content Services and FileNet Navigator** maintain the complete document record in the Order ID folder.
* **Database services** persist and retrieve the shared order context for all three applications.
* **Integrated email** sends procurement, approval, warehouse, tracking, and delivery notifications with document links.
* **IBM Business Automation Insights** visualizes shipment outcomes and process execution metrics.

Together, these capabilities reduce manual entry, procurement and delivery delays, missing documents, operational costs, and limited delivery visibility.

[Go to top](#shipment-management-automation) | [Go to Getting Started Lab](#getting-started-lab)

</details>

***


# Application Architecture

## Business Applications

| Application | Key capabilities |
| :--- | :--- |
| Shipment Application | Scenario selection, order entry, inventory check, AI shortage explanation, supervisor procurement, inventory chatbot, and shipment creation |
| Warehouse Application | Order retrieval, sales forecast, sales chatbot, delivery readiness, carrier confidence, DI delivery-risk assessment, weather analysis, and carrier finalization |
| Carrier Application | Order retrieval, delivery timestamp, delivery status, delivery notes, and process completion |

## Intelligent Automation

| Technology | Implemented function |
| :--- | :--- |
| watsonx AI inventory agent | Detects and explains shortages and recommends procurement |
| watsonx AI inventory chatbot | Answers real-time warehouse inventory questions |
| watsonx AI sales forecast agent | Forecasts product demand using product, month, year, and historical sales data |
| watsonx AI sales chatbot | Answers historical and forecast sales questions |
| Carrier Confidence AI agent | Uses historical carrier performance to recommend a reliable carrier |
| Weather AI agent | Retrieves forecast data through a weather API and calculates safe days, adverse days, and weather confidence |
| IBM Decision Intelligence | Determines transport mode and expected delivery date and executes the predictive delivery-risk model |
| GenAI | Generates content for PO Receipts, Shipping Labels, and Proof of Delivery documents |

## Workflow, Data, Content, and Analytics

| Component | Responsibility |
| :--- | :--- |
| IBM Business Automation Workflow | Orchestrates Shipment, Warehouse, Carrier, and procurement activities |
| Database services | Persist and retrieve order, procurement, shipment, carrier, tracking, and delivery data |
| Integrated email service | Sends stakeholder notifications and clickable document links |
| IBM Content Services | Creates and stores lifecycle documents |
| IBM FileNet Navigator | Provides the centralized Order ID folder and document access |
| IBM Business Automation Insights | Displays live shipment metrics and activity execution times |

## Lifecycle Documents

| Document | Creation point | Content technology | Storage |
| :--- | :--- | :--- | :--- |
| Original-order PO Receipt | Shipment stage | GenAI-assisted | Order ID folder in FileNet |
| Inventory-restock PO Receipt | Shortage procurement | GenAI-assisted | Same Order ID folder |
| Shipping Label | Carrier finalization | GenAI-assisted | Same Order ID folder |
| Proof of Delivery | Delivery completion | GenAI-assisted | Same Order ID folder |
