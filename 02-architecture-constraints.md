# 2 Architecture Constraints

<!-- TOC -->

- [2 Architecture Constraints](#2-architecture-constraints)
    - [2.1 Regulatory Constraints](#21-regulatory-constraints)
    - [2.2 Technical Constraints](#22-technical-constraints)
    - [2.3 Conventions](#23-conventions)
    - [2.4 Project and Documentation constraints](#24-project-and-documentation-constraints)
- [Solution Content](#solution-content)

<!-- /TOC -->

## 2.1 Regulatory Constraints

As per the system and customer are part of the Fintech industry, the system must comply with various regulatory requirements, including but not limited to:

| ID | Regulation | Description |
|---|---|---|
| AC-001 | GDPR | The General Data Protection Regulation (GDPR) is an EU law that mandates how organizations must handle personal data, ensuring privacy and protection. |
| AC-002 | PCI DSS | The Payment Card Industry Data Security Standard (PCI-DSS) sets requirements for organizations handling credit card transactions to ensure secure processing. |
| AC-003 | PSD2 | The Revised Payment Services Directive (PSD2) is an EU regulation that aims to increase competition and innovation in the payment services market while ensuring security. |
| AC-004 | AML | Anti-Money Laundering (AML) regulations require financial institutions to monitor transactions for suspicious activity and report any findings to authorities. |
| AC-005 | KYC | Know Your Customer (KYC) regulations require financial institutions to verify the identity of their customers to prevent fraud and money laundering. |
| AC-006 | MiFID | The Markets in Financial Instruments Directive (MiFID) increases transparency in EU financial markets and standardizes regulatory disclosures. |

## 2.2 Technical Constraints

| ID  | Constraint | Description |
|-----|------------|-------------|
| AC-007 | AWS Cloud | The application must be hosted on AWS. |
| AC-008 | Security Groups Quota | AWS Security Groups have specific quota limitations, such as a maximum of 60 rules per direction and 1000 combined rules per Elastic Network Interface (ENI). |
| AC-009 | Open Banking Interoperability | The system must support interoperability with Open Banking APIs, ensuring compliance with standards. This includes secure data exchange and authentication mechanisms. |
| AC-010 | Third-Party Payment Processors | The application must integrate with third-party payment processors, including credit card networks, crypto wallets, and ACH transactions. This requires handling various APIs and ensuring secure, compliant transactions. |
| AC-011 | Monitoring and Logging | Monitoring and login should be used for both check the system and/or detect any irregular activity |
| AC-012 | Data Encryption | All sensitive data must be encrypted both in transit and at rest. |
| AC-013 | Multi Region (Disaster Recovery) | Disaster Recovery Multi Region must be implemented to ensure data integrity and availability. |
| AC-014 | CI/CD | The system must be deployed using a CI/CD pipeline to ensure consistent and automated deployment processes. |
| AC-015 | External Monitoring Datadog | The system must be monitored using Datadog for performance and security monitoring. |

## 2.3 Conventions

| ID  | Convention | Description |
|-----|------------|-------------|
| CONV-001 | Architecture documentation | Structure based on the english arc42-Template and C4 Model |
| CONV-002 | Architecture Framework | AWS Well-Architected Framework is used as reference for the design |
| CONV-002 | Naming Conventions | Use consistent naming conventions for all components, services, and APIs to ensure clarity and maintainability. |
| CONV-003 | Code Style | Follow a consistent code style and formatting guidelines across all programming languages used in the project. |
| CONV-004 | Language | Use English as the primary language for all documentation, code comments, and communication to ensure clarity and consistency. |
| CONV-005 | BIAN | Use BIAN (Banking Industry Architecture Network) standards for defining APIs and services to ensure interoperability and compliance with industry best practices. |

## 2.4 Project and Documentation constraints

As per this is not a real project and only a proof of concept for the design there ae multiple components of a whole payment processing system that are not included in this architecture. The following components are not included in the design:

- Enterprise Systems: All the systems that might support the company running the application are not included as part of this document
- Customer Registration: The system does not include any customer registration or onboarding process. It is assumed that customers are already registered and authenticated through other means.



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
