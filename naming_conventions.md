# Naming Conventions
[Return to home](README.md)

## Mesh
### Domain Names

All lower case: {organisation}_{functional area/domain}

*e.g. intuitas_domain3*

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
All lower case: {Domain}_{dev/test/prod/ext}

*e.g. intuitas_domain3_dev*

### External (federated) Catalog Names
All lower case: {Domain}_ext__{source_system}

*e.g. intuitas_domain3_ext__sqlonpremsource*


### Schema and object names

* Bronze schema naming: bronze__{sub-zone: ods/pds}__{source name}__{source channel}

* Silver schema naming: silver__{source name}__{source name}__{source channel}

* Silver edw schema naming: silver__{edw}__{domain_name}

* Gold schema naming: gold__{domain name} optional: __{subdomain name(s)}




- **Table Names**

## Streaming
- **Topic Names**

## Storage Account
- **File**
- **Folder**

## dbt
* All lower case

### Documentation and Model Metadata

Within each respective model folder (as needed)

* md: _{path to model folder using _ separators}__docs.md *e.g. models/silver/ambo_sim__kafka__local/_ambo_sim__kafka__local__docs.md*

* model yml: _{path to model folder using _ separators}__models.yml *e.g. models/silver/ambo_sim__kafka__local/_ambo_sim__kafka__local__models.yml*

### Sources
* Folder: models/sources/[bronze/silver/gold]
* yml: {schema}__sources.yml (one for each source schema) *e.g. bronze__ods__ambo_sim__kafka__local__sources.yml*

### Model Names
* Bronze object naming: {source object name}
* Silver base object naming: base__{source name}__{source channel}__{source object name}
* Silver stage object naming: stg__{source name}__{source channel}__{source object name}__{ordinal}_{transformation description}
* Silver enriched object naming: enr__  optional:__{source name}  optional:__{source channel} __{description} // alternatively - just use stg or edw
* Silver edw object naming: edw___{domain_name}__{description}
* Gold object name: mart__{domain name} optional: __{subdomain name(s)}__{description}

### Zone and stage folder structures

```yml
models/bronze/
models/silver/{source system}/{base/staging/enriched/edw}
models/silver/{edw}__{domain_name}
models/gold/{domain_name}/{intermediate/marts}

sources/bronze/
sources/silver/
sources/gold/
```

#### yml
```example
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
.....
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

