# Intuitas Data Modelling Standards and Conventions

[Return to home](README.md)

> Updated 5/12/2025

---

## Table of Contents
---


  - [General Naming Conventions](#general-naming-conventions)
    - [Entity Naming (General Rules)](#entity-naming-general-rules)
    - [Business Names vs Physical Names](#business-names-vs-physical-names)
    - [Events and Transactions](#events-and-transactions)
    - [Relationship Naming Conventions](#relationship-naming-conventions)
    - [Relationship Directionality](#relationship-directionality)
    - [Relationship Cardinality](#relationship-cardinality)
  - [Conceptual Models](#conceptual-models)
    - [Entities](#entities)
    - [Diagrammatic Representation](#diagrammatic-representation)
  - [Logical Models](#logical-models)
    - [Entities](#entities-1)
    - [Attributes](#attributes)
    - [Keys (Logical Models)](#keys-logical-models)
    - [Diagrammatic Representation](#diagrammatic-representation-1)
  - [Physical Models](#physical-models)
    - [Relational Structures](#relational-structures)
    - [Databricks Conventions](#databricks-conventions)
    - [Dimensional Models](#dimensional-models)
    - [Reference Data](#reference-data)
  - [Standard Suffix and Prefix Inventory](#standard-suffix-and-prefix-inventory)
    - [Identity & Keys](#identity--keys)
    - [Temporal Concepts](#temporal-concepts)
    - [State & Classification](#state--classification)
    - [Quantities & Measures](#quantities--measures)
    - [Boolean Indicators (Prefix Convention)](#boolean-indicators-prefix-convention)
    - [Audit & Control](#audit--control)
    - [Summary of Naming Rules](#summary-of-naming-rules)
  - [Examples](#examples)
    - [Keys (Business-facing)](#keys-business-facing)
    - [Date and Time](#date-and-time)
    - [Units](#units)
    - [Events and Transactions (Examples)](#events-and-transactions-examples)

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
- Be specific: add context where needed (e.g., `AdminUser` vs `User`)

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

### Entities

- Business Facing format with capitalised first letters and spaces (e.g., `Order Placed`, `Customer`, `Patient Encounter`)
- Optimised for clarity and business communication

### Diagrammatic Representation

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

### Entities

- Use the same format as conceptual models
- Business Facing format with capitalised first letters and spaces (e.g., `Order Placed`, `Customer`, `Patient Encounter`)
- Optimised for clarity and business communication

### Attributes

- Use Business Facing format with capitalised first letters and spaces
- Names must be business-meaningful
- Avoid embedding data types in names
- Prefer semantic names over technical ones

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

Metadata, and not naming, should be used to identify Primary Keys, Alternate Keys, and Foreign Keys. However, keys are likely to include one of the standard key/identifier suffixes (see [Standard Suffix and Prefix Inventory](#standard-suffix-and-prefix-inventory)).

Examples:

**Primary Keys:**
- `Customer ID` (PK) — may be natural or surrogate
- `Order ID` (PK) — may be natural or surrogate

Note: If a field is named \<Entity\> ID in a canonical view, it must represent a business-meaningful identifier, not a generated surrogate.

**Foreign Keys:**
- `Patient ID` (FK) — references `Patient ID` from another entity (the original key may be natural or surrogate)

**Other unique identifiers are named according to business meaning:**
- `Invoice Number` (PK) — natural primary key
- `Email Address` (AK) — natural alternate key
- `Medicare Number` (AK) — natural alternate key
- `National Health Identifier` (AK) — natural alternate key

**Composite Keys:**
- The fact that they form a composite key is captured as metadata, not in naming
- No special prefixes or suffixes are needed
- Examples: `Flight Number` + `Departure Date` (PK) or `Account Number` + `Transaction Sequence` (PK)

### Diagrammatic Representation
- Consistent with conceptual models, with additional details to show attributes, key types
- Show relationships as lines and cardinality as crows foot or multiplicity notation.


---

## Physical Models

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
---

#### Databricks Conventions

For detailed conventions and examples see [Databricks](standards_and_conventions.md#databricks)

**Catalogs:**
- Catalogs represent domain-level scope (e.g., `intuitas_engineering_dev`, `intuitas_corporate_dev`)
- May optionally include data stage, zone, or subdomain in the catalog name depending on desired granularity for access and sharing controls
- Naming is lowercase, underscore-separated, and meaningful
- Pattern: `{domain_name}{_environment}` (e.g., `intuitas_corporate_dev`)

**Schemas:**
- Schemas represent layers, stages, zones, or source systems within a catalog
- Use double underscores (`__`) as separators for schema components
- Pattern: `{stage}__{zone}__{source_system}__{schema}` (e.g., `bronze__ods__fhirhouse__dbo`)
- Full example: `intuitas_engineering_dev.bronze__ods__fhirhouse__dbo__lakeflow`

**Tables and Views:**
- Tables for persisted data
- Views for logical abstraction
- Names reflect business meaning, not source-system terminology

**Columns:**
- Use `snake_case`
- No abbreviations unless standard

**Keys:**
- Primary keys: `<table>_id`
- Foreign keys: `<referenced_table>_id`
- Warehouse surrogate keys: `<entity>_key`

---

### Dimensional Models

For detailed conventions and examples including staging models see the Information Marts sections of [Schema and Object Conventions](standards_and_conventions.md#schema-and-object-conventions)

**Naming:**
- Fact tables prefixed with `fact_` and use **plural** entity names (e.g., `fact_payments`, `fact_orders`)
- Dimension tables prefixed with `dim_` and use **singular** entity names (e.g., `dim_customer`, `dim_date`)
- Warehouse dimension keys: `<entity>_key` (the surrogate key used for joins in the warehouse)
- Business keys: `<entity>_id` (the identifier from the source system)

**Rationale:** Fact tables contain multiple rows of events/transactions (plural), while dimension tables represent single entity instances (singular).

**Gold Layer Examples:**

*Staging:*
- `intuitas_corporate_dev.stg.fact__late_payments__01__pivoted_by_order`
- `intuitas_corporate_dev.stg__corporate__finance.fact__late_payments__01__pivoted_by_order`
- `stg__dim_account__01_dedupe.sql`

*Information Marts:*
- `intuitas_corporate_dev.mart.fact_late_payments` — fact table
- `intuitas_corporate_dev.mart.dim_customer` — dimension table
- `mart__dim_account.sql` — dbt model file
- `mart__dim_date.sql` — dbt model file

**Keys:**
- `customer_key` — warehouse dimension key (surrogate key for joins)
- `customer_id` — business identifier from source system

**SCD Type 2 Columns:**

| Column | Description |
|--------|-------------|
| `start_at` | Timestamp when this record version became effective |
| `end_at` | Timestamp when this record version expired (NULL for current records) |
| `updated_at` | Timestamp from the source system indicating when the record was last modified |

**Default dbt Snapshot Columns:**

| Column | Description |
|--------|-------------|
| `dbt_scd_id` | Unique identifier for each snapshot record (surrogate key) |
| `dbt_valid_from` | Timestamp when this record version became effective |
| `dbt_valid_to` | Timestamp when this record version expired (NULL for current records) |
| `dbt_updated_at` | Timestamp from the source system indicating when the record was last modified |
| `dbt_deleted` | (Optional) Boolean flag indicating if a record has been deleted from the source system |

---

### Reference Data

Reference data plays a critical role in conformance by providing standardised values that enable mapping of source-specific codes to canonical enterprise definitions.


#### Logical Modelling

Reference entities contain:
- Natural keys (e.g., Country Code, Product Type Code)
- Code and description attributes
- Optional: effective dates, display sequences, parent references (hierarchies), business metadata
- One-to-many relationships to domain entities

**Example: Country Code Reference Entity**

| Country_Code | Country_Name       | ISO_Code_3 | Display_Sequence | Effective_From_Date | Effective_To_Date |
|--------------|--------------------|------------|------------------|---------------------|-------------------|
| AU           | Australia          | AUS        | 10               | 2020-01-01          | 9999-12-31        |
| GB           | United Kingdom     | GBR        | 20               | 2020-01-01          | 9999-12-31        |
| US           | United States      | USA        | 30               | 2020-01-01          | 9999-12-31        |
| NZ           | New Zealand        | NZL        | 40               | 2020-01-01          | 9999-12-31        |
| CA           | Canada             | CAN        | 50               | 2020-01-01          | 9999-12-31        |

*Note: Additional standard attributes (audit columns: created_timestamp, created_by, modified_timestamp, modified_by; optional: parent_code, version, source_system_id) would be included in the physical implementation.*


#### Physical Implementation

Raw Reference Data sourced from upstream systems may require their own staging and transformation pipelines in order to conform them to standard, preserve change history and capture required metadata.

- **Reference tables** are stored in Bronze/ODS layer for wide availability following the [reference data naming standard](standards_and_conventions.md#schema-and-object-conventions)

        - Schema naming convention: `ref{optional: __domain name}{optional: __subdomain name(s)}`
        - Object naming convention: `{reference data set name} (optional:__{source_system}__{source_channel}}`
        
        - e.g: intuitas_corporate.ref.account_code

- Effectivity: `effective_from_date`, `effective_to_date`, (`is_active` is derivable)
- Audit: `created_timestamp`, `created_by`, `modified_timestamp`, `modified_by`
- Hierarchy: `parent_code`
- Business: `description`
- Technical: `version`, `source_system_id`


- **Mapping logic** is applied in Silver/EDW layer staging models during transformation for domain/enterprise-wide application.
- **Consumption** post-mapped data are exposed in marts in Silver or indirectly in Gold (having passed through Silver).


- **As dimension attributes:** Reference values embedded directly in dimension tables (e.g., Product Type Code/Description in Product dimension) for filtering and grouping.


**Change tracking:** 
- Implement Type 1, 2, 4, or 6 slowly changing dimension strategies based on business requirements for point-in-time accuracy. When reference data changes frequently or has many attributes. 
- Consider using mini-dimensions/outriggers—separate dimension tables linked via foreign keys—to efficiently track history without excessive row growth in the main dimension.


**Recommended Practices:**
- Store codes and descriptions in fact tables only when necessary for performance
- Prefer dimension lookups to maintain single source of truth





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

## Examples

### Keys (Business-facing)

- Primary Business ID: `Customer ID` (PK)
- Business Reference: `Invoice Number` (could be PK or AK)
- Alternate Identifier: `National Identifier` (AK)
- Alternate Identifier: `Email Address` (AK)
- Warehouse Dimension Key: `Customer Key` (dimensional models)
- Physical Mapping: `customer_id`, `customer_key`

Note: Whether a key is natural or surrogate is metadata, not encoded in the name.

### Date and Time

- Business Name: `Order DateTime`
- Physical Name (UTC): `order_datetime_utc`
- Physical Name (Local): `order_datetime_aest`

### Units

There are two options for handling units:

**Option 1: Embed the unit in the physical name**
- Business Name: `Weight` → Physical Name: `weight_kg`
- Business Name: `Temperature` → Physical Name: `temperature_c`

**Option 2: Separate unit from the value**
- Measure Value: `measure_value`
- Measure Unit: `measure_unit_code`

Example: Instead of `weight_kg`, use `weight` + `weight_unit_code` where the unit code might be 'kg', 'lb', etc.

### Events and Transactions

- Business Term: `Payment Received` → Model Name: `Payment Received` → Physical Name: `payment_received`
- Business Term: `Invoice Issued` → Model Name: `Invoice Issued` → Physical Name: `invoice_issued`
