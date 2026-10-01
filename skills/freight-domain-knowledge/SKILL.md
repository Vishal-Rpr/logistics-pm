---
name: freight-domain-knowledge
description: "This skill provides freight forwarding and logistics domain expertise for product management work. It should be used when the user mentions shipping, freight, logistics, forwarding, customs, RFQ, BOL, rate quoting, container types, Incoterms, carrier management, consolidation, demurrage, credit limits, accounting integrations, landed cost, or any B2B supply chain workflow. It covers the full logistics lifecycle from procurement to last-mile delivery, freight forwarding execution workflows, documentation and compliance requirements, financial reconciliation, and the digital transformation happening across the industry."
---

# Freight Domain Knowledge for Product Managers

## Purpose

Provide operationally accurate logistics context so that PM artifacts (PRDs, specs, user stories, strategy docs) reflect how freight forwarding actually works on the ground. This is not a glossary. It is working knowledge that should inform every section of a logistics feature spec -- from data models to edge cases to compliance constraints.

---

## Part 1: The Macro Logistics Framework

Logistics manages the lifecycle of a product from raw material to final customer delivery. Think of logistics as the architect designing the entire blueprint, and freight forwarding as the general contractor handling the complex, cross-border transportation.

### Procurement and Sourcing
Acquiring raw materials or finished inventory from suppliers, scheduling receiving slots, and auditing vendor compliance. Systems track purchase orders, supplier lead times, and inbound shipment schedules.

### Storage and Warehouse Management
Overseeing product intake, strategic cross-docking (moving incoming goods directly to outbound trucks without warehousing), and allocating optimal storage locations (slotting). Managed via Warehouse Management Systems (WMS).

### Inventory Optimization
Tracking stock levels in real time via WMS to predict demand and trigger automatic reorders before stockouts happen. Safety stock calculations, ABC classification, and reorder point logic drive these systems.

### Order Fulfillment (Picking and Packing)
Consolidating ordered items via strategic methods (batch picking, zone picking, wave picking) to package and prep them for transit efficiently. Pick accuracy and pack-out speed are key operational metrics.

### Distribution and Last-Mile Delivery
Managing the final leg of transport. Relies heavily on routing algorithms to maximize delivery productivity and reduce "empty miles" (driving without revenue-generating cargo). Last-mile is typically the most expensive segment per unit.

---

## Part 2: Freight Forwarding Execution

When goods need to move internationally or via multi-modal transport, the freight forwarding workflow takes over. This is highly procedural -- sequence and strict documentation are critical. A single missing document can strand cargo at a port for days.

### Stage 1: Quotation and Booking

The shipper issues a Request for Quote (RFQ). The forwarder calculates the total **landed cost** -- including:
- **Base freight rate** (ocean, air, or rail)
- **Fuel surcharges** (BAF -- Bunker Adjustment Factor)
- **Currency adjustment** (CAF -- Currency Adjustment Factor)
- **Terminal handling** (THC -- Terminal Handling Charges)
- **Peak season surcharge** (PSS -- when demand exceeds capacity)
- **Emissions charges** (ETS -- Emissions Trading System, increasingly common in EU trade lanes)
- **Port and documentation fees**

The forwarder's margin is the spread between the **buy rate** (what they pay the carrier or NVOCC) and the **sell rate** (what they charge the client).

**Rate types:**
- **Spot rate** -- One-time rate for a single shipment. Higher price, no commitment.
- **Contract rate** -- Negotiated for a defined period and volume commitment. Lower price, guaranteed capacity.

**Market intelligence:** Rate intelligence platforms provide independent benchmarking data (market low, average, high) across a broad set of global port pairs. A forwarder can validate whether their proposed rate is competitive before sending it to the client. This benchmark step must add no more than 2-3 seconds to the quoting workflow.

Once the client accepts the quote, the forwarder secures a booking with the ocean, air, or rail carrier.

