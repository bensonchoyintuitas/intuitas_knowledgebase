

<a href="images/intuitas.png" target="_blank">
    <img src="images/intuitas.png" width="200" alt="Intuitas Logo">
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

## Standards and conventions
- [Naming Conventions](naming_conventions.md)
  - [Mesh](naming_conventions.md#mesh)
    - [Domain Names](naming_conventions.md#domain-names)
  - [Platform](naming_conventions.md#platform)
    - [Cloud Resources](naming_conventions.md#cloud-resources)
    - [Storage](naming_conventions.md#storage)
      - [Lakehouse Storage](naming_conventions.md#lakehouse-storage)
      - [Generic Blob Storage](naming_conventions.md#generic-blob-storage)
  - [Databricks](naming_conventions.md#databricks)
    - [Workspace and Cluster Names](naming_conventions.md#workspace-and-cluster-names)
    - [Catalog](naming_conventions.md#catalog)
    - [Schema and Object Conventions](naming_conventions.md#schema-and-object-conventions)
      - [Metadata](naming_conventions.md#metadata)
      - [Bronze (Raw)](naming_conventions.md#bronze-raw)
      - [Silver](naming_conventions.md#silver)
      - [Gold](naming_conventions.md#gold)
  - [Streaming](naming_conventions.md#streaming)
  - [dbt](naming_conventions.md#dbt)
    - [Documentation and Model Metadata](naming_conventions.md#documentation-and-model-metadata)
    - [Sources](naming_conventions.md#sources)
    - [Model Folders](naming_conventions.md#model-folders)
    - [Model Names](naming_conventions.md#model-names)
  - [CI/CD](naming_conventions.md#cicd)
  - [Security](naming_conventions.md#security)
  - [Policies](naming_conventions.md#policies)
  - [Frameworks](naming_conventions.md#frameworks)

## Level 0 - Organisational and Domain-Level Context
- [0.1 Org + Domain Definition](level_0.md#0.1)
- [0.2 Strategies and Objectives](level_0.md#0.2)
- [0.3 Key Systems and Data Assets](level_0.md#0.3)
- [0.4 Team Capabilities and Roles](level_0.md#0.4)
- [0.5 Governance Structures](level_0.md#0.5)

## Level 1 - Enterprise-Level architectures
- [1.1 Enterprise Domain Topology](level_1.md#1.1)
    - [1.1.1 Key concepts](level_1.md#1.1.1)    
    - [1.1.2 Reference topologies](level_1.md#1.1.2)
    - [1.1.3 Hybrid federated mesh topology](level_1.md#1.1.3)

- [1.2 Enterprise Data Platform Reference Architecture](level_1.md#1.2)
- [1.3 Enterprise (Logical) Data Warehouse Reference Architecture](level_1.md#1.3)
- [1.4 Enterprise Information and Data Architecture](level_1.md#1.4)
- [1.5 Enterprise Metadata Architecture](level_1.md#1.5)
- [1.6 Enterprise Security](level_1.md#1.6)

## Level 2 - Domain-Level (Solution) architectures
- [2.1 Business architecture](level_2.md#2.1)
    - [2.1.1 Business processes](level_2.md#2.1.1)
    - [2.1.2 Business glossary](level_2.md#2.1.2)
    - [2.1.3 Business metrics](level_2.md#2.1.3)
 
- [2.3 Infrastructure](level_2.md#2.3)
    - [2.3.1 Environments, Workspaces + Clusters](level_2.md#2.3.1)
    - [2.3.2 Secrets](level_2.md#2.3.2)
    - [2.3.3 Storage](level_2.md#2.3.3)
    - [2.3.4 CICD + Repository](level_2.md#2.3.4)
    - [2.3.5 Observability](level_2.md#2.3.5)
    - [2.3.6 Networking](level_2.md#2.3.6)
    - [2.3.7 Orchestration](level_2.md#2.3.7)

- [2.2 Data and information models](level_2.md#2.2)
    - [2.2.1 Domain glossary](level_2.md#2.2.1)
    - [2.2.2 Domain data and warehouse models](level_2.md#2.2.2)

- [2.3 Data Architecture](level_2.md#2.3)
    - [2.3.1 Data zones and stages](level_2.md#2.3.1)  
    - [2.3.2 Lakehouse Catalog to Storage Mapping](level_2.md#2.3.2)

- [2.3 Data Engineering](level_2.md#2.3)
    - [2.3.1 Ingestion](level_2.md#2.3.1)
    - [2.3.2 Transformation](level_2.md#2.3.2)
        - [dbt standards](dbt_standards.md)
    - [2.3.3 Delivery](level_2.md#2.3.3)

- [2.4 Data Product Architecture and Patterns](level_2.md#2.4)
    - [2.4.1 Data Zone Architecture](level_2.md#2.4.1)
    - [2.4.2 Lakehouse Catalog to Storage Mapping](level_2.md#2.4.2)

- [2.5 Data access and governance](level_2.md#2.5)
    - [2.5.1 Data Access](level_2.md#2.5.1)
    - [2.5.2 Data Governance](level_2.md#2.5.2)


