# Standards and Conventions
[Return to home](README.md)

These standards are opinionated and designed to ensure consistency, governance, and automation across an organisation. They largely reflect an Azure Databricks environment, however can be adapted to other platforms. Organisations should adapt these standards to fit their existing internal conventions.
<br>

## Table of Contents
---


   - [Mesh](standards_and_conventions.md#mesh)
      - [Domain Names](standards_and_conventions.md#domain-names)
   - [Platform](standards_and_conventions.md#platform)
      - [Environment](standards_and_conventions.md#environment)
      - [VNET](standards_and_conventions.md#vnet)
      - [Resource Groups](standards_and_conventions.md#resource-groups)
      - [Databricks workspace](standards_and_conventions.md#databricks-workspace)
      - [Key vault](standards_and_conventions.md#key-vault)
      - [Secrets](standards_and_conventions.md#secrets)
      - [Entra Group Names](standards_and_conventions.md#entra-group-names)
      - [Azure Data Factory (standards_and_conventions.mdADF)](standards_and_conventions.md#azure-data-factory-adf)
      - [SQL Server](standards_and_conventions.md#sql-server)
      - [SQL Database](standards_and_conventions.md#sql-database)
      - [Storage](standards_and_conventions.md#storage)
         - [Lakehouse Storage](standards_and_conventions.md#lakehouse-storage)
         - [Lakehouse Storage Containers](standards_and_conventions.md#lakehouse-storage-containers)
         - [Lakehouse Storage Folders](standards_and_conventions.md#lakehouse-storage-folders)
         - [Generic Blob Storage](standards_and_conventions.md#generic-blob-storage)
         - [Generic Blob Files and Folders](standards_and_conventions.md#generic-blob-files-and-folders)
   - [Databricks](standards_and_conventions.md#databricks)
      - [Workspace and Cluster Names](standards_and_conventions.md#workspace-and-cluster-names)
      - [Catalog](standards_and_conventions.md#catalog-naming-and-conventions)
      - [Schema and Object Conventions](standards_and_conventions.md#schema-and-object-conventions)
      - [Delta Sharing](standards_and_conventions.md#delta-sharing)
   - [Azure Data Factory](standards_and_conventions.md#azure-data-factory)
   - [Streaming](standards_and_conventions.md#streaming)
   - [dbt](standards_and_conventions.md#dbt)
      - [Documentation and Model Metadata](standards_and_conventions.md#documentation-and-model-metadata)
      - [Sources](standards_and_conventions.md#sources)
      - [Model and Folder Names](standards_and_conventions.md#model-and-folder-names)
      - [dbt_project.yml](standards_and_conventions.md#dbt_projectyml)
   - [CI/CD](standards_and_conventions.md#cicd)
      - [Repository Naming](standards_and_conventions.md#repository-naming)
      - [Branch Naming](standards_and_conventions.md#branch-naming)
      - [Branch Lifecycle](standards_and_conventions.md#branch-lifecycle)
      - [Databricks Asset Bundles](standards_and_conventions.md#databricks-asset-bundles)
   - [Security](standards_and_conventions.md#security)
      - [Entra](standards_and_conventions.md#entra)
      - [Policies](standards_and_conventions.md#policies)
      - [Frameworks](standards_and_conventions.md#frameworks)
<br>
<br>

## Mesh

### Domain Names

All lower case: `{optional:organisation_}{functional area/domain}_{subdomain}`

   *e.g: intuitas_corporate*

<br>

## Platform

### Environment

- Environment name: dev/test/prod/sandbox/poc (pat - production acceptance testing is optional as prepred)


### VNET

- Name: `vn-{organisation_name}-{domain_name}`
= *e.g: vn-intuitas-corporate*

### Resource Groups

- Name: `rg-{organisation_name}-{domain_name}`
- *e.g: rg-intuitas-corporate*

### Databricks workspace

- Name: `ws-{organisation_name}-{domain_name}`
- *e.g: ws-intuitas-corporate*

### Key vault

- Name: `kv-{organisation_name}-{domain_name}`
- *e.g: kv-intuitas-corporate*

### Secrets

- Name: `{secret_name}`

### Entra Group Names

- Name: `eg-{organisation_name}-{domain_name}`
= *e.g: eg-intuitas-corporate*


### Azure Data Factory (ADF)

- Name: `adf-{organisation_name}-{domain_name}`
- *e.g: adf-intuitas-corporate*

### SQL Server

- Name: `sql-{organisation_name}-{domain_name}`
- *e.g: sql-intuitas-corporate*

### SQL Database

- Name: `sqldb-{purpose}-{organisation_name}-{domain_name}-{optional:environment}`
- *e.g: sqldb-metadata-intuitas-corporate*


### Storage

The section describes naming standards and conventions for cloud storage resources.

#### Lakehouse storage

- Lakehouse storage account name: `dl{organisation_name}{domain_name}`

#### Lakehouse storage containers
- Name: `{environment} (dev/test/preprod/prod)`

##### Lakehouse storage folders
- Level 1 Name: `{layer} (bronze/silver/gold)` // if using medallion approach
- Level 2 Name: `{stage_name}`
- *e.g:*

   - *bronze/landing*
   - *bronze/ods*
   - *bronze/pds*
   - *bronze/schema* (for autoloader metadata)
   - *bronze/checkpoints* (for autoloader metadata)
   - *silver/automatically determined by unity catalog*
   - *gold/automatically determined by unity catalog*

#### Generic Blob storage

Generic Blob storage can be used for all non-lakehouse data; or alternatively within the lakehouse storage account in the appropriate container and folder.

- Resource: ADLSGen2
- Generic storage account name: `sa{organisation_name}{domain_name}{functional_description}`
- Tier: Standard/Premium (depends on workload)
- Redundancy: 

   - Minimum ZRS or GRS for prod
   - Minimum LRS for poc, dev, test and preprod

#### Generic Blob files and folders

No standard naming conventions for files and folders.


<br>

## Databricks
---
This section provides naming standards and conventions for Databricks.



### Workspace and cluster names
---

- Workspace name: `ws-{organisation_name}_{domain_name}`
- Cluster name: `{personal_name/shared_name} Cluster`
- Workflow name: `{dev/test} {workflow_name}`


### Jobs and Pipelines

#### Job names
---

- Job names: `{domain}__{layer}__{purpose}__{source}{optional: __target}{optional: __schedule}{optional: __version}__{env}`
- *e.g. clinical__bronze__ingest__fhircdr__dev*

#### For Delta Live Table (DLT) Pipelines
---
- Pipeline names: `{domain_name}__{layer}__pipeline__{dataset}{optional: __schedule}{optional: __version}__{env}`
- *e.g. clinical__bronze__pipeline__fhircdr__dev*
- *e.g. supplychain__gold__pipeline__inventorymart__prod* 

- Note on {layer}: If the 'business outcome' is Gold, you call it Gold, even if it produces Bronze + Silver on the way.
*i.e "This is the production DLT pipeline in the supply chain domain, which builds and maintains the curated gold-layer dataset called Inventory Mart"*

- Include pipeline so it’s distinguishable from ad hoc jobs.
- Dataset can be a logical grouping (e.g., patient, encounter, claims).

#### Orchestration job names
---

- Orchestration job names: `{domain}__orchestration__{workflow-name}{optional: __schedule}{optional: __version}__{env}`
- *e.g. clinical__orchestrate__fhirworkflow__daily__dev*

which then orchestrates: 

- *clinical__bronze__pipeline__fhircdr__prod*
- *clinical__silver__pipeline__fhirclean__prod*
- *clinical__gold__pipeline__clinicalmart__prod*

#### Job logging

- event log catalog: `{domain}__audit__{env}`
- event log schema: `audit__event_log`


#### Optional
- Versioning (if needed): add v1, v2 if a job is redesigned but old one stays around.
- Scheduling frequency (optional): suffix with _hourly, _daily, _weekly if relevant.

### Catalog naming and conventions
---

Refer to [Data layers and stages](level_2.md#data-layers-and-stages) for further context and definitions applicable to this section.

Catalog name: 

The choice of granularity depends on domain topology, stage/zone convention and desired level of segregation for access and sharing controls (i.e. catalog or schema level)

   - Minimum granularity (domain level): `{domain_name}{_environment (dev/test/pat/prod)}` (prod is implied optional)   *e.g: intuitas_corporate_dev*
   - Optional granularity (domain-data stage level): `{domain_name}{_data_stage: (bronze/silver/gold)}{_environment (dev/test/pat/prod)}`    *e.g: intuitas_corporate_bronze_dev*
   - Optional granularity (domain-data stage and zone level): `{domain_name}{_data_stage: (bronze/silver/gold)}{_data_zone: (ods/pds/edw/im)}{_environment (dev/test/pat/prod)}`    *e.g: intuitas_corporate_bronze_ods_dev*
   - Optional granularity (domain-data zone level): `{domain_name}{_data_stage: (bronze/silver/gold)}{_data_zone: (ods/pds/edw/im)}{_environment (dev/test/pat/prod)}`    *e.g: intuitas_corporate_ods_dev*
   - Optional granularity (subdomain-data stage level): `{domain_name}{_descriptor (subdomain/subject/project*)}(bronze/silver/gold)}{_environment (dev/test/pat/prod)}`    *e.g: intuitas_corporate_finance_bronze_dev*

In the examples provided - we have opted for domain level - with schema separation for the lower levels of grain via prefixes. i.e `intuitas_engineering_dev.bronze__ods__fhirhouse__dbo__lakeflow`

*Note that projects are temporary constructs, and hence are not recommended for naming*

- Catalog storage root: `abfss://{environment}@dl{organisation_name}{domain_name}.dfs.core.windows.net/{domain_name}_{environment}_catalog`

### Externally mounted (lakehouse federation) Catalog Names

- Foreign Catalog name: `{domain_name (owner)} _fc__{source_system}{optional:__other_useful_descriptors e.g:_environment}`
- *e.g: intuitas_corporate_fc__sqlonpremsource*

### Catalog Metadata tags:

The following metadata should be added when creating a catalog:

- Key: domain (owner): `{domain_name}`
- Key: environment: `{environment}`
- Key: managing_domain: `{domain_name}` e.g: if delegating to engineering domain

### Schema and object conventions
---

Refer to [Data layers and stages](level_2.md#data-layers-and-stages) for further context and definitions applicable to this section.

Refer to [Physical Models](modelling_standards_and_conventions.md#physical-models) for the standard relating to column naming, types, and conventions.

#### Schema level external storage locations

Recommendations:

- For managed tables (default): do nothing.  Let dbt create schemas without additional configuration. Databricks will manage storage and metadata.Objects will then be stored in the catalog storage root. *e.g: abfss://dev@dlintutiasengineering.dfs.core.windows.net/intuitas_engineering_dev_catalog/__unitystorage/catalogs/catalog-guid/tables/object-guid*
- For granular control over schema-level storage locations: Pre-create schemas with LOCATION mapped to external paths or configure the catalog-level location.
- Ensure dbt's dbt_project.yml and environment variables align with storage locations.

#### Metadata Schemas and Objects

Refer to [Data layers and stages](level_2.md#data-layers-and-stages) for further context and definitions applicable to this section.

Contains metadata that supports engineering and governance. This will vary depending on engineering and governance toolsets

**Engineering - ingestion framework**:

- Schema naming convention:  `meta__{optional: function}`
- Naming convention: `{function/descriptor}`
- *e.g: intuitas_corporate_dev.meta__ingestion.ingestion_control*

#### Bronze (Raw data according to systems)

The Bronze layer stores raw, immutable data as it is ingested from source systems. See [Data layers and stages](level_2.md#data-layers-and-stages) for definitions and context.

All schemas  may be optionally prefixed with data stage if not already decomposed at domain-level i.e. `bronze__`

In the examples provided - we have opted for domain level catalogs - with schema separation for the lower levels of grain via prefixes. i.e `intuitas_engineering_dev.bronze__ods__fhirhouse__dbo__lakeflow`

**Persistent Landing**:

Persistent Landing uses Unity Catalog Volumes for storing raw files and unstructured data as they arrive from source systems.

- Volume naming convention: `[domain][__env].[layer][__source_system][__source_schema](channel).[source_object/volume_name]`
  - `[domain][__env]`: Domain and environment identifier (e.g., `corporate_dev`, `engineering_prod`)
  - `[layer]`: Data layer identifier (e.g., `landing`)
  - `[__source_system]`: Source system identifier (e.g., `workdayapi`, `saphr`)
  - `[__source_schema]`: Optional source schema or subsystem identifier
  - `(channel)`: Ingestion channel/method (e.g., `adf`, `fivetran`, `api`)
  - `[source_object/volume_name]`: Descriptive volume name or source object identifier
- *e.g: corporate_dev.landing__workdayapi.schedule_volume*
- *e.g: engineering_prod.landing__fhirapi__patients.raw_data*

**Operational Data Store (ODS)**:

The objective of raw layer conventions is to provide clarity over which zone and stage it belongs, what the data relates to, where it was sourced from, and via what channel it arrived (as there may be nuances in data depending on its channel).

ODS can be replicated from source systems, or prepared for use from semi/unstructured data via hard-transformation and hence will have these associated conventions:

Database replicated ODS (structured sources like SQL Server):
- Format: `[domain][__env].[layer][__source_system][__source_schema](channel).[source_object/volume_name]`
  - `[domain][__env]`: Domain and environment (e.g., `clinical__dev`)
  - `[layer]`: Data layer (e.g., `ods`)
  - `[__source_system]`: Source system identifier
  - `[__source_schema]`: Source schema (if applicable, e.g., `dbo`, `reporting`)
  - `(channel)`: Optional ingestion channel (e.g., `adf`, `fivetran`, `lakeflow`)
  - `[source_object/volume_name]`: Table name as per source
- *e.g: clinical__dev.ods__patientflowmanager01__reporting.patients*
- *e.g: clinical__dev.ods__fhirhouse__dbo(adf).encounter*

Prepped semi/unstructured ODS data:
- Format: `[domain][__env].[layer][__source_system][__source_descriptor](channel).[source_object/volume_name]`
  - `[domain][__env]`: Domain and environment (e.g., `clinical__dev`)
  - `[layer]`: Data layer (e.g., `ods`)
  - `[__source_system]`: Source system identifier
  - `[__source_descriptor]`: Source descriptor or subsystem identifier
  - `(channel)`: Optional ingestion channel (e.g., `kafka`, `databricks`, `api`)
  - `[source_object/volume_name]`: Named as per source or unique assigned name (e.g., topic/folder name)
- *e.g: clinical__dev.ods__ambosim__confluent(kafka).encounter*
- *e.g: corporate__dev.ods__workdayapi__employees.raw_data*


**Persistent Data Store (PDS)**:

PDS conventions will mirror ODS conventions:

Database replicated PDS (structured sources like SQL Server):
- Format: `[domain][__env].[layer][__source_system][__source_schema](channel).[source_object/volume_name]`
  - `[domain][__env]`: Domain and environment (e.g., `clinical__dev`)
  - `[layer]`: Data layer (e.g., `pds`)
  - `[__source_system]`: Source system identifier
  - `[__source_schema]`: Source schema (if applicable, e.g., `dbo`, `reporting`)
  - `(channel)`: Optional ingestion channel (e.g., `adf`, `fivetran`, `lakeflow`)
  - `[source_object/volume_name]`: Table name as per source
- *e.g: clinical__dev.pds__patientflowmanager01__reporting.patients*
- *e.g: clinical__dev.pds__fhirhouse__dbo(adf).encounter*

Prepped semi/unstructured PDS data:
- Format: `[domain][__env].[layer][__source_system][__source_descriptor](channel).[source_object/volume_name]`
  - `[domain][__env]`: Domain and environment (e.g., `clinical__dev`)
  - `[layer]`: Data layer (e.g., `pds`)
  - `[__source_system]`: Source system identifier
  - `[__source_descriptor]`: Source descriptor or subsystem identifier
  - `(channel)`: Optional ingestion channel (e.g., `kafka`, `databricks`, `api`)
  - `[source_object/volume_name]`: Named as per source or unique assigned name (e.g., topic/folder name)
- *e.g: clinical__dev.pds__ambosim__confluent(kafka).encounter*
- *e.g: corporate__dev.pds__workdayapi__employees.raw_data*

#### Silver/EDW (Data according to business entities)

The Silver layer focuses on transforming raw data into cleaned, enriched, and validated datasets that are the building blocks for downstream consumption and analysis.

Refer to [Data layers and stages](level_2.md#data-layers-and-stages) for further context and definitions applicable to this section.

These marts are objects that are aligned to business entities and broad requirements, hence they must contain source-specific objects at the lowest grain. There may be further enrichment and joins applied across sources.

In the examples provided - we have opted for domain level catalogs - with schema separation for the lower levels of grain via prefixes. i.e `intuitas_engineering_dev.silver__mart`

- All schemas  may be optionally prefixed with data stage if not already decomposed at domain-level i.e. `silver__`
- All `entity` names which align to facts should be named in plural.
- All `entity` names which align to dims should be named in singular.

<br>

**(Silver)/EDW Staging Objects**:
Staging models serve as intermediary models that transform source data into the target silver model. According to dbt best practices, there is a distinction between Staging and Intermediate models. Under this blueprint the use of Intermediate models is optional. [Reference](https://docs.getdbt.com/best-practices/how-we-structure/1-guide-overview)
   
These models exist to stage silver marts (base marts in the EDW layer) and reference data (in the REF layer).

**Naming Convention:**

`[domain][__env] . [layer] . [object_type][_entity/descriptor](__source)(__channel)(__transformation)`

Where:
- `[domain][__env]` - Domain and environment (e.g., `corporate__prod`, `corporate__dev`)
- `[layer]` - Data layer (e.g., `ref`, `edw`)
- `[object_type]` - Object type prefix (e.g., `stg_`, `ref_`, `dim_`, `fact_`, `keys_`)
- `[_entity/descriptor]` - Entity or object name
- `(__source)` - Optional source system identifier (e.g., `__sap`, `__workday`, `__finance_system`)
- `(__channel)` - Optional source channel (e.g., `__adf`, `__api`)
- `(__transformation)` - Optional transformation step (e.g., `__01_typed`, `__02_cleaned`)

**Reference Data Staging:**

- Source-specific staging (with transformations):
   - *e.g: `corporate__prod.ref.stg_facility__sap__01_filtered`*
   - *e.g: `corporate__prod.ref.stg_facility__sap__02_typed`*
   - *e.g: `corporate__prod.ref.stg_account_code__finance_system__adf__01_renamed`*
   - *e.g: `corporate__prod.ref.stg_location__workday__01_typed`*

- Final reference data (cleaned and conformed):
   - *e.g: `corporate__prod.ref.ref_facility`*
   - *e.g: `corporate__prod.ref.ref_location`*
   - *e.g: `corporate__prod.ref.ref_account_code`*
   - *e.g: `corporate__dev.ref.ref_country_codes`*

**Base Mart (EDW) Staging:**

- Source-specific staging (with transformations):
   - *e.g: `corporate__prod.edw.stg_employee__sap__01_typed`*
   - *e.g: `corporate__prod.edw.stg_employee__workday__01_typed`*
   - *e.g: `corporate__prod.edw.stg_employee__workday__02_cleaned`*
   - *e.g: `corporate__prod.edw.stg_customer__crm__adf__01_renamed`*
   - *e.g: `corporate__prod.edw.stg_customer__crm__adf__02_cleaned`*
   - *e.g: `corporate__prod.edw.stg_payment__new_finance_system__adf__01_typed`*
   - *e.g: `corporate__prod.edw.stg_payment__old_finance_system__adf__01_typed`*
   - *e.g: `corporate__prod.edw.stg_account__finance_system__api__01_renamed`*

- Non-source specific staging (after combining/deduplicating):
   - *e.g: `corporate__prod.edw.stg_employee__01_deduped`*
   - *e.g: `corporate__prod.edw.stg_employee__02_validated`*
   - *e.g: `corporate__prod.edw.stg_payment__01_unified`*

**Base Mart Keysets:**

- Conforming/deduplicating keysets across multiple sources:
   - *e.g: `corporate__prod.edw.keys_employee`* (conforms sap and workday)
   - *e.g: `corporate__prod.edw.keys_customer`* (conforms multiple crm sources)
   - *e.g: `corporate__prod.edw.keys_account`* (conforms finance systems)
   - *e.g: `health__prod.edw.keys_patient`* (conforms clinical systems)

**Base Marts (Final Dimensional Models):**

- Source-specific dimensions (preserving source identity):
   - *e.g: `corporate__prod.edw.dim_employee__sap`*
   - *e.g: `corporate__prod.edw.dim_employee__workday`*
   - *e.g: `corporate__prod.edw.dim_account__old_finance_system`*
   - *e.g: `corporate__prod.edw.dim_account__new_finance_system`*

- Conformed dimensions (unified, non-source specific):
   - *e.g: `corporate__prod.edw.dim_employee`* (type 1 SCD implied)
   - *e.g: `corporate__prod.edw.dim_employee__type2`* (with history)
   - *e.g: `corporate__prod.edw.dim_customer`*
   - *e.g: `corporate__prod.edw.dim_account`* (unified across finance systems)

- Fact tables:
   - *e.g: `corporate__prod.edw.fact_payment`*
   - *e.g: `corporate__prod.edw.fact_transaction`*
   - *e.g: `health__prod.edw.fact_encounter`*

**Common Transformation Suffixes:**

   - `__01_renamed` or `__01_typed`
   - `__02_cleaned`
   - `__03_deduped`
   - `__04_filtered`
   - `__05_split`
   - `__06_validated`
   - `__07_desensitised`

**Additional Notes:**

For further context on reference data modeling, refer to [Reference Data Standards and Conventions](modelling_standards_and_conventions.md#reference-data).

<br>

**Raw Vault**:
Optional warehousing construct.

- Schema naming convention: `edw_rv`
- Object naming convention: `{vault object named as per data vault standards}`
- *e.g: intuitas_corporate_dev.edw_rv.hs_payments__finance_system__adf*

<br>

**Business Vault**:
Optional warehousing construct.

- Schema naming convention: `edw_bv`
- Object naming convention: `{vault object named as per data vault standards}`
- *e.g: intuitas_corporate_dev.edw_bv.hs_late_payments__finance_system__adf*

<br>

#### Gold (Data according to requirements)

The Gold layer focuses on requirement-aligned products (datasets, aggregations, and reporting structures). Products are predominantly source agnostic, however optionality exists when source-specific views are needed.

Refer to [Data layers and stages](level_2.md#data-layers-and-stages) for further context and definitions applicable to this section.

**Naming Convention:**

`[domain][__env] . mart . [object_type][product_name/descriptor](__source)(__transformation)`

Where:
- `[domain][__env]` - Domain and environment (e.g., `corporate__prod`, `health__dev`)
- `mart` - Gold layer schema (product marts)
- `[object_type]` - Object type prefix (e.g., `stg_`, `fact_`, `dim_`, `obt_`)
- `[product_name/descriptor]` - Product or business-aligned name
- `(__source)` - Optional source system identifier (only when source-specific view is needed)
- `(__transformation)` - Optional transformation step (for staging only)

**Naming Guidelines:**
- All `entity` names which align to facts should be named in plural
- All `entity` names which align to dimensions should be named in singular
- Product marts are business/requirement-aligned, not technical constructs

<br>

**(Gold)/Product Mart Staging Models**:

Staging models serve as intermediary models that transform base marts (EDW) into requirement-aligned product marts. According to dbt best practices, there is a distinction between Staging and Intermediate models. Under this blueprint the use of Intermediate models is optional. [Reference](https://docs.getdbt.com/best-practices/how-we-structure/1-guide-overview)

These models exist to stage gold product marts with business-specific transformations:

**Common Transformations:**
- Pivoting
- Aggregation
- Joining across multiple base marts
- Business rule application
- Desensitization
- Complex calculations

**Examples:**

- *e.g: `corporate__prod.mart.stg_downtime_by_region__01_pivoted`*
- *e.g: `corporate__prod.mart.stg_downtime_by_region__02_desensitised`*
- *e.g: `corporate__prod.mart.stg_late_payments__01_aggregated`*
- *e.g: `corporate__prod.mart.stg_late_payments__02_joined`*
- *e.g: `corporate__prod.mart.stg_fte_calculations__01_pivoted`*
- *e.g: `corporate__prod.mart.stg_fte_calculations__02_aggregated`*
- *e.g: `health__prod.mart.stg_patient_outcomes__01_joined`*
- *e.g: `health__prod.mart.stg_patient_outcomes__02_desensitised`*

<br>

**(Gold)/Product Marts (Final Products)**:

Final business-aligned data products ready for consumption by analytics, reporting, and BI tools.

**Examples:**

- Fact tables (aggregated/business-focused):
   - *e.g: `corporate__prod.mart.fact_downtime_by_region`*
   - *e.g: `corporate__prod.mart.fact_late_payments`*
   - *e.g: `corporate__prod.mart.fact_regional_account_payments`*
   - *e.g: `health__prod.mart.fact_patient_outcomes`*

- Dimension tables (product-specific):
   - *e.g: `corporate__prod.mart.dim_region`*
   - *e.g: `corporate__prod.mart.dim_payment_category`*

- One Big Tables (OBT) - Denormalized aggregates:
   - *e.g: `corporate__prod.mart.obt_fte_calculations`*
   - *e.g: `corporate__prod.mart.obt_financial_summary`*
   - *e.g: `corporate__prod.mart.obt_executive_dashboard`*

- Source-specific products (when needed):
   - *e.g: `corporate__prod.mart.fact_account_payments__old_finance_system`*
   - *e.g: `corporate__prod.mart.fact_account_payments__new_finance_system`*
   - *e.g: `corporate__prod.mart.obt_employee_metrics__sap`*
   - *e.g: `corporate__prod.mart.obt_employee_metrics__workday`*

### Delta Sharing

- Share names: {domain_name}__{optional:subdomain_name}__{optional:purpose}__{schema_name or description}__{object_name or description}__{share_purpose and or target_audience} *e.g: intuitas_corporate__finance__reporting__account_payments__payments*


<br>

## Azure Data Factory

- Linked service names: ls_{database_name}(if not in database_name:{_organisation_name}_{domain_name}) *e.g: ls_financedb_intuitas_corporate*
- Dataset names: ds_{database_name}_{object_name}
- Pipeline names: pl_{description: e.g copy_{source_name}_to_{destination_name}}
- Trigger names: tr_{pipeline_name}_{optional:start_time / frequency}


<br>  

## Streaming

- Cluster name: `{domain_name}__cluster__{optional:environment}`
- Topic names: `{domain_name}__{object/entity?}__{optional:source_system}___{optional:source_channel}__{optional:environment}`
- Consumer group names: `{domain_name}__{unique_group_name}__{optional:environment}`



<br>  

## dbt

The following standards and conventions relate to dbt projects.

### Documentation and model metadata

Within each respective model folder (as needed)

- md: _{path to model folder using _ separators}__docs.md 
- *e.g: models/silver/ambo_sim__kafka__local/_silver__ambo_sim__kafka__local__docs.md*

- model yml: _{path to model folder using _ separators}__models.yml 
- *e.g: models/silver/ambo_sim__kafka__local/_silver__ambo_sim__kafka__local__models.yml*

### Sources

- Folder: models/sources/{bronze/silver/gold}
- yml: {schema}__sources.yml (one for each source schema) 
- *e.g: bronze__ods__ambo_sim__kafka__local__sources.yml*

### Model and Folder Names

dbt model names are verbose (inclusive of zone and domain) to ensure global uniqueness and better traceability to folders. Actual object names should be aliased to match object naming standards.

#### Bronze

*Bronze objects are likely to be referenced in sources/bronze or as seeds*

- Folder: `models/bronze/{optional: domain name}{optional: __subdomain name(s)}/`
- Folder: `sources/bronze/{optional: domain name}{optional: __subdomain name(s)}/`
- Folder: `seeds/{optional: domain name}{optional: __subdomain name(s)}/`

#### Silver

**Staging Source-specific:** 

   - Folder: `models/silver/{optional: domain name}{optional: __subdomain name(s)}/stg/{source_system}__{source_channel}/`
   - Models: `{optional: domain name} {optional: __subdomain name(s)} {optional:__silver} __stg{__entity /_object_description} __{ordinal}_{transformation description} __{source_system} __{source_channel}`

```md
   *e.g:*

      - *silver\new_finance_system__adf\stg\intuitas_corporate__silver__stg__accounts__01_renamed_and_typed__new_finance_system__adf.sql*
      - or *silver\new_finance_system__adf\stg\stg__accounts__01_renamed_and_typed__new_finance_system__adf.sql*
      - materialises to: *intuitas_corporate_dev.stg__new_finance_system__adf.accounts__01_renamed_and_typed__new_finance_system__adf*
```


**Staging Non-source-specific (entity centric):**

   - Folder: `models/silver/{optional: domain name}{optional: __subdomain name(s)}/mart/{entity}/stg`
   - Models: `{optional: domain name} {optional: __subdomain name(s)} {optional:__silver__} stg{__optional:dim_/fact_}{__entity /_object_description} __{ordinal}_{transformation description} `

      - *e.g: intuitas_corporate_dev.stg.accounts__01_deduped*
      - *e.g: intuitas_corporate_dev.stg.accounts__02_business_validated* 


```md
   *e.g:*

      - *silver\mart\accounts\stg\intuitas_corporate__silver__stg__accounts__01_deduped.sql*
      - or *silver\mart\accounts\stg\stg__accounts__01_deduped.sql*
      - materialises to: *e.g: intuitas_corporate_dev.stg.accounts__01_deduped*
```

**Mart Source-specific:** 

   - Folder: `models/silver/{optional: domain name}{optional: __subdomain name(s)}/mart/{entity}`
   - Models: `{optional: domain name} {optional: __subdomain name(s)} {optional:__silver__} mart{__optional:dim_/fact_}{__entity /_object_description}__{source_system}__{source_channel}`

**Mart Non-source specific:** 

   - Folder: `models/silver/{optional: domain name}{optional: __subdomain name(s)}/mart/{entity}`
   - Models: `{optional: domain name} {optional: __subdomain name(s)} {optional:__silver__} mart{__optional:dim_/fact_}{__unified entity /_object_description}`

**Reference Data:** 

   - Folder: `models/silver/{optional: domain name}{optional: __subdomain name(s)}/ref/{entity}`
   - Models: `{optional: domain name} {optional: __subdomain name(s)} {optional:__silver__} ref{__optional:dim_/fact_} {__reference data set name} {optional:__{source_system}__{source_channel}}`

**Raw Vault:** 

   - Folder: `models/silver/{optional: domain name}{optional: __subdomain name(s)}/edw/rv`
   - Models: `edw_rv__{vault object named as per data vault standards}`

- Schema naming convention:** 

   - Folder: `models/silver/{optional: domain name}{optional: __subdomain name(s)}/edw/bv`
   - Models: `edw_bv__{vault object named as per data vault standards}`

#### Gold

-  Staging:**

   - Folder: `models/gold/{optional: domain name}{optional: __subdomain name(s)}/mart/{entity / product description}/stg`
   - Models: `{optional: domain name} {optional: __subdomain name(s)} {optional:__gold__} mart__stg{__entity / product description} __{ordinal}_{transformation description} {optional: __{source_system} __{source_channel}}`

-  Dimensions: 

   - Folder: `models/gold/{optional: domain name}{optional: __subdomain name(s)}/mart/{entity / product description}`
   - Models: `{optional: domain name} {optional: __subdomain name(s)} {optional:__gold__}  mart__dim_{__entity / product description} (optional: __{source_system} __{source_channel})`

-  Facts: 

   - Folder: `models/gold/{optional: domain name}{optional: __subdomain name(s)}/mart/{entity / product description}`
   - Models: `{optional: domain name} {optional: __subdomain name(s)} {optional:__gold__} mart__fact_{__entity / product description} (optional: __{source_system} __{source_channel})`

-  Denormalized (One Big Table): 

   - Folder: `models/gold/{optional: domain name}{optional: __subdomain name(s)}/mart/{entity / product description}`
   - Models: `{optional: domain name} {optional: __subdomain name(s)} {optional:__gold__} mart__{entity / product description} {optional: __{source_system} __{source_channel}}`




#### Example dbt model structure:
The model structure below reflects a single catalog for domain+environment and schema separation for layers and stages:

```md
{{domain/enterprise} _project_name}
├── README.md
├── analyses
├── seeds
│   └── ref_entity_data_file.csv
├── dbt_project.yml
├── macros
│   └── custom_macro.sql
│   ├── utilities
│       └── all_dates.sql
├── models/bronze
│   /{optional: domain and subdomains}
│   └── _bronze.md
├── models/silver/{optional: domain and subdomains}
│   /{optional: domain and subdomains}
│   ├── _silver.md
│   ├── mart  (entity centric objects)
│   |    └── account
│   |    |   └── mart__dim_account.sql
│   |    |       └── stg
│   |    |           └── stg__dim_account__01_dedupe.sql
│   |    |           └── stg__dim_account__02_filter.sql
│   |    └── date
│   |        └── mart__dim_date.sql
│   └── ref
│       ├── _reference_data__models.yml
│       ├── _reference_data__sources.yml
│       └── ref_{entity}.sql
│   ├── stg (source centric staging objects)
│   |   └── source_system_1 
│   |       ├── _source_system_1__docs.md
│   |       ├── _source_system_1__models.yml
│   |       ├── stg__object__source_system_1.sql
│   |       ├── stg__(new object)__source_system_1.sql
│   |       ├── stg__object_desensitised__source_system_1.sql
│   |       └── stg
│   |           ├── stg__object__01_step__source_system_1.sql
│   |           └── stg__object__02_step__source_system_1.sql
│   ├── sources
│        └── {optional: domain}
│        └── {optional: bronze/silver/gold}
│             └── _source_system_1__sources.yml 
├── models/gold
│   /{optional: domain and subdomains}
│   ├── _gold.md
│   └── domain_name e.g: finance 
│       └── mart
│           ├── _finance__models.yml
│           ├── orders.sql
│           └── payments.sql
│               └── stg
│                   └── stg_payments_pivoted_to_orders.sql
├── packages.yml
├── snapshots
└── tests
    └── assert_positive_value_for_total_amount.sql
```

### dbt_project.yml
The yml structure below reflects a single catalog for domain+environment and schema separation for layers and stages:

```yml
models:
  health_lakehouse__engineering__dbt:
    +persist_docs: #enables injection of metadata into unity catalog
        relation: true
        columns: true

    bronze:
      +schema: bronze

    silver:
      +schema: silver
      source_system_1:
        +schema: silver__source_system_1
        base:
          +materialized: view
        staging:
          +materialized: table
      edw__domain_name:
        +description: "Domain-centric EDW objects."
        +schema: silver__edw__domain_name
        +materialized: table

    gold:
      +materialized: view # default for speed
      +schema: gold
      domain_name:
        +schema: gold__domain_name
        subdomain_name:
          +schema: gold__domain_name__subdomain_name
```

<br>

## CI/CD

The following standards and conventions relate to Continuous Improvement and Continuous Delivery constructs.

### Repository naming 

- All lowercase with hyphens as separators
- Format: `{org}-{domain}-{purpose}-{optional:descriptor}`

   Examples:
   - intuitas-corporate-dbt
   - intuitas-corporate-ingestion-framework  
   - intuitas-corporate-cicd-templates
   - intuitas-corporate-infrastructure

### Branch naming

- All lowercase with hyphens as separators
- Naming convention: `{type}-{optional:ticket-id}-{description}`

   Types:
   - feature: New functionality
   - bugfix: Bug fixes
   - hotfix: Critical fixes for production
   - release: Release branches
   - docs: Documentation updates only
   - refactor: Code improvements with no functional changes
   - test: Test-related changes

   Examples:
   - feature-eng123-add-new-data-source
   - bugfix-eng456-fix-null-values
   - hotfix-prod-outage-fix
   - release-v2.1.0
   - docs-update-readme
   - refactor-optimize-transforms
   - test-add-integration-tests

### Branch lifecycle

#### Simple branch lifecycle:
- main/master: Primary branch
- branch: Short-lived branches development branch, merged or rebased to main/master

#### Comprehensive team branch lifecycle:
1. Main/master: Primary branch
2. Development: Active development branch
3. Feature/bugfix: Short-lived branches merged to development
4. Release: Created from development, merged to main/master
5. Hotfix: Created from main/master for urgent fixes


### Databricks Asset Bundles

Databricks asset bundles are encouraged for all Databricks projects.

- project/bundle name: 
   - `{domain_name}__databricks` (for general databricks projects)
   - `{domain_name}__dbt` (for dbt databricks bundles)

- Bundle tags:
   - Key: `environment: {environment}`
   - Key: `manager: {team_name}` and or `{email_address}`
   - Key: `managing_domain: `{domain_name}` e.g: if delegating to engineering domain
   - Key: `owner: {owner_name}`
   - Key: `owning_domain: {domain_name}`
   - Key: `dab: {bundle_name}`
   - Key: `project: {project_name}`

   e.g:
   ```yml
   tags:
      environment: ${bundle.target}
      project: health-lakehouse
      dab: health_lakehouse__engineering__databricks
      owning_domain: intuitas_engineering
      owner: engineering-admin@intuitas.com
      manager: engineering-engineer@intuitas.com
      managing_domain: intuitas_engineering
   ```

- Resources:
   - Folder level 1: `{meaningful sub-project name}`
   - Folder level 2: 
      - notebooks
      - workflows

- Databricks.yml
   - For both dev and prod: `root_path: /Workspace/Users/engineering-engineer@intuitas.com/.bundle/${bundle.name}/${bundle.target}`

Example databricks.yml
```yml
# This is a Databricks asset bundle definition for health_lakehouse__engineering.
# See https://docs.databricks.com/dev-tools/bundles/index.html for documentation.
bundle:
  name: health_lakehouse__engineering__databricks

variables:
  default_cluster_id:
    value: "-----"  


include:
  - resources/*.yml
  - resources/**/*.yml
  - resources/**/**/*.yml

targets:
  dev:
    mode: development
    default: true
    workspace:
      host: https://------.15.azuredatabricks.net
      root_path: /Workspace/Users/engineering-engineer@intuitas.com/.bundle/${bundle.name}/${bundle.target}
  prod:
    mode: production
    workspace:
      host: https://------.15.azuredatabricks.net
      root_path: /Workspace/Users/engineering-engineer@intuitas.com/.bundle/${bundle.name}/${bundle.target}
    permissions:
      - user_name: engineering-engineer@intuitas.com
        level: CAN_MANAGE
    run_as:
      user_name: engineering-engineer@intuitas.com
```
<br>

## Security

Security standards and conventioned provided here provide a starter set, however existing organisational and applicable industry standards should take precedence. Consult with your cybersecurity advisor.

### Entra
> Under development. (Contact us to know more).

Most organisations will already have an established set of groups and conventions. Where there are gaps, the following can still be considered.

Recommended areas to align to organisational governance and cyber requirements:

- Naming conventions for admin, service, and user groups
- Role-based access alignment (least privilege, separation of duties)
- Alignment to domains - Cross-domain vs. domain-specific group patterns


**Entra Group Names**:

- Pattern:  `grp-<org>-<domain>-<plat>-<scope>-<role>-<env>[-<region>][-ext-<partner>][-idx]`
- Lowercase, hyphen-separated; no spaces.
- Keep to ≤ 120 chars total.
- No PII in names.
- Use Security groups (not M365) for RBAC; enable PIM where appropriate e.g. Admins.

**role**:

- owner — full control of the named scope
- admin — administrative (non-ownership) rights
- contrib — create/modify within scope
- editor — modify data/artifacts, not permissions
- reader — read-only
- steward — governance/metadata rights
- custodian — key/secret/storage control
- operator — run/ops rights (pipelines, jobs)
- viewer — read dashboards/reports

<br>

**plat:**

- dbx (Databricks), uc (Unity Catalog), pbi (Power BI), adf (Data Factory),
- dlk (Data Lake), sql (Azure SQL), kva (Key Vault), syn (Synapse)

<br>

**scope** (or object):

- Databricks Workspace: ws-<WorkspaceName>
- Unity Catalog: uc-meta (metastore), uc-cat-<Catalog>, uc-sch-<Catalog>.<Schema>, uc-obj-<Catalog>.<Schema>.<Object>
- Power BI: pbi-ws-<Workspace>
- Data Lake: dlk-path-/datalake/<area>/<path>

<br>

*Examples*:

   - *GRP-INTUITAS-CLIN-DBX-WS-Analytics-ADMIN-PRD*
   - *GRP-INTUITAS-CLIN-UC-UC-CAT-Claims-OWNER-PRD*
   - *GRP-INTUITAS-CLIN-UC-UC-SCH-Claims.Curated-READER-UAT*
   - *GRP-INTUITAS-FIN-PBI-PBI-WS-ExecDash-VIEWER-PRD*
   - *GRP-INTUITAS-ENT-KVA-KVA-Keys-CUSTODIAN-PRD*
   - *GRP-INTUITAS-CLIN-DLK-DLK-PATH-/curated/claims/READER-PRD-AUE*
   - *GRP-INTUITAS-CLIN-DBX-WS-PartnerLake-READER-PRD-EXT-ACME*

<br>
<br>

### Policies
> Under development. (Contact us to know more).

Recommended areas to align to non-functional requirements:

- Data retention (duration, archival, legal hold)
- Key retention and rotation cycles
- Backup and recovery standards
- Incident response and escalation procedures
- Access review and recertification

<br>
### Frameworks
> Under development. (Contact us to know more).

Recommended areas to align to industry and cyber compliance:

- Engineering standards (e.g., code repositories, CI/CD security, IaC policies)
- Security frameworks (e.g., NIST, ISO 27001, CIS Benchmarks, Zero Trust)
- Compliance mappings (HIPAA, GDPR, SOC2, local regulatory obligations)