### Stage 2: Cargo Pickup and Consolidation (First Mile)

The forwarder arranges **inland haulage** to bring goods from the shipper's warehouse to an origin facility (port, airport, or consolidation warehouse).

**Consolidation** is where cost efficiency happens:
- Multiple shippers' **LCL** (Less-than-Container Load) cargo is combined into a single **FCL** (Full Container Load) to reduce per-unit transit costs
- Pricing for LCL is per CBM (cubic meter) or weight ton, whichever is greater
- Pricing for FCL is per container regardless of how full it is

**Container types and their implications:**
| Type | Use case | Pricing note |
|------|----------|-------------|
| 20' dry (TEU) | Standard cargo, the base unit of container shipping | Base rate unit |
| 40' dry (FEU) | Standard cargo, higher volume | Typically 1.5-1.8x a 20' rate, not 2x |
| 40' High Cube (HC) | Tall or voluminous cargo | Slight premium over standard 40' |
| 20' / 40' Reefer | Temperature-controlled (perishables, pharma) | Significant premium + power supply fees |
| Open Top / Flat Rack | Oversized or heavy machinery | Priced per case, often breakbulk rates |

### Stage 3: Export Customs Clearance

Before cargo can touch international waters or airspace, the forwarder (often acting as customs broker) prepares and submits regulatory paperwork:
- **Commercial Invoice** -- Value, quantity, buyer/seller details. Determines customs duties.
- **Packing List** -- Itemized contents of each package/container by weight and volume.
- **Certificate of Origin (COO)** -- Proves where goods were manufactured. Affects tariff rates and trade agreement eligibility.
- **Export permits** -- Required for controlled goods (dual-use technology, hazmat, sanctioned destinations).
- **ISF (Importer Security Filing)** -- Required by US CBP 24 hours before vessel departure for US-bound cargo. Missing this triggers fines and potential cargo holds.

A single missing document or incorrect HS (Harmonized System) classification code can trigger:
- Cargo holds at the port
- **Demurrage charges** (fees for cargo sitting at a port/terminal beyond free time)
- **Detention charges** (fees for keeping a container beyond the allotted free time after pickup)
- Regulatory penalties

### Stage 4: Main Carriage and Milestone Tracking

Goods are handed over to the primary carrier, who issues a **Master Bill of Lading (MBL)**. The forwarder issues a **House Bill of Lading (HBL)** to the shipper.

**The BOL hierarchy matters:**
- **MBL** -- Contract between the ocean carrier (major shipping lines) and the freight forwarder. The carrier only recognizes the forwarder.
- **HBL** -- Contract between the freight forwarder and the shipper (their client). The carrier does not see the HBL.
- This two-layer structure is what allows forwarders to consolidate cargo from multiple shippers under one MBL.

**Milestone tracking** follows the shipment through key events:
1. Departed origin port
2. Transshipment (if not a direct service -- cargo moves between vessels at a hub port)
3. Arrived destination port
4. Customs hold (if triggered)
5. Cleared customs
6. Out for delivery
7. Delivered / POD (Proof of Delivery)

Tracking data comes from carrier APIs, AIS (Automatic Identification System) vessel data, or third-party carrier tracking platforms. Modern systems trigger exception-based alerts when a shipment deviates from its expected timeline.

### Stage 5: Import Customs and Final Delivery (Last Mile)

Upon arrival at the destination country, the cargo must clear customs again. The import process mirrors export but with additional duties and tax calculations based on:
- HS classification code
- Declared value on the commercial invoice
- Country of origin (trade agreements may reduce or eliminate duties)
- Whether anti-dumping duties apply

Once released, the forwarder organizes final inland haulage (truck or rail) to deliver goods to the consignee.

---

## Part 3: Documentation and Financial Workflows

Running parallel to physical cargo movement are the data workflows that keep freight from getting stranded at borders and cash from getting stuck in reconciliation.

### The Document Stack

A single international shipment can involve 10-20 documents. The key ones:

