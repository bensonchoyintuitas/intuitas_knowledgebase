# Naming Conventions
[Return to home](README.md)

## Table of Contents
- [Mesh](#mesh)
   - [Domain Names](#domain-names)
- [Platform](#platform)
   - [Cloud Resource Names](#cloud-resource-names)
   - [Storage Account Names](#storage-account-names)
   - [File Names](#file-names)
   - [Folder Names](#folder-names)
- [Databricks Engineering](#databricks-engineering)
   - [Workspace Names](#workspace-names)
   - [Workflow Names](#workflow-names)
   - [Cluster Names](#cluster-names)
- [Databricks Catalog](#databricks-catalog)
   - [Catalog Names](#catalog-names)
   - [External (federated) Catalog Names](#external-federated-catalog-names)
   - [Schema and object names](#schema-and-object-names)
   - [Metadata](#metadata)
   - [Bronze (Raw)](#bronze-raw)


## Mesh
### Domain Names

All lower case: {optional:organisation_}{functional area/domain}_{subdomain}

   *e.g. intuitas_domain3_dev*

## Platform
- **Cloud Resource Names**
- **Storage Account Names**
- **File Names**
- **Folder Names**


## Databricks Engineering
- **Workspace Names**
- **Workflow Names**
- **Cluster Names**

## Databricks Catalog
### Catalog Names
All lower case: {Domain}_{dev/test/prod}

   *e.g. intuitas_domain3_dev*

### External (federated) Catalog Names
All lower case: {Domain}_ext__{source_system}

   *e.g. intuitas_domain3_ext__sqlonpremsource*


### Schema and object names

#### Metadata

Contains metadata that supports engineering and governance. This will vary depending on engineering and governance toolsets

1. **Engineering - ingestion framework**:
   - Schema naming convention:  `meta__[optional: function]`
   - Naming convention: `[function/descriptor]`

   *e.g. intuitas_domain3_dev.meta__ingestion.ingestion_control*

#### Bronze (Raw)
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

## Streaming
- **Topic Names**

## Storage Account
- **File**
- **Folder**





## dbt
All the following are in lower case:

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



### Model Folders

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


## CI/CD
- Repo naming 
- **Databricks Asset Bundles**

## Security
- **Key Vault Names**
- **Secret Names**
- **Entra Group Names**

## Policies
- **Data Retention**
- **Key Retention**

## Frameworks
- **Engineering**
- **Security**

