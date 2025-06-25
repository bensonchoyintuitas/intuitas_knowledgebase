# Naming Conventions
{Return to home}(README.md)

<br>

## Table of Contents
---


   - [Mesh](naming_standards_and_conventions.md#mesh)
      - [Domain Names](naming_standards_and_conventions.md#domain-names)
   - [Platform](naming_standards_and_conventions.md#platform)
      - [Environment](naming_standards_and_conventions.md#environment)
      - [VNET](naming_standards_and_conventions.md#vnet)
      - [Resource Groups](naming_standards_and_conventions.md#resource-groups)
      - [Databricks workspace](naming_standards_and_conventions.md#databricks-workspace)
      - [Key vault](naming_standards_and_conventions.md#key-vault)
      - [Secrets](naming_standards_and_conventions.md#secrets)
      - [Entra Group Names](naming_standards_and_conventions.md#entra-group-names)
      - [Azure Data Factory (naming_standards_and_conventions.mdADF)](naming_standards_and_conventions.md#azure-data-factory-adf)
      - [SQL Server](naming_standards_and_conventions.md#sql-server)
      - [SQL Database](naming_standards_and_conventions.md#sql-database)
      - [Storage](naming_standards_and_conventions.md#storage)
         - [Lakehouse Storage](naming_standards_and_conventions.md#lakehouse-storage)
         - [Lakehouse Storage Containers](naming_standards_and_conventions.md#lakehouse-storage-containers)
         - [Lakehouse Storage Folders](naming_standards_and_conventions.md#lakehouse-storage-folders)
         - [Generic Blob Storage](naming_standards_and_conventions.md#generic-blob-storage)
         - [Generic Blob Files and Folders](naming_standards_and_conventions.md#generic-blob-files-and-folders)
   - [Databricks](naming_standards_and_conventions.md#databricks)
      - [Workspace and Cluster Names](naming_standards_and_conventions.md#workspace-and-cluster-names)
      - [Catalog](naming_standards_and_conventions.md#catalog-naming-and-conventions)
      - [Schema and Object Conventions](naming_standards_and_conventions.md#schema-and-object-conventions)
      - [Delta Sharing](naming_standards_and_conventions.md#delta-sharing)
   - [Azure Data Factory](naming_standards_and_conventions.md#azure-data-factory)
   - [Streaming](naming_standards_and_conventions.md#streaming)
   - [dbt](naming_standards_and_conventions.md#dbt)
      - [Documentation and Model Metadata](naming_standards_and_conventions.md#documentation-and-model-metadata)
      - [Sources](naming_standards_and_conventions.md#sources)
      - [Model and Folder Names](naming_standards_and_conventions.md#model-and-folder-names)
      - [dbt_project.yml](naming_standards_and_conventions.md#dbt_projectyml)
   - [CI/CD](naming_standards_and_conventions.md#cicd)
      - [Repository Naming](naming_standards_and_conventions.md#repository-naming)
      - [Branch Naming](naming_standards_and_conventions.md#branch-naming)
      - [Branch Lifecycle](naming_standards_and_conventions.md#branch-lifecycle)
      - [Databricks Asset Bundles](naming_standards_and_conventions.md#databricks-asset-bundles)
   - [Security](naming_standards_and_conventions.md#security)
      - [Entra Group Names](naming_standards_and_conventions.md#entra-group-names)
      - [Policies](naming_standards_and_conventions.md#policies)
      - [Frameworks](naming_standards_and_conventions.md#frameworks)
<br>

## Mesh

### Domain Names

All lower case: {optional:organisation_}{functional area/domain}_{subdomain}

   *e.g. intuitas_corporate*

<br>

## Platform

### Environment

- Environment: dev/test/prod/sandbox/poc (pat - production acceptance testing is optional as prepred)


### VNET

- Name: vn-{organisation_name}-{domain_name}

   *e.g. vn-intuitas-corporate*

### Resource Groups

- Name: rg-{organisation_name}-{domain_name}

   *e.g. rg-intuitas-corporate*

### Databricks workspace

- Name: ws-{organisation_name}-{domain_name}
   
   *e.g. ws-intuitas-corporate*

### Key vault

- Name: kv-{organisation_name}-{domain_name}

   *e.g. kv-intuitas-corporate*

### Secrets

- Name: {secret_name}


### Entra Group Names

- Name: eg-{organisation_name}-{domain_name}

   *e.g. eg-intuitas-corporate*


### Azure Data Factory (ADF)

- Name: adf-{organisation_name}-{domain_name}

   *e.g. adf-intuitas-corporate*

### SQL Server

- Name: sql-{organisation_name}-{domain_name}

   *e.g. sql-intuitas-corporate*

### SQL Database

- Name: sqldb-{purpose}-{organisation_name}-{domain_name}-{optional:environment}

   *e.g. sqldb-metadata-intuitas-corporate*


### Storage

#### Lakehouse storage

- Lakehouse storage account name: dl{organisation_name}{domain_name}

#### Lakehouse storage containers
- Name: {environment} (dev/test/preprod/prod)

##### Lakehouse storage folders
- Level 1 Name: {layer} (bronze/silver/gold) // if using medallion approach
- Level 2 Name: {stage_name}
   - bronze/landing
   - bronze/ods
   - bronze/pds
   - bronze/schema (for autoloader metadata)
   - bronze/checkpoints (for autoloader metadata)
   - silver/automatically determined by unity catalog
   - gold/automatically determined by unity catalog

#### Generic Blob storage

Generic Blob storage can be used for all non-lakehouse data; or alternatively within the lakehouse storage account in the appropriate container and folder.

- Resource: ADLSGen2
- Generic storage account name: sa{organisation_name}{domain_name}{functional_description}
- Tier: Standard/Premium (depends on workload)
- Redundancy: 
   - Minimum ZRS or GRS for prod
   - Minimum LRS for poc, dev, test and preprod

#### Generic Blob files and folders

No standard naming conventions for files and folders.


<br>

## Databricks

### Workspace and cluster names

- Workspace name: ws-{organisation_name}_{domain_name}
- Cluster name: {personal_name/shared_name} Cluster
- Workflow name: {dev/test} {workflow_name}

### Catalog naming and conventions

Refer to [Data layers and stages](level_2.md#data-layers-and-stages) for further context and definitions applicable to this section.

All lower case:

- Catalog name: 
   - Minimum granularity {domain_name}{_environment (dev/test/pat/prod)} (prod is implied optional)   *e.g. intuitas_corporate_dev*
   - Optional granularity {domain_name}{_data_stage: (bronze/silver/gold)}{_environment (dev/test/pat/prod)}    *e.g. intuitas_corporate_bronze_dev*
   - Optional granularity {domain_name}{_descriptor (subdomain/subject/project*)}(bronze/silver/gold)}{_environment (dev/test/pat/prod)}    *e.g. intuitas_corporate_finance_bronze_dev*

   *Note that projects are temporary constructs, and hence are not recommended for naming*

- Catalog storage root: abfss://{environment}@dl{organisation_name}{domain_name}.dfs.core.windows.net/{domain_name}_{environment}_catalog

### Externally mounted (lakehouse federation) Catalog Names

- Catalog name:
   - {domain_name (owner)} _ext__{source_system}{optional:__other_useful_descriptors e.g._environment}

   *e.g. intuitas_corporate_ext__sqlonpremsource*

### Catalog Metadata tags:
   - Key: domain (owner): {domain_name}
   - Key: environment: {environment}
   - Key: managing_domain: {domain_name} e.g. if delegating to engineering domain

### Schema and object conventions

Refer to [Data layers and stages](level_2.md#data-layers-and-stages) for further context and definitions applicable to this section.

#### Schema level external storage locations

Recommendations:

- For managed tables (default): do nothing.  Let dbt create schemas without additional configuration. Databricks will manage storage and metadata.Objects will then be stored in the catalog storage root.

   *e.g. abfss://dev@dlintutiasengineering.dfs.core.windows.net/intuitas_engineering_dev_catalog/__unitystorage/catalogs/catalog-guid/tables/object-guid*

- For granular control over schema-level storage locations: Pre-create schemas with LOCATION mapped to external paths or configure the catalog-level location.
- Ensure dbt's dbt_project.yml and environment variables align with storage locations.


#### Metadata Schemas and Objects

Refer to [Data layers and stages](level_2.md#data-layers-and-stages) for further context and definitions applicable to this section.

Contains metadata that supports engineering and governance. This will vary depending on engineering and governance toolsets

1. **Engineering - ingestion framework**:
   - Schema naming convention:  `meta__{optional: function}`
   - Naming convention: `{function/descriptor}`

   *e.g. intuitas_corporate_dev.meta__ingestion.ingestion_control*

#### Bronze (Raw data according to systems)
The Bronze layer stores raw, immutable data as it is ingested from source systems. See [Data layers and stages](level_2.md#data-layers-and-stages) for definitions and context.

All schemas are may be optionally prefixed with `bronze__`

1. **Persistent Landing**:
   - N/A (see file naming)

2. **Operational Data Store (ODS)**:
   - Schema naming convention: `ods{optional: __domain name}{optional: __subdomain name(s)}`
   - Object naming convention: `ods__{source_system}__{source_channel}__{object}`

   *e.g. intuitas_corporate_dev.ods.ods__finance_system__adf__accounts*

3. **Persistent Data Store (PDS)**:
   - Schema naming convention: `pds{optional: __domain name}{optional: __subdomain name(s)}`
   - Object naming convention: `pds__{source_system}__{source_channel}__{object}`

   *e.g. intuitas_corporate_dev.pds.pds__finance_system__adf__accounts*


#### Silver (Data according to business entities)

The Silver layer focuses on transforming raw data into cleaned, enriched, and validated datasets that are the building blocks for downstream consumption and analysis.

Refer to [Data layers and stages](level_2.md#data-layers-and-stages) for further context and definitions applicable to this section.

These marts are objects that are aligned to business entities and broad requirements, hence they must contain source-specific objects at the lowest grain. There may be further enrichment and joins applied across sources.

- All schemas are may be optionally prefixed with `silver`
- All `entity` names which align to facts should be named in plural.
- All `entity` names which align to dims should be named in singular.

1. **(Silver) Staging Objects**:
   Staging models serve as intermediary models that transform source data into the target silver model. According to dbt best practices, there is a distinction between Staging and Intermediate models. Under this blueprint the use of Intermediate models is optional. [Reference](https://docs.getdbt.com/best-practices/how-we-structure/1-guide-overview)
   
   These models exist to stage silver marts only.

   - Source-specific:

      - Schema naming convention: `stg__{source_system}__{source_channel}`
      - Object naming convention: `{entity}__{object_description}__{n}__{transformation}__{source_system}__{source_channel}`

         - *e.g. intuitas_corporate_dev.stg__new_finance_system__adf.accounts__01_renamed_and_typed__new_finance_system__adf*
         - *e.g. intuitas_corporate_dev.stg__new_finance_system__adf.accounts__02_cleaned__new_finance_system__adf*

         - *e.g. intuitas_corporate_dev.stg__old_finance_system__adf.accounts__01_renamed_and_typed__old_finance_system__adf*
 
   - Non-source specific

      - Schema naming convention: `stg{optional: __domain name}{optional: __subdomain name(s)}`
      - Object naming convention to align with target mart: `stg__(optional:dim/fact)_{entity}__{object_description}__{n}__{transformation}`

         - *e.g. intuitas_corporate_dev.stg.accounts__01_deduped*
         - *e.g. intuitas_corporate_dev.stg.accounts__02_business_validated* 

   - Examples of transformations:

      - `01_renamed_and_typed`
      - `02_deduped`
      - `03_cleaned`
      - `04_filtered/split`
      - `05_column_selected`
      - `06_business_validated`
      - `07_desensitised`

   *e.g. intuitas_corporate_dev.stg__finance_system__adf.stg__finance_system__adf__account__01_renamed_and_typed*

3. **(Silver) Marts**:

   Final products after staging

   - Source-specific:

      - Schema naming convention: `mart__{source_system}__{source_channel}`
      - Object naming convention: `(optional:dim/fact)_{entity / _object_description}__{source_system}__{source_channel}`

         - *e.g. intuitas_corporate_dev.mart__new_finance_system__adf.payment__new_finance_system__adf*
         - *e.g. intuitas_corporate_dev.mart__new_finance_system__adf.account__new_finance_system__adf*
         - *e.g. intuitas_corporate_dev.mart__old_finance_system__adf.account__old_finance_system__adf*

   - Non-source specific

      - Schema naming convention: `mart{optional: __domain name}{optional: __subdomain name(s)}`
      - Object naming convention: `(optional:dim/fact)__{unified entity / _object_description}`

         - *e.g. intuitas_corporate_dev.mart.account* (unified)
         - *e.g. intuitas_corporate_dev.mart__corporate__finance.account* (unified)
         - *e.g. intuitas_corporate_dev.mart__finance.account* (unified)
         - *e.g. intuitas_corporate_dev.mart.account_join_with_payments* (joined across two systems)

4. **Reference Data**:

   Reference data objects that are aligned to business entities and broad requirements. These may also be staged in stg as per silver marts. These are typically not source-aligned but optionality for capturing sources exists.

   - Schema naming convention: `ref{optional: __domain name}{optional: __subdomain name(s)}`
   - Object naming convention: `{reference data set name} (optional:__{source_system}__{source_channel})`

   *e.g. intuitas_corporate_dev.ref.account_code*

5. **Raw Vault**:

   Optional warehousing construct.

   - Schema naming convention: `edw_rv`
   - Object naming convention: `{vault object named as per data vault standards}`

   *e.g. intuitas_corporate_dev.edw_rv.hs_payments__finance_system__adf*

6. **Business Vault**:

   - Schema naming convention: `edw_bv`
   - Object naming convention: `{vault object named as per data vault standards}`

   *e.g. intuitas_corporate_dev.edw_bv.hs_late_payments__finance_system__adf*

#### Gold (Data according to requirements)

The Gold layer focuses on requirement-aligned products (datasets, aggregations, and reporting structures). Products are predominantly source agnostic, however optionality exists in case its needed.

Refer to [Data layers and stages](level_2.md#data-layers-and-stages) for further context and definitions applicable to this section.

- All schemas are may be optionally prefixed with `gold`
- All `entity` names which align to facts should be named in plural.
- All `entity` names which align to dims should be named in singular.


2. **(Gold) Staging Models**:
   Staging models serve as intermediary models that transform source data into the target mart model. According to dbt best practices, there is a distinction between Staging and Intermediate models. Under this blueprint the use of Intermediate models is optional. [Reference](https://docs.getdbt.com/best-practices/how-we-structure/1-guide-overview)

   These models exist to stage gold marts only.

   - Schema naming convention: `stg__{optional: __domain name}{optional: __subdomain name(s)}`
   - Object naming convention to align to target mart: `(optional: dim/fact__){entity / product description}__{n}__{transformation}`

   *e.g. intuitas_corporate_dev.stg.fact__late_payments__01__pivoted_by_order*
   *e.g. intuitas_corporate_dev.stg__corporate.fact__late_payments__01__pivoted_by_order*
   *e.g. intuitas_corporate_dev.stg__corporate__finance.fact__late_payments__01__pivoted_by_order*


3. **(Gold) Marts**:

   - Schema naming convention: `mart{optional: __domain name}{optional: __subdomain name(s)}`
   - Dimension naming convention: `dim__{entity / product description} (optional: __{source_system}__{source_channel})`
   - Fact naming convention: `fact__{entity / product description} (optional: __{source_system}__{source_channel})`
   - Denormalized (One Big Table) Object naming convention: `{entity / product description} (optional: __{source_system}__{source_channel})`


   - Required transformation: Business-specific transformations such as:
     - `pivoting`
     - `aggregation`
     - `joining`
     - `funnel creation`
     - `conformance`
     - `desensitization`

   *e.g. intuitas_corporate_dev.mart.fact__late_payments*

   *e.g. intuitas_corporate_dev.mart.regionally_grouped_account_payments__old_finance_system__adf*

   *e.g. intuitas_corporate_dev.mart.regionally_grouped_account_payments__new_finance_system__adf*

   *e.g. intuitas_corporate_dev.mart.regionally_grouped_account_payments* (union of old and new)

### Delta Sharing

   - Share names: {domain_name}__{optional:subdomain_name}__{optional:purpose}__{schema_name or description}__{object_name or description}__{share_purpose and or target_audience}

   *e.g. intuitas_corporate__finance__reporting__account_payments__payments*


<br>

## Azure Data Factory

   The following are in lower case:

   - Linked service names: ls_{database_name}(if not in database_name:{_organisation_name}_{domain_name})
   *e.g. ls_financedb_intuitas_corporate*

   - Dataset names: ds_{database_name}_{object_name}
   - Pipeline names: pl_{description: e.g copy_{source_name}_to_{destination_name}}
   - Trigger names: tr_{pipeline_name}_{optional:start_time / frequency}


<br>  

## Streaming

The following are in lower case:

- Cluster name: {domain_name}__cluster__{optional:environment}
- Topic names: {domain_name}__{object/entity?}__{optional:source_system}___{optional:source_channel}__{optional:environment}
- Consumer group names: {domain_name}__{unique_group_name}__{optional:environment}



<br>  

## dbt

The following are in lower case:

### Documentation and model metadata

Within each respective model folder (as needed)

* md: _{path to model folder using _ separators}__docs.md 

   *e.g. models/silver/ambo_sim__kafka__local/_silver__ambo_sim__kafka__local__docs.md*

* model yml: _{path to model folder using _ separators}__models.yml 

   *e.g. models/silver/ambo_sim__kafka__local/_silver__ambo_sim__kafka__local__models.yml*

### Sources
* Folder: models/sources/{bronze/silver/gold}
* yml: {schema}__sources.yml (one for each source schema) 

   *e.g. bronze__ods__ambo_sim__kafka__local__sources.yml*

### Model and Folder Names

dbt model names are verbose (inclusive of zone and domain) to ensure global uniqueness and better traceability to folders. Actual object names should be aliased to match object naming standards.

#### Bronze
*Bronze objects are likely to be referenced in sources/bronze or as seeds*

- Folder: `models/bronze/{optional: domain name}{optional: __subdomain name(s)}/`
- Folder: `sources/bronze/{optional: domain name}{optional: __subdomain name(s)}/`
- Folder: `seeds/{optional: domain name}{optional: __subdomain name(s)}/`

#### Silver

- Staging Source-specific: 

   - Folder: `models/silver/{optional: domain name}{optional: __subdomain name(s)}/stg/{source_system}__{source_channel}/`
   - Models: `{optional: domain name} {optional: __subdomain name(s)} {optional:__silver} __stg{__entity /_object_description} __{ordinal}_{transformation description} __{source_system} __{source_channel}`

```md
   Example:

   `silver\new_finance_system__adf\stg\intuitas_corporate__silver__stg__accounts__01_renamed_and_typed__new_finance_system__adf.sql`
   or

   `silver\new_finance_system__adf\stg\stg__accounts__01_renamed_and_typed__new_finance_system__adf.sql`
   
   materialises to:
   *intuitas_corporate_dev.stg__new_finance_system__adf.accounts__01_renamed_and_typed__new_finance_system__adf*
```


- Staging Non-source-specific (entity centric): 

   - Folder: `models/silver/{optional: domain name}{optional: __subdomain name(s)}/mart/{entity}/stg`
   - Models: `{optional: domain name} {optional: __subdomain name(s)} {optional:__silver__} stg{__optional:dim/fact}{__entity /_object_description} __{ordinal}_{transformation description} `

      - *e.g. intuitas_corporate_dev.stg.accounts__01_deduped*
      - *e.g. intuitas_corporate_dev.stg.accounts__02_business_validated* 


```md
   Example:

   `silver\mart\accounts\stg\intuitas_corporate__silver__stg__accounts__01_deduped.sql`
   or

   `silver\mart\accounts\stg\stg__accounts__01_deduped.sql`
   
   materialises to:

   *e.g. intuitas_corporate_dev.stg.accounts__01_deduped*
```

- Mart Source-specific: 

   - Folder: `models/silver/{optional: domain name}{optional: __subdomain name(s)}/mart/{entity}`
   - Models: `{optional: domain name} {optional: __subdomain name(s)} {optional:__silver__} mart{__optional:dim/fact}{__entity /_object_description}__{source_system}__{source_channel}`

- Mart Non-source specific: 

   - Folder: `models/silver/{optional: domain name}{optional: __subdomain name(s)}/mart/{entity}`
   - Models: `{optional: domain name} {optional: __subdomain name(s)} {optional:__silver__} mart{__optional:dim/fact}{__unified entity /_object_description}`

- Reference Data: 

   - Folder: `models/silver/{optional: domain name}{optional: __subdomain name(s)}/ref/{entity}`
   - Models: `{optional: domain name} {optional: __subdomain name(s)} {optional:__silver__} ref{__optional:dim/fact} {__reference data set name} {optional:__{source_system}__{source_channel}}`

- Raw Vault: 

   - Folder: `models/silver/{optional: domain name}{optional: __subdomain name(s)}/edw/rv`
   - Models: `edw_rv__{vault object named as per data vault standards}`

- Schema naming convention: 

   - Folder: `models/silver/{optional: domain name}{optional: __subdomain name(s)}/edw/bv`
   - Models: `edw_bv__{vault object named as per data vault standards}`

#### Gold

-  Staging: 

   - Folder: `models/gold/{optional: domain name}{optional: __subdomain name(s)}/mart/{entity / product description}/stg`
   - Models: `{optional: domain name} {optional: __subdomain name(s)} {optional:__gold__} mart__stg{__entity / product description} __{ordinal}_{transformation description} {optional: __{source_system} __{source_channel}}`

-  Dimensions: 

   - Folder: `models/gold/{optional: domain name}{optional: __subdomain name(s)}/mart/{entity / product description}`
   - Models: `{optional: domain name} {optional: __subdomain name(s)} {optional:__gold__}  mart__dim{__entity / product description} (optional: __{source_system} __{source_channel}) {optional: __{source_system} __{source_channel}}`

-  Facts: 

   - Folder: `models/gold/{optional: domain name}{optional: __subdomain name(s)}/mart/{entity / product description}`
   - Models: `{optional: domain name} {optional: __subdomain name(s)} {optional:__gold__} mart__fact{__entity / product description} (optional: __{source_system} __{source_channel}) {optional: __{source_system} __{source_channel}}`

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
│   └── domain_name e.g. finance 
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

### Repository naming 

- All lowercase with hyphens as separators
- Format: {org}-{domain}-{purpose}-{optional:descriptor}

   Examples:
   - intuitas-corporate-dbt
   - intuitas-corporate-ingestion-framework  
   - intuitas-corporate-cicd-templates
   - intuitas-corporate-infrastructure

### Branch naming

- All lowercase with hyphens as separators
- Naming convention: {type}-{optional:ticket-id}-{description}

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
   - {domain_name}__databricks (for general databricks projects)
   - {domain_name}__dbt (for dbt databricks bundles)

- Bundle tags:
   - Key: environment: {environment}
   - Key: manager: {team_name} and or {email_address}
   - Key: managing_domain: {domain_name} e.g. if delegating to engineering domain
   - Key: owner: {owner_name}
   - Key: owning_domain: {domain_name}
   - Key: dab: {bundle_name}
   - Key: project: {project_name}

   e.g.
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
   - Folder level 1: {meaningful sub-project name}
   - Folder level 2: 
      - notebooks
      - workflows

- Databricks.yml
   - For both dev and prod: root_path: /Workspace/Users/engineering-engineer@intuitas.com/.bundle/${bundle.name}/${bundle.target}

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

### Entra Group Names
TBC

### Policies
- **Data Retention**
- **Key Retention**

### Frameworks
- **Engineering**
- **Security**

