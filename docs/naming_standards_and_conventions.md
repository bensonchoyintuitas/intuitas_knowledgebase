# Naming Conventions
[Return to home](README.md)

<br>
## Table of Contents
---

   - [Mesh](#mesh)
      - [Domain Names](#domain-names)
   - [Platform](#platform)
      - [Environment](#environment)
      - [VNET](#vnet)
      - [Resource Groups](#resource-groups)
      - [Databricks workspace](#databricks-workspace)
      - [Key vault](#key-vault)
      - [Secrets](#secrets)
      - [Entra Group Names](#entra-group-names)
      - [Azure Data Factory (ADF)](#azure-data-factory-adf)
      - [SQL Server](#sql-server)
      - [SQL Database](#sql-database)
      - [Storage](#storage)
         - [Lakehouse Storage](#lakehouse-storage)
         - [Lakehouse Storage Containers](#lakehouse-storage-containers)
         - [Lakehouse Storage Folders](#lakehouse-storage-folders)
         - [Generic Blob Storage](#generic-blob-storage)
         - [Generic Blob Files and Folders](#generic-blob-files-and-folders)
   - [Databricks](#databricks)
      - [Workspace and Cluster Names](#workspace-and-cluster-names)
      - [Catalog](#catalog)
      - [Externally Mounted Catalog Names](#externally-mounted-catalog-names)
      - [Schema and Object Conventions](#schema-and-object-conventions)
         - [Schema Level External Storage Locations](#schema-level-external-storage-locations)
         - [Metadata Schemas and Objects](#metadata-schemas-and-objects)
         - [Bronze Schemas and Objects](#bronze-schemas-and-objects)
         - [Silver Schemas and Objects](#silver-schemas-and-objects)
         - [Gold Schemas and Objects](#gold-schemas-and-objects)
      - [Delta Sharing](#delta-sharing)
   - [Azure Data Factory](#azure-data-factory)
   - [Streaming](#streaming)
   - [dbt](#dbt)
      - [Documentation and Model Metadata](#documentation-and-model-metadata)
      - [Sources](#sources)
      - [Model Folders](#model-folders)
      - [Model Names](#model-names)
      - [dbt_project.yml](#dbt_projectyml)
   - [CI/CD](#cicd)
      - [Repository Naming](#repository-naming)
      - [Branch Naming](#branch-naming)
      - [Branch Lifecycle](#branch-lifecycle)
      - [Databricks Asset Bundles](#databricks-asset-bundles)
   - [Security](#security)
      - [Entra Group Names](#entra-group-names)
      - [Policies](#policies)
      - [Frameworks](#frameworks)

<br>
## Mesh
---

### Domain Names

All lower case: {optional:organisation_}{functional area/domain}_{subdomain}

   *e.g. intuitas_domain3*

<br>
## Platform
---

### Environment

- Environment: dev/test/prod/sandbox/poc (pat - production acceptance testing is optional as prepred)


### VNET

- Name: vn-{organisation_name}-{domain_name}

   *e.g. vn-intuitas-domain3*

### Resource Groups

- Name: rg-{organisation_name}-{domain_name}

   *e.g. rg-intuitas-domain3*

### Databricks workspace

- Name: ws-{organisation_name}-{domain_name}
   
   *e.g. ws-intuitas-domain3*

### Key vault

- Name: kv-{organisation_name}-{domain_name}

   *e.g. kv-intuitas-domain3*

### Secrets

- Name: {secret_name}


### Entra Group Names

- Name: eg-{organisation_name}-{domain_name}

   *e.g. eg-intuitas-domain3*


### Azure Data Factory (ADF)

- Name: adf-{organisation_name}-{domain_name}

   *e.g. adf-intuitas-domain3*

### SQL Server

- Name: sql-{organisation_name}-{domain_name}

   *e.g. sql-intuitas-domain3*

### SQL Database

- Name: sqldb-{purpose}-{organisation_name}-{domain_name}-{optional:environment}

   *e.g. sqldb-metadata-intuitas-domain3*


### Storage

#### Lakehouse storage

- Lakehouse storage account name: dl{organisation_name}{domain_name}

#### Lakehouse storage containers
- Name: {environment} (dev/test/preprod/prod)

##### Lakehouse storage folders
- Level 1 Name: {layer} (bronze/silver/gold) // if using medallion approach
- Level 2 Name: {stage_name}
   - bronze/landing
   --- tbc --- might be managed by databricks within the catalog storage root
   - silver/base
   - silver/staging
   - silver/enriched
   - silver/edw_rv
   - silver/edw_bv
   - gold/mart

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
---

### Workspace and cluster names

- Workspace name: ws-{organisation_name}_{domain_name}
- Cluster name: {personal_name/shared_name} Cluster
- Workflow name: {dev/test} {workflow_name}

### Catalog 

- Catalog name: {domain_name}_{environment}

   *e.g. intuitas_domain3_dev*

- Catalog storage root: abfss://{environment}@dl{organisation_name}{domain_name}.dfs.core.windows.net/{domain_name}_{environment}_catalog

### Externally mounted (lakehouse federation) Catalog Names

- All lower case: {Domain (owner)}_ext__{source_system}{optional:__other_useful_descriptors}

   *e.g. intuitas_domain3_ext__sqlonpremsource*

- Metadata tags:
   - Key: domain (owner): {domain_name}
   - Key: environment: {environment}
   - Key: managing_domain: {domain_name} e.g. if delegating to engineering domain

### Schema and object conventions

#### Schema level external storage locations

Recommendations:

- For managed tables (default): do nothing.  Let dbt create schemas without additional configuration. Databricks will manage storage and metadata.Objects will then be stored in the catalog storage root.

   *e.g. abfss://dev@dlintutiasengineering.dfs.core.windows.net/intuitas_engineering_dev_catalog/__unitystorage/catalogs/catalog-guid/tables/object-guid*

- For granular control over schema-level storage locations: Pre-create schemas with LOCATION mapped to external paths or configure the catalog-level location.

- Ensure dbt's dbt_project.yml and environment variables align with storage locations.


#### Metadata Schemas and Objects

Contains metadata that supports engineering and governance. This will vary depending on engineering and governance toolsets

1. **Engineering - ingestion framework**:
   - Schema naming convention:  `meta__[optional: function]`
   - Naming convention: `[function/descriptor]`

   *e.g. intuitas_domain3_dev.meta__ingestion.ingestion_control*

#### Bronze (Raw) Schemas and Objects
The Bronze layer stores raw, immutable data as it is ingested from source systems.

All schemas are may be optionally prefixed with `bronze__`

1. **Persistent Landing**:
   - N/A (see file naming)

2. **Operational Data Store (ODS)**:
   - Schema naming convention: `ods`
   - Object naming convention: `ods__[source_system]__[source_channel]__[object]`

   *e.g. intuitas_domain3_dev.ods.ods__finance_system__adf__accounts*

3. **Persistent Data Store (PDS)**:
   - Schema naming convention: `pds`
   - Object naming convention: `pds__[source_system]__[source_channel]__[object]`

   *e.g. intuitas_domain3_dev.pds.pds__finance_system__adf__accounts*


#### Silver (Source-Centric - Filtered, Cleaned, Augmented)
The Silver layer focuses on transforming raw data into cleaned, enriched, and validated datasets.

All schemas are may be optionally prefixed with `silver__`

1. **Base Views**:
   - Schema naming convention: `base__[source_system]__[source_channel]`
   - Object naming convention: `base__[source_system]__[source_channel]__[object]`

   *e.g. intuitas_domain3_dev.base__finance_system__adf.base__finance_system__adf__accounts*

2. **Staging Objects (Optional)**:
   - Schema naming convention: `stg__[source_system]__[source_channel]`
   - Object naming convention: `stg__[source_system]__[source_channel]__[object]__[n]__[transformation]`
   - Examples of transformations:
     - `01_renamed_and_typed`
     - `02_deduped`
     - `03_cleaned`
     - `04_filtered/split`
     - `05_column_selected`
     - `06_business_validated`
     - `07_desensitised`

   *e.g. intuitas_domain3_dev.stg__finance_system__adf.stg__finance_system__adf__accounts__01_renamed_and_typed*

3. **Enriched Data**:
   - Schema naming convention: `enr__[source_system]__[source_channel]`
   - Object naming convention: `enr__[source_system]__[source_channel]__[new_object]`

   *e.g. intuitas_domain3_dev.enr__finance_system__adf.enr__finance_system__adf__accounts_join_with_payments*

4. **Reference Data**:
   - Schema naming convention: `ref__[source_system]__[source_channel]`
   - Object naming convention: `ref__[source_system]__[source_channel]__[entity]`

   *e.g. intuitas_domain3_dev.ref__finance_system__adf.ref__finance_system__adf__account_codes*

5. **Raw Vault**:
   - Schema naming convention: `edw_rv`
   - Object naming convention: `edw_rv__[vault object]`

   *e.g. intuitas_domain3_dev.edw_rv.hs_payments__finance_system__adf*

#### Gold (Business-Centric - Optionally Source-Decomposed)
The Gold layer focuses on business-ready datasets, aggregations, and reporting structures.

All schemas are may be optionally prefixed with `gold__`

1. **Business Vault**:
   - Schema naming convention: `edw_bv`
   - Object naming convention: `edw_bv__[vault object]`

   *e.g. intuitas_domain3_dev.edw_bv.hs_late_payments__finance_system__adf*

2. **Intermediate Models**:
   - Schema naming convention: `int`
   - Object naming convention: `int__[entity]__[optional_transformation]`
   - Purpose: Business-specific transformations such as:
     - `pivoting`
     - `aggregation`
     - `joining`
     - `funnel creation`
     - `conformance`
     - `desensitization`

   *e.g. intuitas_domain3_dev.int.int__payments_pivoted_to_orders*

3. **Dimensions and Facts**:
   - Schema naming convention: `mart`
   - Dimension naming convention: `dim__[entity (singular)]__[optional_source_system]__[optional_source_channel]`
   - Fact naming convention: `fact__[entity (plural)]__[optional_source_system]__[optional_source_channel]`

   *e.g. intuitas_domain3_dev.mart.dim__account*

   *e.g. intuitas_domain3_dev.mart.fact__payments*

3. **Denormalized Views (One Big Table)**:
   - Schema naming convention: `mart`
   - Object naming convention: `mart__[product]__[optional_source_system]__[optional_source_channel]__[transformation]`

   *e.g. intuitas_domain3_dev.mart.mart__account_payments__old_finance_system__adf*

   *e.g. intuitas_domain3_dev.mart.mart__account_payments__new_finance_system__adf*

   *e.g. intuitas_domain3_dev.mart.mart__account_payments* (union of old and new)


4. **Reference Data**:
   - Schema naming convention: `ref`
   - Object naming convention: `ref__[entity (singular)]`

   *e.g. intuitas_domain3_dev.ref.ref__account_code*

### Delta Sharing

   - Share names: {domain_name}__{optional:subdomain_name}__{optional:purpose}__{schema_name or description}__{object_name or description}__{share_purpose and or target_audience}

   *e.g. intuitas_domain3__finance__reporting__account_payments__payments*




<br>
## Azure Data Factory
---

   The following are in lower case:

   - Linked service names: ls_{database_name}(if not in database_name:{_organisation_name}_{domain_name})
   *e.g. ls_financedb_intuitas_domain3*

   - Dataset names: ds_{database_name}_{object_name}
   - Pipeline names: pl_{description: e.g copy_{source_name}_to_{destination_name}}
   - Trigger names: tr_{pipeline_name}_{optional:start_time / frequency}


<br>  
## Streaming
---

The following are in lower case:

- Cluster name: {domain_name}__cluster__{optional:environment}
- Topic names: {domain_name}__{object/entity?}__{optional:source_system}___{optional:source_channel}__{optional:environment}
- Consumer group names: {domain_name}__{unique_group_name}__{optional:environment}



<br>  
## dbt
---

The following are in lower case:

### Documentation and model metadata

Within each respective model folder (as needed)

* md: _{path to model folder using _ separators}__docs.md 

   *e.g. models/silver/ambo_sim__kafka__local/_ambo_sim__kafka__local__docs.md*

* model yml: _{path to model folder using _ separators}__models.yml 

   *e.g. models/silver/ambo_sim__kafka__local/_ambo_sim__kafka__local__models.yml*

### Sources
* Folder: models/sources/[bronze/silver/gold]
* yml: {schema}__sources.yml (one for each source schema) 

   *e.g. bronze__ods__ambo_sim__kafka__local__sources.yml*

### Model folders

```yml
models/bronze/
models/silver/{source system}/{base/staging/enriched/edw}
models/silver/{edw}__{domain_name}
models/gold/{domain_name}/{intermediate/marts}

sources/bronze/
sources/silver/
sources/gold/
```
### Model Names

* Bronze objects are likely to be referenced in sources/bronze

* Silver base object naming: base__{source name}__{source channel}__{source object name}

* Silver stage object naming: stg__{source name}__{source channel}__{source object name}__{ordinal}_{transformation description}

* Silver enriched object naming: enr__  optional:__{source name}  optional:__{source channel} __{description} // alternatively - just use stg or edw

* Silver edw object naming: edw___{domain_name}__{description}

* Gold object name: mart__{domain name} optional: __{subdomain name(s)}__{description}

#### Example:
```
[[domain/enterprise] _project_name]
├── README.md
├── analyses
├── seeds
│   └── ref_entity_data_file.csv
├── dbt_project.yml
├── macros
│   └── custom_macro.sql
│   ├── utilities
│       └── all_dates.sql
├── models/bronze [domain/enterprise] 
│   ├── _bronze.md
│   ├── [domain/enterprise] sources
├── models/silver
│   ├── _silver.md
│   ├── edw__[domain_name] 
│        └──dim_date
│        └── reference_data
│            ├── _reference_data__models.yml
│            ├── _reference_data__sources.yml
│            └── ref_[entity].sql
│   ├── source_system_1
│   │       ├── _source_system_1__docs.md
│   │       ├── _source_system_1__models.yml
│   │       ├── base
│   │       │   ├── base_source_system_1__object.sql
│   │       │   └── base_source_system_1__deleted_object.sql
│   │       ├── stg_source_system_1__object
│   │       ├── stg_source_system_1__(new object)
│   │       ├── stg_source_system_1__object_desensitised
│   │       ├── stg
│   │       │   ├── stg_source_system_1__object_01step.sql
│   │       │   └── stg_source_system_1__object_02step_.sqll
│   │       └── enrichment (optional)
│   │           ├── enr_source_system_1__object.sql
│   │           ├── enr_source_system_1__object_01step.sql
│   │           └── enr_source_system_1__object_02step.sql
│   ├── sources
│   │       ├── _source_system_1__sources.yml
├── models/gold
│   ├── _gold.md
│   ├── domain_name e.g. finance 
│       └── intermediate (building blocks for marts)
│       |   ├── _int_finance__models.yml
│       |   └── int_payments_pivoted_to_orders.sql
│       ├── marts
│           ├── _finance__models.yml
│           ├── orders.sql
│           └── payments.sql
├── packages.yml
├── snapshots
└── tests
    └── assert_positive_value_for_total_amount.sql
```
### dbt_project.yml

example
```yml
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
---

### Repository naming 

- All lowercase with hyphens as separators
- Format: {org}-{domain}-{purpose}-{optional:descriptor}

   Examples:
   - intuitas-domain3-dbt
   - intuitas-domain3-ingestion-framework  
   - intuitas-domain3-cicd-templates
   - intuitas-domain3-infrastructure

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
---

### Entra Group Names
TBC

### Policies
- **Data Retention**
- **Key Retention**

### Frameworks
- **Engineering**
- **Security**

