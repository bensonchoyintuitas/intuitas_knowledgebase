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
- Level 1 Name: `{zone} (raw/edw/infomart)` // (bronze/silver/gold)` if using medallion approach
- Level 2 Name: `{layer_name}`
- *e.g:*

   - *raw/landing*
   - *raw/ods*
   - *raw/pds*
   - *raw/schema* (for autoloader metadata)
   - *raw/checkpoints* (for autoloader metadata)
   - *edw/automatically determined by unity catalog*
   - *infomart/automatically determined by unity catalog*

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

- Job names: `{domain}__{zone}__{purpose}__{source}{optional: __target}{optional: __schedule}{optional: __version}__{env}`
- *e.g. clinical__raw__ingest__fhircdr__dev*

#### For Delta Live Table (DLT) Pipelines
---
- Pipeline names: `{domain_name}__{zone}__pipeline__{dataset}{optional: __schedule}{optional: __version}__{env}`
- *e.g. clinical__raw__pipeline__fhircdr__dev*
- *e.g. supplychain__infomart__pipeline__inventorymart__prod* 

- Note on {zone}: If the 'business outcome' is Infomart, you call it Infomart, even if it produces Raw + EDW on the way.
*i.e "This is the production DLT pipeline in the supply chain domain, which builds and maintains the curated Infomart zone dataset called Inventory Mart"*

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