| Document | Issued by | Purpose | Risk if missing |
|----------|-----------|---------|-----------------|
| House BOL (HBL) | Forwarder to shipper | Receipt of goods, contract of carriage | Shipper cannot claim cargo |
| Master BOL (MBL) | Carrier to forwarder | Carrier's contract, controls physical cargo release | Cargo stranded at port |
| Commercial Invoice | Shipper | Value declaration for customs | Customs hold, incorrect duty calculation |
| Packing List | Shipper | Weight/volume/contents detail | Inspection delays |
| Certificate of Origin | Chamber of Commerce or shipper | Proves manufacturing origin | Loss of preferential tariff rates |
| ISF (US-bound) | Forwarder or broker | US security filing | $5,000+ fine per violation |
| Arrival Notice | Carrier/forwarder | Alerts consignee of arrival | Missed free time, demurrage charges |

### Financial Reconciliation

Modern forwarding systems tie financial milestones directly to shipment milestones. This is where the money moves:

**Cost capture:** Tariffs, fuel surcharges, port fees, inland haulage, and customs duties are captured as the shipment progresses. Each cost line ties to a specific shipment event.

**Revenue recognition:** Accounts Receivable triggers an invoice when a container hits a "Cleared Customs" or "Delivered" milestone, reducing payment friction and accelerating cash flow.

**Credit exposure formula:**
```
Customer credit exposure =
    Sum of all outstanding invoices (net of payments and adjustments)
  + Debit notes issued at customer level
  - Credit notes issued at customer level
  - Unadjusted advances/payments

Available credit = Assigned credit limit - Customer credit exposure
```


**Accounting system sync:** Most forwarders sync invoicing and payment data with an accounting or ERP system. Key architectural decisions:
- Direction: The accounting system is typically the source of truth for payments; the forwarding app is the source of truth for shipment-linked charges
- Frequency: Scheduled polling (15-30 min) vs. webhooks. Polling is simpler; webhooks are faster but harder to debug
- Failure handling: Failed syncs must be logged, retried, and surfaced in a reconciliation dashboard
- Partial payments: Invoices can be partially paid; the system must track partial application

---

## Part 4: The Digital Transformation

### Where the industry was
Historically, these workflows relied on fragmented emails, spreadsheets, phone calls, and paper documents passed between parties. A single shipment might generate 50+ emails across 5+ parties.

### Where it is moving
Integrated software platforms (established TMS vendors and custom-built TMS applications) are replacing manual coordination with:
- **AI-powered OCR** to read shipping documents automatically and extract structured data
- **Exception-based alerting** -- instead of tracking every shipment, the system surfaces only shipments with problems (customs hold, missed vessel, document discrepancy)
- **Milestone-triggered automation** -- auto-generate invoices on delivery, auto-send arrival notices, auto-update credit positions
- **Rate intelligence platforms** providing market benchmarking data via API
- **MCP/API integrations** connecting forwarding apps to carrier systems, accounting platforms, and customs filing systems

### The gaps that remain
- Most platforms are generic. No industry-specific AI tooling for freight PM work.
- Document processing is improving but still requires human review for complex or non-standard documents.
- Rate benchmarking exists but is not embedded in most quoting workflows -- it's a separate step.
- Credit risk assessment is still largely manual (spreadsheet-based reviews).
- No standardized eval frameworks for measuring whether AI-generated logistics specs are operationally accurate.

---

## Part 5: Key Integrations Map

