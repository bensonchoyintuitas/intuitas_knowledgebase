# Intuitas Data Modelling Framework
[Return to home](README.md)

> Updated 5/12/2025

This resource provides a lightweight framework for describing and developing data models. It draws from a range of suggested practices, with references provided where appropriate.

See [Modelling Standards and Conventions](modelling_standards_and_conventions.md) for further detail on notation and modelling standards.


## Table of Contents
---

  **Core Model Types**

  - [Conceptual Models](#conceptual-information-models)
  - [Logical Models](#logical-information-models)
  - [Physical Models](#physical-data-models)

  **Enterprise Context**

  - [Domain Topology](#domain-topology)
  - [Domain Models](#domain-models)
    - [Relationship to Business Processes and Functions](#relationship-to-business-processes-and-functions)
    - [Relationship to Business Objects](#relationship-to-business-objects)
  - [Canonical Models](#canonical-models)

  **Specialised Model Types**

  - [Business Process Models](#business-process-models)
  - [Data Warehouse Models](#data-warehouse-models)
    - [Data Vault Model](#data-vault-model)
    - [Dimensional (Kimball) Model](#dimensional-kimball-model)
    - [Canonical Models and Conformed Dimensions](#canonical-models-and-conformed-dimensions)
    - [Dimensional Bus Matrix](#dimensional-bus-matrix)
  - [Semantic Layers](#semantic-layers)
  - [Master Data, Reference Data and associated physical models](#master-data-reference-data-and-associated-physical-models)
  - [Hierarchies](#hierarchies)
    - [Modelling Hierarchies: Subtypes vs. Relationships](#modelling-hierarchies-subtypes-vs-relationships)


## Modelling Concepts

We prefer to emphasise the semantic difference between business information design and technical data implementation:

- **Information Models:** Conceptual and Logical
- **Data Models:** Physical

Some organisations use "data model" as an umbrella term for all three layers (conceptual data model, logical data model, physical data model), and that is also acceptable.

Business information concepts that are applicable across the Enterprise or belong to specific domains are modelled across two layers: **Conceptual** and **Logical**. These models:

- Are **semantic** models because they focus on the meaning (semantics) of information and business concepts
- Describe what information exists and how it relates
- Are technology-agnostic
- Are concerned with business rules, definitions, and relationships
- Audience: business stakeholders, analysts, architects

### Conceptual (Information) Models

Conceptual models represent concepts and business meaning only, without implementation concerns.

#### Identifying Concepts

What constitutes a concept?

- **Identifiable:** Can it be uniquely recognised?
- **Meaningful:** Is it relevant to business processes or needs to be tracked?
- **Named:** What do we call it? Align to agreed terms within the domain

Concept names may differ across the enterprise. Use the **business glossary** to map domain-specific terms as synonyms to canonical enterprise terms.

#### What to Include

Conceptual models should focus on:

- Core business concepts (entities)
- Key relationships between concepts
- High-level business rules

See [Modelling Standards and Conventions](modelling_standards_and_conventions.md) for further detail on notation and modelling standards.

#### What NOT to Include

Conceptual models should **not** include implementation details such as:

- Primary keys
- Surrogate keys
- Data types
- Cardinality precision beyond "one" vs "many"

#### On levelling, Subtypes and Supertypes

**Determining the Right Level:**
Choosing the appropriate level of abstraction is challenging:

- **Too abstract:** Not practical (e.g., "Thing" as a supertype)
- **Too granular:** Too much to manage, not reflective of actual data variations (should be attributes or variants of a class instead)

**General guidance** (refer to Simsion's data modelling principles):

- **Subtypes** represent meaningful specialisations of a concept (e.g., "Individual Customer" and "Corporate Customer" as subtypes of "Customer")
- **Classification dimensions** help determine what to model as subtypes vs attributes (e.g., "Customer Type" could be an attribute, while fundamental differences in what data is captured might warrant subtypes)
- Some modelling tools allow multiple classification hierarchies—avoid this complexity where possible; choose the primary classification dimension

**Modelling rules for subtypes:**

- **Mutually exclusive:** Yes—an instance must belong to only one subtype
- **Collectively exhaustive:** Flexible—while desirable in theory, models are often works in progress and not all subtypes may be identified initially


### Logical (Information) Models

Logical models are also technology-independent and business-oriented. They extend conceptual models by introducing structure and precision while remaining implementation-agnostic.

#### What to Include

Logical models add detail to conceptual models:

- **Keys and identifiers:** Natural and alternate keys that uniquely identify entities
- **Cardinality:** Precise relationship cardinality (1:1, 1:M, M:N)
- **Optionality:** Whether relationships and attributes are mandatory or optional
- **Attribute definitions:** All attributes with clear definitions and business rules
- **Normalisation:** Apply normalisation principles where relevant to ensure data integrity

See [Modelling Standards and Conventions](modelling_standards_and_conventions.md) for further detail on notation and modelling standards.

### Physical (Data) Models 

These conceptual and logical models are then implemented as **Physical** models.

These models:

- Focus on data storage and implementation
- Describe how data is actually stored and accessed
- Are technology-specific (e.g., tables, columns, indexes, partitions)
- Are concerned with performance, storage optimisation, and technical constraints
- Audience: database developers, engineers

#### Common Physical Model Formats

Physical models take different forms depending on the storage technology and use case:

**Relational (Normalised):**

- Traditional 3NF (Third Normal Form) relational models
- Optimised for transactional systems (OLTP)
- Minimises redundancy and ensures data integrity
- Common in operational databases (e.g., PostgreSQL, SQL Server, Oracle)

**Dimensional:**

- Star and snowflake schemas
- Optimised for analytical queries and reporting (OLAP)
- Denormalised for query performance
- See [Dimensional (Kimball) Models](#dimensional-kimball-model) for details

**Semi-Structured:**

- JSON, XML, Parquet, Avro
- Flexible schema supporting nested and hierarchical data
- Common in data lakes, NoSQL databases, and modern cloud platforms (e.g., MongoDB, Delta Lake, Databricks)
- Balances structure with flexibility

**Graph:**

- Node and edge structures
- Optimised for relationship-heavy queries
- Common in graph databases (e.g., Neo4j, Amazon Neptune)

**Key-Value and Document:**

- Simple key-value pairs or document stores
- Optimised for high-speed lookups and scalability
- Common in NoSQL databases (e.g., DynamoDB, Cosmos DB, Redis)

> **Note:** In practice, we often only have access to a physical data model without documentation of the underlying conceptual or logical models. Regardless, a logical model is still implied by the physical implementation and can be reverse-engineered to understand the business meaning.

See [Modelling Standards and Conventions](modelling_standards_and_conventions.md) for further detail on notation and modelling standards.

## Enterprise Context

Enterprise Information/Data models seek to model all of the information across the enterprise as a whole. In practice, this is challenging as the semantics across functional areas of the business may differ, and standardisation can be difficult, overly abstract to be practical, and result in loss of fidelity.

Instead, we prefer to focus on common touch points across systems and domains, combining Domain models and Canonical Models where we bridge systems and/or domains.

These are best described through:

### Domain Topology

[Refer to Domain-Centric Design](level_1.md#domain-centric-design) 

Domains relate to functional and organisational boundaries, and represent closely related areas of responsibility and focus.

- Each domain encapsulates functional responsibilities, services, processes, information, expertise, costing and governance.
- Domains serve their own objectives while also offering products and services of value to other domains and the broader enterprise.
- Domain can exist at different levels of granularity and their boundaries may not be obvious. They are not necessarily a reflection of the organisational chart.

A **Domain Topology** describes the various domains (or subject areas) across the Enterprise and how they relate to each other. These are typically distinguished in terms of functional areas; however, other dimensions may be considered (e.g., region).

<div align="center">

<em>Illustative example domains for a Healthcare organisation</em>
<br>
<a href="../img/domain-example.png" target="_blank">
    <img src="../img/domain-example.png"  alt="Example domains for a Healthcare organisation" width="75%">
</a>
<br>
</div>

### Domain Models

Under Domain-Centric Design, domains are considered authoritative for their own scope of information definitions. Domains:

- Own the meaning of their concepts
- Define lifecycle and validity rules
- Act as the system of semantic truth for their scope

A **Domain Model** describes the business concepts relevant to a particular domain. These models:

- Focus on business meaning rather than technical implementation  
- Define entities, relationships, and rules within the domain boundary  
- Use terminology agreed and owned by domain stakeholders  
- Establish the semantic foundation for domain data products, APIs, and analytics  

> **Example:** Specific domain model (e.g., Clinical domain) would be inserted here

---

### Relationship to Business Processes and Functions

Understanding business processes is essential for developing accurate domain models because:

- **Domain Models** define the **WHAT** — the business objects, their attributes, and relationships  
- [**Business Process Models**](#business-process-models) define the **HOW** — how those objects are created, transformed, and consumed

Specifically:

- Business processes reveal which objects matter  
- They expose lifecycle states and transitions  
- They uncover implicit rules and constraints  
- They clarify real business language (which often differs from system terminology)

Conversely, a clear domain model enables process design by providing a shared vocabulary and stable business meaning.

---

### Relationship to Business Objects

A **Business Object** is a conceptual entity that represents a real or abstract thing the business manages, observes, or transacts upon.

Business Objects:

- Have identity  
- Have a lifecycle  
- Participate directly in business processes  
- Represent operational business meaning  

Business Objects are **a subset of business concepts**, not the full vocabulary of a domain.

Not all business concepts are business objects.  
Examples of concepts that are *not* business objects include:

- Qualities (e.g., Risk, Capacity, Trust)  
- Principles (e.g., Compliance, Policy)  
- States (e.g., Availability, Readiness)  

Domain Models therefore contain:

- **Business Objects** (the operational core—the things the business acts upon), and  
- **Supporting concepts** that describe qualities, rules, conditions, classifications, and interpretations.

#### Terminology Note

**Subject Area** is traditional data warehousing terminology (Kimball, Inmon), while **Domain** is more modern terminology (Data Mesh, Domain-Driven Design). They are conceptually similar, but "domain" often implies business ownership and autonomy.

**In this framework, we prefer "Domain"** as it ties more readily to governance and ownership boundaries, supporting clear accountability for data products and business outcomes.

### Canonical Models

Where conceptual and logical concepts need to be standardised across domains and systems (e.g., for common service APIs, interoperability, or conformed dimensions), these models can be considered **canonical**—i.e., they represent the authoritative, agreed-upon definition across the enterprise. 

Canonical models:

- Can be conceptual and logical at a minimum
- Can also be described physically if applicable to a particular cross-system or cross-business-domain context
- Represent standardised information contracts between domains and systems
- Support interoperability and consistent interpretation of shared concepts

Canonical models should only exist where:

- Multiple domains integrate
- Semantic meaning must be consistent
- Interoperability is required

#### Canonical Models and Cross-Domain Processes

Canonical models are often discovered and defined **in the context of cross-domain processes**—understanding how work flows across boundaries reveals what needs to be standardised. They play a critical role in enabling and documenting processes that bridge across domains:

- **Define integration points:** They specify the standardised entities and attributes that domains must agree upon when collaborating (e.g., a canonical "Customer" or "Order" definition)
- **Enable process flows:** Cross-domain processes (e.g., spanning Sales, Fulfilment, and Finance) rely on canonical models as the shared language for exchanging information
- **Document contracts:** They make explicit what data is exchanged at each process handoff between domains


## Specialised Model Types

Different types of models serve distinct purposes, audiences, levels of detail, and governance standards. Beyond the core modelling layers, several specialised model types support specific architectural needs:

### Business Process Models

Business process models do not represent data or information directly, but they offer vital context for data modelling efforts. They illustrate the sequence of activities, decision points, participants (roles), and flow of work within and across business domains.

Business process models often define **business objects**—the key conceptual entities that the business cares about, such as Customer, Order, Product, Invoice, or Contract. These business objects embody the real-world things or concepts around which business activity is organised, and are typically defined directly by business stakeholders. These business objects often form the basis of **conceptual models** within [**Domain Models**](#domain-models).

See also: [Fact Tables and Business Processes in Dimensional Modeling](#dimensional-kimball-model)


### Data Warehouse Models

Data warehouses transform data through a range of stages, each with inherent physical model structures, in a way that is ultimately optimised for performance and analysis—while preserving the underlying conceptual and logical semantics. 

These stages typically:

- **Conform to standards:** Transform disparate data sources and their physical models to align with [canonical naming, structures, and formats](#canonical-models)
- **Apply reference data:** Map source values to standardised reference data and business terminology
- **Ensure quality:** Perform data quality checks and cleansing processes to ensure reliability
- **Preserve history:** Maintain historical records as required for time-based analysis
- **Enrich with metadata:** Add technical and business metadata to support discovery and governance
- **Optimise for consumption:** Structure data according to business requirements and access patterns (e.g., [dimensional models](#dimensional-kimball-model), [data vault](#data-vault-model))
- **Aggregate:** Summarise data at appropriate levels for analytical consumption

Each stage may have distinct physical model structures optimised for its specific purpose, while maintaining alignment to the underlying logical and conceptual models.

#### Data Vault Model

The core of the Data Vault model are core business concepts and their associations as described in Enterprise/Domain Conceptual Models. We do not elaborate on them here. Refer to Data Vault 2.0 standards for further guidance.

#### Dimensional (Kimball) Model

Kimball Dimensional Modelling refers to a methodology for designing data warehouses that centres on the creation of **Information Marts**—subject-oriented collections of data optimised for analytical use. In this approach, data is organised into **Star Schemas**, which are characterised by a central fact table surrounded by related dimension tables. 

- **Fact tables** capture the quantitative metrics or events of **business processes** (such as sales transactions, shipments, or payments).
- **Dimension tables** provide descriptive context (such as customer, product, date, or sales region) to enable rich, flexible analysis.

Star Schemas are intentionally denormalised to make querying and reporting more efficient for business users. By structuring data in this way, the Kimball methodology enables rapid, consistent, and user-friendly analysis across key business metrics and dimensions. This practice leads to high query performance, easier navigation for business users, and the ability to perform drill-across and cross-domain analytics within the data warehouse.

#### Canonical Models and Conformed Dimensions

Canonical models form the **conceptual and logical basis** for conformed dimensions in data warehousing:

- **Conformed dimensions** are the physical implementation of canonical models in dimensional/star schema designs
- They ensure consistent definitions across fact tables and data marts (e.g., a canonical "Customer" model becomes a conformed Customer dimension shared across Sales, Support, and Marketing facts)
- They enable cross-process analysis and drill-across queries by providing standardised attributes and hierarchies
- Following Kimball methodology, conformed dimensions are how enterprises achieve "single version of the truth" across analytical systems

#### Dimensional Bus Matrix

A **Dimensional Bus Matrix** (or **Bus Matrix**) is a tool, popularized by Ralph Kimball, used in dimensional modelling to systematically map the intersection between business processes (facts) and the descriptive dimensions they use. It is especially valuable for scalable data warehouse or analytics platform design, as it enables modular development and clear alignment to business needs.

**The bus matrix:**

- Is a two-dimensional grid or table
- Has business processes (typically facts, e.g., "Orders," "Shipments," "Payments") as rows
- Has dimensions (e.g., "Customer," "Product," "Time," "Geography") as columns
- Each cell indicates whether a dimension is associated with (applies to) a given process

##### Key benefits:

- **Identifies conformed dimensions:** Shows which dimensions should be shared and standardised across different areas of the business for consistency and integrative analysis
- **Supports modular architecture:** Each intersection can be implemented and released independently, following a pattern
- **Enables cross-process analytics:** By reusing conformed dimensions, the enterprise facilitates seamless reporting across business processes

##### Example:

| Business Process \ Dimension | Customer | Product | Time | Sales Rep | Channel |
|-----------------------------|----------|---------|------|-----------|---------|
| Orders                      |    ✓     |   ✓     |  ✓   |     ✓     |    ✓    |
| Shipments                   |    ✓     |   ✓     |  ✓   |           |    ✓    |
| Payments                    |    ✓     |         |  ✓   |           |         |

- ✓ = dimension is used by the process

> The bus matrix guides both the implementation of star schemas and the standardisation of dimension definitions across the data warehouse or analytical platform.


### Semantic Layers

In modern data architecture, a *semantic layer* (or *semantic model*) is a business-oriented abstraction layer that makes data easier to understand and use by presenting it in business terms rather than physical structures.

Semantic layers are interpretive layers that depend on domain and canonical models for the definition of base entities. However, they may establish authoritative definitions for derived metrics, business calculations, and analytical measures within their consumption context.

They:

- Translate physical data structures into business concepts  
- Encapsulate business logic, measures, and calculations  
- Provide consistent definitions for metrics within a given analytical context  
- Simplify access for analytics, reporting, and self-service use  

Semantic layers emphasise conceptual and logical meaning over physical storage and implementation. While they have physical representations (e.g., models, views, metadata objects), these are abstractions over authoritative sources rather than systems of record.

Examples:

- Power BI semantic models (business logic layer)  
- dbt Semantic Layer  

#### Measures and Metrics

In this framework, **measures** and **metrics** are first-class modelling concepts in the semantic layer. They sit on top of domain and canonical models and provide the vocabulary for how the business quantifies performance and outcomes.

- **Measures**: Numeric values directly aggregated from data (e.g., `Sales Amount`, `Number of Encounters`, `Bed Days`). They are typically:
  - Derived from fact-like events or transactions  
  - Defined with a clear **grain** (e.g., per encounter, per invoice line, per day)  
  - Aggregated using simple functions (SUM, COUNT, MIN/MAX, AVG)  
  - Sensitive to filter context (e.g., time period, domain, customer segment)  

- **Metrics**: Business expressions or ratios that **combine one or more measures** and sometimes reference data (e.g., `Readmission Rate`, `Average Length of Stay`, `Gross Margin %`). They:
  - Express business performance or quality rather than raw volume  
  - Often have more complex logic (conditional logic, windows, exclusions)  
  - Require clear **definitions, assumptions, and exclusions** to avoid misinterpretation  

**Additivity and aggregation behaviour** (additive, semi-additive, non-additive) applies **primarily to base measures**, but the resulting behaviour of derived metrics must also be understood:

- Classify **base measures** by how they aggregate across key dimensions (especially time):  
  - *Additive*: safe to sum across all relevant dimensions (e.g., `Sales Amount`, `Encounter Count`)  
  - *Semi-additive*: can be summed across some dimensions but **not** others (e.g., end-of-day `Inventory Level` is additive across products but not over time)  
  - *Non-additive*: cannot be summed meaningfully (e.g., ratios like `Readmission Rate`, `Gross Margin %`)  
- **Metrics built from measures** inherit these behaviours—ratios and rates are usually non-additive, and roll-ups must use appropriate aggregations (e.g., weighted averages rather than simple sums).  
- In models and semantic layers, record additivity as explicit metadata for each governed measure/metric (e.g., in the business glossary or semantic model), and use it to constrain or guide which aggregations are allowed in marts and BI tools.  

From a modelling perspective, semantic measures and metrics should be:

- **Logically defined** against domain and canonical models (e.g., "Bed Days = count of Encounter Days where Encounter Status = Admitted")  
- **Described in the business glossary** with:
  - Name, description, owning domain, and primary stakeholders  
  - Formal definition and calculation rule  
  - Input entities, attributes, and filters used  
  - Validity period and version history (when the definition changed)  
  - Related KPIs, dashboards, and reports  

Physically, measures and metrics can appear in multiple layers while remaining **logically consistent**:



- **Information marts / fact tables**:
  - Base measures materialised as columns (e.g., `amount`, `quantity`, `length_of_stay_days`)  
  - Sometimes pre-aggregated metrics for performance, with careful documentation  

- **dbt models and semantic layer**:
  - Base measures defined in dbt models and exposed via a dbt Semantic Layer or similar metadata  
  - Centralised definitions reused across downstream tools, aligned to the business glossary  
  - Version-controlled SQL logic, with tests to protect key metric behaviour  

- **Power BI or BI tool semantic models**:
  - Measures implemented as expressions (e.g., DAX), which:
    - Apply semantic rules over imported or DirectQuery tables  
    - Should align with central metric definitions rather than re-implementing ad hoc logic  
  - Where possible, complex logic should be **pushed upstream** into governed semantic definitions (e.g., dbt metrics) to reduce local variation.

**Placement principles**:

- **Favour upstream definitions** (dbt/semantic layer) when a measure/metric:
  - Is reused across many dashboards, domains, or products  
  - Represents an enterprise KPI or is used in regulatory/contractual reporting  
  - Has complex or subtle business rules that must be tested and versioned  
  - Needs to be discoverable and governable as a shared asset  
- **Allow BI-layer-only definitions** when:
  - The metric is experimental, exploratory, or scoped to a single report/team  
  - It is a simple presentation variant of an existing governed metric (e.g., reformatting, basic re-aggregation)  
  - There is a clear understanding that the BI definition is **not** the system of record  
- For each important metric, choose a **single system of record** (typically the upstream semantic/dbt layer) and avoid duplicating or re-implementing its logic in multiple tools.

> **Example – DAX-only interactive metric**  
> A governed upstream measure such as `Total Encounters` is defined and tested in dbt.  
> In Power BI, a DAX measure can then express an interactive, filter-aware metric that only really makes sense in the BI layer:
>
> ```DAX
> Readmission Rate (Current Filters) =
> DIVIDE ( [Readmission Count], [Total Encounters] )
> ```
>
> This measure always returns the readmission rate for **whatever filters and slicers are currently applied** (e.g., ward, clinician, date), while still relying on governed upstream definitions for the base measures.


**Versioning and governance**:

- Important measures and metrics should be **governed like reference data**:
  - Changes to definitions follow a review and approval process  
  - Effective dates and prior versions are retained  
  - Consumers are notified when definitions change, especially where contractual or regulatory reporting is affected  
- Metric definitions should be **discoverable** via catalog or glossary tooling so that analysts, engineers, and business stakeholders can easily see:
  - How a number is calculated  
  - Which tables/columns it depends on  
  - Which products (dashboards, reports, APIs) use it  


### Master Data, Reference Data and associated physical models

#### Master Data

Master Data refers to the core business entities that are critical to operations and are shared across multiple systems and processes (such as Customer, Product, Location, or Provider). From a modelling perspective, master data is modeled like any other key entity, but is distinct in that it is carefully governed, centrally managed, and intended to maintain consistency and quality across the enterprise.

In data warehousing, well-governed master data typically forms the backbone of conformed dimensions, enabling consistent analytics and reporting across subject areas.

- Central to business operations and shared across systems (e.g., Patient, Product, Provider, Location, Organization, Payer)
- Managed as authoritative, "single source of truth" entities
- Rich in descriptive attributes and relationships
- Subject to governance and data quality processes

#### Reference Data

Reference data is a form of master data that provides stable, standardised values—such as country codes, product categories, or status codes—used for categorisation, classification, or validation. Its purpose is to ensure consistency and a shared understanding across systems, using externally or internally defined standards. Reference data may be externally standardised (e.g., ISO), hierarchical (see [Hierarchies](#hierarchies)), and sometimes requires mapping between sets if multiple exist for a domain.

These data sets are aligned to business entities, staged in `stg` as for silver marts, and are generally managed enterprise-wide for broad applicability, though source-specific variants can optionally be captured if needed.

- Used for categorisation, validation, and ensuring consistency (e.g., Status codes, Country codes, Product Categories)
- May be externally standardised (e.g., ISO codes) or internally maintained
- Can be simple (code/name pairs) or complex (hierarchical, with attributes)

#### Logical Representation

At the logical level, Master and Reference data is organised as:

```
LOGICAL VIEW

Master Data Entities          Reference Data Entities
┌──────────────────┐         ┌─────────────────────────┐
│ • Patient        │         │ • Status Code           │
│ • Product        │         │ • Country Code          │
│ • Provider       │         │ • Product Category      │
│ • Location       │         │ • Location Type         │
│ • Organization   │         │ • Therapeutic Class     │
│ • Payer          │         │ • Unit of Measure       │
└──────────────────┘         └─────────────────────────┘
```


#### Physical Implementation

The same logical entities can take different physical forms depending on analytical requirements, query patterns, and performance needs.

The physical form depends on:
- **Analytical use:** Will users filter, group, or aggregate by these attributes? → Dimension
- **Query patterns:** Is this accessed in most queries? → Consider embedding in facts
- **History requirements:** Do changes over time impact reporting? → Dimension with SCD Type 2
- **Complexity:** Does it have rich attributes or hierarchies? → Dimension
- **Simplicity:** Is it just code/name lookup? → Simple lookup table
- **Performance:** Are join costs impacting query performance? → Consider denormalisation


##### Physical Forms
**1. Simple Lookup Tables**:
- Used for basic code-to-description translation without analytical use
- Minimal attributes (typically just code, name, description)
- No history tracking required
- Typical for: Simple reference data used only for validation or display
- Example: `ref_status_code`, `ref_currency`, `ref_yes_no_indicator`

**2. Dimension Tables** (Star Schema):
- Used when attributes are needed for slicing, dicing, filtering, and grouping
- Supports rich descriptive attributes and hierarchies
- Enables history tracking through SCD (Slowly Changing Dimensions)
- Typical for: Master data entities, complex reference data used analytically
- Example: `dim_product`, `dim_provider`, `dim_location`, `dim_product_category`
- Well-managed master data simplifies conformance, though minor alignment steps may still be needed.

**3. Embedded in Fact Tables** (Denormalised):
- Reference data attributes embedded directly into facts for performance
- Used when query patterns frequently filter/group by these attributes
- Trades storage for query performance (avoids joins)
- Typical for: Frequently used classifications, type codes, status values
- Example: Fact table includes `status_code`, `status_name` columns rather than joining to a dimension

```
PHYSICAL IMPLEMENTATION OPTIONS

┌─────────────────────────────────────────────────────────────┐
│                     SAME LOGICAL ENTITY                     │
│                  (e.g., Product Category)                   │
└─────────────────────────────────────────────────────────────┘
                            │
        ┌───────────────────┼───────────────────┐
        │                   │                   │
        ▼                   ▼                   ▼
┌───────────────┐  ┌─────────────────┐  ┌──────────────────┐
│ Dimension     │  │ Lookup Table    │  │ Embedded in Fact │
│ Table         │  │                 │  │                  │
│               │  │                 │  │                  │
│ Full SCD      │  │ Simple code/    │  │ Denormalised     │
│ Rich attrs    │  │ name pairs      │  │ for performance  │
│ Hierarchies   │  │ No history      │  │                  │
└───────────────┘  └─────────────────┘  └──────────────────┘
```

For reference data modelling standards and guidelines [Reference Data Standards and Conventions](modelling_standards_and_conventions.md#reference-data).


### Hierarchies

**Hierarchies** are structured relationships between concepts or entities where a parent-child or ancestor-descendant arrangement exists. They represent natural groupings and levels of aggregation within data, reflecting how businesses organise and categorise information.

#### Relationship to Data Models

Hierarchies appear across all model types:

- **Conceptual/Logical models:** Hierarchies represent business classifications and organisational structures (e.g., product categories, geographic regions, organisational units)
- **Physical models:** Hierarchies may be implemented through self-referencing foreign keys, separate hierarchy tables, or path-based encodings depending on performance needs
- **Dimensional models:** Hierarchies within dimensions enable drill-down and roll-up analysis (e.g., Day → Month → Quarter → Year in time dimensions)
- **Reference data:** Many reference data sets are inherently hierarchical, providing standardised classification schemes


#### Modelling Hierarchies: Subtypes vs. Relationships

Not all hierarchies are modelled the same way. The choice depends on the nature of the relationship:

**Use Subtypes/Supertypes for "Is A" Taxonomies:**

When entities represent specialisations of the same fundamental concept, use subtype/supertype relationships (as described in [Subtypes and Supertypes](#on-levelling-subtypes-and-supertypes)). These hierarchies:

- Express **type distinctions** with strict rules (mutually exclusive subtypes)
- Represent "Is A" relationships (e.g., "Individual Customer *is a* Customer", "Inpatient Encounter *is an* Encounter")
- Are used as the **primary classification dimension** to distinguish fundamentally different variants of a concept
- Follow subtyping rules: mutually exclusive, optionally collectively exhaustive

**Use Relationships for Other Hierarchies:**

For compositional, organisational, or classification hierarchies that don't represent type specialisation, use standard relationships between entities. These hierarchies:

- Express compositional (Part Of), categorical (Belongs To), and organisational (Reports To) relationships
- Support multiple concurrent hierarchies

**Examples:**

- **Subtype approach:** Account can be specialised into Savings Account and Checking Account subtypes based on their fundamental account type
- **Relationship approach:** Location entities (Room, Ward, Building, Campus) related through "Part Of" relationships to represent physical containment



#### Examples

**Relationship Hierarchy: Healthcare Facility Structure**

A healthcare location hierarchy represents the physical hierarchy of location (types):

```
[facility] Hospital Campus
├── [facility] Building A
│   ├── [ward] Ward 1
│   │   ├── [room] Room 101
│   │   └── [room] Room 102
│   └── [ward] Ward 2
│       └── [room] Room 201
└── [facility] Building B
    └── [ward] Outpatient Clinic
        └── [room] Consultation Room 1
```
This simplified hierarchy can be represented physically in two common ways:

- **Parent-Child (Adjacency List) Table:** Each location record references its parent, enabling recursive traversal.

    | location_id | location_name        | location_type | parent_location_id |
    |-------------|---------------------|---------------|--------------------|
    | 1           | Hospital Campus     | Facility      | NULL               |
    | 2           | Building A          | Facility      | 1                  |
    | 3           | Ward 1              | Ward          | 2                  |
    | 4           | Room 101            | Room          | 3                  |
    | 5           | Room 102            | Room          | 3                  |
    | 6           | Ward 2              | Ward          | 2                  |
    | 7           | Room 201            | Room          | 6                  |
    | 8           | Building B          | Facility      | 1                  |
    | 9           | Outpatient Clinic   | Ward          | 8                  |
    | 10          | Consultation Room 1 | Room          | 9                  |

- **Flattened Attributes in a Dimension Table:** Each row holds the lowest-grain entity with columns for each hierarchy level.


    | room_key | campus_name      | building_name | ward_name         | room_name            | start_at            | end_at              | updated_at          |
    |----------|------------------|---------------|-------------------|----------------------|---------------------|---------------------|---------------------|
    | 1001     | Hospital Campus  | Building A    | Ward 1            | Room 101             | 2024-01-01 00:00:00 | NULL                | 2024-01-01 00:00:00 |
    | 1002     | Hospital Campus  | Building A    | Ward 1            | Room 102             | 2024-01-01 00:00:00 | NULL                | 2024-01-01 00:00:00 |
    | 1003     | Hospital Campus  | Building A    | Ward 2            | Room 201             | 2024-01-01 00:00:00 | NULL                | 2024-01-01 00:00:00 |
    | 1004     | Hospital Campus  | Building B    | Outpatient Clinic | Consultation Room 1  | 2024-01-01 00:00:00 | NULL                | 2024-01-01 00:00:00 |



This dimension attribute pattern is common for star schemas, enabling easy filtering and reporting by hierarchy level. The adjacency list enables flexible and dynamic hierarchy navigation, while the flattened structure optimizes for query performance in analytic scenarios.


**Reference Data Hierarchy: Product Classification**

A logical product classification hierarchy organises products from specific items to broader categories. 

```
All Products
├── Medical Equipment
│   ├── Diagnostic Equipment
│   │   ├── Imaging Systems
│   │   │   ├── MRI Scanners
│   │   │   └── CT Scanners
│   │   └── Laboratory Equipment
│   └── Therapeutic Equipment
└── Pharmaceuticals
    ├── Prescription Drugs
    └── Over-the-Counter
```
This hierarchy can be represented physically in two common ways:

- As a parent-child relationship table, where each record references its parent (for example, a `product_classification` table with `product_classification_id` and `parent_classification_id` columns)
- As attributes within a dimension table, such as a `dim_product` table with columns for each hierarchical level (e.g., `product_category`, `product_subcategory`, `product_type`)

**Example: Product Classification Reference Table (Parent-Child and Flattened Forms)**

*Parent-Child (Adjacency List) Table:*

| product_classification_id | classification_name       | parent_classification_id |
|--------------------------|--------------------------|-------------------------|
| 1                        | All Products             | NULL                    |
| 2                        | Medical Equipment        | 1                       |
| 3                        | Diagnostic Equipment     | 2                       |
| 4                        | Imaging Systems          | 3                       |
| 5                        | MRI Scanners             | 4                       |
| 6                        | CT Scanners              | 4                       |
| 7                        | Laboratory Equipment     | 3                       |
| 8                        | Therapeutic Equipment    | 2                       |
| 9                        | Pharmaceuticals          | 1                       |
| 10                       | Prescription Drugs       | 9                       |
| 11                       | Over-the-Counter         | 9                       |

*Flattened Dimension Table Representation (lowest-level rows, hierarchical attributes as columns):*

| product_key | classification_lvl1      | classification_lvl2      | classification_lvl3   | classification_lvl4   | start_at            | end_at              | updated_at          |
|-------------|-------------------------|--------------------------|-----------------------|-----------------------|---------------------|---------------------|---------------------|
| 100         | Medical Equipment        | Diagnostic Equipment     | Imaging Systems       | MRI Scanners          | 2022-01-01 00:00:00 | 9999-12-31 23:59:59 | 2022-02-14 10:30:00 |
| 101         | Medical Equipment        | Diagnostic Equipment     | Imaging Systems       | CT Scanners           | 2022-01-01 00:00:00 | 9999-12-31 23:59:59 | 2022-02-14 10:30:00 |
| 102         | Pharmaceuticals          | Prescription Drugs       |                       |                       | 2022-01-01 00:00:00 | 9999-12-31 23:59:59 | 2022-02-14 10:30:00 |
| 103         | Pharmaceuticals          | Over-the-Counter        |                       |                       | 2022-01-01 00:00:00 | 9999-12-31 23:59:59 | 2022-02-14 10:30:00 |

*Note: This example conforms to standard naming conventions as described in [modelling standards](modelling_standards_and_conventions.md). Surrogate keys use `_key` suffix (`product_key`). Date columns for SCD Type 2 tracking use `start_at`, `end_at`, and `updated_at`. Naming is singular for entities, and hierarchy attributes use logical prefixes and incremental numbering (e.g., `classification_lvl1`). Codes and descriptions, if present, should use `_code` and `_description` suffixes, respectively.*

| product_key | classification_lvl1 | classification_lvl2   | classification_lvl3     | classification_lvl4   | start_at            | end_at              | updated_at          |
|-------------|---------------------|-----------------------|-------------------------|-----------------------|---------------------|---------------------|---------------------|
| 100         | Medical Equipment   | Diagnostic Equipment  | Imaging Systems         | MRI Scanners          | 2024-01-01 00:00:00 | NULL                | 2024-01-01 00:00:00 |
| 101         | Medical Equipment   | Diagnostic Equipment  | Imaging Systems         | CT Scanners           | 2024-01-01 00:00:00 | NULL                | 2024-01-01 00:00:00 |
| 102         | Pharmaceuticals     | Prescription Drugs    |                         |                       | 2024-01-01 00:00:00 | NULL                | 2024-01-01 00:00:00 |
| 103         | Pharmaceuticals     | Over-the-Counter      |                         |                       | 2024-01-01 00:00:00 | NULL                | 2024-01-01 00:00:00 |

This approach allows joining facts by the appropriate key and filtering or grouping at any hierarchy level via the flattened columns, or traversing relationships via the parent-child structure.
