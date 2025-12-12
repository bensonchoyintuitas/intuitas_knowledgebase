# Intuitas Data Modelling Standards and Conventions

[Return to home](README.md)

> Updated 5/12/2025

> **Related Documents:** This document focuses on naming conventions and standards. For platform-specific implementation patterns and architectural details, see [Standards and Conventions](standards_and_conventions.md). For conceptual framework and model types, see [Modelling Framework](modelling_framework.md).

---

## Table of Contents


  - [General Naming Conventions](modelling_standards_and_conventions.md#general-naming-conventions)
    - [Entity Naming (General Rules)](modelling_standards_and_conventions.md#entity-naming-general-rules)
    - [Business Names vs Physical Names](modelling_standards_and_conventions.md#business-names-vs-physical-names)
    - [Events and Transactions](modelling_standards_and_conventions.md#events-and-transactions)
    - [Relationship Naming Conventions](modelling_standards_and_conventions.md#relationship-naming-conventions)
    - [Relationship Directionality](modelling_standards_and_conventions.md#relationship-directionality)
    - [Relationship Cardinality](modelling_standards_and_conventions.md#relationship-cardinality)
  - [Conceptual Models](modelling_standards_and_conventions.md#conceptual-models)
    - [Entities](modelling_standards_and_conventions.md#entities-conceptual)
    - [Diagrammatic Representation](modelling_standards_and_conventions.md#diagrammatic-representation-conceptual)
  - [Logical Models](modelling_standards_and_conventions.md#logical-models)
    - [Entities](modelling_standards_and_conventions.md#entities-logical)
    - [Attributes](modelling_standards_and_conventions.md#attributes)
    - [Measures and Metrics](modelling_standards_and_conventions.md#measures-and-metrics)
    - [Keys (Logical Models)](modelling_standards_and_conventions.md#keys-logical-models)
    - [Diagrammatic Representation](modelling_standards_and_conventions.md#diagrammatic-representation-logical)
  - [Physical Models](modelling_standards_and_conventions.md#physical-models)
    - [Relational Structures](modelling_standards_and_conventions.md#relational-structures)
      - [Naming](modelling_standards_and_conventions.md#naming)
      - [Diagrammatic Representation](modelling_standards_and_conventions.md#diagrammatic-representation)
      - [Databricks Conventions](modelling_standards_and_conventions.md#databricks-conventions)
    - [Dimensional Models](modelling_standards_and_conventions.md#dimensional-models)
      - [Data Warehouse Keys](modelling_standards_and_conventions.md#data-warehouse-keys)
        - [Key resolution](modelling_standards_and_conventions.md#key-resolution)
        - [Business Keys](modelling_standards_and_conventions.md#business-keys)
        - [Surrogate Keys](modelling_standards_and_conventions.md#surrogate-keys)
        - [Type 2 Fact Resolution](modelling_standards_and_conventions.md#type-2-fact-resolution)
        - [Databricks Surrogate Keys](modelling_standards_and_conventions.md#databricks-surrogate-keys)
        - [dbt Surrogate Keys](modelling_standards_and_conventions.md#dbt-surrogate-keys)
    - [Reference Data](modelling_standards_and_conventions.md#reference-data)
      - [Logical Modelling](modelling_standards_and_conventions.md#logical-modelling)
      - [Physical Implementation](modelling_standards_and_conventions.md#physical-implementation)
  - [Standard Suffix and Prefix Inventory](modelling_standards_and_conventions.md#standard-suffix-and-prefix-inventory)
    - [Identity & Keys](modelling_standards_and_conventions.md#identity-keys)
    - [Temporal Concepts](modelling_standards_and_conventions.md#temporal-concepts)
    - [State & Classification](modelling_standards_and_conventions.md#state-classification)
    - [Quantities & Measures](modelling_standards_and_conventions.md#quantities-measures)
    - [Boolean Indicators (Prefix Convention)](modelling_standards_and_conventions.md#boolean-indicators-prefix-convention)
    - [Audit & Control](modelling_standards_and_conventions.md#audit-control)
    - [Summary of Naming Rules](modelling_standards_and_conventions.md#summary-of-naming-rules)

---

## General Naming Conventions

### Entity Naming (General Rules)

- Use clear business language
- Use terminology familiar to business stakeholders and consistent with the business glossary
- In domain models, use terms appropriate to the business context and document synonyms in the business glossary
- Canonical entities must use terms that are formally agreed and recognised across domains
- Use singular nouns for entities (e.g., `Customer`, not `Customers`)
- Plurality is allowed only in dimensional fact tables (e.g., `fact_payments`). Dimension tables use singular (e.g., `dim_customer`)
- Avoid system-specific or technical terminology
- Avoid abbreviations unless universally understood
- Be specific: add context where needed (e.g., `Admin User` vs `User`)

### Business Names vs Physical Names

- Business names use natural language with spaces (e.g., `Order Placed`)
- Business names are authoritative for semantic meaning
- Machine-safe identifiers (e.g., GUIDs) are internal metadata only
- Physical names follow platform-specific conventions (e.g., `order_placed`)
- The business glossary is the semantic source of truth

**Example:**

- Business Name: `Order Placed`
- Internal Identifier: `GUID-1234-5678`
- Physical Name: `order_placed`

### Events and Transactions

Business events and transactions may be modelled in two valid ways:

- Model the **entity** itself, treating its lifecycle state as an attribute (e.g., `Order` with `Order Status` as an attribute)
- Model the entity in a **state-specific form** where the status is intrinsic to the concept (e.g., `Order Placed`, `Invoice Issued`)

Choose the approach that best represents how the business actually thinks and works with the concept.

### Relationship Naming Conventions

- Use business verbs, not technical terms
- Names must read as a business sentence
- Use present tense and active voice
- Avoid vague terms (e.g., "related to", "linked to")
- Choose precise verbs that express meaning
- Reflect ownership or composition explicitly where applicable
- Use domain-specific language
- Direction should be clear

**Examples:**

- `Customer places Order`
- `Policy covers Claim`
- `Doctor refers Patient`
- `Employer employs Worker`
- `Patient receives Treatment`
- `Supplier delivers Product`

**Avoid:**

- `has_fk`
- `references`
- `linked to`
- `related to`

#### Relationship Directionality

- Every relationship has one authoritative direction and name
- Model one semantic relationship with one verb. Allow alternate read-paths in tooling or documentation, not duplicate relationships
- Do not create passive inversions (e.g., "is placed by" instead of "places")
- If you need a different semantic perspective, model it as a separate relationship

### Relationship Cardinality
There are two options:

- Use crow's foot notation (shows 'one' with a line, 'many' with a branching crow's foot; commonly denotes relationships such as one-to-many, many-to-many, etc.)
- Use multiplicity notation (explicitly labels relationship ends with numbers or ranges, e.g., '0..1', '1', '0..*', '1..*', indicating how many entities participate in the relationship)

**Single direction (preferred):**

- `Customer places Order` ✓

**Avoid passive inversion (same relationship, just reversed grammar):**

- `Order is placed by Customer` ✗

**Separate relationships (different business concepts):**

- `Customer places Order` (ordering action)
- `Customer is billed for Order` (financial relationship)

---

## Conceptual Models

> For conceptual modelling framework and principles, see [Conceptual Models](modelling_framework.md#conceptual-information-models) in the Modelling Framework.

### Entities (Conceptual)

- Business-Facing format with capitalised first letters and spaces (e.g., `Order Placed`, `Customer`, `Patient Encounter`)
- Optimised for clarity and business communication

### Diagrammatic Representation (Conceptual)

- Boxes represent business concepts (entities)
- Lines represent relationships

**Show:**

- Core concepts
- Key relationships
- Major business rules

**Do not show:**

- Keys
- Data types
- Technical constraints
- Detailed cardinality beyond simple one-to-many

---

## Logical Models

> For logical modelling framework and principles, see [Logical Models](modelling_framework.md#logical-information-models) in the Modelling Framework.

### Entities (Logical)

- Use the same format as conceptual models
- Business-Facing format with capitalised first letters and spaces (e.g., `Order Placed`, `Customer`, `Patient Encounter`)
- Optimised for clarity and business communication

### Attributes

- Use Business-Facing format with capitalised first letters and spaces
- Names must be business-meaningful
- Avoid embedding data types in names
- Prefer semantic names over technical ones

### Measures and Metrics

> For measures and metrics framework, governance, and placement principles, see [Measures and Metrics](modelling_framework.md#measures-and-metrics) in the Modelling Framework.

- Use Business-Facing format with capitalised first letters and spaces (e.g., `Invoice Amount`, `Item Count`)
- Make names consistent with related entities and attributes
- Use semantic suffixes [Quantities & Measures](modelling_standards_and_conventions.md#quantities-measures) in the Standard Suffix Inventory.

**Example:**

- **Measure:** `Total Sales Amount` — aggregated sum of sales transactions
- **Metric:** `Average Order Value` — calculated as `Total Sales Amount / Order Count`

**Important:** 

- **Keep metric names dimension-agnostic**: Avoid including dimension references like "by Product" or "by Region" in metric names—the dimensional model handles slicing (e.g., use `Sales Amount` not `Sales Amount by Product by Time`). 
- **Exception—Intrinsic dimensions**: Include dimensional qualifiers only when the dimension is intrinsic to the calculation logic and defines how the metric works (e.g., `Customer Lifetime Value` is calculated at customer grain by definition; `Monthly Revenue Growth Rate` compares month-to-month by definition).
- **Temporal metrics**: Include time window qualifiers when the period defines the calculation logic (e.g., `Year-to-Date Sales Amount` accumulates from year start; `90-Day Rolling Average` uses a 90-day window).
- **Aggregation behavior**: Averages and ratios cannot be summed across periods; they must be recalculated at each grain. BI tools like Power BI default to SUM, producing incorrect results for non-additive metrics—define these explicitly with appropriate DAX measures (AVERAGE, DIVIDE) rather than column aggregations. Document additivity to prevent incorrect rollups.

### Keys (Logical Models)

#### Key Types

**Natural or Surrogate (implementation characteristic):**

- **Natural Key**: Uses a real-world business identifier (e.g., `Invoice Number`, `Medicare Number`)
- **Surrogate Key**: System-generated identifier (e.g., auto-increment, UUID)
- This is a metadata property, not encoded in the name

**Primary or Alternate (role in the model):**

- **Primary Key (PK)**: The chosen unique identifier for the entity
- **Alternate Key (AK)**: Any other unique identifier
- Tag primary keys as `(PK)` in logical models
- All other unique keys are considered alternate keys

#### Key Naming Pattern

Naming follows these patterns for readability, but metadata remains the source of truth for key roles (i.e., metadata—not naming—should be used to identify Primary Keys, Alternate Keys, and Foreign Keys).


> **Default:** When source systems or business glossaries explicitly define key names, preserve their terminology even if it deviates from this convention. Whether a key is natural or surrogate, primary or alternate is captured as metadata

> **Missing name:** Where a name hasn't been provided, then preference `ID` suffix with natural/business keys and `Key` suffix with surrogate warehouse keys. 

> **Canonical Models Rule:** In canonical/enterprise views, fields named \<Entity\> `ID` must represent business-meaningful identifiers recognizable across domains, not warehouse-generated surrogates. Use \<Entity\> `Key` for warehouse surrogate keys.

**Scenario Example:**

- **Source system (CRM):** Provides a natural business identifier for the customer `customer_number`.
- **Logical model:** The identifier is named `Customer Number`.
- **Physical staging layer:** The identifier is stored as the column `customer_number` to reflect source system (CRM).
- **Canonical view:** Exposes `Customer Number` with metadata indicating it is a natural primary key (NPK).
- **Dimensional model (Customer dimension):** Introduces a warehouse surrogate key called `Customer Key` (physical column `customer_key`) to support SCD Type 2.
- **Fact tables (e.g. Sales fact):** Store a foreign key column `customer_key` that references the `Customer` dimension.
- **Domain contracts (other marts using the conformed dimension):** A `Sales` mart and a `Billing` mart both join their fact tables to the conformed `Customer` dimension via `customer_key` internally. The dimension still carries the natural identifier as `Customer Number` (physical column `customer_number`), and this is what domains expose in their canonical contracts so downstream consumers see a consistent business identifier rather than the surrogate key.

**Examples:**

*Applying the "missing name" rule (when creating new identifiers):*

**Primary Keys:**

- `Customer ID` (PK) — natural business identifier (when no source name exists)
- `Customer Key` (PK) — surrogate warehouse key (e.g., auto-increment, UUID)
- `Order ID` (PK) — natural business identifier (when no source name exists)

*Applying the "default" rule (preserving source system names):*

**Primary Keys from Source:**

- `Customer Number` (PK) — natural business identifier from source system
- `Account Code` (PK) — natural business identifier from source system

**Foreign Keys:**

FKs can point to either natural or surrogate PKs and that the FK naming follows the target’s business term where possible:

- `Patient ID` (FK) — references `Patient ID` from another entity (the original key may be natural or surrogate)
- `Customer Order Key` (FK) — references the surrogate composite key 

**Warehouse Dimension Keys:**

- `Customer Key` — surrogate key for dimensional models (used for joins in the warehouse)
- Physical mapping: `customer_key`

**Other unique identifiers named according to business meaning:**

- `Invoice Number` (PK) — natural business reference number for an invoice
- `Medicare Number` (AK) — natural alternate key for a patient
- `National Health Identifier` (AK) — natural alternate key for a patient

**Composite Keys:**

- Our preference is to give composite keys their own distinct name if the modelling tools allow for it.
- Each component follows the same naming conventions (Default or Missing name rules apply)
- If the composite is a natural key, use `ID` suffix; if any single component is surrogate use `Key` suffix

Examples:

- `Flight ID` (Natural PK) = `Flight Number` + `Departure Date` — logical name for the natural composite (all components natural)
- `Enrollment ID` (Natural PK) = `Student Number` + `Course Number` — logical name for natural composite (all components natural)
- `Daily Product Sales Key` (Surrogate PK) = `Product ID` + `Date Key (from DW)` + `Transaction Key (from DW)` — logical name for surrogate composite (at least one surrogate component)
- `Customer Order Key` (Surrogate PK) = `Customer ID` + `Order Key (from DW)` — logical name for surrogate composite (mixed: surrogate + natural)



### Diagrammatic Representation (Logical)

- Consistent with conceptual models, with additional details to show attributes, key types
- Show relationships as lines and cardinality as crows foot or multiplicity notation.


---

## Physical Models

> For physical modelling framework and common formats, see [Physical Models](modelling_framework.md#physical-data-models) in the Modelling Framework.

Physical models represent implemented storage structures and must follow deterministic, platform-safe naming conventions.

**Singular vs Plural Naming:**

- **Default**: Use singular nouns for table names (e.g., `customer`, `invoice`, `payment`)
- **Exception - Dimensional Facts**: Use plural nouns for fact tables (e.g., `fact_payments`, `fact_orders`)
- **Dimensional Dimensions**: Use singular nouns (e.g., `dim_customer`, `dim_product`)

### Relational Structures

#### Naming

All physical objects must use **lowercase with underscores** (`snake_case`).

**General Rules:**

- No mixed case
- No spaces
- Use singular table names (except for dimensional model facts - see below)
- Prefer clarity over brevity
- Avoid source-system naming where possible

**Tables:**

- Express business meaning
- Use prefixes where appropriate (e.g., `fact_`, `dim_`)
- Examples: `customer`, `invoice`, `fact_orders` (dimensional fact), `dim_patient` (dimensional dimension)

**Columns:**

- Map directly from logical attributes
- Use semantic names
- Do not encode data types or storage formats
- There are two options for handling units:
  - A: embed the unit in the column name
  - B: separate value and unit type as separate columns
- Examples: `customer_id`, `invoice_number`, `order_datetime`, `total_amount`, `is_active`

#### Diagrammatic Representation

- Consistent with logical models
- Show relationships as lines and cardinality as crows foot or multiplicity notation.

#### Databricks Conventions

For detailed conventions and examples see [Databricks](standards_and_conventions.md#databricks)

**Catalogs:**

- Catalogs represent domain-level scope (e.g., `engineering__dev`, `corporate__dev`)
- May optionally include data stage, zone, or subdomain in the catalog name depending on desired granularity for access and sharing controls
- Naming is lowercase, underscore-separated, and meaningful
- Pattern: `{domain}{__env}` (e.g., `corporate__dev`)

**Schemas:**

- Schemas represent layers and source systems within a catalog
- Use double underscores (`__`) as separators for schema components
- Optional channel suffix as an additional `__`-separated component (avoid special characters in identifiers)
- Pattern: `{layer}{__source_system}{__source_schema}{__channel}` (e.g., `ods__fhirhouse__dbo__lakeflow`)
- Full example: `engineering__dev.ods__fhirhouse__dbo__lakeflow.encounter`

**Tables and Views:**

- Tables for persisted data
- Views for logical abstraction
- Names reflect business meaning, not source-system terminology

**Columns:**

- Use lowercase `snake_case`
- No abbreviations unless standard




---

### Dimensional Models

> For dimensional modelling framework, conformed dimensions, and dimensional bus matrix, see [Dimensional (Kimball) Model](modelling_framework.md#dimensional-kimball-model) in the Modelling Framework.

For detailed conventions and examples including staging models see the Information Marts sections of [Schema and Object Conventions](standards_and_conventions.md#schema-and-object-conventions)

**Naming:**

- Fact tables prefixed with `fact_` and use **plural** entity names (e.g., `fact_payments`, `fact_orders`) (pluralisation here is an exception in naming)
- Dimension tables prefixed with `dim_` and use **singular** entity names (e.g., `dim_customer`, `dim_date`)
- Warehouse dimension keys: `<entity>_key` (the surrogate key used for joins in the warehouse)


**SCD Type 2 Columns:**

| Column | Description |
|--------|-------------|
| `effective_from_datetime` | Timestamp when this record version became effective |
| `effective_to_datetime` | Timestamp when this record version expired (NULL for current records) |
| `updated_datetime` | Timestamp from the source system indicating when the record was last modified |

**Default dbt Snapshot Columns:**

| Column | Description |
|--------|-------------|
| `dbt_scd_id` | Unique identifier for each snapshot record (surrogate key) |
| `dbt_valid_from` | Timestamp when this record version became effective |
| `dbt_valid_to` | Timestamp when this record version expired (NULL for current records) |
| `dbt_updated_at` | Timestamp from the source system indicating when the record was last modified |
| `dbt_deleted` | (Optional) Boolean flag indicating if a record has been deleted from the source system |

**Measures and Metrics:**

- Align to logical model naming but apply lowercase snake_case (e.g., invoice_amount)

#### Data Warehouse Keys

##### Key resolution

Mappings of keys are often required to resolve and integrate heterogeneous identifiers originating from multiple source systems into a consistent warehouse-wide identifier. Key mapping tables support integration by translating source-system identifiers into a common business key and/or surrogate key used by downstream dimensional models.

**Example:**
- `corporate__prod.edw.keys_employee`  (base mart keyset conforming SAP and Workday employee identifiers into a single enterprise employee key)

---

##### Business Keys

**Business keys (BKs)** represent the real-world identifier of an entity and are sourced from operational systems.

Business keys may be:

  - Single-column or composite
  - Source-specific or enterprise-conformed

The basis of our conformed BK pattern is: `source_system`|`source_identifier` in string format

**Usage guidance:**

- Business keys **may** be used directly as dimension primary keys (and fact foreign keys) for simple **Type 1** dimensions, however this creates inconsistency if other dimensions use surrogate keys for Type 2 tracking.
- Business keys alone are insufficient for **Type 2** dimensions because they do not distinguish historical versions of the same entity. Hence the need for Surrogate Keys.

---

##### Surrogate Keys

**Surrogate keys (SKs)** are system-generated identifiers that uniquely represent a *specific version* of a dimensional entity.

They are required for:

- Type 2 (and higher) SCD dimensions
- Stable fact-to-dimension joins
- Decoupling fact tables from volatile or composite business keys

For Type 2 dimensions, the **logical version grain** is:

`business_key` + `effective_from_datetime`

The surrogate key is the **physical primary key** representing this grain and is used by fact tables as the foreign key.

---

##### Type 2 Fact Resolution

For Type 2 behaviour, facts must resolve the correct dimensional version using an effective-date window.

```sql
fact.business_key = dim.business_key
AND fact.event_timestamp >= dim.effective_from_datetime
AND fact.event_timestamp <  COALESCE(dim.effective_to_datetime, TIMESTAMP '9999-12-31 00:00:00')
```

This resolution **requires a lookup** to the dimension (or a dimension version map). It cannot be inferred independently by the fact row.

---

##### Databricks Surrogate Keys

In Databricks, there are multiple approaches to creating surrogate keys.

- **IDENTITY**

  Databricks recommended approach [here](https://www.databricks.com/blog/2022/08/08/identity-columns-to-generate-surrogate-keys-are-now-available-in-a-lakehouse-near-you.html):

  - `BIGINT GENERATED ALWAYS AS IDENTITY`
  - Sequence- or merge-based incrementing keys

  Characteristics:

  - High performance for joins and aggregations
  - No collision risk
  - Environment-specific values (acceptable in most warehouse designs)

  Limitations:

  - Not compatible with dbt-managed tables
  - Concurrent writes to the table are not supported

  These keys are generated in the **silver or gold layer**.

Other valid approaches (depending on downstream requirements):

- **Natural key only (BK as PK/FK)**: valid for **Type 1** dimensions; simplest, but typically wider keys and slower joins.
- **Type 2 composite key (string)**: store `(business_key + effective_from_datetime)` as a single concatenated key; simple and transparent, but increases key width and join cost.
- **Deterministic hash key**: hash `(business_key + effective_from_datetime)` into a fixed-width value (string or numeric) for smaller keys and cross-environment stability; manage collision risk by choosing an appropriate hash and store BK/effective_from_datetime alongside for traceability.

---

##### dbt Surrogate Keys

dbt supports surrogate key generation using deterministic hashing constructs, such as:

- `dbt_utils.surrogate_key(...)` (MD5 string hash)
- Platform-specific numeric hashes (e.g. `xxhash64`) cast to `BIGINT`

Usage guidance:

- Hash-based SKs should include:

  - Business key
  - `effective_from_datetime` (for Type 2 dimensions)

- Hash-based SKs enable deterministic keys across environments:

  - MD5 returns a 32-character hex string; xxhash64 returns a BIGINT
  - Numeric (BIGINT) keys offer better join performance
  - Consider SHA256 if hash collision risk is a concern

---

### Key Design Summary

- Business keys define **entity identity**
- `(business_key + effective_from_datetime)` defines **Type 2 version identity**
- Surrogate keys provide the **physical join mechanism**
- Type 2 correctness **always requires a version lookup**
- Prefer **numeric** SKs for performance (especially with PowerBI); use **deterministic** SKs when cross-environment stability is required
- **Date dimension tip**: Use integer with YYYYMMDD format as the surrogate key for optimal performance






---

### Reference Data

> For reference data and master data framework, logical representation, and physical implementation patterns, see [Master Data and Reference Data](modelling_framework.md#master-data-and-reference-data) in the Modelling Framework.

Reference data plays a critical role in conformance by providing standardised values that enable mapping of source-specific codes to canonical enterprise definitions.

#### Logical Modelling

Reference entities contain:

- Natural keys (e.g., Country Code, Product Type Code)
- Code and description attributes
- Optional: effective datetimes, display sequences, parent references (hierarchies), business metadata
- One-to-many relationships to domain entities

**Example: Country Code Reference Entity**

| country_code | country_name       | iso_code_3 | display_sequence | effective_from_datetime   | effective_to_datetime     |
|--------------|--------------------|------------|------------------|---------------------------|---------------------------|
| AU           | Australia          | AUS        | 10               | 2020-01-01 00:00:00       | 9999-12-31 00:00:00       |
| GB           | United Kingdom     | GBR        | 20               | 2020-01-01 00:00:00       | 9999-12-31 00:00:00       |
| US           | United States      | USA        | 30               | 2020-01-01 00:00:00       | 9999-12-31 00:00:00       |
| NZ           | New Zealand        | NZL        | 40               | 2020-01-01 00:00:00       | 9999-12-31 00:00:00       |
| CA           | Canada             | CAN        | 50               | 2020-01-01 00:00:00       | 9999-12-31 00:00:00       |

*Note: Additional standard attributes (audit columns: created_timestamp, created_by, modified_timestamp, modified_by; optional: parent_code, version, source_system_id) would be included in the physical implementation.*

#### Physical Implementation

Raw Reference Data sourced from upstream systems may require their own staging and transformation pipelines in order to conform them to standard, preserve change history and capture required metadata.

- **Reference tables** are stored in Bronze/ODS layer for wide availability following the [reference data naming standard](standards_and_conventions.md#schema-and-object-conventions)
  - Schema naming convention: `ref{optional: __domain name}{optional: __subdomain name(s)}`
  - Object naming convention: `{reference_data_set_name}{optional:__source_system}{optional:__source_channel}`
  - e.g.: `corporate__dev.ref.account_code`

- Effectivity: `effective_from_date`, `effective_to_date`, (`is_active` is derivable)
- Audit: `created_datetime`, `created_by`, `updated_datetime`, `updated_by`
- Hierarchy: `parent_code`
- Business: `description`
- Technical: `version`, `source_system_id`

**Usage:**

- **Mapping logic** is applied in Silver/EDW layer staging models during transformation for domain/enterprise-wide application.
- **Consumption:** Post-mapped data are exposed in marts in Silver or indirectly in Gold (having passed through Silver).
- **As dimension attributes:** Reference values embedded directly in dimension tables (e.g., Product Type Code/Description in Product dimension) for filtering and grouping.

**Change tracking:**

- Implement Type 1, 2, 4, or 6 slowly changing dimension strategies based on business requirements for point-in-time accuracy. This is especially important when reference data changes frequently or has many attributes.
- Consider using mini-dimensions/outriggers—separate dimension tables linked via foreign keys—to efficiently track history without excessive row growth in the main dimension.

**Recommended Practices:**

- Store codes and descriptions in fact tables only when necessary for performance
- Prefer dimension lookups to maintain single source of truth

---

## Standard Suffix and Prefix Inventory

### Identity & Keys

| Suffix | Meaning | Logical/Conceptual | Physical |
|--------|--------|---------------------|----------|
| `ID` | Primary business identifier | `Customer ID` | `customer_id` |
| `Key` | Warehouse dimension key (dimensional models only) | `Customer Key` | `customer_key` |
| `Number` | Business reference number | `Invoice Number` | `invoice_number` |
| `Code` | Coded business value | `Diagnosis Code` | `diagnosis_code` |
| `Reference` | External reference | `External Reference` | `external_reference` |
| `Identifier` | Explicit identifier (potentially many identifiers) | `National Identifier` | `national_identifier` |

### Temporal Concepts

| Suffix | Meaning | Logical/Conceptual | Physical |
|--------|--------|---------------------|----------|
| `Date` | Calendar date only | `Admission Date` | `admission_date` |
| `DateTime` | Timestamp (assumed UTC) | `Order DateTime` | `order_datetime` |
| `DateTime <Timezone>` | Timestamp in explicit timezone | `Order DateTime AEST` | `order_datetime_aest` |
| `Time` | Time only | `Appointment Time` | `appointment_time` |
| `From Date` | Validity start | `Policy From Date` | `policy_from_date` |
| `To Date` | Validity end | `Policy To Date` | `policy_to_date` |

Example:

- Business Name: `Order DateTime`
- Physical Name (UTC, implied): `order_datetime`
- Physical Name (Local timezone explicit): `order_datetime_aest`


### State & Classification

| Suffix | Meaning | Logical/Conceptual | Physical |
|--------|--------|---------------------|----------|
| `Status` | Lifecycle state | `Order Status` | `order_status` |
| `Type` | Classification | `Customer Type` | `customer_type` |
| `Category` | Grouping | `Product Category` | `product_category` |
| `Class` | Structural grouping | `Asset Class` | `asset_class` |

### Quantities & Measures

| Suffix | Meaning | Logical/Conceptual | Physical |
|--------|--------|---------------------|----------|
| `Count` | Quantity | `Item Count` | `item_count` |
| `Amount` | Monetary value | `Invoice Amount` | `invoice_amount` |
| `Value` | General numeric | `Score Value` | `score_value` |
| `Rate` | Rate | `Interest Rate` | `interest_rate` |
| `Ratio` | Proportion | `Utilisation Ratio` | `utilisation_ratio` |
| `Percentage` | Percentage | `Discount Percentage` | `discount_percentage` |

### Boolean Indicators (Prefix Convention)

| Prefix | Meaning | Logical/Conceptual | Physical |
|--------|--------|---------------------|----------|
| `Is` | State check | `Is Active` | `is_active` |
| `Has` | Ownership | `Has Consent` | `has_consent` |
| `Can` | Capability | `Can Transact` | `can_transact` |

### Audit & Control

| Suffix | Meaning | Logical/Conceptual | Physical |
|--------|--------|---------------------|----------|
| `Created DateTime` | Creation timestamp | `Created DateTime` | `created_datetime` |
| `Updated DateTime` | Last update timestamp | `Updated DateTime` | `updated_datetime` |
| `Deleted DateTime` | Soft deletion | `Deleted DateTime` | `deleted_datetime` |
| `Effective From Date` | Valid from date | `Effective From Date` | `effective_from_date` |
| `Effective To Date` | Valid to date | `Effective To Date` | `effective_to_date` |

---

### Summary of Naming Rules

Suffixes encode **meaning**, not technical data type or implementation details.

| Rule | Logical/Conceptual | Physical |
|------|---------------------|----------|
| Primary business identifier | `Customer ID` | `customer_id` |
| Warehouse dimension key | `Customer Key` | `customer_key` |
| Business reference | `Invoice Number`, `Diagnosis Code` | `invoice_number`, `diagnosis_code` |
| Temporal meaning | `Order DateTime` (assumed UTC) | `order_datetime` |
| Classification | `Order Status`, `Customer Type` | `order_status`, `customer_type` |
| Measures | `Invoice Amount`, `Interest Rate` | `invoice_amount`, `interest_rate` |
| Boolean indicators | `Is Active`, `Has Consent` | `is_active`, `has_consent` |

---
