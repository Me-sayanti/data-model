# 📐 Enterprise Data Model — Indian E-Commerce Sales Platform

> **Role demonstrated:** Data Modeller  
> **Tools:** dbdiagram.io · draw.io · SQL DDL  
> **Dataset:** Same as P1 — [E-Commerce Sales — Kaggle](https://www.kaggle.com/datasets/benroshan/ecommerce-data)  
> **Deliverables:** Conceptual model → Logical model → Physical model → ER diagram → Data dictionary → STT mapping

---

## Business Problem

Before any pipeline is built, a data modeller must answer: *What does the data mean? How should it be structured? What are the rules?*

This project documents the **full data modelling lifecycle** for the same e-commerce platform as Project 1 — from a blank page to a production-ready physical model. This is the artefact a Data Modeller would hand to a Data Engineer before sprint kickoff.

---

## Modelling Lifecycle Deliverables

```
[Business Requirements]
         │
         ▼
[Conceptual Model]  ← entities & relationships, no data types
         │
         ▼
[Logical Model]     ← attributes, primary/foreign keys, normalised
         │
         ▼
[Physical Model]    ← data types, constraints, indexes, DDL
         │
         ▼
[ER Diagram]        ← visual — generated from dbdiagram.io
         │
         ▼
[Data Dictionary]   ← every field defined in business language
         │
         ▼
[STT Mapping]       ← source CSV field → target Gold table field
```

---

## Repository Structure

```
P2-data-model/
├── README.md
├── docs/
│   ├── 01_conceptual_model.md       ← Entities, relationships, business rules
│   ├── 02_logical_model.md          ← Attributes, keys, normalisation notes
│   ├── 03_data_dictionary.md        ← Full field definitions
│   └── 04_business_rules.md        ← All rules that govern data validity
├── sql/
│   └── physical_model_ddl.sql       ← Full CREATE TABLE DDL with types + constraints
├── diagrams/
│   ├── conceptual_erd.png           ← Entity boxes and relationship lines (no attributes)
│   ├── logical_erd.png              ← Attributes, PKs, FKs, normalised
│   └── physical_star_schema.png     ← Final star schema — fact + dims
└── screenshots/
    └── dbdiagram_screenshot.png     ← Screenshot of dbdiagram.io canvas
```

---

## Step-by-Step Build Plan

### Day 1 — Understand the Domain (Requirements)
1. Download the 3 CSVs; open in Excel — understand what every column means
2. Write down in plain English:
   - What is an Order? (a customer buying something on a date)
   - What is a Line Item? (a specific product category in that order)
   - What is a Target? (monthly sales goal for a category)
3. Identify the **entities**: Customer, Order, Product, Geography, Date, SalesTarget
4. Identify **relationships**: Customer *places* Orders · Orders *contain* Line Items · Line Items *belong to* Product

### Day 1–2 — Conceptual Model
- In draw.io (free): draw entity boxes with just the entity name
- Draw relationship lines with cardinality labels (1:N, M:N)
- No attributes yet — just the business concepts
- Export as PNG → `diagrams/conceptual_erd.png`
- Write `docs/01_conceptual_model.md` explaining each entity in business language

### Day 2 — Logical Model
- Add attributes to each entity
- Define primary keys (natural: Order ID; or surrogate: customer_key)
- Define foreign key relationships
- Resolve M:N relationships (Orders ↔ Products becomes the line_item bridge)
- Check for normalisation: is anything repeated unnecessarily?
- Build the logical model in dbdiagram.io (free) — export PNG
- Write `docs/02_logical_model.md`

### Day 3 — Physical Model (Star Schema)
- Convert logical model to star schema for analytics:
  - fact_sales (1 row per line item — the fact)
  - dim_customer, dim_product, dim_date, dim_geography (the dimensions)
- Assign exact SQL data types (STRING, INT, DOUBLE, DATE, BOOLEAN)
- Add NOT NULL constraints
- Write `sql/physical_model_ddl.sql` — full CREATE TABLE statements
- Export final star schema diagram → `diagrams/physical_star_schema.png`

### Day 3–4 — Data Dictionary & Business Rules
- Fill `docs/03_data_dictionary.md` — every field with: name, type, nullable, definition, example value
- Write `docs/04_business_rules.md` — every constraint with: rule ID, field, rule, action on violation

### Day 4 — GitHub & Polish
- Upload all diagrams, docs, and SQL to repo
- Add a screenshot of your dbdiagram.io canvas

---

## Entities & Relationships (Conceptual Level)

| Entity | Description | Key Attributes |
|---|---|---|
| **Customer** | Person who places an order | Name, City, State |
| **Order** | A single transaction — one customer, one date | Order ID, Order Date |
| **Line Item** | One product category within an order | Amount, Quantity, Profit |
| **Product** | Category + sub-category of goods sold | Category, Sub-Category |
| **Geography** | Location of the customer | State, Region |
| **Date** | Calendar attributes of the order date | Date, Year, Month, Quarter |
| **Sales Target** | Monthly revenue goal by category | Month, Category, Target Amount |

**Relationships:**
- Customer PLACES one or more Orders (1:N)
- Order CONTAINS one or more Line Items (1:N)
- Line Item BELONGS TO one Product category (N:1)
- Order is PLACED IN a Geography (N:1)
- Order occurs ON a Date (N:1)
- Sales Target SET FOR a Product category each Month (M:1)

---

## Normalisation Decisions

| Decision | Justification |
|---|---|
| Customer separated from Order | Same customer can place multiple orders — deduplication required |
| Geography separated from Customer | State/region data reused across multiple customers |
| Date dimension generated | Enables time-intelligence in Power BI (YoY, MTD, QoQ) without recalculating in reports |
| Sub-Category kept in dim_product | Avoids repeating category name in fact — 3NF compliant |
| profit_margin_pct kept in fact | Derived measure — calculated once in Silver, not repeatedly in BI |

---

## CV Bullet Points

*Add these under "Projects" on your CV once complete:*

- Delivered the **full data modelling lifecycle** for an e-commerce analytics platform — from conceptual ER model through logical normalisation to physical star schema DDL — producing a **data dictionary**, **business rules document**, and **source-to-target mapping** ready for engineering handoff.
- Designed a **star schema** (1 fact table, 4 dimension tables) using dimensional modelling best practices — resolving M:N relationships, assigning surrogate keys, and specifying NOT NULL constraints and data types in production-ready **SQL DDL**.
- Applied **3NF normalisation principles** and documented all modelling decisions with business justifications — enabling engineering teams to build without ambiguity.
