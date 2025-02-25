<a href="img/intuitas.png" target="_blank">
    <img src="img/intuitas.png" width="200" alt="Intuitas Logo">
</a> 
<br>
<br>


# Intuitas knowledgebase

This knowledgebase is a collection of articles, tutorials, and other resources that describe Intuitas' approach to designing and delivering Data and AI solutions.


## Copyright

This knowledge base and its contents are © Intuitas PTY LTD, 2025. All rights reserved. Any referenced or third-party materials remain the property of their respective copyright holders. Every effort has been made to accurately reference and attribute existing content, and no claim of ownership is made over such materials.

## Licence

Permission is granted for free use, reproduction, and adaptation of this material, provided prior consent is obtained and appropriate attribution is given to the original author(s). Referenced third-party content is subject to the copyright terms of their respective owners.

# Table of Contents


## Level 0 - Organisational and Domain-Level Context
- [Org + Domain Definition](level_0.md#org-domain-definition)
- [Strategies and Objectives](level_0.md#strategies-and-objectives)
- [Key Systems and Data Assets](level_0.md#key-systems-and-data-assets)
- [Team Capabilities and Roles](level_0.md#team-capabilities-and-roles)
- [Governance Structures](level_0.md#governance-structures)

## Level 1 - Enterprise-Level architectures
- [Enterprise Domain Topology](level_1.md#enterprise-domain-topology)
    - [Key concepts](level_1.md#key-concepts)
        - [Domain](level_1.md#domain)
        - [Subdomain](level_1.md#subdomain)
        - [Domain-Centric Design](level_1.md#domain-centric-design) 
        - [Data Mesh](level_1.md#data-mesh)
        - [Domain Topology](level_1.md#domain-topology)
        - [Data Fabric](#level_1.md#data-fabric)
        - [Data Mesh vs Fabric](#level_1.md#data-mesh-vs-fabric)
    - [Reference topologies](level_1.md#reference-topologies)
    - [Hybrid federated mesh topology](level_1.md#hybrid-federated-mesh-topology)

- [Enterprise Data Platform Reference Architecture](level_1.md#enterprise-data-platform-reference-architecture)

- [Enterprise (Logical) Data Warehouse Reference Architecture](level_1.md#enterprise-logical-data-warehouse-reference-architecture)
    - [Logical Data Warehouse topology](level_1.md#logical-data-warehouse-topology)

- [Enterprise Information and Data Architecture](level_1.md#enterprise-information-and-data-architecture)

- [Enterprise Metadata Architecture](level_1.md#enterprise-metadata-architecture)
    - [Databricks Unity Catalog Metastore](level_1.md#databricks-unity-catalog-metastore)

- [Enterprise Security](level_1.md#enterprise-security)

## Level 2 - Domain-Level (Solution) architectures
- [Business architecture](level_2.md#business-architecture)
    - [Business processes](level_2.md#business-processes)
    - [Business glossary](level_2.md#business-glossary)
    - [Business metrics](level_2.md#business-metrics)

- [Infrastructure](level_2.md#infrastructure)
    - [Environments, Workspaces + Clusters](level_2.md#environments-workspaces-clusters)
    - [Secrets](level_2.md#secrets)
    - [Storage](level_2.md#storage)
    - [CICD + Repository](level_2.md#cicd-repository)
    - [Observability](level_2.md#observability)
    - [Networking](level_2.md#networking)
    - [Orchestration](level_2.md#orchestration)

- [Data and information models](level_2.md#data-and-information-models)
    - [Domain glossary](level_2.md#domain-glossary)
    - [Domain data and warehouse models](level_2.md#domain-data-warehouse-models)

- [Data Architecture](level_2.md#data-architecture)
    - [Data zones and stages](level_2.md#data-zones-stages)
    - [Lakehouse Catalog to Storage Mapping](level_2.md#lakehouse-catalog-storage-mapping)

- [Data Engineering](level_2.md#data-engineering)
    - [Ingestion](level_2.md#ingestion)
    - [Transformation](level_2.md#transformation)
    - [Delivery](level_2.md#delivery)

- [Data access and governance](level_2.md#data-access-governance)


## Naming standards and conventions
- [Naming standards and conventions](naming_standards_and_conventions.md)
  - [Mesh](naming_standards_and_conventions.md#mesh)
    - [Domain Names](naming_standards_and_conventions.md#domain-names)
  - [Platform](naming_standards_and_conventions.md#platform)
    - [Cloud Resources](naming_standards_and_conventions.md#cloud-resources)
    - [Storage](naming_standards_and_conventions.md#storage)
      - [Lakehouse Storage](naming_standards_and_conventions.md#lakehouse-storage)
      - [Generic Blob Storage](naming_standards_and_conventions.md#generic-blob-storage)
  - [Databricks](naming_standards_and_conventions.md#databricks)
    - [Workspace and Cluster Names](naming_standards_and_conventions.md#workspace-and-cluster-names)
    - [Catalog](naming_standards_and_conventions.md#catalog)
    - [Schema and Object Conventions](naming_standards_and_conventions.md#schema-and-object-conventions)
      - [Metadata](naming_standards_and_conventions.md#metadata)
      - [Bronze (Raw)](naming_standards_and_conventions.md#bronze-raw)
      - [Silver](naming_standards_and_conventions.md#silver)
      - [Gold](naming_standards_and_conventions.md#gold)
- [Azure Data Factory](naming_standards_and_conventions.md#azure-data-factory)
  - [Streaming](naming_standards_and_conventions.md#streaming)
  - [dbt](naming_standards_and_conventions.md#dbt)
    - [Documentation and Model Metadata](naming_standards_and_conventions.md#documentation-and-model-metadata)
    - [Sources](naming_standards_and_conventions.md#sources)
    - [Model Folders](naming_standards_and_conventions.md#model-folders)
    - [Model Names](naming_standards_and_conventions.md#model-names)
  - [CI/CD](naming_standards_and_conventions.md#cicd)
  - [Security](naming_standards_and_conventions.md#security)
  - [Policies](naming_standards_and_conventions.md#policies)
  - [Frameworks](naming_standards_and_conventions.md#frameworks)