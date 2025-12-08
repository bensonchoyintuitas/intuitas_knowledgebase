# Intuitas Data Modelling Framework
[Return to home](README.md)

> Updated 5/12/2025

This resource provides a lightweight framework for describing and developing data models. It draws from a range of suggested practices, with references provided where appropriate.

> **Note:** See [Modelling Standards and Conventions](modelling_standards_and_conventions.md) for notation and modelling standards.

## Table of Contents

**Core Model Types**
- [Conceptual Models](#conceptual-information-models)
- [Logical Models](#logical-information-models)
- [Physical Models](#physical-data-models)

**Enterprise Context**
- [Domain Topology](#domain-topology)
- [Domain Models](#domain-models)
- [Canonical Models](#canonical-models)

**Specialised Model Types**
- [Business Process Models](#business-process-models)
- [Data Warehouse Models](#data-warehouse-models)
  - [Data Vault Model](#data-vault-model)
  - [Dimensional (Kimball) Model](#dimensional-kimball-model)
  - [Dimensional Bus Matrix](#dimensional-bus-matrix)
- [Semantic Layers](#semantic-layers)
- [Master Data and Reference Data](#master-data-reference-data-and-associated-physical-models)
- [Hierarchies](#hierarchies)


## Modelling Concepts

We distinguish between business information design and technical data implementation:

- **Information Models** (Conceptual and Logical): Semantic models focused on business meaning, relationships, and rules. Technology-agnostic, designed for business stakeholders, analysts, and architects.
- **Data Models** (Physical): Implementation-specific models focused on storage, performance, and technical constraints. Designed for database developers and engineers.

> Some organisations use "data model" as an umbrella term for all three layers, which is also acceptable.

### Conceptual (Information) Models

Conceptual models represent business meaning without implementation concerns.

**What to Include:**
- Core business concepts (entities) that are identifiable, meaningful, and properly named
- Key relationships between concepts
- High-level business rules

**What to Exclude:**
- Primary or surrogate keys
- Data types
- Precise cardinality (beyond "one" vs "many")

> Use the **business glossary** to map domain-specific terms as synonyms to canonical enterprise terms.

#### Subtypes and Supertypes

**Choosing the Right Level:**
- **Too abstract:** Not practical (e.g., "Thing")
- **Too granular:** Should be attributes instead (e.g., "Customer Type")
- **Just right:** Meaningful specialisations with distinct data requirements (e.g., "Individual Customer" vs "Corporate Customer")

**Subtype Rules:**
- **Mutually exclusive:** Each instance belongs to one subtype only
- **Collectively exhaustive:** Desirable but flexible—models evolve over time

> Avoid multiple classification hierarchies; choose the primary classification dimension.


### Logical (Information) Models

Logical models extend conceptual models with structure and precision while remaining technology-independent.

**What to Include:**
- **Keys and identifiers:** Natural and alternate keys
- **Cardinality:** Precise relationships (1:1, 1:M, M:N)
- **Optionality:** Mandatory vs optional relationships and attributes
- **Attribute definitions:** Clear definitions and business rules
- **Normalisation:** Applied where relevant for data integrity

### Physical (Data) Models 

Physical models implement conceptual and logical models in specific technologies. They focus on storage, performance, and technical constraints for database developers and engineers.

**Common Physical Formats:**

- **Relational (3NF):** Traditional normalised models for transactional systems (OLTP). Used in PostgreSQL, SQL Server, Oracle.
- **Dimensional:** Star/snowflake schemas optimised for analytics (OLAP). See [Dimensional Models](#dimensional-kimball-model).
- **Semi-Structured:** JSON, Parquet, Avro supporting nested data. Used in data lakes, MongoDB, Delta Lake, Databricks.
- **Graph:** Node/edge structures for relationship-heavy queries. Used in Neo4j, Amazon Neptune.
- **Key-Value:** Simple stores optimised for speed and scale. Used in DynamoDB, Cosmos DB, Redis.

> **Note:** Physical models often exist without documented conceptual/logical models, but these can be reverse-engineered to understand business meaning.

## Enterprise Context

Enterprise-wide modelling is challenging due to semantic differences across functional areas. Instead, we focus on common touch points using Domain Models and Canonical Models where systems and domains integrate.

### Domain Topology

[Refer to Domain-Centric Design](level_1.md#domain-centric-design)

Domains represent functional and organisational boundaries. Each domain encapsulates responsibilities, services, processes, information, expertise, and governance. Domains serve their own objectives while offering value to other domains and the enterprise.

A **Domain Topology** maps domains across the enterprise and their relationships, typically organised by functional areas (though other dimensions like region may apply).

<div align="center">

<em>Illustative example domains for a Healthcare organisation</em>
<br>
<a href="../img/domain-example.png" target="_blank">
    <img src="../img/domain-example.png"  alt="Example domains for a Healthcare organisation" width="75%">
</a>
<br>
</div>

### Domain Models

Under Domain-Centric Design, domains are authoritative for their information definitions. They own concept meanings, define lifecycle rules, and act as the system of semantic truth for their scope.

A **Domain Model** describes business concepts relevant to a domain, including:
- **Business Objects:** Operational entities with identity, lifecycle, and direct participation in business processes (e.g., Patient, Order, Product)
- **Supporting Concepts:** Qualities, rules, conditions, and classifications (e.g., Risk, Compliance, Status)

**Relationship to Business Processes:**
- Domain Models define **WHAT** (objects, attributes, relationships)
- [Business Process Models](#business-process-models) define **HOW** (creation, transformation, consumption)
- Processes reveal which objects matter, expose lifecycles, uncover rules, and clarify business language
- Clear domain models enable process design through shared vocabulary

> **Terminology:** We prefer "Domain" over "Subject Area" as it ties to governance and ownership boundaries, supporting clear accountability for data products and outcomes.

### Canonical Models

Canonical models represent authoritative, agreed-upon definitions for concepts standardised across domains and systems (e.g., APIs, interoperability, conformed dimensions).

**Characteristics:**
- Conceptual and logical at minimum; physical where applicable
- Represent standardised information contracts
- Support interoperability and consistent interpretation

**Use canonical models where:**
- Multiple domains integrate
- Semantic consistency is required
- Cross-domain processes exchange information

**Role in Cross-Domain Processes:**
- Define integration points and standardised entities (e.g., canonical "Customer")
- Enable process flows across domains (e.g., Sales → Fulfilment → Finance)
- Document data contracts at each handoff point


## Specialised Model Types

### Business Process Models

Business process models illustrate activity sequences, decision points, participants, and work flows within and across domains. They define **business objects** (e.g., Customer, Order, Invoice) that form the basis of **conceptual models** within [Domain Models](#domain-models) and [Fact Tables and Business Processes in Dimensional Modeling](#dimensional-kimball-model).


### Data Warehouse Models

Data warehouses transform data through stages optimised for performance and analysis while preserving conceptual and logical semantics:

- Conform to [canonical standards](#canonical-models)
- Apply reference data and business terminology
- Ensure data quality and reliability
- Preserve history for time-based analysis
- Enrich with metadata for discovery and governance
- Optimise for consumption ([dimensional](#dimensional-kimball-model), [data vault](#data-vault-model))
- Aggregate for analytical use

Each stage has distinct physical structures aligned to underlying logical models.

#### Data Vault Model

Data Vault models centre on business concepts and associations from Enterprise/Domain Conceptual Models. Refer to Data Vault 2.0 standards for guidance.

#### Dimensional (Kimball) Model

Kimball Dimensional Modelling creates **Information Marts** using **Star Schemas**:
- **Fact tables:** Quantitative metrics/events from business processes (sales, shipments, payments)
- **Dimension tables:** Descriptive context (customer, product, date, region)

Intentionally denormalised for query performance and user-friendly analysis.

**Conformed Dimensions:**
Conformed dimensions are physical implementations of [canonical models](#canonical-models) in Dimensional models:
- Ensure consistent definitions across fact tables and marts (e.g., shared Customer dimension across Sales, Support, Marketing)
- Enable cross-process analysis and drill-across queries
- Achieve "single version of truth" across analytical systems

#### Dimensional Bus Matrix

A **Bus Matrix** maps intersections between business processes (facts) and dimensions for scalable warehouse design.

**Structure:**
- Rows: Business processes (Orders, Shipments, Payments)
- Columns: Dimensions (Customer, Product, Time, Geography)
- Cells: ✓ indicates dimension usage

**Benefits:**
- Identifies common/conformed dimensions for consistency
- Supports modular, independent implementation
- Enables cross-process analytics

**Example:**

| Process \ Dimension | Customer | Product | Time |
|---------------------|----------|---------|------|
| Orders              |    ✓     |   ✓     |  ✓   |
| Shipments           |    ✓     |   ✓     |  ✓   |

> Guides star schema implementation and dimension standardisation.


### Semantic Layers

Semantic layers are business-oriented abstraction layers that present data in business terms rather than physical structures.

**Purpose:**
- Translate physical structures into business concepts
- Encapsulate business logic, measures, and calculations
- Provide consistent metric definitions for analytics
- Simplify access for reporting and self-service use

**Examples:** Power BI semantic models, dbt Semantic Layer

Semantic layers depend on domain and canonical models for base entities but establish authoritative definitions for derived metrics within their consumption context.  

#### Measures and Metrics

**Measures** and **metrics** are first-class semantic layer concepts that quantify business performance.

**Measures:** Numeric values directly aggregated from data (e.g., `Sales Amount`, `Bed Days`)
- Derived from fact events/transactions at a clear grain (per encounter, per day)
- Aggregated using simple functions (SUM, COUNT, AVG)
- Sensitive to filter context (time, domain, segment)

**Metrics:** Business expressions combining measures (e.g., `Readmission Rate`, `Gross Margin %`)
- Express performance or quality vs raw volume
- Often use complex logic (conditionals, windows, exclusions)
- Require clear definitions and assumptions

**Additivity:** Classify measures by aggregation behaviour:
- *Additive:* Sum across all dimensions (e.g., `Sales Amount`)
- *Semi-additive:* Sum across some dimensions only (e.g., `Inventory Level` - products yes, time no)
- *Non-additive:* Cannot sum meaningfully (e.g., ratios, rates)

Metrics inherit these behaviours. Record additivity as metadata to constrain aggregations in marts and BI tools.  

**Logical Definition:**
Measures/metrics should be defined against domain and canonical models and documented in the business glossary with:
- Name, description, owning domain, stakeholders
- Formal definition and calculation rule
- Input entities, attributes, filters
- Validity period and version history
- Related KPIs, dashboards, reports

**Physical Implementation:**

- **Fact tables:** Base measures as columns (e.g., `amount`, `quantity`); sometimes pre-aggregated metrics
- **dbt models:** Centralised, version-controlled definitions aligned to glossary
- **BI tools (Power BI):** Expressions (DAX) aligned with central definitions; push complex logic upstream when possible

**Placement Principles:**

*Favour upstream (mart / dbt layer) when:*
- Reused across dashboards/domains
- Enterprise KPI or regulatory use
- Complex business rules requiring testing
- Needs governance and discoverability

*Allow BI-layer-only when:*
- Experimental or single-report scope
- Simple presentation variant
- Clearly not the system of record

> **Example:** Base measure `Total Encounters` defined in dbt. Power BI DAX adds interactive filter-aware metric: `Readmission Rate (Current Filters) = DIVIDE([Readmission Count], [Total Encounters])`

**Governance:**
- Govern like reference data with review/approval process
- Retain effective dates and versions
- Notify consumers of changes
- Make discoverable via catalog/glossary (calculation, dependencies, usage)  


### Master Data and Reference Data

**Master Data:** Core business entities critical to operations and shared across systems (e.g., Patient, Product, Provider, Location)
- Managed as authoritative "single source of truth"
- Subject to governance and quality processes
- Forms backbone of conformed dimensions in data warehousing

**Reference Data:** Stable, standardised values for categorisation and validation (e.g., Status codes, Country codes, Product Categories)
- May be externally standardised (ISO) or internally maintained
- Can be simple (code/name pairs) or hierarchical (see [Hierarchies](#hierarchies))
- Managed enterprise-wide with optional source-specific variants

#### Logical Representation

Master Data: Patient, Product, Provider, Location, Organisation, Payer
Reference Data: Status Code, Country Code, Product Category, Location Type, Therapeutic Class, Unit of Measure

#### Physical Implementation

Physical form depends on analytical use, query patterns, history requirements, complexity, and performance needs.

**Physical Forms:**

**1. Simple Lookup Tables**
- Basic code-to-description translation
- Minimal attributes (code, name, description)
- No history tracking
- Example: `ref_status_code`, `ref_currency`

**2. Dimension Tables (Star Schema)**
- Rich attributes and hierarchies
- History tracking via SCD (Slowly Changing Dimensions)
- Used for slicing, filtering, grouping
- Example: `dim_product`, `dim_provider`, `dim_location`

**3. Embedded in Fact Tables (Denormalised)**
- Attributes embedded directly for performance
- Trades storage for speed (avoids joins)
- Example: Fact includes `status_code`, `status_name` columns

> For reference data modelling standards, see [Reference Data Standards and Conventions](modelling_standards_and_conventions.md#reference-data).


### Hierarchies

Hierarchies are structured parent-child relationships representing natural groupings and aggregation levels.

**Across Model Types:**
- **Conceptual/Logical:** Business classifications and organisational structures (product categories, regions, org units)
- **Physical:** Implemented via self-referencing foreign keys, hierarchy tables, or path encodings
- **Dimensional:** Enable drill-down/roll-up analysis (Day → Month → Quarter → Year)
- **Reference data:** Standardised classification schemes


#### Modelling Hierarchies: Subtypes vs. Relationships

**Use Subtypes for "Is A" Taxonomies:**
- Type distinctions with mutually exclusive subtypes
- Example: Individual Customer *is a* Customer; Inpatient Encounter *is an* Encounter
- Primary classification dimension for fundamentally different variants

**Use Relationships for Other Hierarchies:**
- Compositional (Part Of), categorical (Belongs To), organisational (Reports To)
- Support multiple concurrent hierarchies
- Example: Location entities (Room, Ward, Building, Campus) related via "Part Of"



#### Examples

**Relationship Hierarchy: Healthcare Facility Structure**

```
Hospital Campus
├── Building A
│   ├── Ward 1 → Room 101, Room 102
│   └── Ward 2 → Room 201
└── Building B
    └── Outpatient Clinic → Consultation Room 1
```

**Physical Representations:**

**1. Parent-Child (Adjacency List):** Each record references its parent for recursive traversal.

| location_id | location_name     | location_type | parent_location_id |
|-------------|-------------------|---------------|--------------------|
| 1           | Hospital Campus   | Facility      | NULL               |
| 2           | Building A        | Facility      | 1                  |
| 3           | Ward 1            | Ward          | 2                  |
| 4           | Room 101          | Room          | 3                  |
| 5           | Room 102          | Room          | 3                  |

**2. Flattened Dimension:** Lowest-grain entity with hierarchy level columns.

| room_key | campus_name     | building_name | ward_name | room_name | effective_from_datetime | effective_to_datetime | updated_datetime    |
|----------|-----------------|---------------|-----------|-----------|-------------------------|----------------------|---------------------|
| 1001     | Hospital Campus | Building A    | Ward 1    | Room 101  | 2024-01-01 00:00:00     | NULL                 | 2024-01-01 00:00:00 |
| 1002     | Hospital Campus | Building A    | Ward 1    | Room 102  | 2024-01-01 00:00:00     | NULL                 | 2024-01-01 00:00:00 |

Adjacency lists enable flexible navigation; flattened structures optimise star schema query performance.


**Reference Data Hierarchy: Product Classification**

```
All Products
├── Medical Equipment
│   ├── Diagnostic Equipment → Imaging Systems (MRI, CT Scanners)
│   └── Therapeutic Equipment
└── Pharmaceuticals
    ├── Prescription Drugs
    └── Over-the-Counter
```

**Physical Representations:**

**1. Parent-Child (Adjacency List):**

| product_classification_id | classification_name   | parent_classification_id |
|--------------------------|-----------------------|-------------------------|
| 1                        | All Products          | NULL                    |
| 2                        | Medical Equipment     | 1                       |
| 3                        | Diagnostic Equipment  | 2                       |
| 4                        | Imaging Systems       | 3                       |
| 5                        | MRI Scanners          | 4                       |

**2. Flattened Dimension:**

| product_key | classification_lvl1 | classification_lvl2   | classification_lvl3 | classification_lvl4 | effective_from_datetime | effective_to_datetime | updated_datetime    |
|-------------|---------------------|-----------------------|---------------------|---------------------|-------------------------|----------------------|---------------------|
| 100         | Medical Equipment   | Diagnostic Equipment  | Imaging Systems     | MRI Scanners        | 2024-01-01 00:00:00     | NULL                 | 2024-01-01 00:00:00 |
| 101         | Medical Equipment   | Diagnostic Equipment  | Imaging Systems     | CT Scanners         | 2024-01-01 00:00:00     | NULL                 | 2024-01-01 00:00:00 |
| 102         | Pharmaceuticals     | Prescription Drugs    |                     |                     | 2024-01-01 00:00:00     | NULL                 | 2024-01-01 00:00:00 |

> **Note:** Naming follows [modelling standards](modelling_standards_and_conventions.md): surrogate keys use `_key` suffix; SCD Type 2 uses `effective_from_datetime`, `effective_to_datetime`, `updated_datetime`; entities are singular; hierarchy attributes use logical prefixes with incremental numbering.