Refer to [Data zones and layers](level_2.md#data-zones-and-layers) for further context and definitions applicable to this section.

Catalog name: 

The choice of granularity depends on domain topology, zone and layer conventions, and desired level of segregation for access and sharing controls (i.e. catalog or layer level)

   - Minimum granularity (domain level): `{domain_name}{__environment (dev/test/pat/prod)}` (prod is implied optional)   *e.g: corporate__dev*
   - Optional granularity (domain-zone level): `{domain_name}{_zone: (raw/edw/infomart)}{__environment (dev/test/pat/prod)}`    *e.g: corporate_raw__dev*
   - Optional granularity (domain-zone and layer level): `{domain_name}{_zone: (raw/edw/infomart)}{_layer: (ods/pds/stg/ref/mart)}{__environment (dev/test/pat/prod)}`    *e.g: corporate_raw_ods__dev*
   - Optional granularity (domain-layer level): `{domain_name}{_layer: (ods/pds/stg/ref/mart)}{__environment (dev/test/pat/prod)}`    *e.g: corporate_ods__dev*
   - Optional granularity (subdomain-zone level): `{domain_name}{_descriptor (subdomain/subject/project*)}{_zone: (raw/edw/infomart)}{__environment (dev/test/pat/prod)}`    *e.g: corporate_finance_raw__dev*

In the examples provided - we have opted for domain level - with schema separation for the lower levels of grain via prefixes. i.e `engineering__dev.ods__fhirhouse__dbo(lakeflow).encounter`

*Note that projects are temporary constructs, and hence are not recommended for naming*

- Catalog storage root: `abfss://{environment}@dl{organisation_name}{domain_name}.dfs.core.windows.net/{domain_name}_{environment}_catalog`

### Externally mounted (lakehouse federation) Catalog Names

- Foreign Catalog name: `{domain_name (owner)}__fc__{source_system}{optional:__other_useful_descriptors e.g:__environment}`
- *e.g: corporate__fc__sqlonpremsource*
- *e.g: corporate__fc__sqlonpremsource__prod*

### Catalog Metadata tags:

The following metadata should be added when creating a catalog:

- Key: domain (owner): `{domain_name}`
- Key: environment: `{environment}`
- Key: managing_domain: `{domain_name}` e.g: if delegating to engineering domain

### Schema and object conventions
---

Refer to [Data zones and layers](level_2.md#data-zones-and-layers) for further context and definitions applicable to this section.

Refer to [Physical Models](modelling_standards_and_conventions.md#physical-models) for the standard relating to column naming, types, and conventions.

**Terminology Hierarchy:**

Data is organized in a three-level hierarchy:
- **Zone** = Top-level organizational boundary (Raw, EDW, Infomart)
  - *Within each zone, data flows through...*
- **Layer** = Processing layers within a zone (landing, ods, pds, stg, ref, mart, etc.)
  - *Each layer typically maps to...*
- **Schema** = Unity Catalog schema that stores objects (ods, pds, edw_stg, edw_ref, edw, im_stg, im)

Example: The **Raw zone** contains the **ODS layer** which maps to the **ods schema**.

#### Schema level external storage locations

Recommendations:

- For managed tables (default): do nothing.  Let dbt create schemas without additional configuration. Databricks will manage storage and metadata.Objects will then be stored in the catalog storage root. *e.g: abfss://dev@dlintutiasengineering.dfs.core.windows.net/engineering__dev_catalog/__unitystorage/catalogs/catalog-guid/tables/object-guid*
- For granular control over schema-level storage locations: Pre-create schemas with LOCATION mapped to external paths or configure the catalog-level location.
- Ensure dbt's dbt_project.yml and environment variables align with storage locations.

#### Metadata Schemas and Objects

Refer to [Data zones and layers](level_2.md#data-zones-and-layers) for further context and definitions applicable to this section.

Contains metadata that supports engineering and governance. This will vary depending on engineering and governance toolsets

**Engineering - ingestion framework**:

- Schema naming convention:  `meta {optional: __function}`
- Naming convention: `{function/descriptor}`
- *e.g: corporate__dev.meta.ingestion_control*
- *e.g: corporate__dev.meta__ingestion.ingestion_control*

#### Raw (aka Bronze) (Data according to systems)

The Raw zone stores raw, immutable data as it is ingested from source systems. See [Data zones and layers](level_2.md#data-zones-and-layers) for definitions and context.

The Raw zone uses two primary schemas:
- **`ods`** - Operational Data Store (current state)
- **`pds`** - Persistent Data Store (historical snapshots)

In the examples provided - we have opted for domain level catalogs - with schema separation for the lower levels of grain via prefixes. i.e `engineering__dev.ods__fhirhouse__dbo__lakeflow.encounter`

**Persistent Landing**:

Persistent Landing uses Unity Catalog Volumes for storing raw files and unstructured data as they arrive from source systems.

Volume naming convention: `[domain][__env].[layer][__source_system][__source_schema](channel).[source_object/volume_name]`

  - `[domain][__env]`: Domain and environment identifier (e.g., `corporate_dev`, `engineering_prod`)
  - `[layer]`: Data layer identifier within the Raw zone (e.g., `landing`)
  - `[__source_system]`: Source system identifier (e.g., `workdayapi`, `saphr`)
  - `[__source_schema]`: Optional source schema or subsystem identifier
  - `(channel)`: Ingestion channel/method (e.g., `adf`, `fivetran`, `api`)
  - `[source_object/volume_name]`: Descriptive volume name or source object identifier

- *e.g: corporate__dev.landing__workdayapi.schedule_volume*
- *e.g: engineering__prod.landing__fhirapi__patients.raw_data*

**Operational Data Store (ODS)**:

The objective of raw zone conventions is to provide clarity over which zone, layer, and schema it belongs to, what the data relates to, where it was sourced from, and via what channel it arrived (as there may be nuances in data depending on its channel).

ODS can be replicated from source systems, or prepared for use from semi/unstructured data via hard-transformation and hence will have these associated conventions:

Database replicated ODS (structured sources like SQL Server):

Format: `[domain][__env].[layer][__source_system][__source_schema](channel).[source_object/volume_name]`

  - `[domain][__env]`: Domain and environment (e.g., `clinical__dev`)
  - `[layer]`: Data layer within the Raw zone (e.g., `ods`)
  - `[__source_system]`: Source system identifier
  - `[__source_schema]`: Source schema (if applicable, e.g., `dbo`, `reporting`)
  - `(channel)`: Optional ingestion channel (e.g., `adf`, `fivetran`, `lakeflow`)
  - `[source_object/volume_name]`: Table name as per source

- *e.g: clinical__dev.ods__patientflowmanager01__reporting.patients*
- *e.g: clinical__dev.ods__fhirhouse__dbo(adf).encounter*

Prepped semi/unstructured ODS data:

Format: `[domain][__env].[layer][__source_system][__source_descriptor](channel).[source_object/volume_name]`

  - `[domain][__env]`: Domain and environment (e.g., `clinical__dev`)
  - `[layer]`: Data layer within the Raw zone (e.g., `ods`)
  - `[__source_system]`: Source system identifier
  - `[__source_descriptor]`: Source descriptor or subsystem identifier
  - `(channel)`: Optional ingestion channel (e.g., `kafka`, `databricks`, `api`)
  - `[source_object/volume_name]`: Named as per source or unique assigned name (e.g., topic/folder name)

- *e.g: clinical__dev.ods__ambosim__confluent(kafka).encounter*
- *e.g: corporate__dev.ods__workdayapi__employees.raw_data*


**Persistent Data Store (PDS)**:

PDS conventions will mirror ODS conventions:

Database replicated PDS (structured sources like SQL Server):

Format: `[domain][__env].[layer][__source_system][__source_schema](channel).[source_object/volume_name]`

  - `[domain][__env]`: Domain and environment (e.g., `clinical__dev`)
  - `[layer]`: Data layer within the Raw zone (e.g., `pds`)
  - `[__source_system]`: Source system identifier
  - `[__source_schema]`: Source schema (if applicable, e.g., `dbo`, `reporting`)
  - `(channel)`: Optional ingestion channel (e.g., `adf`, `fivetran`, `lakeflow`)
  - `[source_object/volume_name]`: Table name as per source

- *e.g: clinical__dev.pds__patientflowmanager01__reporting.patients*
- *e.g: clinical__dev.pds__fhirhouse__dbo(adf).encounter*

Prepped semi/unstructured PDS data:

Format: `[domain][__env].[layer][__source_system][__source_descriptor](channel).[source_object/volume_name]`

  - `[domain][__env]`: Domain and environment (e.g., `clinical__dev`)
  - `[layer]`: Data layer within the Raw zone (e.g., `pds`)
  - `[__source_system]`: Source system identifier
  - `[__source_descriptor]`: Source descriptor or subsystem identifier
  - `(channel)`: Optional ingestion channel (e.g., `kafka`, `databricks`, `api`)
  - `[source_object/volume_name]`: Named as per source or unique assigned name (e.g., topic/folder name)

- *e.g: clinical__dev.pds__ambosim__confluent(kafka).encounter*
- *e.g: corporate__dev.pds__workdayapi__employees.raw_data*

#### EDW (aka Silver) (Data according to business entities)

The EDW (Enterprise Data Warehouse) zone focuses on transforming raw data into cleaned, enriched, and validated datasets that are the building blocks for downstream consumption and analysis.

Refer to [Data zones and layers](level_2.md#data-zones-and-layers) for further context and definitions applicable to this section.

The EDW zone uses three primary schemas:
- **`edw_stg`** - Source-centric staging objects (transformations, cleaning, normalization)
- **`edw_ref`** - Reference data (conformed lookups, master data)
- **`edw`** - Base marts (dimensions, facts - curated entities)

These marts are objects that are aligned to business entities and broad requirements, hence they must contain source-specific objects at the lowest grain. There may be further enrichment and joins applied across sources.

In the examples provided - we have opted for domain level catalogs - with schema separation for the lower levels of grain via prefixes. i.e `engineering__dev.edw.dim_customer`

- All schemas may be optionally prefixed with additional descriptors if not already decomposed at domain-level i.e. `edw_stg__`, `edw_ref__`, `edw__`
- All `entity` names which align to facts should be named in plural.
- All `entity` names which align to dims should be named in singular.

<br>

**EDW Staging Objects**:
Staging models serve as intermediary models that transform source data into EDW models. According to dbt best practices, there is a distinction between Staging and Intermediate models. Under this blueprint the use of Intermediate models is optional. [Reference](https://docs.getdbt.com/best-practices/how-we-structure/1-guide-overview)

**Source-Centric Staging Approach:**

Staging objects are organized around source systems and source objects, cleaning and normalizing data according to expectations/standards. This approach:
- Cleans and normalizes data from each source independently
- May feed data quality test results to reports for actioning
- Prepares reference data for broad use and maps it for mart conformance
- Creates mappings of keys to resolve and map system keys to universal surrogate keys or business keys, which can then be reused downstream for integration

**Naming Convention:**

`[domain][__env] . [schema] . stg__[source_system](optional__[source_channel])__[source_object](optional__[other_descriptors])__[id]__[transformation_descriptors]`

Where:

- `[domain][__env]` - Domain and environment (e.g., `corporate__prod`, `corporate__dev`)
- `[schema]` - EDW schema (`edw_stg` for staging, `edw_ref` for reference staging)
- `stg__` - Staging object prefix
- `[source_system]` - Source system identifier (e.g., `sap`, `workday`, `finance_system`)
- `(optional__[source_channel])` - Optional source channel (e.g., `__adf`, `__api`)
- `[source_object]` - Source object/table name
- `(optional__[other_descriptors])` - Optional additional descriptors
- `[id]` - Unique identifier suffix
- `[transformation_descriptors]` - Numbered transformation step(s)

**Standard Transformation Steps (optional, applied as needed):**

- `00_keyed` - Key assignment/generation
- `01_renamed_and_typed` - Column renaming and type casting
- `02_deduped` - Deduplication
- `03_cleaned` - Data cleaning
- `04_filtered` or `04_split` - Filtering or splitting data
- `05_column_selected` - Column selection
- `06_business_validated` - Business rule validation
- `07_desensitised` - Sensitive data masking/removal

**Reference Data Staging:**

Source-specific staging (with transformations in `edw_stg` schema):

   - *e.g: `corporate__prod.edw_stg.stg__sap__facilities__01_renamed_and_typed`*
   - *e.g: `corporate__prod.edw_stg.stg__sap__facilities__02_cleaned`*
   - *e.g: `corporate__prod.edw_stg.stg__finance_system__adf__account_codes__01_renamed_and_typed`*
   - *e.g: `corporate__prod.edw_stg.stg__workday__locations__01_renamed_and_typed`*
   - *e.g: `corporate__prod.edw_stg.stg__workday__locations__02_deduped`*

Final reference data (cleaned and conformed in `edw_ref` schema):

   - *e.g: `corporate__prod.edw_ref.ref_facility`*
   - *e.g: `corporate__prod.edw_ref.ref_location`*
   - *e.g: `corporate__prod.edw_ref.ref_account_code`*
   - *e.g: `corporate__dev.edw_ref.ref_country_codes`*

**Base Mart (EDW) Staging:**

Source-specific staging (with transformations in `edw_stg` schema):

   - *e.g: `corporate__prod.edw_stg.stg__sap__employees__01_renamed_and_typed`*
   - *e.g: `corporate__prod.edw_stg.stg__workday__employees__01_renamed_and_typed`*
   - *e.g: `corporate__prod.edw_stg.stg__workday__employees__02_cleaned`*
   - *e.g: `corporate__prod.edw_stg.stg__crm__adf__customers__01_renamed_and_typed`*
   - *e.g: `corporate__prod.edw_stg.stg__crm__adf__customers__02_cleaned`*
   - *e.g: `corporate__prod.edw_stg.stg__new_finance_system__adf__payments__01_renamed_and_typed`*
   - *e.g: `corporate__prod.edw_stg.stg__old_finance_system__adf__payments__01_renamed_and_typed`*
   - *e.g: `corporate__prod.edw_stg.stg__finance_system__api__accounts__01_renamed_and_typed`*

**Base Mart Keysets:**

Keysets are created in the `edw` schema to resolve and map system keys to universal surrogate keys or business keys. These can be reused downstream for integration and conforming/deduplicating entities across multiple sources:

   - *e.g: `corporate__prod.edw.keys__employee`* (conforms sap and workday employee keys)
   - *e.g: `corporate__prod.edw.keys__customer`* (conforms multiple crm source keys)
   - *e.g: `corporate__prod.edw.keys__account`* (conforms finance system keys)
   - *e.g: `health__prod.edw.keys__patient`* (conforms clinical system keys)

**Base Marts (Final Dimensional Models):**

Source-specific dimensions (preserving source identity):

   - *e.g: `corporate__prod.edw.dim_employee__sap`*
   - *e.g: `corporate__prod.edw.dim_employee__workday`*
   - *e.g: `corporate__prod.edw.dim_account__old_finance_system`*
   - *e.g: `corporate__prod.edw.dim_account__new_finance_system`*

Conformed dimensions (unified, non-source specific):

   - *e.g: `corporate__prod.edw.dim_employee`* (type 1 SCD implied)
   - *e.g: `corporate__prod.edw.dim_employee__type2`* (with history)
   - *e.g: `corporate__prod.edw.dim_customer`*
   - *e.g: `corporate__prod.edw.dim_account`* (unified across finance systems)

Fact tables:

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
- *e.g: corporate__dev.edw_rv.hs_payments__finance_system__adf*

<br>

**Business Vault**:
Optional warehousing construct.

- Schema naming convention: `edw_bv`
- Object naming convention: `{vault object named as per data vault standards}`
- *e.g: corporate__dev.edw_bv.hs_late_payments__finance_system__adf*

<br>

#### Infomart (aka Gold) (Data according to requirements)

The Infomart zone focuses on requirement-aligned products (datasets, aggregations, and reporting structures). Products are predominantly source agnostic, however optionality exists when source-specific views are needed.

Refer to [Data zones and layers](level_2.md#data-zones-and-layers) for further context and definitions applicable to this section.

The Infomart zone uses two primary schemas:
- **`im_stg`** - Product mart staging (intermediate transformations)
- **`im`** - Information marts (final business-aligned products)

**Naming Convention:**

`[domain][__env] . [schema] . [object_type][product_name/descriptor](__source)(__transformation)`

Where:

- `[domain][__env]` - Domain and environment (e.g., `corporate__prod`, `health__dev`)
- `[schema]` - Infomart zone schema (`im_stg` for staging, `im` for final products)
- `[object_type]` - Object type prefix (e.g., `stg_`, `fact_`, `dim_`, `obt_`)
- `[product_name/descriptor]` - Product or business-aligned name
- `(__source)` - Optional source system identifier (only when source-specific view is needed)
- `(__transformation)` - Optional transformation step (for staging only)

**Naming Guidelines:**

- All `entity` names which align to facts should be named in plural
- All `entity` names which align to dimensions should be named in singular
- Product marts are business/requirement-aligned, not technical constructs

<br>

**Infomart Staging Models**:

Staging models serve as intermediary models that transform base marts (EDW) into requirement-aligned product marts. According to dbt best practices, there is a distinction between Staging and Intermediate models. Under this blueprint the use of Intermediate models is optional. [Reference](https://docs.getdbt.com/best-practices/how-we-structure/1-guide-overview)

These models exist in the `im_stg` schema to stage information marts with business-specific transformations:

**Common Transformations:**

- Pivoting
- Aggregation
- Joining across multiple base marts
- Business rule application
- Desensitization
- Complex calculations

**Examples:**

- *e.g: `corporate__prod.im_stg.stg_downtime_by_region__01_pivoted`*
- *e.g: `corporate__prod.im_stg.stg_downtime_by_region__02_desensitised`*
- *e.g: `corporate__prod.im_stg.stg_late_payments__01_aggregated`*
- *e.g: `corporate__prod.im_stg.stg_late_payments__02_joined`*
- *e.g: `corporate__prod.im_stg.stg_fte_calculations__01_pivoted`*
- *e.g: `corporate__prod.im_stg.stg_fte_calculations__02_aggregated`*
- *e.g: `health__prod.im_stg.stg_patient_outcomes__01_joined`*
- *e.g: `health__prod.im_stg.stg_patient_outcomes__02_desensitised`*

<br>

**Information Marts (Final Products)**:

Final business-aligned data products ready for consumption by analytics, reporting, and BI tools in the `im` schema.

**Examples:**

Fact tables (aggregated/business-focused):

   - *e.g: `corporate__prod.im.fact_downtime_by_region`*
   - *e.g: `corporate__prod.im.fact_late_payments`*
   - *e.g: `corporate__prod.im.fact_regional_account_payments`*
   - *e.g: `health__prod.im.fact_patient_outcomes`*

Dimension tables (product-specific):

   - *e.g: `corporate__prod.im.dim_region`*
   - *e.g: `corporate__prod.im.dim_payment_category`*

One Big Tables (OBT) - Denormalized aggregates:

   - *e.g: `corporate__prod.im.obt_fte_calculations`*
   - *e.g: `corporate__prod.im.obt_financial_summary`*
   - *e.g: `corporate__prod.im.obt_executive_dashboard`*

Source-specific products (when needed):

   - *e.g: `corporate__prod.im.fact_account_payments__old_finance_system`*
   - *e.g: `corporate__prod.im.fact_account_payments__new_finance_system`*
   - *e.g: `corporate__prod.im.obt_employee_metrics__sap`*
   - *e.g: `corporate__prod.im.obt_employee_metrics__workday`*

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
- *e.g: models/edw/ambo_sim__kafka__local/_edw__ambo_sim__kafka__local__docs.md*

- model yml: _{path to model folder using _ separators}__models.yml
- *e.g: models/edw/ambo_sim__kafka__local/_edw__ambo_sim__kafka__local__models.yml*

### Sources

- Folder: models/sources/{raw/edw/infomart}
- yml: {schema}__sources.yml (one for each source schema) 
- *e.g: ods__ambo_sim__kafka__sources.yml*

### Model and Folder Names

dbt model names are verbose (inclusive of schema and domain) to ensure global uniqueness and better traceability to folders. Actual object names should be aliased to match object naming standards.

#### Raw

*Raw zone contains operational data store (ODS) for current state and persistent data store (PDS) for historical tracking*

**ODS (Operational Data Store) - Current State:**
- Folder: `seeds/{optional: domain name}{optional: __subdomain name(s)}/`  → materializes to `[domain__env].ods`
- Folder: `sources/raw/{optional: domain name}{optional: __subdomain name(s)}/`  → references external sources to `[domain__env].ods`

**Seed Naming Convention:**

Seeds (CSV files) should follow a descriptive naming pattern and materialize to the ODS schema:
- `ref_[entity]_[descriptor].csv` for reference/lookup data
- `config_[descriptor].csv` for configuration data
- *e.g: `ref_country_codes.csv`* → `corporate__dev.ods.ref_country_codes`
- *e.g: `ref_currency_rates.csv`* → `corporate__dev.ods.ref_currency_rates`
- *e.g: `config_holidays.csv`* → `corporate__dev.ods.config_holidays`

**PDS (Persistent Data Store) - Historical Tracking:**
- Folder: `models/raw/{optional: domain name}{optional: __subdomain name(s)}/pds/`  → materializes to `[domain__env].pds`
- Models in PDS capture type 2 SCD (slowly changing dimensions) or snapshots of ODS data

**Type 2 Snapshot Model Examples:**
- *e.g: `models/raw/pds/ref_country_codes_snapshot.sql`* → `corporate__dev.pds.ref_country_codes_snapshot`
- *e.g: `models/raw/pds/ref_currency_rates_snapshot.sql`* → `corporate__dev.pds.ref_currency_rates_snapshot`
- *e.g: `models/raw/pds/config_holidays_snapshot.sql`* → `corporate__dev.pds.config_holidays_snapshot`

These models typically reference their corresponding seeds from ODS and add historical tracking columns (valid_from, valid_to, is_current, etc.)

#### EDW

**Staging Source-specific:** 

   - Folder: `models/edw/{optional: domain name}{optional: __subdomain name(s)}/stg/{source_system}__{source_channel}/`
   - Models: `{optional: domain name} {optional: __subdomain name(s)} {optional:__edw__} stg__{source_system} __{source_channel} __{source_object} __{ordinal}_{transformation description}`

```md
   *e.g:*

      - *edw\new_finance_system__adf\stg\corporate__edw__stg__new_finance_system__adf__accounts__01_renamed_and_typed.sql*
      - or *edw\new_finance_system__adf\stg\stg__new_finance_system__adf__accounts__01_renamed_and_typed.sql*
      - materialises to: *corporate__dev.edw_stg.stg__new_finance_system__adf__accounts__01_renamed_and_typed*
```

**Mart Source-specific:** 

   - Folder: `models/edw/{optional: domain name}{optional: __subdomain name(s)}/mart/{entity}`
   - Models: `{optional: domain name} {optional: __subdomain name(s)} {optional:__edw__} mart{__optional:dim_/fact_}{__entity /_object_description}__{source_system}__{source_channel}`

**Mart Non-source specific:** 

   - Folder: `models/edw/{optional: domain name}{optional: __subdomain name(s)}/mart/{entity}`
   - Models: `{optional: domain name} {optional: __subdomain name(s)} {optional:__edw__} mart{__optional:dim_/fact_}{__unified entity /_object_description}`

**Reference Data:** 

   - Folder: `models/edw/{optional: domain name}{optional: __subdomain name(s)}/ref/{entity}`
   - Models: `{optional: domain name} {optional: __subdomain name(s)} {optional:__edw__} ref{__optional:dim_/fact_} {__reference data set name} {optional:__{source_system}__{source_channel}}`

**Raw Vault:** 

   - Folder: `models/edw/{optional: domain name}{optional: __subdomain name(s)}/edw/rv`
   - Models: `edw_rv__{vault object named as per data vault standards}`

**Business Vault:** 

   - Folder: `models/edw/{optional: domain name}{optional: __subdomain name(s)}/edw/bv`
   - Models: `edw_bv__{vault object named as per data vault standards}`

#### Infomart

**Staging:**

   - Folder: `models/infomart/{optional: domain name}{optional: __subdomain name(s)}/mart/{entity / product description}/stg`
   - Models: `{optional: domain name} {optional: __subdomain name(s)} {optional:__im__} stg{__entity / product description} __{ordinal}_{transformation description} {optional: __{source_system} __{source_channel}}`

**Dimensions:** 

   - Folder: `models/infomart/{optional: domain name}{optional: __subdomain name(s)}/mart/{entity / product description}`
   - Models: `{optional: domain name} {optional: __subdomain name(s)} {optional:__im__} dim_{__entity / product description} (optional: __{source_system} __{source_channel})`

**Facts:** 

   - Folder: `models/infomart/{optional: domain name}{optional: __subdomain name(s)}/mart/{entity / product description}`
   - Models: `{optional: domain name} {optional: __subdomain name(s)} {optional:__im__} fact_{__entity / product description} (optional: __{source_system} __{source_channel})`

**Denormalized (One Big Table):** 

   - Folder: `models/infomart/{optional: domain name}{optional: __subdomain name(s)}/mart/{entity / product description}`
   - Models: `{optional: domain name} {optional: __subdomain name(s)} {optional:__im__} obt_{__entity / product description} {optional: __{source_system} __{source_channel}}`




#### Example dbt model structure:
The model structure below reflects a single catalog for domain+environment and schema separation for zones and layers:

```md
{{domain/enterprise} _project_name}
├── README.md
├── analyses
├── seeds  <ods>
│   └── ref_country_codes.csv  <e.g: corporate__dev.ods.ref_country_codes>
│   └── config_holidays.csv  <e.g: corporate__dev.ods.config_holidays>
├── dbt_project.yml
├── macros
│   └── custom_macro.sql
│   ├── utilities
│       └── all_dates.sql
├── models/raw
│   /{optional: domain and subdomains}
│   ├── _raw.md
│   ├── ods  <ods>
│   |   └── (typically empty - ODS populated by seeds/sources)
│   └── pds  <pds>
│       └── ref_country_codes_snapshot.sql  <e.g: corporate__dev.pds.ref_country_codes_snapshot>
│       └── config_holidays_snapshot.sql  <e.g: corporate__dev.pds.config_holidays_snapshot>
├── models/edw/
│   ├── _edw.md
│   ├── mart  (entity centric base marts) <edw> {optional: domain, subdomains and or subject areas}
│   /{optional: domain and subdomains}
│   |    └── account
│   |    |   └── dim_account.sql  <e.g: corporate__dev.edw.dim_account>
│   |    |       └── stg
│   |    |           └── stg__dim_account__01_dedupe.sql  <e.g: corporate__dev.edw.stg__dim_account__01_dedupe>
│   |    |           └── stg__dim_account__02_filter.sql  <e.g: corporate__dev.edw.stg__dim_account__02_filter>
│   |    └── date
│   |        └── dim_date.sql  <e.g: corporate__dev.edw.dim_date>
│   ├── ref  (reference data) <edw_ref>
│       ├── _reference_data__models.yml
│       ├── _reference_data__sources.yml
│       └── ref_{entity}.sql  <e.g: corporate__dev.edw_ref.ref_facility>
│   ├── stg (source centric staging objects) <edw_stg>
│   |   ├── source_system_1 
│   |   |   ├── _source_system_1__docs.md
│   |   |   ├── _source_system_1__models.yml
│   |   |   ├── stg__source_system_1__object.sql  <e.g: corporate__dev.edw_stg.stg__source_system_1__object>
│   |   |   ├── stg__source_system_1__new_object.sql  <e.g: corporate__dev.edw_stg.stg__source_system_1__new_object>
│   |   |   └── stg
│   |   |       ├── stg__source_system_1__object__01_renamed_and_typed.sql  <e.g: corporate__dev.edw_stg.stg__source_system_1__object__01_renamed_and_typed>
│   |   |       └── stg__source_system_1__object__02_cleaned.sql  <e.g: corporate__dev.edw_stg.stg__source_system_1__object__02_cleaned>
│   |   └── source_system_2__adf
│   |       ├── _source_system_2__adf__docs.md
│   |       ├── _source_system_2__adf__models.yml
│   |       ├── stg__source_system_2__adf__accounts.sql  <e.g: corporate__dev.edw_stg.stg__source_system_2__adf__accounts>
│   |       └── stg
│   |           ├── stg__source_system_2__adf__accounts__01_renamed_and_typed.sql  <e.g: corporate__dev.edw_stg.stg__source_system_2__adf__accounts__01_renamed_and_typed>
│   |           └── stg__source_system_2__adf__accounts__02_cleaned.sql  <e.g: corporate__dev.edw_stg.stg__source_system_2__adf__accounts__02_cleaned>
│   ├── sources
│        └── {optional: domain}
│        └── {optional: raw/edw/infomart}
│             └── _source_system_1__sources.yml 
├── models/infomart  <im>
│   /{optional: domain and subdomains}
│   ├── _infomart.md
│   └── domain_name e.g: finance 
│       └── mart
│           ├── _finance__models.yml
│           ├── orders.sql  <e.g: corporate__dev.im.orders>
│           └── payments.sql  <e.g: corporate__dev.im.payments>
│               └── stg  <im_stg>
│                   └── stg_payments_pivoted_to_orders.sql  <e.g: corporate__dev.im_stg.stg_payments_pivoted_to_orders>
├── packages.yml
├── snapshots
└── tests
    └── assert_positive_value_for_total_amount.sql
```

### dbt_project.yml
The yml structure below reflects a single catalog for domain+environment and schema separation for zones and layers:

```yml
models:
  health_lakehouse__engineering__dbt:
    +persist_docs: #enables injection of metadata into unity catalog
        relation: true
        columns: true

    raw:
      ods:
        +description: "Operational data store - current state"
        +schema: ods
        +materialized: table
      pds:
        +description: "Persistent data store - historical snapshots (Type 2 SCD)"
        +schema: pds
        +materialized: table

    edw:
      ref:
        +description: "Reference data (conformed lookups, master data)"
        +schema: edw_ref
        +materialized: table
      stg:
        +description: "Source-centric staging objects"
        +schema: edw_stg
        +materialized: view
      mart:
        +description: "Entity-centric base marts (dimensions, facts)"
        +schema: edw
        +materialized: table
        stg:
          +description: "Entity-centric staging (intermediate transformations)"
          +schema: edw
          +materialized: view

    infomart:
      +materialized: view # default for speed
      mart:
        +description: "Information marts (business-aligned products)"
        +schema: im
        +materialized: table
        stg:
          +description: "Information mart staging (intermediate transformations)"
          +schema: im_stg
          +materialized: view

seeds:
  health_lakehouse__engineering__dbt:
    +schema: ods
    +description: "Static reference and configuration data loaded from CSV files to ODS"
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

