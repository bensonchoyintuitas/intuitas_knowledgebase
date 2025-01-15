# Level 2 - Domain-Level (Solution) Architecture and Patterns
[Return to home](README.md)

## Table of Contents
- [2.1 Business architecture](level_2.md#21)
    - [2.1.1 Business processes](level_2.md#211)
    - [2.1.2 Business glossary](level_2.md#212)
    - [2.1.3 Business metrics](level_2.md#213)

- [2.2 Infrastructure](level_2.md#22)
    - [2.2.1 Environments, Workspaces + Clusters](level_2.md#221)
    - [2.2.2 Secrets](level_2.md#222)
    - [2.2.3 Storage](level_2.md#storage)
    - [2.2.4 CICD + Repository](level_2.md#224)
    - [2.2.5 Observability](level_2.md#225)
    - [2.2.6 Networking](level_2.md#226)
    - [2.2.7 Orchestration](level_2.md#227)

- [2.3 Data and information models](level_2.md#23)
    - [2.3.1 Domain glossary](level_2.md#231)
    - [2.3.2 Domain data and warehouse models](level_2.md#232)

- [2.4 Data Architecture](level_2.md#24)
    - [2.4.1 Data zones and stages](level_2.md#241)
    - [2.4.2 Lakehouse Catalog to Storage Mapping](level_2.md#242)

- [2.5 Data Engineering](level_2.md#25)
    - [2.5.1 Ingestion](level_2.md#251)
    - [2.5.2 Transformation](level_2.md#252)
    - [2.5.3 Delivery](level_2.md#253)

- [2.6 Data access and governance](level_2.md#26)


Domain-level solutions are instantiations of the enterprise-level reference architecture. (See [1.2 Enterprise Data Platform Reference Architecture](level_1.md#1.2))

Example reference architecture:

<a href="images/logical_platform_and_pipeline_reference_architecture.png" target="_blank">
    <img src="images/logical_platform_and_pipeline_reference_architecture.png" width="700" alt="Platform and Pipeline Reference Architecture">
</a>

## 2.1 Business architecture
### 2.1.1 Business processes
Business processes are the activities and tasks that are performed to achieve the goals of the business.
Understanding them is necessary to understand:
- the context in which data is captured and used
- concepts and entities that are relevant to the domain
- the relationships between different processes and data


### 2.1.2 Business glossary

A business glossary is a list of terms and definitions that are relevant to the business. see Domain Glossary.

### 2.1.3 Business metrics
Metrics are the measurements of the performance of the business processes. They should be documented according to a defined template that captures, at a minimum, the following:
- name
- definition
- formula (with reference to data elements and definitions in the business glossary)
- dimensions
- source(s)
- metric owner
- frequency

## 2.2 Infrastructure
> This section is a work in progress
### 2.2.1 Environments, Workspaces + Storage
<a href="images/workspaces_environments_storage.png" target="_blank">
    <img src="images/workspaces_environments_storage.png" width="700" alt="Workspaces, Environments and Storage">
</a>
<br>
<br>

This diagram illustrates a **data lakehouse architecture** with the following components and flow:


#### **1. Data Sources**
- Data originates from multiple sources such as:
  - Databases
  - Kafka or event streaming
  - APIs or Python scripts
  - SharePoint (or similar sources)


#### **2. Enterprise Engineering Layer**
- Centralized enterprise workspaces are managed here with multiple environments. 
While work can be achieved within a single workspace and lakehouse storage account, decoupling the workspaces and storage accounts allow for more isolated change at the infrastructure level - in line with engineering requirements:
- Each workspace contains:
- Data from prod catalogs can be **shared** to other domains.


#### **3. Domain-Specific Layer**
- Each domain (e.g., business units or specific applications) operates independently within a single workspace that houses multiple environments. **PROD**, **TEST**, and **DEV** storage containers within a single lakehouse storage account for domain-specific data management.
  - Local **Bronze** for domain-specific engineering of domain-local data (not managed by enterprise engineering)
- Data from prod catalogs can be **shared** to other domains.

#### **4. Data Catalog**
- A centralized data catalog (unity catalog) serves as a metadata repository for the entire architecture:
  - Enables discovery and governance of data.
- Optional external catalog storage.


---

### 2.2.2 Secrets
> This section is a work in progress
- Management
- Areas of use


### Storage

#### Lakehouse storage

Lakehouse data for all environments and zones, by default, share a single storage account with LRS or GRS redundancy.
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


### 2.2.4 CICD + Repository
> This section is a work in progress

#### Tools
- Github
- Azure Devops

#### Repositories

- Infrastructure
- dbt projects (separate for each domain)
- Data engineering code (separate for each domain) using Databricks Asset Bundles

### 2.2.5 Observability

#### Tools

- dbt observability - Elementary
- Databricks observability - Databricks monitoring dashboards
- ADF - Native adf monitoring

### 2.2.6 Networking

By default - all resources reside within the same VNet with private endpoints.

Service endpoints and policies are enabled.

### 2.2.7 Orchestration

#### Tools

- Azure Data Factory (if needed)
- Databricks Workflows (for both databricks and dbt)


### 2.2.8 Security

#### Tools

- Azure Entra
- Azure Key Vault
- Unity Catalog 
    - System access reports

## 2.3 Data and information models

Domain-level data and information models are typically closer aligned to real-world business semantics and business rules, which may not necessarily align with the broader enterprise or other domains. 

See [Bounded context](https://martinfowler.com/bliki/BoundedContext.html)

### 2.3.1 Domain glossary
- Expand on the enterprise glossary and add domain specific terms and definitions.
    - In cases where domain definitions are synonymous with enterprise definitions, the enterprise glossary should be referenced.
    - In cases where definitions are conflicting, governance should be applied to resolve the conflict.

### 2.3.2 Domain data and warehouse models
- Domain-level data and warehouse models reflect domain-specific scope, requirements and semantics as expressed in models and glossaries.
- Conformed dimensions may serve as a bridge between domains for common entities.



## 2.4 Data Architecture

### 2.4.1 Data zones and stages 

Data and analytics pipelines flow through data zones and stages. Conventions vary across organisations, however the following is an effective approach:

* Top level zones follow the [Medallion architecture](https://www.databricks.com/glossary/medallion-architecture).
* Within each zone, data is transformed through a series of stages.
<br>
<br>
<a href="images/data_zones_and_stages.png" target="_blank">
    <img src="images/data_zones_and_stages.png" width="700" alt="Data zones and stages">
</a>
<br>
<br>

#### **Metadata**

Contains metadata that supports engineering and governance. This will vary depending on engineering and governance toolsets


#### **Bronze**

The Bronze layer stores raw, immutable data as it is ingested from source systems.

*(Persistent) Landing*

- Initial storage area for raw data from source systems.
- Stores raw events as JSON or CDC/tabular change records.
- Data is maintained in a primarily raw format, with the possibility of adding extra fields that might be useful later, such as for identifying duplicates. These fields could include the source file name and the load date.

    - Partitioned by load date (YYYY/MM/DD/HH)
    - Raw data preserved in original format
    - Append-only immitable data.
    - Schema changes tracked but not enforced

*ODS (Operational Data Store)*

- Current state of source system data with latest changes applied.
    - Maintains latest version of each record
    - Supports merge operations for change data capture (CDC)
    - Preserves source system relationships

*PDS (Persistent Data Store)*

- Historical storage of all changes over time.
    - Append-only for all changes
    - Supports point-in-time analysis
    - Configurable retention periods
    - As these may be available in landing - may be realised through views over landing


#### **Silver**
The Silver layer is source centric and focuses on transforming raw data into cleaned, enriched, and validated datasets.

*Base Models*

- Representation of source data with no changes. Used as a foundation for staging models as well as data quality checks.


*Staging Models*

- Source-system and object centric transformations that are core to all downstream consumption.

- Examples of transformations:
    - `01_renamed_and_typed`
    - `02_deduped`
    - `03_cleaned`
    - `04_filtered/split`
    - `05_column_selected`
    - `06_business_validated`
    - `07_desensitised`

*Enrichment Models*

Still source-centric, however:
- more complex business logic and transformations are applied e.g. common calculations and derivations. 
- may combine multiple staging objects from the same source

- By separating enrichment from core staging, we can schedule these processes independently. This allows for flexibility in updating or refreshing only the parts of the data pipeline that need it, reducing unnecessary computation and improving efficiency. It also allows for change and versioning of those business rules with minimal impact on core staging objects.

*Source Reference Data*

- For convenience, reference data specific to the source can be segregated here and aligned to standards and downstream needs.

*Raw Vault*

- Data vault 2.0 aligned raw data warehouse.

#### **Gold**

The Gold layer focuses on business-ready datasets, aggregations, and reporting structures.

*Business Vault*

- Data vault 2.0 aligned business data warehouse where business rules and transformations are applied.

*Intermediate Models*

- These act as building blocks for marts, transforming and aggregating data further. Then be thought of as mart staging  https://docs.getdbt.com/best-practices/how-we-structure/3-intermediate

- Business-specific transformations such as:
    - Pivoting
    - Aggregation
    - Joining
    - Funnel creation
    - Conformance
    - Desensitization

*Enterprise Reference Data*

- Reference data, independent of source can be aggregated here for broad consumption.

*Marts - Facts and dimensions*

- Kimball style marts that represent business entities and processes. They may 
    * serve foundational or narrow requirements. 
    * be scoped to specific systems or conformed across the enterprise

*Marts - Denormalised*

- Single table / view objects that combine data from multiple objects (e.g. facts and dimensions) 


### 2.4.2 Lakehouse Catalog to Storage Mapping

Unity catalog objects (catalogs, schemas, objects) are mapped to:
- Storage accounts
- Environments (containers: dev, test, prod)
- Zones (Level 1 folders: dev.bronze, dev.silver, dev.gold, etc)
- Stages (Level 2 folders: dev.bronze\landing, dev.bronze\ods, dev.silver\base, dev.silver\staging etc)

<br>
<br>
Example:
<a href="images/catalog_to_storage_mapping.png" target="_blank">
    <img src="images/catalog_to_storage_mapping.png" width="700" alt="Catalog to storage mapping">
</a>
<br>
<br>

## 2.5 Data Engineering
### 2.5.1 Ingestion


> This section is a work in progress


Ingestion is the process of acquiring data from external sources and landing it in the platform landing zone.

It should be:
- Scalable, Resilient, Maintainable, Governed
- Pattern-based, automated and Metadata-driven where possible
- Batch and stream-based

<br>
<br>
Example batch ingestion options:
<a href="images/ingestion_patterns.png" target="_blank">
    <img src="images/ingestion_patterns.png" width="700" alt="Ingestion patterns">
</a>
<br>
<br>

#### Pattern specific notes:

Pattern 1: SQL Server source -> ADF -> Landing | Databricks Autoloader merge to ODS


Rejected patterns:
1. ADF -> Deltalake (does not support unity catalog)


### 2.5.2 Transformation
> This section is a work in progress
- [dbt standards](dbt_standards.md)
### 2.5.3 Delivery
> This section is a work in progress

### 2.6 Data access and governance
> This section is a work in progress


