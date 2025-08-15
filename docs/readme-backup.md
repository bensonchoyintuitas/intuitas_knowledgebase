<img src="img/intuitas.png" width="200"/>

# Enterprise Data Intelligence Blueprint

<br>

This blueprint consists of a collection of resources that describe Intuitas' approach to designing and delivering Data, AI and Governance solutions. 

It distills insights from our R&D, answers to common questions, and lessons learned from real-world experience.

The ideas are intentionally opinionated, shaped by the Intuitas design philosophy, and proven in practice—supported by working demonstration environments. While grounded in large, multi-domain enterprise deployments, they can be adapted to suit organisations of any scale or type.

## Design Principles

 Principles provide the foundational guidance for our approach, ensuring transparency in decision-making, effective trade-off evaluation, and alignment to strategic priorities. 
 
 The ideas and architecture patterns presented here are opinionated - reflecting Intuitas' design philosophy, established practices and the product landscape.
 
 Organisations are encouraged to consider these as well as any existing principles that matter most to them.
 
| Principle | Description |
|-----------|-------------|
| **1. "Think big, start small"** | Balance rapid delivery with the right strategic positioning and design so that investments deliver sustained benefits without creating new challenges. |
| **2. Empowered Autonomy** | Enable domain experts in business and clinical environments to manage data independently while leveraging shared, scalable, and consistent foundations. |
| **3. Disciplined Core, Flexible Edge** | Implement a federated governance and core engineering model that enhances policy consistency and reliability across Queensland Health, while enabling rapid, local/domain-specific product delivery. |
| **4. Actionable Data** | Default to real-time and supported data extraction across all data of all types—including unstructured—supported by platforms that are ready to deliver timely, decision-enabling insights + AI/ML applications. |
| **5. Make It Easy to Do the Right Thing** | Provide automation, services, and platforms that simplify workflows and guide users to adopt effective, secure, and policy-aligned practices effortlessly. |
| **6. Cost Transparency and Efficiency** | Promote reuse and economies of scale with transparent cost models, ensuring consumers pay only for value-driven usage while removing barriers to sharing. |
| **7. Adaptability for Growth and Change** | Ensure platforms adapt seamlessly to evolving business needs, data growth, and diverse workloads. |
| **8. Interoperability and Inclusion** | Facilitate smooth integration across cloud, on-prem, and diverse technology stacks (i.e. byo), reducing participation barriers. |
| **9. Flexibility Through Standards** | Leverage standards-based, technology-agnostic components to maintain flexibility and prevent vendor lock-in. |

## How to use this resource

### **Getting Started**

- **Leaders and Architects**: Focus on Levels 0-1 to establish strategic alignment, enterprise patterns and governance
- **Domain/Functional and Technical Teams**: Emphasize Level 2 for practical implementation guidance within established enterprise frameworks  


### **Iterative Approach**
- The framework supports "think big, start small" - establish enterprise context and patterns early, then implement incrementally
- Each level builds upon the previous, but can be revisited and refined as your organisation's capabilities mature
- Living guidance that evolves with your implementation experience and changing requirements

- **Level 0 Context Setting**: Start by defining organisational objectives, domain structures, and strategic objectives before making any architectural and governance decisions
- **Level 1 Enterprise Architecture**: Apply enterprise-wide architectural patterns and reference topologies that align with your established context
- **Level 2 Domain/Solution Architecture**: Implement domain-specific solutions that instantiate the enterprise patterns for your particular use cases
- Use the **Naming Standards and Conventions** guide alongside any level to ensure consistency

## Disclaimer

This is a living, continuously updated resource — which means it's unpolished in places. Every effort is made to ensure the currency, accuracy, and representation of products and features at the time of writing. Positions reflect only features in general availability, and this knowledgebase will be updated as they evolve.

Readers are encouraged to verify the content and consider their local context. No liability is accepted for any error or omission arising from the use of this resource.

## Contact

For access to any of the tools presented here, our live showcase environment or further information, contact us via [office@intuitas.com](mailto:office@intuitas.com)

## Copyright

This knowledge base and its contents are the original works of © Intuitas PTY LTD, 2025. All rights reserved. Any referenced  or third-party materials remain the property of their respective copyright holders. Every effort has been made to accurately reference and attribute existing content, and no claim of ownership is made over such materials.

## Licence

