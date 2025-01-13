## Level 2 - Domain-Level (Solution) Architecture and Patterns
[Return to home](README.md)


<a href="images/logical_platform_and_pipeline_reference_architecture.png" target="_blank">
    <img src="images/logical_platform_and_pipeline_reference_architecture.png" width="700" alt="Platform and Pipeline Reference Architecture">
</a>

### 2.1 Information and Data Architecture
### 2.1.1 Domain glossary
### 2.1.2 Domain data and warehouse models

### 2.2 Infrastructure
#### 2.2.1 Environments, Workspaces + Clusters
#### 2.2.2 Secrets
#### 2.2.3 Storage
#### 2.2.4 CICD + Repository
#### 2.2.5 Observability
#### 2.2.6 Networking
#### 2.2.7 Orchestration

### 2.3 Data Engineering
#### 2.3.1 Ingestion
#### 2.3.2 Transformation
- [dbt standards](dbt_standards.md)
#### 2.3.3 Delivery

### 2.4 Data Architecture and Patterns
#### 2.4.1 Data zones and stages 
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

**_Bronze_**

*Landing*

Initial storage area for raw data from source systems.

Data is maintained in a primarily raw format, with the possibility of adding extra fields that might be useful later, such as for identifying duplicates. These fields could include the source file name and the load date.

- Partitioned by load date (YYYY/MM/DD/HH)
- Raw data preserved in original format
- Append-only immitable data.
- Schema changes tracked but not enforced

*ODS (Operational Data Store)*

Current state of source system data with latest changes applied.
- Maintains latest version of each record
- Supports merge operations for change data capture (CDC)
- Preserves source system relationships

*PDS (Persistent Data Store)*

Historical storage of all changes over time.
- Append-only for all changes
- Supports point-in-time analysis
- Configurable retention periods


**_Silver_**

Data from different source systems (aside from reference data) remains source-centric and is generally not combined yet.

*Base Models*

Focus on data cleaning and standardisation from the source system of base objects.
Data is filtered, cleaned, and augmented. This process may involve deduplicating the data, handling missing information, removing incorrect data, or fixing corrupted entries.
Data validation rules are applied, which may include ensuring there are no null values, verifying the uniqueness of data, confirming that data is of the correct type and format, and conducting logical checks.

*Staging Models*

Initial transformations are applied, preparing data for consumption or optional enrichment.
Models represent business processes and entities, abstracted from the data sources that they are based on. Desensitized views may be provided.

*Enrichment Models*

More complex business logic and transformations are applied e.g. common calculations and derivations. By separating enrichment from core staging, we can schedule these processes independently. This allows for flexibility in updating or refreshing only the parts of the data pipeline that need it, reducing unnecessary computation and improving efficiency. It also allows for change and versioning of those business rules with minimal impact on core staging objects.

*Silver EDW Models*
Silver - common Enterprise Data Warehoused models - fit for downstream consumption
e.g Raw Vault, Business Vault, Enterprise and Domain base facts and dimensions.

**_Gold_**

Business-Domain level transformation and aggregation is applied for specific use cases.


*Intermediate Models*
These act as building blocks for marts, transforming and aggregating data further. Then be thought of as mart staging

*Mart Models*
Marts deliver data in a format ready for analysis and reporting. Splitting them by domain (e.g., finance, marketing) helps maintain clarity.

One domain may reference marts from other domains.


#### 2.4.2 Lakehouse Catalog to Storage Mapping

### 2.5 Data access and governance



