# Level 2 - Domain-Level (Solution) Architecture and Patterns
[Return to home](README.md)

This section describes Domain-level instantiations of the enterprise-level reference architecture. i.e. solutions (See [Enterprise Data Platform Reference Architecture](level_1.md#enterprise-data-platform-reference-architecture))

<br>
<br>


## Table of Contents

- [Business architecture](level_2.md#business-architecture)
    - [Business processes](level_2.md#business-processes)
    - [Business glossary](level_2.md#business-glossary)
    - [Business metrics](level_2.md#business-metrics)
- [Infrastructure](level_2.md#infrastructure)
    - [Environments, Workspaces and Storage](level_2.md#environments-workspaces-and-storage)
    - [Secrets](level_2.md#secrets)
    - [Storage](level_2.md#storage)
    - [CICD and Repository](level_2.md#cicd-and-repository)
    - [Observability](level_2.md#observability)
    - [Networking](level_2.md#networking)
    - [Orchestration](level_2.md#orchestration)
- [Data Architecture](level_2.md#data-architecture)
    - [Data and information models](level_2.md#data-and-information-models)
    - [Domain glossary](level_2.md#domain-glossary)
    - [Domain data and warehouse models](level_2.md#domain-data-and-warehouse-models)
    - [Data layers and stages](level_2.md#data-layers-and-stages)
    - [Lakehouse Catalog to Storage Mapping](level_2.md#lakehouse-catalog-to-storage-mapping)
- [Data Engineering](level_2.md#data-engineering)
    - [Ingestion](level_2.md#ingestion)
    - [Transformation](level_2.md#transformation)
    - [Data Sharing and Delivery Patterns](level_2.md#data-sharing-and-delivery-patterns)
- [Data governance](level_2.md#data-governance)
    - [Data lifecycle and asset management](level_2.md#data-lifecycle-and-asset-management)
    - [Data access management](level_2.md#data-access-management)
    - [Data quality](level_2.md#data-quality)
    - [Data understandability](level_2.md#data-understandability)
    - [Privacy Preservation](level_2.md#privacy-preservation)
    - [Audit](level_2.md#audit)
- [Billing](level_2.md#billing)



Example reference architecture:

<a href="../img/logical_platform_and_pipeline_reference_architecture.png" target="_blank">
    <img src="../img/logical_platform_and_pipeline_reference_architecture.png"  alt="Platform and Pipeline Reference Architecture">
</a>

<br>

## Business architecture


### Business processes
---

Business processes are the activities and tasks that are performed to achieve the goals of the business.
Understanding them is necessary to understand:
- the context in which data is captured and used
- concepts and entities that are relevant to the domain
- the relationships between different processes and data

### Business glossary
---

A business glossary is a list of terms and definitions that are relevant to the business. see Domain Glossary.

### Business metrics
---
Metrics are the measurements of the performance of the business processes. They should be documented according to a defined template that captures, at a minimum, the following:
- name
- definition
- formula (with reference to data elements and definitions in the business glossary)
- dimensions
- source(s)
- metric owner
- frequency

<br>
## Infrastructure
---
> This section is a work in progress
### Environments, Workspaces and Storage
<a href="../img/workspaces_environments_storage.png" target="_blank">
    <img src="../img/workspaces_environments_storage.png"  alt="Workspaces, Environments and Storage">
</a>

This diagram illustrates a **data lakehouse architecture** with the following components and flow:


**Data Sources**

Data originates from multiple sources such as:
  - Databases
  - Kafka or event streaming
  - APIs or Python scripts
  - SharePoint (or similar sources)


**Enterprise Engineering Layer**

- Centralized enterprise workspaces are managed here with multiple environments. 
While work can be achieved within a single workspace and lakehouse storage account, decoupling the workspaces and storage accounts allow for more isolated change at the infrastructure level - in line with engineering requirements:
- Each workspace contains:
- Data from prod catalogs can be **shared** to other domains.


**Domain-Specific Layer**

- Each domain (e.g., business units or specific applications) operates independently within a single workspace that houses multiple environments. 
- **PROD**, **TEST**, and **DEV** storage containers within a single lakehouse storage account for domain-specific data management.
- Local **Bronze** for domain-specific engineering of domain-local data (not managed by enterprise engineering)
- Data from prod catalogs can be **shared** to other domains.

**Data Catalog**

- A centralized data catalog (unity catalog) serves as a metadata repository for the entire architecture:
- Enables discovery and governance of data.
- Optional external catalog storage.


### Secrets

---
> This section is a work in progress
- Management
- Areas of use
- Handling practices

### Storage
---

#### Lakehouse storage

Lakehouse data for all environments and layers, by default, share a single storage account with LRS or GRS redundancy.
This can then be modified according to costs, requirements, policies, projected workload and resource limits from both Azure and Databricks.

- Resource: ADLSGen2
- Tier: Standard/Premium (depends on workload)
- Redundancy: 
   - Minimum ZRS or GRS for prod
   - Minimum LRS for poc, dev, test and preprod


#### Generic Blob storage

Generic Blob storage can be used for all non-lakehouse data; or alternatively within the lakehouse storage account in the appropriate container and folder.

- Resource: ADLSGen2
- Generic storage account name: sa{organisation_name}{domain_name}{functional_description}
- Tier: Standard/Premium (depends on workload)
- Redundancy: 
   - Minimum ZRS or GRS for prod
   - Minimum LRS for poc, dev, test and preprod

### CICD and Repository
---
> This section is a work in progress
- Description of git workflows for CICD in terms of:
    - Infrastructure
    - Data engineering
    - Analytics engineering
    - Data science / AIML
    - BI, Reports and other products

#### Tools
- Github
- Azure Devops
- Databricks Asset Bundles

#### Repositories

- Infrastructure
- dbt projects (separate for each domain)
    - potential for enterprise level stitching of lineage
- Data engineering code (separate for each domain) using Databricks Asset Bundles

### Observability
---

Various tools can be used to provide insight into different aspects of the architecture:

- dbt observability - Elementary
- Databricks observability - Databricks monitoring dashboards
- ADF - Native adf monitoring

#### dbt observability - Elementary
Elementary is a dbt observability tool available in both Open Source and Cloud Service forms. For more information, visit: <a href="https://docs.elementary-data.com/introduction" target="_blank">Elementary Documentation</a>

<a href="../img/observability.png" target="_blank">
    <img src="../img/observability.png"  alt="Observability Diagram">
</a>
<br>

<a href="https://dlintuitasweb.z8.web.core.windows.net/index.html#/report/dashboard" target="_blank">Example observability dashboard for Intuitas Engineering Domain</a>

Elementary acts as a health monitor and quality checker for dbt projects by automatically tracking, alerting, and reporting on:

- Data freshness: Ensures your data is up to date.
- Volume anomalies: Detects unexpected changes in row counts.
- Schema changes: Monitors additions, deletions, or modifications of columns.
- Test results: Checks if your dbt tests are passing or failing.
- Custom metrics: Allows you to define your own checks.

It leverages dbt artifacts (such as run results and sources) to send alerts to Slack, email, or other tools. Additionally, it can automatically generate reports after dbt runs, enabling early detection of issues without manual intervention.


### Networking
---
By default - all resources reside within the same VNet with private endpoints.

Service endpoints and policies are enabled.

### Orchestration
---

#### Tools

- Azure Data Factory (if needed)
- Databricks Workflows (for both databricks and dbt)

### Security
---

#### Tools

- Azure Entra
- Azure Key Vault
- Unity Catalog 
    - System access reports

<br>




<br>

## Data Architecture
---

Data Architecture refers to how data is physically structured, stored, and accessed within an organization. It encompasses the design and management of data storage systems, data models, data integration processes, and data governance practices. 

### Data and information models
---

Domain-level data and information models are typically closer aligned to real-world business semantics and business rules, which may not necessarily align with the broader enterprise or other domains. 

See [Bounded context](https://martinfowler.com/bliki/BoundedContext.html)

### Domain glossary
---

- Expand on the enterprise glossary and add domain specific terms and definitions.
    - In cases where domain definitions are synonymous with enterprise definitions, the enterprise glossary should be referenced.
    - In cases where definitions are conflicting, governance should be applied to resolve the conflict.

### Domain data and warehouse models
---

- Domain-level data and warehouse models reflect domain-specific scope, requirements and semantics as expressed in models and glossaries.
- Conformed dimensions may serve as a bridge between domains for common entities.

### Data layers and stages 
---

Data and analytics pipelines flow through data layers and stages. Conventions vary across organisations, however the following is an effective approach:

* Within each layer, data is transformed through a series of stages.
* Top level layers follow the [Medallion architecture](https://www.databricks.com/glossary/medallion-architecture).

    - **Bronze: Data according to source.**
    - **Silver: Data according to business.** ([see Data and information models](level_2.md#data-and-information-models))
    - **Gold: Data according to requirements.**

<a href="../img/data_layers_and_stages.png" target="_blank">
    <img src="../img/data_layers_and_stages.png"  alt="Data layers and stages">
</a>

<br>

These map to naming standards and conventions for [Catalog](naming_standards_and_conventions.md#catalog-naming-and-conventions), [Schemas](naming_standards_and_conventions.md#schema-and-object-conventions) and [dbt](naming_standards_and_conventions.md#dbt).

#### **Metadata layer**

Contains engineering and governance of data managed within the platform. The format of this will vary depending on the choice of engineering and governance toolsets and associated metamodels within the solution as well as across the broader enterprise.  [see Enterprise Metadata Architecture](level_1.md#enterprise-metadata-architecture) 


#### **Bronze layer: Data according to source**

The Bronze layer stores raw, immutable data as it is ingested from source systems. The choice of persistence level will depend on requirements.

**(Persistent) Landing**

- Initial storage area for raw data from source systems.
- Stores raw events as JSON or CDC/tabular change records.
- Data is maintained in a primarily raw format, with the possibility of adding extra fields that might be useful later, such as for identifying duplicates. These fields could include the source file name and the load date.

    - Partitioned by load date (YYYY/MM/DD/HH)
    - Raw data preserved in original format
    - Append-only immitable data.
    - Schema changes tracked but not enforced

**ODS (Operational Data Store)**

- Current state of source system data with latest changes applied.
    - Maintains latest version of each record
    - Supports merge operations for change data capture (CDC)
    - Preserves source system relationships

**PDS (Persistent Data Store)**

- Historical storage of all changes over time.
    - Append-only for all changes
    - Supports point-in-time analysis
    - Configurable retention periods
    - As these may be available in landing - may be realised through views over landing


#### **Silver layer: Data according to business**
The Silver layer focuses on transforming raw data into cleaned, enriched, and validated datasets. These datasets are aligned with broadly accepted business standards and models, making them suitable for a range of downstream requirements.

While some interpretations consider Silver to be primarily *source-centric*, this blueprint adopts a more flexible approach—allowing for integration and harmonization of assets across multiple data sources.


**Silver Staging**

Transformations used to shape source data into standardised, conformed, and fit-for-use Reference Data, Data Vault and Base Information Mart objects.

- Examples of transformations:
    - `01_renamed_and_typed`
    - `02_deduped`
    - `03_cleaned`
    - `04_filtered/split`
    - `05_column_selected`
    - `06_business_validated`
    - `07_desensitised`

**Data Quality**

Data quality test results from checks applied to source data. Further transformation of these results may be applied to shape them into data quality reports.

**Reference Data**

Reference data, being a common asset and provided for broad consumption should be aligned to standards and downstream needs. Historical versioning requirements of reference data may need to be considered.

**Raw Vault**

- Optional Data vault 2.0 aligned raw data warehouse.

**Business Vault**

- Optional Business rule applied objects as per Data vault 2.0.

**Base Information Marts**

The term *base* is used to distinguish these marts from the domain- or enterprise-specific marts found in the Gold layer. Base marts are designed for broad usability across multiple downstream use cases—for example, a conformed customer dimension.

In some scenarios, it may be beneficial to maintain *source-centric* base marts alongside a final *consolidation* (UNION) mart—all conforming to a common logical model. This approach supports decoupled pipelining across multiple sources, improving modularity and maintainability.

These marts may be implemented as **Kimball-style** dimensional models or **denormalized** flat tables, depending on performance and reporting requirements. However, dimensional modelling is generally encouraged for its clarity, reusability, and alignment with analytic workloads.

#### **Gold layer: Data according to requirements**

The Gold layer focuses on delivering business-ready datasets, including aggregations and reporting structures that directly reflect specific business requirements.

In some instances, Gold assets may be reused across multiple use cases or domains—blurring the line with Silver. While this is not inherently problematic, it is important to consider supportability and scalability to ensure these assets remain trustworthy, maintainable, and accessible over time. 

Consider shifting logic left into the Silver layer—such as common aggregations, reusable business rules, or conformed dimensions. This improves consistency, reduces duplication, and enables faster development of Gold-layer assets by building on stronger, more standardized foundations.

**Gold Staging**

Transformations used to shape source data into business-ready datasets, aligned to business requirements.

- Examples of Business-specific transformations include:
    - `Pivoting`
    - `Aggregation`
    - `Joining`
    - `Conformance`
    - `Desensitization`

- While dbt best practices use the term *'Intermediates'* as reuseable building blocks for marts, this is considered a form of staging and are hence optional under this blueprint. https://docs.getdbt.com/best-practices/how-we-structure/3-intermediate

**Business Information Marts (Requirement Specific)**
The term 'business' here is use to distinguish marts in this layer from marts in the Silver layer. These marts are designed for a defined requirement. *e.g. sales fact aggregated by region.*

These marts may be Kimball or denormalised flat tables depending on requirements; although Kimball dimensional models are encouraged.

A solution served to the consumption layer is likely to contain a mix of Silver and Gold mart objects. e.g:
- silver.dim_customer
- gold.fact_sales_aggregated_by_region

<br>
<br>

### Lakehouse Catalog to Storage Mapping
---


Unity catalog objects (catalogs, schemas, objects) are mapped to:

- Storage accounts
- Environments (containers: dev, test, prod)
- Layers (Level 1 folders: dev.bronze, dev.silver, dev.gold, etc)
- Stages (Level 2 folders: dev.bronze\landing, dev.bronze\ods, dev.silver\base, dev.silver\staging etc)

<br>

Example:
<br>

<a href="../img/catalog_to_storage_mapping.png">
    <img src="../img/catalog_to_storage_mapping.png" alt="Catalog to storage mapping" width="50%">
</a>

<br>

## Data Engineering
---

### Ingestion
---

Ingestion is the process of acquiring data from external sources and landing it in the platform landing layer.

It should be:
- Scalable, Resilient, Maintainable, Governed
- Pattern-based, automated and Metadata-driven where possible
- Batch and stream-based

<br>
<br>
Example batch ingestion options:
<a href="../img/ingestion_patterns.png" target="_blank">
    <img src="../img/ingestion_patterns.png"  alt="Ingestion patterns">
</a>

#### Ingestion patterns and notes:

- Pattern 1: streaming: kafka -> landing -> databricks autoloader -> ods
    - see repo [Bronze Landing to ODS Project](https://github.com/bensonchoyintuitas/health_lakehouse__engineering__databricks)

- Pattern 2: batch: source -> adf -> landing -> databricks autoloader merge to ods
    - see repo [Bronze landing SQL Server to ODS Project](https://github.com/bensonchoyintuitas/health_lakehouse__engineering__databricks)
    - adf requires azure sql and on-premise integration runtime

    - see repo [External Database to ODS Project](https://github.com/bensonchoyintuitas/health_lakehouse__engineering__databricks)
    - requires network access to source

- Pattern 4: batch/streaming: source -> custom python -> deltalake -> external table

- Pattern 5: sharepoint -> fivetran -> databricks sql warehouse (ods)
    - see repo [fivetran](https://github.com/bensonchoyintuitas/health_lakehouse__engineering__custom)

Rejected patterns:

- batch: adf -> deltalake -> ods (does not support unity catalog, requires target tables to be pre-initialised)
- batch: adf -> databricks sql endpoint -> ods (no linked service for databricks)
- batch: adf + databricks notebook -> landing, ods, pds (more undesireable coupling of adf and databricks an associated risks)

### Transformation
---

> This section is a work in progress
#### Batch and Micro-batch SQL transformation
- dbt [see dbt standards](naming_standards_and_conventions.md#dbt)

#### Streaming SQL transformation
> This section is a work in progress

#### Non SQL transformation
> This section is a work in progress


### Data sharing and delivery patterns
---

Data can be shared and delivered to consumers through various channels, each differing in:

- Cost
- Functionality
- Scalability
- Security
- Maintainability

The following subsections offer more details about the channels depicted below.

<a href="../img/sharing_delivery_visualisation.png" target="_blank">
    <img src="../img/sharing_delivery_visualisation.png"  alt="Sharing and delivery visualisation">
</a>


#### Row Level Security
see [Row Level Security](#row-level-security)


#### Pull / direct access
---

#### **Databricks Delta sharing practices**

- Databricks Delta Sharing allows read-only access directly to data (table, view, change feed) in the lakehouse storage account. This allows for the use of the data in external tools such as BI tools, ETL tools, etc. without the need to use a databricks cluster / sql endpoint. 
-Permissions: Delta sharing is a feature of Databricks Unity Catalog that requires enablement and authorised user/group permissions for the feature as well as the shared object.
- Costs: In Delta Sharing, the cost of compute is generally borne by the data consumer, not the data provider. Other costs include storage API calls and data transfer.
- Naming standards and conventions [see naming standards](naming_standards_and_conventions.md#delta-sharing)
- Tightly scope the share as per the principal of least privilege:
    - Share only the necessary data
    - Single purpose, single recipient
    - Granular access control
    - Set an expiry
- Use audit logging to track access and usage
    ```sql
    SELECT *
    FROM system.access.audit
    WHERE 
    action_name LIKE 'deltaSharing%'
    ORDER BY event_time DESC
    LIMIT 100;
    ```
- Limitations:
    - No Row Level Security and Masking support (dynamic views required)

- Reference: [Security Best Practices for Delta Sharing](https://www.databricks.com/blog/2022/08/01/security-best-practices-for-delta-sharing.html)

#### **ADLSGen2 Access to Data**

- Provide direct ADLSGen2 access via Managed Identity, SAS or Account Key
- Note: While technically possible, ADLSGen2 access is not generally recommended for end user consumption as it bypasses the Unity Catalog and associated governance and observabilit controls.
- Example Scenarios: Direct ADLS file sharing might be preferable in certain cases, even when Delta Sharing is available:
    - Unstructured data
    - Large non-delta file transfer
    - Consumers that don't support Delta Sharing

##### **DuckDB Access to Data (via Unity Catalog)**

- Example: DuckDB is a popular open-source SQL engine that can be used to access data in the lakehouse. It can be run on a local machine or in-process in a Databricks cluster.
- Costs: DuckDB data access will incur costs of the underlying compute, storage access, data transfer, etc., similar to Delta Sharing.
- Example Opportunities/Uses:
    - Last mile analysis
    - SQL interface to Delta, Iceberg, Parquet, CSV, etc.
    - dbt compatibility
    - Local execution and storage of queries and data
    - Use as feed visualization tools, e.g., Apache Superset

- See repo [DuckDB](https://github.com/bensonchoyintuitas/health_lakehouse__engineering__custom)

- Limitations:
    - Unity Catalog not yet supported
    - Delta Kernel not yet supported

#### **SQL Access**
---
- SQL Access is provided by the Databricks SQL (serverless) endpoint.

#### **API Access**
---
> This section is a work in progress
- The Databricks SQL Statement Execution API can be used to execute SQL statements on a SQL warehouse and fetch the result.

References:

 - https://docs.databricks.com/api/workspace/statementexecution
 - https://docs.databricks.com/en/dev-tools/sql-execution-tutorial.html

#### **Snowflake Access**
---
> This section is a work in progress

- Snowflake access is provided by Databricks Delta Sharing.
- Snowflake access is also provided by Databricks Delta Lake external tables over ADLSGen2 [see external tables](https://docs.snowflake.com/en/sql-reference/sql/create-iceberg-table-delta)

References:
- [tutorial](https://harke-harkema.medium.com/reading-delta-tables-in-snowflake-1396d21bf970)



#### **Microsoft Fabric Access**
---

The following describes options for providing access to Microsoft Fabric / PowerBI 

*Option 1. Share via Delta Sharing*

Steps:

1. Create a delta share
2. Use the delta share to get data from within PowerBI

Evaluation:

- Pros: 
    - No duplication
    - Centralised control over access policies
    - Compute costs on consumer 
- Cons: 
    - Row Level Security and Masking support via dynamic views only
    - See [limitations](https://learn.microsoft.com/en-au/azure/databricks/partners/bi/power-bi). e.g. The data that the Delta Sharing connector loads must fit into the memory of your machine. To ensure this, the connector limits the number of imported rows to the Row Limit that was set earlier.

*Option 2. Directlake via ADLSGen2*

Steps:

1. Create a new connection to ADLSGen2 using a provided credential / token / Service principal
2. Create a lakehouse shortcut in Fabric

Evaluation:

- Pros: 
    - No duplication
    - Potentially better PowerBI performance (untested)
    - Compute costs on consumer 
    - No views
- Cons: 
    - Less control over access policies than Delta Sharing (outside of Unity Catalog)
    - Requires granular ADLSGen2 access controls and service principals, and associated management overhead
    - No Row Level Security and Masking support 
    - May require OneLake

*Option 3. Fabric mirrored unity catalog*

Steps:

1. Within a Fabric Workspace, create a new item `Mirrored Azure Databricks Catalog`
2. Enter the Databricks workspace config to create a new connection

Evaluation:

- Pros: 
    - No duplication
    - Convenient access to all Databricks Unity Catalog objects (within credential permissions)
- Cons: 
    - not GA or tested
    - service-principal level identity required to enforce permissions


*Option 4. PowerBI Import Via SQL Endpoint*

Steps:

[Databricks documentation](https://learn.microsoft.com/en-au/azure/databricks/partners/bi/power-bi)

Evaluation:

- Pros: 
    - Potentially the best PowerBI performance and feature completeness
    - Predictable costs on Databricks
- Cons: 
    - Some, but manageable Compute costs on Databricks

*Option 5. PowerBI DirectQuery Via SQL Endpoint*

Steps:

[Databricks documentation](https://learn.microsoft.com/en-au/azure/databricks/partners/bi/power-bi)

Evaluation:

- Pros: 
    - No duplication
    - Unity Catalog Enforced Row Level Security and Masking 

- Cons: 
    - High Compute costs on Databricks on every report interaction
    - Likely deprecated in favour of DirectLake
    - Less feature rich that import mode

*Option 6. Replicate into Fabric*

- Pros:
    - Possibly reduced networking costs (depending on workload and networking topology)

- Cons: 
    - Duplicated data
    - Engineering costs and overheads
    - Latency in data updates (SQL Endpoint lag)
    - Less governance control compared to Unity Catalog
    - No Row Level Security and Masking support 
    - Requires use of Onelake and associated CU consumption

#### Push
---

> This section is a work in progress

For consideration:

- adf
- databricks
- lakeflow

### Visualisation
---
> This section is a work in progress

For consideration:

- Powerbi
- Databricks dashboards
- Apps
- Open source visual options 

<br>

## AI/ML

> This section is a work in progress

For consideration:

- MLOps
- Training
- Databricks
- Azure ML

<br>

## Data governance

This section describes how Enterprise-level governance will be implemented through solutions at the domain level.

### Data lifecycle and asset management
---
> This section is a work in progress

For consideration:

- data contracts and policy
- data asset tagging


### Data access management
---
> This section is a work in progress

For consideration:

- data access request management
- data contracts
- access audit
- activity audit


### Data quality
---
> This section is a work in progress

For consideration:

- data quality checking and reporting
- data standards and quality business rule management


### Data understandability
---
> This section is a work in progress

For consideration:

- data lineage
- data object metadata



### Privacy Preservation 
---
> This section is a work in progress

For consideration:

- row level security
- data masking
- column level security
- data anonymisation
- data de-identification

https://docs.databricks.com/en/tables/row-and-column-filters.html#limitations

"If you want to filter data when you share it using Delta Sharing, you must use dynamic views."

Use dynamic views if you need to apply transformation logic, such as filters and masks, to read-only tables and if it is acceptable for users to refer to the dynamic views using different names.

#### Row Level Security
---
> This section is a work in progress

For consideration:

- dynamic views
- precomputed views

#### Audit
---

> This section is a work in progress

For consideration:

- audit log queries

##### Example questions and associated queries
```
As a Domain (workspace) Admin:

1. Where are there misconfigured catalogs / schemas / objects?
2. Who is sharing what to who and is that permitted (as per access approvals?)
3. Who is accessing data and are they permitted (as per access approvals?)

```



<br>

## Billing

For more context and concepts, refer to the [Enterprise Billing](level_1.md#enterprise-billing) section in Level 1.

