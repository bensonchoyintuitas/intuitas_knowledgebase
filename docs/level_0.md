# Level 0 - Enterprise-level context
[Return to home](README.md)

This section outlines the foundational enterprise-wide concepts that inform and guide data and platform architecture decisions across the organisation. It sets the strategic and business context within which all lower-level architectural decisions should be made.

<br>
## Why It Matters

Establishing a clear enterprise-level context ensures that investments in data, capabilities, and infrastructure are purposefully aligned with the organisation’s strategic vision, operating model, and capacity. This alignment is essential for delivering meaningful, sustainable outcomes that reflect the organisation’s unique context and long-term goals.


<br>

## Table of Contents
---

- [Org + Domain Definition](#org-domain-definition)
- [Strategies and Objectives](#strategies-and-objectives)
- [Key Systems and Data Assets](#key-systems-and-data-assets)
- [Team Capabilities and Roles](#team-capabilities-and-roles)
- [Governance Structures](#governance-structures)
- [Funding and costing structures](#funding-and-costing-structures)

<br>
<br>

## Org + Domain Definition

Understanding the organisation’s structure and domains is key to setting governance, environment, and metadata boundaries. While industry patterns and reference architectures can guide, each organisation is unique. See [Domain-Centric Design](level_1.md#domain-centric-design) for more.


**Recommended artefacts:**

- Description of the organisational boundary, including any external organisations in scope.
- Description of domains and subdomains that impact the data governance and management model.

<div align="center">

<em>Illustrative example of domains for a Healthcare organisation</em>

<br>
<a href="../img/domain-example.png" target="_blank">
    <img src="../img/domain-example.png"  alt="Example domains for a Healthcare organisation" width="75%">
</a>
<br>
</div>


## Strategies and Objectives

Investing in data initiatives without a clear strategic context risks misalignment and wasted effort. A well-defined strategic context for data, technology, and governance ensures initiatives align with organisational goals, set the right priorities, have a clear scope, support effective governance, and build strong investment and benefits cases.

**Recommended artefacts**

- Assessment of relevant business, technology, and data/AI strategies, plans, and initiatives.

## Key Systems and Data Assets

Identifying and profiling key systems and data assets in the context of strategic benefits and use cases provides a strong foundation for effective planning, design, and risk management.

**Recommended artefacts**

- Description of the organisation's relevant systems and data sets (e.g. CMDB, Information Asset Register), including associated governance arrangements and technical characteristics.


## Team Capabilities and Roles

Clarity on the responsibilities and capabilities of key teams is essential to executing the data strategy, maintaining accountability, ensuring all support functions are adequately fulfilled, and  costs remain predictable.

**Recommended artefacts**

- RACI matrix for associated teams/parties with consideration of functions including but not limted to:
  - Data lifecycle – creation, management, governance (quality, access), security, and consumption
  - Data enablement – engineering, integration, analysis, reporting, and application development
  - Data foundations – information architecture, infrastructure, and platform provisioning and management
  - Operational support – DataOps, monitoring, and ongoing maintenance

- Current and target operating and service management model
- Maturity and skill assessment of key teams/parties relative to role definition
- Strategy for using external partnerships and vendors to address capability gaps


## Governance Structures

Governance frameworks and processes define how decisions are made, enforced, and improved across the data lifecycle. These may already exist in some organisations, while in others they need to be developed.

**Recommended artefacts**

- Description of governance frameworks, policies, standards, processes, and bodies relevant to the scope of concern.
- In some cases - proposals and terms of reference for new governance structures.


## Funding and costing structures

How funding and costs are allocated directly shape the internal “marketplace” for data and analytics. These structures determine whether teams collaborate or compete, influence speed of delivery and standards alignment, and define the organisation’s capacity to scale and innovate. The architecture reflects this through:

- system-wide, domain, and role-level accountabilities. 
- policies and guardrails to prevent bill-shock 
- observability and optimisation 

See:
- [Enterprise Billing Solutions](level_1.md#enterprise-billing)
- [Observability Solutions](level_2.md#observability)


**Recommended artefacts**

- Billing structures – including how cost centres map to reports, delegations, and observability (important for transparency and accountability)
- Cost allocation and chargeback models – critical for encouraging reuse, managing shared services, and clarifying ownership
- Budgeting and forecasting – ensures sustainable funding for data platforms, products, and capabilities
- Cost optimisation and monitoring – helps manage platform efficiency and avoid waste, and how the accountabilities are distributed for their control

<br>
<br>

## Next Steps

Once the enterprise-level context is established, proceed to [Level 1 - Domain Architecture](level_1.md) to explore domain-centric design principles and architectural patterns that support the organisation's strategic objectives.

---