Permission is granted for free use, reproduction, and adaptation of this material, provided prior consent is obtained and appropriate attribution is given. Referenced third-party content is subject to the copyright terms of their respective owners. 



<br>
<br>

# Table of Contents
<br>

## Naming standards and conventions
- [Naming standards and conventions](naming_standards_and_conventions.md)
<br>
<br>
## Level 0 - Enterprise-level context
- [Org + Domain Definition](level_0.md#org-domain-definition)
- [Strategies and Objectives](level_0.md#strategies-and-objectives)
- [Key Systems and Data Assets](level_0.md#key-systems-and-data-assets)
- [Team Capabilities and Roles](level_0.md#team-capabilities-and-roles)
- [Governance Structures](level_0.md#governance-structures)
- [Billing Structures](level_0.md#billing-structures)
<br>

## Level 1 - Enterprise-level architecture
- [Key concepts](level_1.md#key-concepts)
- [Reference topologies](level_1.md#reference-topologies)
- [Enterprise Data Platform Reference Architecture](level_1.md#enterprise-data-platform-reference-architecture)
- [Enterprise (Logical) Data Warehouse Reference Architecture](level_1.md#enterprise-logical-data-warehouse-reference-architecture)
- [Enterprise Information and Data Architecture](level_1.md#enterprise-information-and-data-architecture)
- [Enterprise Metadata Architecture](level_1.md#enterprise-metadata-architecture)
    - [Metadata Architecture Principles](level_1.md#metadata-architecture-principles)
    - [Semantic and Data lineage](level_1.md#semantic-and-data-lineage)
    - [Metadata Objects and Elements](level_1.md#metadata-objects-and-elements)
    - [Metadata Consolidation and Synchronisation](level_1.md#metadata-consolidation-and-synchronisation)
    - [Data Architecture and Governance Metadata](level_1.md#data-architecture-and-governance-metadata)
        - [Semantic modelling, mastering and lineage](level_1.md#semantic-modelling-mastering-and-lineage)
        - [Unified metadata repository](level_1.md#unified-metadata-repository)
        - [Analytics engineering metadata](level_1.md#analytics-engineering-metadata)
        - [Databricks Unity Catalog Metastore](level_1.md#databricks-unity-catalog-metastore)
- [Enterprise Security](level_1.md#enterprise-security)
- [Enterprise Data Governance](level_1.md#enterprise-data-governance)
    - [Audit](level_1.md#audit)
- [Enterprise Billing](level_1.md#enterprise-billing)
<br>

## Level 2 - Domain-level (solution) architecture
- [Business architecture](level_2.md#business-architecture)
    - [Business processes](level_2.md#business-processes)
    - [Business glossary](level_2.md#business-glossary)
    - [Business metrics](level_2.md#business-metrics)
- [Infrastructure](level_2.md#infrastructure)
    - [Environments, Workspaces and Storage](level_2.md#environments-workspaces-and-storage)
    - [Secrets](level_2.md#secrets)
    - [Storage](level_2.md#storage)
    - [CICD and Repository](level_2.md#cicd-and-repository)
    - [Observability](level_2.md#observability)
    - [Networking](level_2.md#networking)
    - [Orchestration](level_2.md#orchestration)
- [Data and information models](level_2.md#data-and-information-models)
    - [Domain glossary](level_2.md#domain-glossary)
    - [Domain data and warehouse models](level_2.md#domain-data-and-warehouse-models)
- [Data Architecture](level_2.md#data-architecture)
    - [Data layers and stages](level_2.md#data-layers-and-stages)
    - [Lakehouse Catalog to Storage Mapping](level_2.md#lakehouse-catalog-to-storage-mapping)
- [Data Engineering](level_2.md#data-engineering)
    - [Ingestion](level_2.md#ingestion)
    - [Transformation](level_2.md#transformation)
    - [Data Sharing and Delivery Patterns](level_2.md#data-sharing-and-delivery-patterns)
- [Data governance](level_2.md#data-governance)
    - [Data lifecycle and asset management](level_2.md#data-lifecycle-and-asset-management)
    - [Data access management](level_2.md#data-access-management)
    - [Data quality](level_2.md#data-quality)
    - [Data understandability](level_2.md#data-understandability)
    - [Privacy Preservation](level_2.md#privacy-preservation)
    - [Audit](level_2.md#audit)


