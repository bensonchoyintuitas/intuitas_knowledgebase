# Intuitas Data Modelling Framework

> Updated 5/12/2025

## Overview

This resource provides a lightweight framework for describing and developing data models. It draws from a range of suggested practices, with references provided where appropriate.


## Quick Jump To

**Core Model Types**
- [Conceptual Models](#conceptual-information-models)
- [Logical Models](#logical-information-models)
- [Physical Models](#physical-data-models)
- [Domain Topology](#domain-topology)
- [Domain Models](#domain-models)
- [Canonical Models](#canonical-models)

**Specialised Model Types**
- [Business Process Models](#business-process-models)
- [Data Warehouse Models](#data-warehouse-models)
  - [Data Vault Model](#data-vault-model)
  - [Dimensional (Kimball) Model](#dimensional-kimball-model)
  - [Conformed Dimensions](#canonical-models-and-conformed-dimensions)
  - [Dimensional Bus Matrix](#dimensional-bus-matrix)
- [Semantic Layers](#semantic-layers)

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

See [Modelling Standards and Conventions](#modelling-standards-and-conventions) for further detail on notation and modelling standards.

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

See [Modelling Standards and Conventions](#modelling-standards-and-conventions) for further detail on notation and modelling standards.

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

See [Modelling Standards and Conventions](#modelling-standards-and-conventions) for further detail on notation and modelling standards.

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





## Modelling Standards and Conventions

*Work in progress*

Conceptual
Naming
Diagrammatic represenations

Logical
Naming
- Entity
- Attribute
- Key
Diagrammatic represenation
(crows foot or multiplicity)

Physical
- Relational
- Databricks naming
    - catalog, schema/database
    - Dimensional
    - table/view, column

## Modelling Tools
No standard as long as it conforms
Ideally backed by a metamodel for better reuseability, change tracking, and built in integrity and constraints