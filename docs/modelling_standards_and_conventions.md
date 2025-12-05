# Intuitas Data Modelling Standards and Conventions
[Return to home](README.md)

> Updated 5/12/2025


---
## Naming (General Rules)

- Use clear business language  
- Use terminology familiar to business stakeholders and consistent with the business glossary  
- In domain models, use terms appropriate to the business context and document synonyms in the business glossary  
- Canonical entities must use terms that are formally agreed and recognised across domains  
- Use singular nouns for entities (e.g., `Customer`, not `Customers`)  
- Avoid system-specific or technical terminology  
- Avoid abbreviations unless universally understood  
- Be specific: add context where needed (e.g., `AdminUser` vs `User`)  

---

### Entity Labels and Names

Different artefacts use different naming conventions depending on their purpose.

#### Business Glossary (Labels)
- Free text with spaces  
- Optimised for readability  
- Used in documentation, reports, and business communication  
- Example: `Order Placed`

#### Conceptual and Logical Model Entities
- Treated as identifiers  
- Use PascalCase without separators as the authoritative model name  
- Assign a business-facing label from the glossary for display and documentation  
- Used across models, APIs, and metadata  
- Example: `OrderPlaced`

#### Physical Models (Database Objects)
- `snake_case` (lowercase, underscore-separated)  
- Used in schemas, tables, and columns  
- Example: `order_placed`

---

### Naming Events and Transactions

All business facts are named in **past tense**.

#### Standard Pattern
`<Noun> <PastTenseVerb>`

#### Examples
- Business Term: `Order Placed`  
- Model Entity: `OrderPlaced`  
- Physical Name: `order_placed`  

Additional examples:
- `Payment Received`  
- `Patient Admitted`  
- `Invoice Issued`  
- `Case Closed`

---

## Conceptual Models

### Diagrammatic Representation

- Boxes represent business concepts (entities)  
- Lines represent relationships  

#### Show:
- Core concepts  
- Key relationships  
- Major business rules  

#### Do not show:
- Keys  
- Data types  
- Technical constraints  
- Detailed cardinality beyond simple one-to-many  


---

## Logical Models

