# 6 Design Decisions

<!-- TOC -->

- [6 Design Decisions](#6-design-decisions)
- [Solution Content](#solution-content)

<!-- /TOC -->

This Section aim to document all the architectural decisions taken

| Problem | Constraints | Alternatives | Decision |
| --- | --- | --- | --- |
| Whether to include a separate deployment view | The architecture already includes detailed building blocks and AWS components | Create a specific deployment diagram       | **Not included**, as the existing diagrams already show network boundaries and component distribution |
| Selecting a centralized monitoring solution   | Must be cloud-friendly, support multi-account/multi-region, and be cost-efficient | Datadog, AppDynamics, Instana              | **Datadog** chosen due to strong integration capabilities, industry adoption, and better cost/quality ratio |
| MultiRegion Vs MultiZone | Must ensure high availability and disaster recovery | MultiRegion, MultiZone                     | **MultiRegion** chosen for better disaster recovery and availability, despite higher costs  and to comply with regulatory contraints and customer need for critical (money) operations |

<!-- CONTENTTABLE:START -->
# Solution Content

1. [Introduction and Goals](01-introduction-and-goals.md)
2. [Technical Constraints](02-technical-constraints.md)
3. [System Context and Scope](03-system-context-and-scope.md)
4. [Building Block View](04-building-block-view.md)
5. [Runtime Overview](05-RuntimeOverview.md)
6. [Design Decisions](06-design-decisions.md)
7. [Technical Risks](07-technical-risks.md)
8. [Quality Requirements](08-quality.md)
9. [Glossary](09-glossary.md)
<!-- CONTENTTABLE:END -->