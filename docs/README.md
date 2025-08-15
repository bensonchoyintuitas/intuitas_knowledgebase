| <a href="https://www.intuitas.com"><img src="img/intuitas.png" width="200"/></a> | *We help organisations navigate the complexity and noise to achieve their unique objectives with data.*|
|:---:|---|

<br>

# Enterprise Data Intelligence Blueprint

This blueprint consists of a collection of resources that describe Intuitas' approach to designing and delivering Data, AI and Governance solutions. 

It distills insights from our R&D, answers to common questions, and lessons learned from real-world experience.

The ideas are intentionally opinionated, shaped by the Intuitas design philosophy, and proven in practice—supported by working demonstration environments. While grounded in large, multi-domain enterprise deployments, they can be adapted to suit organisations of any scale or type.


## Important Notes

**Living resource** - This content is continuously updated and refined through ongoing R&D, lessons learned, and feature evaluations. Some sections may remain unpolished as they evolve.

**Currency** – Positions and guidance reflect the product landscape and functionality available in general release at the time of writing. While every effort is made to maintain accuracy and update information promptly as features evolve, no guarantee of timeliness is provided. Readers are encouraged to independently validate all claims.

See the [Copyright, Licencing and Disclaimer information](#terms)

## Get help

 at [office@intuitas.com](mailto:office@intuitas.com) to:

- Find out more, or provide feedback.
- Access our demonstration environment or any of the tools and technologies presented
- Get help with any of our professional services: strategy, architecture, training, implementation, advice and guidance

---
<br>
<br>

# Overview

This comprehensive blueprint provides practical guidance for designing and delivering enterprise-scale Data, AI, and Governance solutions. It uses 

**What you'll find here:**
- Real-world tested architecture patterns
- Opinionated design principles based on proven practices  
- Step-by-step implementation guidance
- Working demo environments and examples

Built from extensive R&D, customer engagements, lessons learned and benchmarks.


## Design Principles

Our approach is guided by nine foundational principles that ensure transparency in decision-making, effective trade-off evaluation, and strategic alignment. 

Organisations should consider these principles alongside any existing frameworks that matter most to their context.


| # | Principle | Description |
|---|-----------|-------------|
| **1** | **Think big, start small** | Balance rapid delivery with strategic positioning. Deliver value iteratively. Ensure investments deliver sustained benefits without creating technical debt. |
| **2** | **Empowered Autonomy** | Enable domain experts to manage data independently while leveraging shared, scalable foundations. |
| **3** | **Disciplined Core, Flexible Edge** | Federated governance model that ensures policy consistency while enabling rapid, domain-specific delivery. |
| **4** | **Actionable Data** | Real-time, comprehensive data extraction (structured & unstructured) with platforms ready for immediate insights and AI/ML. |
| **5** | **Make It Easy to Do the Right Thing** | Provide automation and platforms that guide users toward secure, policy-aligned practices effortlessly. |
| **6** | **Cost Transparency and Efficiency** | Transparent cost models with pay-for-value usage while promoting reuse and removing sharing barriers. |
| **7** | **Adaptability for Growth** | Platforms that seamlessly adapt to evolving business needs, data growth, and diverse workloads. |
| **8** | **Interoperability and Inclusion** | Smooth integration across cloud, on-premises, and diverse technology stacks (bring-your-own). |
| **9** | **Flexibility Through Standards** | Technology-agnostic, standards-based components that maintain flexibility and prevent vendor lock-in. |

---
<br>
<br>

# Getting Started

### **Choose Your Starting Point**

| Role | Recommended Path | Focus Areas |
|------|------------------|-------------|
| **Leaders & Architects** | **Levels 0-1** | Strategic alignment, enterprise patterns, governance frameworks |
| **Domain & Technical Teams** | **Level 2** | Practical implementation within established enterprise frameworks |

### **Iterative Approach**

An iterative approach means each level builds upon the previous but can be revisited and refined as capabilities mature.

1. **Level 0: Context Setting** - Define organisational objectives, domain structures, and strategic goals
2. **Level 1: Enterprise Architecture** - Apply enterprise-wide patterns and reference topologies  
3. **Level 2: Domain Solutions** - Implement domain-specific solutions using enterprise patterns
4. **Standards & Conventions** - Apply consistent naming and conventions throughout


---

## Documentation Structure

### **Level 0: Enterprise Context**
- [organisational & Domain Definition](level_0.md#org-domain-definition)
- [Strategies and Objectives](level_0.md#strategies-and-objectives)  
- [Key Systems and Data Assets](level_0.md#key-systems-and-data-assets)
- [Team Capabilities and Roles](level_0.md#team-capabilities-and-roles)
- [Governance Structures](level_0.md#governance-structures)
- [Billing Structures](level_0.md#billing-structures)

### **Level 1: Enterprise Architecture**
- [Key Concepts](level_1.md#key-concepts)
- [Reference Topologies](level_1.md#reference-topologies)
- [Enterprise Data Platform Reference Architecture](level_1.md#enterprise-data-platform-reference-architecture)
- [Enterprise Data Warehouse Reference Architecture](level_1.md#enterprise-logical-data-warehouse-reference-architecture)
- [Enterprise Information and Data Architecture](level_1.md#enterprise-information-and-data-architecture)
- **[Enterprise Metadata Architecture](level_1.md#enterprise-metadata-architecture)**
    - [Architecture Principles](level_1.md#metadata-architecture-principles)
    - [Semantic and Data Lineage](level_1.md#semantic-and-data-lineage)
    - [Metadata Objects and Elements](level_1.md#metadata-objects-and-elements)
    - [Consolidation and Synchronisation](level_1.md#metadata-consolidation-and-synchronisation)
    - [Governance Metadata](level_1.md#data-architecture-and-governance-metadata)
- [Enterprise Security](level_1.md#enterprise-security)
- [Enterprise Data Governance](level_1.md#enterprise-data-governance)
- [Enterprise Billing](level_1.md#enterprise-billing)

### **Level 2: Domain Architecture**
- **[Business Architecture](level_2.md#business-architecture)**
    - [Business Processes](level_2.md#business-processes)
    - [Business Glossary](level_2.md#business-glossary)  
    - [Business Metrics](level_2.md#business-metrics)
- **[Infrastructure](level_2.md#infrastructure)**
    - [Environments, Workspaces & Storage](level_2.md#environments-workspaces-and-storage)
    - [Secrets Management](level_2.md#secrets)
    - [Storage Architecture](level_2.md#storage)
    - [CI/CD and Repository](level_2.md#cicd-and-repository)
    - [Observability](level_2.md#observability)
    - [Networking](level_2.md#networking)
    - [Orchestration](level_2.md#orchestration)
- **[Data & Information Models](level_2.md#data-and-information-models)**
    - [Domain Glossary](level_2.md#domain-glossary)
    - [Domain Data & Warehouse Models](level_2.md#domain-data-and-warehouse-models)
- **[Data Architecture](level_2.md#data-architecture)**
    - [Data Layers and Stages](level_2.md#data-layers-and-stages)
    - [Lakehouse Catalog to Storage Mapping](level_2.md#lakehouse-catalog-to-storage-mapping)
- **[Data Engineering](level_2.md#data-engineering)**
    - [Ingestion Patterns](level_2.md#ingestion)
    - [Transformation](level_2.md#transformation)
    - [Data Sharing & Delivery](level_2.md#data-sharing-and-delivery-patterns)
- **[Data Governance](level_2.md#data-governance)**
    - [Lifecycle & Asset Management](level_2.md#data-lifecycle-and-asset-management)
    - [Access Management](level_2.md#data-access-management)
    - [Data Quality](level_2.md#data-quality)
    - [Data Understandability](level_2.md#data-understandability)
    - [Privacy Preservation](level_2.md#privacy-preservation)
    - [Audit Capabilities](level_2.md#audit)

### **Standards**
- **[Naming Standards & Conventions](naming_standards_and_conventions.md)** 
<br>
<br>
---

## Terms

**Copyright:** This knowledge base and its contents are the original works of © Intuitas PTY LTD, 2025.  All rights reserved. Any referenced  or third-party materials remain the property of their respective copyright holders. Every effort has been made to accurately reference and attribute existing content, and no claim of ownership is made over such materials.

**License:** Free use, reproduction, and adaptation permitted with prior consent and appropriate attribution to Intuitas PTY LTD. Referenced third-party content is subject to the copyright terms of their respective owners. 

**Disclaimer:** Content is provided for general information only.  It does not constitute professional advice and should not be relied on as a substitute for advice tailored to your circumstances. No liability is accepted for errors or omissions. Always consider your local context and verify applicability before considering its use.



<div align="center">



### 


<br>
<a href="https://www.intuitas.com"><img src="img/intuitas.png" width="200"/></a>
<br>

📧 [office@intuitas.com](mailto:office@intuitas.com)


</div>