#### Entities
- Use the same identifiers as conceptual models  
- PascalCase without separators (e.g., `OrderPlaced`, `Customer`, `PatientEncounter`
- Assign a business-facing label from the glossary for display and documentation  

#### Attributes
- Use PascalCase or lowerCamelCase for identifiers (no underscores)  
- Assign a business-facing label from the glossary for display and documentation  
- Names must be business-meaningful  
- Avoid embedding data types or units in names  
- Prefer semantic names over technical ones   

#### Keys
- Primary keys clearly identified  
- Natural keys preferred at logical level  
- Surrogate keys only where natural keys are unstable or complex  
- Alternate keys explicitly marked  

#### Keys (Logical Models)

All keys must follow conceptual and logical naming rules (PascalCase, no separators).

##### Primary Key
- Must uniquely identify the entity
- Named using the pattern:

  `<EntityName>Id`

Example:
- `CustomerId`
- `OrderId`
- `PatientId`

---

##### Natural Key
- Uses a real-world business identifier
- Named according to its business meaning

Examples:
- `InvoiceNumber`
- `NationalHealthId`
- `EmployeeNumber`

---

##### Surrogate Key
- Only used where a natural key is unstable or composite
- Always explicitly named as:

  `<EntityName>Key`

Example:
- `CustomerKey`
- `ProductKey`
- `OrderKey`

---

##### Alternate Key
- Any additional unique identifier
- Named according to business meaning
- Explicitly flagged in the model as `AK`

Examples:
- `EmailAddress` (AK)
- `MedicareNumber` (AK)



### Diagrammatic Representation
- Use crow’s foot or multiplicity notation  
- Show:
  - Keys  
  - Cardinality  
  - Optionality  
- Clearly distinguish:
  - Identifying vs non-identifying relationships  

---

## Physical Models

### Relational

#### Naming
- Consistent case and separator convention  
- Table names singular and descriptive  
- Column names reflect logical attributes  
- Index and constraint names standardised  

---

### Databricks Naming

#### Catalogs and Schemas
- Catalogs represent platform or organisation scope  
- Schemas represent domains or layers  
- Naming is:
  - Lower case  
  - Underscore-separated  
  - Meaningful  

**Example:**
enterprise.clinical_silver.patient


#### Tables and Views
- Tables for persisted data  
- Views for logical abstraction  
- Names reflect business meaning, not source-system terminology  

#### Columns
- `snake_case`
- No abbreviations unless standard  
- Avoid units or data types in names  

---

### Dimensional Models

#### Naming
- Fact tables prefixed with `fact_`  
- Dimension tables prefixed with `dim_`  
- Surrogate keys named `<entity>_key`  
- Natural keys named `<entity>_code` or `<entity>_id`  

**Examples:**
fact_order
dim_customer
customer_key
customer_id

SCD type 2 Columns
valid_from - Timestamp when this record version became effective
valid_to - Timestamp when this record version expired (NULL for current records)
updated_at - Timestamp from the source system indicating when the record was last modified

Default dbt Snapshot Columns
dbt_scd_id - Unique identifier for each snapshot record (surrogate key)
dbt_valid_from - Timestamp when this record version became effective
dbt_valid_to - Timestamp when this record version expired (NULL for current records)
dbt_updated_at - Timestamp from the source system indicating when the record was last modified



## Standard Suffix and Prefix Inventory

### Identity & Keys (Logical / Conceptual)

| Suffix | Meaning | Example |
|--------|--------|---------|
| `Id` | Natural / business identifier | `CustomerId` |
| `Key` | Surrogate identifier | `CustomerKey` |
| `Number` | Business reference number | `InvoiceNumber` |
| `Code` | Coded business value | `DiagnosisCode` |
| `Reference` | External reference | `ExternalReference` |
| `Identifier` | Explicit identifier | `NationalIdentifier` |

---

### Temporal Concepts

| Suffix | Meaning | Example |
|--------|--------|---------|
| `Date` | Calendar date only | `AdmissionDate` |
| `DateTime` | Timestamp | `OrderDateTime` |
| `Time` | Time only | `AppointmentTime` |
| `FromDate` | Validity start | `PolicyFromDate` |
| `ToDate` | Validity end | `PolicyToDate` |

---

### State & Classification

| Suffix | Meaning | Example |
|--------|--------|---------|
| `Status` | Lifecycle state | `OrderStatus` |
| `Type` | Classification | `CustomerType` |
| `Category` | Grouping | `ProductCategory` |
| `Class` | Structural grouping | `AssetClass` |

---

### Quantities & Measures

| Suffix | Meaning | Example |
|--------|--------|---------|
| `Count` | Quantity | `ItemCount` |
| `Amount` | Monetary value | `InvoiceAmount` |
| `Value` | General numeric | `ScoreValue` |
| `Rate` | Rate | `InterestRate` |
| `Ratio` | Proportion | `UtilisationRatio` |
| `Percentage` | Percentage | `DiscountPercentage` |

---

### Boolean Indicators (Prefix Convention)

| Prefix | Meaning | Example |
|--------|--------|---------|
| `Is` | State check | `IsActive` |
| `Has` | Ownership | `HasConsent` |
| `Can` | Capability | `CanTransact` |

---

### Audit & Control

| Suffix | Meaning | Example |
|--------|--------|---------|
| `CreatedDateTime` | Creation timestamp | `CreatedDateTime` |
| `UpdatedDateTime` | Last update timestamp | `UpdatedDateTime` |
| `DeletedDateTime` | Soft deletion | `DeletedDateTime` |
| `EffectiveFromDate` | Valid from date | `EffectiveFromDate` |
| `EffectiveToDate` | Valid to date | `EffectiveToDate` |

---

### Physical Naming Mapping (snake_case)

All logical and conceptual names are mapped to physical names using `snake_case`.

| Logical / Conceptual | Physical |
|---------------------|----------|
| `CustomerId` | `customer_id` |
| `OrderDateTime` | `order_date_time` |
| `IsActive` | `is_active` |
| `InvoiceAmount` | `invoice_amount` |

---

### Minimal Rule

Suffixes encode **meaning**, not technical data type.

| Rule | Description |
|------|-------------|
| `Id` | Natural identifier |
| `Key` | Surrogate identifier |
| `Code`, `Number` | Business reference |
| `Date`, `DateTime` | Temporal meaning |
| `Status`, `Type`, `Category` | Classification |
| `Amount`, `Value`, `Rate` | Measures |
| `Is`, `Has`, `Can` | Boolean indicators |