| System | Purpose | Examples | Data flow |
|--------|---------|----------|-----------|
| TMS | Core operational platform | Established TMS platforms or custom builds | Bidirectional hub |
| Ocean carriers | Booking, tracking, BOL | Major global shipping lines | API: booking requests out, tracking events in |
| NVOCCs | Buy rates, consolidation | NVOCC consolidators | Rates in, booking requests out |
| Rate intelligence | Market benchmarking | Rate intelligence platforms | API: rate queries out, market data in |
| Accounting | Invoicing, payments, credit | Accounting or ERP platforms | Sync: invoices out, payments in |
| Carrier tracking | Real-time visibility | Carrier tracking / visibility platforms | Events in, status queries out |
| Customs / compliance | Filing, classification | Government customs systems (e.g., US CBP ABI) | Filings out, clearance status in |
| CRM | Client management | Enterprise CRM platforms | Client data bidirectional |
| Document management | BOLs, commercial docs | Internal systems, DocuSign | Documents in, approvals out |

---

## Part 6: Incoterms (2020)

Incoterms define where cost and risk transfer from seller to buyer. They directly affect which party the forwarder bills for which legs of transport.

| Term | Risk transfers at | Seller pays for | Common in |
|------|-------------------|-----------------|-----------|
| **EXW** (Ex Works) | Seller's premises | Nothing beyond making goods available | Domestic, buyer-controlled supply chains |
| **FCA** (Free Carrier) | Named place of delivery | Delivery to carrier at origin | Growing alternative to FOB for containers |
| **FOB** (Free On Board) | Ship's rail at origin port | Inland transport + export clearance | Most common for ocean freight |
| **CIF** (Cost, Insurance, Freight) | Ship's rail at origin port (cost paid further) | Freight + insurance to destination port | Common when seller controls shipping |
| **DAP** (Delivered at Place) | Named destination (before unloading) | All transport to destination | E-commerce, door-to-door |
| **DDP** (Delivered Duty Paid) | Buyer's premises | Everything including import duties | Maximum seller obligation |

**Why this matters for PMs:** Incoterms determine which cost lines appear on which party's invoice. A feature that generates quotes must know the Incoterm to calculate which charges to include.

---

## Part 7: Domain-Specific PM Considerations

When writing specs or PRDs for logistics features, always address:

1. **Data sync direction and source of truth** -- Which system owns which data? The forwarding app owns shipment data; the accounting system owns payment data. Conflicts between them need a reconciliation workflow, not a silent overwrite.

2. **Speed constraints** -- Quoting is time-sensitive. Any feature in the quoting path must add minimal latency (<3 seconds). Carrier booking APIs can be slow; design for async confirmation where possible.

3. **Multi-party data flows** -- A single shipment involves the shipper, consignee, carrier, customs broker, port authority, and the forwarder. Data flows between all of them, often in different formats. Every feature should map which parties produce and consume which data.

4. **Compliance as a hard requirement** -- Customs regulations, trade sanctions, Incoterms obligations, and ISF filing deadlines are not "nice to have" sections. A missed ISF filing is a $5,000+ fine. A wrong HS code is a customs hold. Specs must treat compliance as blocking, not advisory.

5. **Financial exposure and credit risk** -- Features that touch money need explicit handling of: credit limits, payment terms, currency conversion, partial payments, and the cascading effect of over-limit clients on booking flows.

6. **Demurrage and detention economics** -- Delays are expensive. Demurrage (port/terminal fees) and detention (container fees) accrue daily. Features that affect cargo velocity (customs clearance, document processing, booking confirmation) should quantify the cost of delays.

7. **Audit trails** -- Regulated industry. Every change to a BOL, invoice, credit limit, or customs filing needs a who/when/why log. Audit is not a phase-2 feature; it ships with v1.

8. **Fallback behavior** -- When an API (carrier, rate provider, accounting system) is down, what does the user experience? Define graceful degradation: cached rates, manual override, queued sync retry.

9. **Document-driven workflows** -- Many logistics processes are blocked until a specific document is received, signed, or filed. Features should model document state (draft, submitted, accepted, rejected) as a first-class entity.

10. **Exception handling over happy-path tracking** -- Ops teams don't need dashboards for shipments going smoothly. They need systems that surface exceptions: customs holds, missed vessels, document discrepancies, credit limit breaches. Design for the unhappy path first.
