<p align="center">
  <a href="" rel="noopener">

<img src="./assets/banner.webp" alt="GiHub"></a>


</p>

<h3 align="center">High-volume Payment Processing Application</h3>
<div align="center">
	<code><img width="50" src="https://raw.githubusercontent.com/marwin1991/profile-technology-icons/refs/heads/main/icons/aws.png" alt="AWS" title="AWS"/></code>
</div>

# Table of Contents

<!-- TOC -->

- [Table of Contents](#table-of-contents)
- [Executive Summary](#executive-summary)
    - [Architecture Overview](#architecture-overview)
    - [Design Principles](#design-principles)
    - [Highlights](#highlights)
    - [Strategic Thinking](#strategic-thinking)
    - [A Personal Note](#a-personal-note)
    - [Background](#background)
    - [Objectives](#objectives)
- [Solution Strategy](#solution-strategy)
- [Solution Content](#solution-content)
    - [1.1 Requirements Overview](#11-requirements-overview)
    - [1.2 Quality Goals](#12-quality-goals)
    - [1.3 Systems Stakeholders](#13-systems-stakeholders)
- [2 Architecture Constraints](#2-architecture-constraints)
    - [2.1 Regulatory Constraints](#21-regulatory-constraints)
    - [2.2 Technical Constraints](#22-technical-constraints)
    - [2.3 Conventions](#23-conventions)
    - [2.4 Project and Documentation constraints](#24-project-and-documentation-constraints)
- [3 System Context and Scope](#3-system-context-and-scope)
    - [3.1 System Context Diagram](#31-system-context-diagram)
        - [3.1.1 Interaction Details](#311-interaction-details)
- [4 System Context and Scope](#4-system-context-and-scope)
    - [4.1 Container Diagram](#41-container-diagram)
    - [4.2 Components Diagrams](#42-components-diagrams)
        - [4.2.1 Payment Hub - Payment API Gateway](#421-payment-hub---payment-api-gateway)
        - [4.2.2 Payment Hub - Payment Orchestrator](#422-payment-hub---payment-orchestrator)
        - [4.2.3 Payment Hub - Payment Ledger & Data Manager](#423-payment-hub---payment-ledger--data-manager)
        - [4.2.4 Payment Hub - Risk Assessment Engine](#424-payment-hub---risk-assessment-engine)
        - [4.2.5 Integration Hub - Open Banking Gateway](#425-integration-hub---open-banking-gateway)
        - [4.2.6 Integration Hub - Open Banking Services](#426-integration-hub---open-banking-services)
        - [4.2.7 Integration Hub - 3rd Party Gateway](#427-integration-hub---3rd-party-gateway)
        - [4.2.8 Monitoring & Logging - Alert Engine](#428-monitoring--logging---alert-engine)
        - [4.2.9 Monitoring & Logging - Support Advisor](#429-monitoring--logging---support-advisor)
        - [4.2.10 Reporting Hub - Analytics Engine](#4210-reporting-hub---analytics-engine)
        - [4.2.11 Administration Hub - Configuration Platform](#4211-administration-hub---configuration-platform)
        - [4.2.12 Security Hub - Security Services](#4212-security-hub---security-services)
        - [4.2.13 Security Hub - Identity Provider (IdP)](#4213-security-hub---identity-provider-idp)
- [5 Runtime View](#5-runtime-view)
    - [5.1 Scenario 1 - Payment (Simplified)](#51-scenario-1---payment-simplified)
- [6 Design Decisions](#6-design-decisions)
- [7 Technical Risks](#7-technical-risks)
- [8 Quality Requirements](#8-quality-requirements)
- [9 Glossary](#9-glossary)

<!-- /TOC -->



# Executive Summary

This document presents the **High-Volume Payment Processing Platform**, a modern, scalable, and secure architecture designed to support complex financial transactions with high performance, compliance, and resilience. It is built with a **microservices-oriented**, **API-first**, and **event-driven** approach, leveraging the full capabilities of **Amazon Web Services (AWS)** and integrating seamlessly with external systems such as **payment processors**, **financial institutions**, **business intelligence tools** and **advanced monitoring solutions**.

## Architecture Overview

The solution has been broken down into clearly defined building blocks, as illustrated in the **System Context** and **Container Views**, providing both high-level vision and deep insight into functional decomposition. Each subsystem (or “hub”) is responsible for a well-scoped set of responsibilities, promoting separation of concerns and ease of maintenance. In all the sections you may found what has been assumed and what has been designed, including the **C4 model** and **Arc42** principles.

- **Payment Hub**: The heart of the platform — handling validations, orchestrations, risk assessments, and settlement logic.
- **Integration Hub**: Bridges the platform to financial institutions and third-party gateways, including full Open Banking capabilities.
- **Reporting Hub**: Enables real-time and batch reporting, with anonymized data, ETL pipelines, and support for external BI tools.
  - **Analytics Engine**: Transforms anonymized payment data via Glue/EMR and surfaces business insights using QuickSight and OpenSearch.
- **Monitoring & Alerting**: Centralized logging and alert management using CloudWatch and SNS/SES, enhanced with **Datadog** for observability.
  - **Support Advisor**: A smart assistant that helps agents resolve alerts with contextual data, AI-powered insights (SageMaker, Bedrock), and external knowledge lookup (Kendra).
- **Administration Hub**: Supports platform configuration and cloud resource management using AWS-native services like Control Tower and SSM.
- **Security Hub**: Ensures compliance with security standards through centralized identity, cryptographic, and threat detection services.
  - **Identity Provider**: Authenticates all actors using AWS Cognito and IAM, supporting SSO, OAuth2, and federated identity integrations.

## Design Principles

The architecture was built on the following core design pillars:

- **Security-First**: Implements zero trust principles, strong encryption, strict least privilege IAM, token-based authentication, and modular security enforcement across all hubs.
- **Modular and Scalable**: Each component is independently deployable and can scale horizontally, supporting growth and traffic surges.
- **Resilience & Observability**: Built-in alerting, structured logging, health checks, and recovery mechanisms ensure system robustness.
- **Open & Compliant**: Complies with industry regulations (GDPR, PSD2, AML), supports external integrations, and provides secure data exchange mechanisms.
- **Cloud Native**: Fully AWS-native, using the right services for the right job — combining ECS/Fargate, S3, DynamoDB, Aurora, Kinesis, and more.
- **Data-Centric**: Provides real-time and batch capabilities for data analytics and operations, all while preserving privacy and supporting re-identification when needed.

## Highlights
![Context](assets/context.png)

- **Multi-Region Architecture**: Key services are deployed in multiple AWS regions with DNS-based routing (Route 53), ensuring low latency and failover.
- **Event-Driven Workflows**: Decoupled components communicate via Kinesis, SQS, and EventBridge, supporting async and reactive workflows.
- **Microservices**: Each complex component derivate in a sum of microservices when a native solution can't provide the solution, allowing for independent development, deployment, and scaling.
- **Smart Support**: The Support Advisor empowers human agents with contextual, AI-assisted diagnostics powered by LangChain, SageMaker, and Amazon Bedrock.
- **Self-Service Configuration**: Admin Console allows fine-grained control over the system via low-touch, secure interfaces using bastion hosts and configuration platforms.
- **Professional Observability**: Datadog was selected as the observability platform for its seamless AWS integration and excellent cost-to-value ratio, supporting metrics, traces, and logs from all hubs.

## Strategic Thinking

The architectural choices aim to balance innovation with best practices. For example:

- Instead of reinventing the wheel, AWS-native services are used to maximize efficiency.
- Modularity and strict boundaries between domains reduce blast radius and foster independent evolution.
- Decision logs are kept transparent and include tradeoffs (e.g., why **Datadog** over AppDynamics or Instana).

## A Personal Note
<img align="right" width="300" height="300" src="./assets/happy.gif">

This solution wasn’t just about designing something that might work in real life — it was about building something elegant, scalable, and practical for a real-world scenario. I've combined my experience designing systems at scale with careful attention to details such as:

- regulatory compliance,
- operational resilience,
- developer ergonomics, and
- business agility.


## Background

This document aim to describe the architecture and design of a new high-volume payment processing platform that handle hundreds of thousands of transactions per daily. 

The goal is to design a scalable, secure, and highly available system that ensures seamless and reliable payment processing.


> [!WARNING] 
> **AI Usage** : Some of the content of this documentation has been redacted by AI and reviewed later by the author for optimizing or styling purposes. Tools used include OpenAI and GH Copilot.

> [!WARNING] 
> **Raw Data** : All the images can be checked in the `assets` folder. Also all the pages served are docummented as markdown files inside the repository.

## Objectives

The main objectives of this platform are:

**Scalability**: The platform should be able to scale horizontally to handle increasing transaction volumes.
**Security**: Implement robust security measures to protect sensitive financial data and ensure compliance with industry regulations (e.g., PCI DSS).
**High Availability**: Ensure the platform is highly available with minimal downtime.
**Performance**: Optimize for low latency and high throughput to provide a smooth user experience.
**Fault Tolerance**: Ensure the system can recover quickly from failures and maintain data consistency.
**Regulatory Compliance**: Ensure the platform meets all relevant regulatory and compliance requirements.
**Monitoring**: Implement comprehensive monitoring and logging to track performance and detect issues.

#  Solution Strategy
![AllInOne](assets/allinone.gif)  

The architecture of the High-Volume Payment Processing Platform (HVP3) is designed to support a modern, scalable, and resilient payment infrastructure tailored for a Fintech environment. It adheres to the **Arc42** and **C4 model** principles, combining clear structural organization with visual architectural representations to ensure transparency and maintainability.

The chosen approach is based on the following strategic pillars:

- **Event-Driven Architecture**: The system heavily relies on asynchronous event handling using services such as Amazon SQS, Kafka, and AWS Lambda to decouple components and enable scalable real-time processing.
- **Microservices-Based Design**: Each major function (e.g., Payment Orchestrator, Risk Assessment Engine, Open Banking Services) is implemented as an independently deployable service, enabling continuous delivery, isolated fault domains, and modular evolution of the platform.
- **API-Centric Interaction**: All external and internal communications are API-first. Public-facing APIs are exposed through secured API Gateways using OAuth2 and mTLS, supporting seamless integration with customers, merchants, and financial institutions.
- **Cloud-Native Deployment**: AWS is used as the primary cloud provider, leveraging services such as ECS Fargate, API Gateway, Redshift, Lambda, and Bedrock to achieve elasticity, observability, and managed operations at scale.
- **Security by Design**: Core security mechanisms (IAM, KMS, Cognito, Network Firewall, GuardDuty, and Secrets Manager) are embedded across all layers to ensure end-to-end encryption, least privilege access, and regulatory compliance (e.g., PCI DSS, GDPR).
- **Multiregion Architecture**: All critical components are replicated across regions to ensure high availability, fault tolerance, and disaster recovery. Internal Route 53 zones, NAT Gateways, and cross-region Kafka syncs are used to support this strategy.
- **Monitoring and Observability**: The solution integrates AWS-native tools (CloudWatch, CloudTrail, SNS) with external platforms like **Datadog** to provide deep observability, alerting, and operational intelligence across accounts and regions.
- **Analytics and Reporting**: Anonymized data is stored in S3 and processed using Glue and EMR, queried via Athena, and served through Redshift, OpenSearch, and QuickSight. AWS Data Exchange allows external sharing with other BI tools.
- **Support and Automation**: An intelligent support assistant powered by Amazon Lex, Kendra, SageMaker, and Bedrock guides support agents through incidents using historical data, documentation, and generative AI.

This solution strategy provides a solid foundation for building a secure, high-performance, scalable financial transaction platform that can adapt to growth and regulatory evolution.

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
<!-- CONTENTTABLE:END --># 1 Introduction and Goals


The High Volume Payment processing platform (HVP3) is a system designed to handle high transaction volumes while ensuring security, scalability, and compliance with industry standards. 

The architecture leverages microservices, cloud-native technologies, and robust security measures to achieve these goals.

As per this is designed for a Fintech company, it is needed to consider the customer need of an instant and seamless financial transaction in multiple currencies and payment methods. The system should be able to handle high transaction volumes, provide real-time processing, and ensure security and compliance with industry regulations.

This financial regulations include PCI DSS, GDPR, and other relevant standards. The system should also be designed to be scalable, flexible, and maintainable to accommodate future growth and changes in the financial landscape.

For integration with other financial institutions, the system should support various APIs and protocols to facilitate seamless communication and data exchange.


## 1.1 Requirements Overview

![Use Case Diagram](assets/uml-render/01-uc/01-uc.svg)


| ID   | Use Case (Functional Requirement) | Description |
|------|-----------------------------------|-------------|
| UC-001 | Payment                           | Initiate a payment from customer or integrator and process it through validation, authorization, and settlement. |
| UC-002 | Validate Payment                  | Validate the payment against business rules, customer identity, balance, and compliance policies. |
| UC-003 | Authorize Payment                 | Request authorization from a payment processor to approve the transaction. |
| UC-004 | Capture/Settle Payment            | Capture authorized payments and settle them with financial institutions or processors. |
| UC-005 | Refund Payment                    | Process full or partial refunds back to the customer via the original payment route. |
| UC-006 | View Payment Status               | Allow users/systems to view or receive real-time updates about the status of their payments. |
| UC-007 | Payment Failure                   | Handle timeouts, technical issues, and declined payments through retry logic and failure tracking. |
| UC-008 | Monitor & Alert                   | Detect errors, anomalies, or suspicious activity using event-driven monitoring and alerting. |
| UC-009 | Reconcile Transactions            | Reconcile transactions with external financial institutions to ensure consistency and correctness. |
| UC-010 | Admin Configurations              | Manage dynamic platform settings such as limits, roles, fees, and access controls. |
| UC-011 | Connect Bank Account              | Allow customers to connect their bank accounts using Open Banking protocols. |
| UC-012 | Instant Payment                   | Enable direct bank-to-bank payments using PISP (Payment Initiation Service Provider) mechanisms. |
| UC-013 | Risk Assessment                   | Analyze risk score of a transaction based on internal and external data sources. Also Evaluate AML, KYC, and other risk factor assessments |
| UC-014 | Anonymized Data                   | Generate anonymized datasets for reporting or ML analysis. |

## 1.2 Quality Goals

Based on [ISO 25010](https://www.iso.org/es/contents/data/standard/07/81/78176.html) we have defined the following quality goals for the project:


| ID | Quality | Motivation |
|---|---|---|
| QG-001 | Security | Implement robust security measures to protect sensitive financial data and ensure compliance with industry regulations:</br>- GDPR</br>- PSD2</br> - Anti Money Laundering</br></br> |
| QG-002 | Security | Main Principles for security includes E2E Encryption, Zero Trust and Least Privilege, strong multifactor authentication and multiple modular controls. |
| QG-003 | Compatibility | System should be able to exchange information with other financial institutions |
| QG-004 | Compatibility | System should be able to exchange information with third party payment gateways |
| QG-005 | Maintainability | Implement comprehensive monitoring and logging to track performance and detect issues. |
| QG-006 | Transferability | System should be able to run in any of the Jenkins instances |
| QG-007 | Performance Efficiency | The platform should be able to scale horizontally to handle increasing transaction volumes. |
| QG-008 | Performance Efficiency | Optimize for low latency and high throughput to provide a smooth user experience. |
| QG-009 | Reliability | Ensure the platform is highly available with minimal downtime. |
| QG-010 | Reliability | Ensure the system can recover quickly from failures and maintain data consistency. |
| QG-011 | Usability | Ensure the platform is user-friendly and easy to navigate for both end-users and administrators. |


## 1.3 Systems Stakeholders

| Stakeholder           | Description                                           | Goal/Intention |
|-----------------------|-------------------------------------------------------|----------------|
| Customer              | End-user who initiates and tracks payments.           | - Make secure and fast payments<br>- View transaction status<br>- Request refunds |
| Merchant              | Business that receives payments or initiates refunds. | - Receive customer payments<br>- Issue refunds<br>- Monitor settlement |
| Integrator            | External system integrating with the platform APIs.   | - Initiate and track payments via APIs<br>- Access payment status<br>- Integrate Open Banking flows |
| Financial Institution | Bank or Open Banking provider interacting with the platform. | - Approve and settle transactions<br>- Provide account and payment data via APIs |
| Compliance Officer    | User who reviews and ensures adherence to regulations. | - Monitor alerts<br>- Review KYC/AML data<br>- Ensure platform compliance |
| Support Agent         | Resolves issues and monitors payment workflows.       | - Handle disputes<br>- Support end-users<br>- Monitor failed transactions |
| Admin                 | Manages platform rules, access, and system parameters. | - Configure fees, limits, and roles<br>- Review platform health and alerts |
| Reporting User        | Accesses system data for analytics or compliance.     | - Generate reports<br>- Review historical transactions<br>- Monitor KPIs |
| Payment Processor     | Third-party system to execute payment instructions.   | - Authorize and settle payments<br>- Relay payment status back to platform |
| Business Intelligence Platform | Internal or external tool for dashboard and data insights. | - Consume anonymized data<br>- Provide analytics<br>- Enable business decision-making |
| External Monitoring System | External system for monitoring and alerting. | - Monitor system health<br>- Trigger alerts based on predefined rules<br>- Provide insights into system performance |

# 2 Architecture Constraints


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



# 3 System Context and Scope


This architecture follow the patterns of [C4 Model](https://c4model.com/) and the [4+1 Architectural View Model](https://en.wikipedia.org/wiki/4%2B1_architectural_view_model)

This section contain all the delimits of the systems and communications. The specific nodes and components will be defined in the next sections.

## 3.1 System Context Diagram

![Business System Context Diagram](assets/uml-render/02.01-SystemContext/02-system-context.svg)

![Technical System Context Diagram](assets/uml-render/02.02-TechContext/02-tech-context.svg)


| Neighbor              | Description                                                                                       | Channel/Interface                                                                                       |
|------------------------|---------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------|
| Customer               | Uses the platform to make payments and track their status. Interacts with the Payment Hub Services via HTTPS/Web/Mobile. | **Channel:** HTTPS (Internet) <br> **Interface:** Web/Mobile applications connecting to Payment Hub Services |
| Merchant               | Receives payments and initiates refunds. Interacts with the Payment Hub Services via HTTPS/Dashboard. | **Channel:** HTTPS (Internet) <br> **Interface:** Web/Dashboard applications connecting to Payment Hub Services |
| Integrator             | External system that integrates with the platform via APIs to initiate payments. Interacts with the Integration Hub via REST APIs/Webhooks. | **Channel:** HTTPS (REST APIs/Webhooks) <br> **Interface:** REST APIs/Webhooks connecting to Integration Hub |
| Compliance Officer     | Monitors regulatory compliance and reviews alerts. Interacts with the Monitoring & Alerting system and Reporting Hub via Admin Console/API. | **Channel:** HTTPS (VPN) <br> **Interface:** Admin Console/API connecting to Monitoring & Alerting and Reporting Hub |
| Support Agent          | Handles disputes and monitors payment workflows. Interacts with the Monitoring & Alerting system and Reporting Hub via Support Dashboard. | **Channel:** HTTPS (VPN) <br> **Interface:** Support Dashboard connecting to Monitoring & Alerting and Reporting Hub |
| Admin                  | Configures platform rules, roles, and settings. Manages security and compliance technical components. Interacts with the Administration Hub and Security Hub via Admin Console. | **Channel:** HTTPS (VPN) <br> **Interface:** Admin Console connecting to Administration Hub and Security Hub |
| Reporting User         | Accesses reporting and transaction insights. Interacts with the Reporting Hub via BI Dashboard. | **Channel:** HTTPS (VPN) <br> **Interface:** BI Dashboard connecting to Reporting Hub |
| Financial Institution  | Bank or provider for Open Banking and settlements. Initiates/settles bank payments and fetches data. Interacts with the Integration Hub and Payment Hub Services via Open Banking APIs. | **Channel:** HTTPS APIs <br> **Interface:** Open Banking APIs connecting to Integration Hub and Payment Hub Services |
| Payment Processor      | 3rd-party system to execute payments. Sends/receives payment instructions. Interacts with the Payment Hub Services via HTTPS/Secure Channel. | **Channel:** HTTPS (VPN) <br> **Interface:** Secure Channel connecting to Payment Hub Services |
| Business Intelligence Platform | Consumes anonymized data for reporting. Interacts with the Reporting Hub via Kinesis/S3/Redshift. | **Channel:** HTTPS <br> **Interface:** Kinesis/S3/Redshift connecting to Reporting Hub |
| External Monitoring System | Monitors system health and performance. Interacts with the Monitoring & Alerting system via APIs. | **Channel:** HTTPS (VPN) <br> **Interface:** APIs connecting to Monitoring & Alerting system  |

### 3.1.1 Interaction Details

**Customer, Merchant, Integrator**: These actors interact with the Payment Hub Services primarily through HTTPS over the internet, however we can support VPN for high secure customers in case that it is required. Customers and merchants use web, desktop or mobile applications, while integrators use REST APIs and webhooks for integration.

**Compliance Officer, Support Agent, Admin, Reporting User**: These internal users interact with the system through secure VPN channels. They use various dashboards and consoles to access the Monitoring & Alerting system, Reporting Hub, Administration Hub, and Security Hub.

**Financial Institution, Payment Processor**: These external systems interact with the Payment Hub Services and Integration Hub through secure HTTPS APIs and channels. They handle payment instructions, settlements, and data exchange. In general the interaction between the system and the app will happen through Open Banking APIs, but in some cases we can use other channels like secure channels or even direct connections with the different payment processors (P2P, Credit Card, Crypto, etc.).

**Business Intelligence Platform**: This platform consumes anonymized data from the Reporting Hub through secure HTTPS channels, using services like Kinesis, S3, and Redshift for data streaming and storage.

**External Monitoring System**: This system monitors the health and performance of the Payment Hub Services and Integration Hub. It interacts with the Monitoring & Alerting system through secure HTTPS channels, providing insights and alerts based on predefined rules. Also provides logging analytics and performance metrics that enhances the overall monitoring capabilities of the system.


# 4 System Context and Scope


Based on the context diagrams we can detail the different components of the solution

All the C4 Code Diagrams have been skipped for simplicity.

## 4.1 Container Diagram

Understanding the Containers a separately runnable/deployable unit (e.g. a separate process space) that executes code or stores data, we can define the following containers:

![Container Diagram](./assets/uml-render/03-ContainerDiag/03-container-diagram.svg)

It is assumed that all the components interacts with the security components and therefore this relations are not shown. 

It is assumed that all the components are administered by the admin console and therefore this relations are not shown.

It is assumed that all the components might be deployed using CI/CD outside of the system and therefore this relations are not shown.

It is assumed that all the components report logs and alerts to the alerting system


| Container | Domain | Description | Responsibilities |
|---|---|---|---|
| Payment API Gateway   | Payment Hub | Public-facing API Gateway exposing REST endpoints for initiating and tracking payments.       | - Receive payment and refund requests<br>- Route requests to orchestrator<br>- Handle auth tokens |
| Payment Orchestrator  | Payment Hub | Core orchestrator of the payment lifecycle (validate, authorize, settle).       | - Coordinate payment steps<br>- Manage state and workflows<br>- Interact with processor and banks |
| Payment Ledger & Data Manager        | Payment Hub | Central DBS that store all events and their states. Also store the customer Data  <br> Handles transformation and anonymization of transactional data. | - Persist transaction data<br>- Enable querying by status<br>- Power reporting and analysis<br>- Format and anonymize data<br>- Feed streams for reporting<br>- Interface with analytics |
| Risk Assessment Engine| Payment Hub | Service to calculate risk scores and perform fraud detection checks.            | - Analyze payment context<br>- Check for fraud indicators<br>- Respond with risk level |
| Open Banking Gateway  | Integration Hub          | Exposes RESTful APIs for integrators to connect with Open Banking services.     | - Provide access to bank/payment initiation<br>- Route external payment flows securely |
| Open Banking Services | Integration Hub          | Internal services that perform Open Banking actions (initiation, consent, confirmation).      | - Orchestrate Open Banking flows<br>- Manage token exchange and compliance<br>- Stores banking information |
| Alert Engine          | Monitoring & Alerting    | Detects anomalies or failure conditions and triggers alerts via EventBridge.    | - Watch transaction events<br>- Generate alerts on threshold violations or patterns |
| Logging Service       | Monitoring & Alerting    | Central log collection and correlation layer.     | - Aggregate service logs<br>- Push logs to external systems<br>- Enable traceability |
| Support Advisor       | Monitoring & Alerting    | Support-facing service providing insights from logs and context during incidents.             | - Correlate logs to errors<br>- Display incidents and events<br>- Assist human triage |
| Analytics Engine      | Reporting Hub            | Processes anonymized data to provide aggregated insights.      | - Run queries on transformed data<br>- Interface with BI platforms<br>- Support compliance metrics |
| Configuration Platform| Administration Hub       | Backend for applying configuration changes to services.        | - Validate and apply settings<br>- Notify affected components<br>- Audit configuration events |
| Identity Provider     | Security Hub             | Manages authentication and access control for all users.       | - OAuth2/OpenID flows<br>- Token issuance<br>- User session validation |
| Security Services     | Security Hub             | Handles encryption, key rotation, secrets, and IAM policies (That can be centralized).   | - Enforce platform-wide security<br>- Store/manage secrets and keys<br>- Integrate with AWS security |      

## 4.2 Components Diagrams

### 4.2.1 Payment Hub - Payment API Gateway

The Payment API Gateway acts as the secure entry point for all external interactions with the payment platform. It:

1. Exposes public-facing REST APIs for initiating and tracking payments.
2. Applies WAF and Shield protections for security and DDoS mitigation.
3. Integrates with Identity Provider (IdP) for authentication and authorization.
4. Routes validated requests to backend services through NLB and PrivateLink.
5. Publishes events to Kafka and SQS for real-time and asynchronous processing.
6. Supports a multiregion deployment with static content delivered via CloudFront + S3.


![Payment API Gateway](./assets/uml-render/04.01-ComponentPaymentGW/04-component-payment-api.svg)


| Component/Actor | Description | Inputs | Outputs |
|:---:|:---:|:---:|:---:|
| Route 53 | DNS resolution for external requests to APIs      | DNS queries from customers/integrators     | Resolved DNS records pointing to API Gateway or CloudFront     |
| AWS Shield       | DDoS protection service         | External malicious traffic        | Drops malicious requests   |
| AWS WAF  | Web Application Firewall for traffic filtering    | HTTPS requests from Route 53      | Sanitized and inspected traffic forwarded to API Gateway        |
| CloudFront       | CDN for static content delivery | Requests for UI/static content    | Delivers static files (HTML, JS, CSS) |
| S3 (Static)      | Stores static web content served through CloudFront | Content upload (CI/CD, admin)     | Static files to CloudFront |
| API Gateway      | Primary API gateway for payment platform | Authenticated REST calls from customers, merchants, integrators      | Routed requests to NLB, calls to Identity Provider, publishes events to queues   |
| Auth API Gateway | API gateway dedicated to acquiring JWT tokens (Auth flows) | Authentication requests from client apps   | Token acquisition, redirects, token validation results  |
| Identity Provider| Central identity/authentication service  | Token validation and acquisition requests  | Returns claims, session data, access tokens  |
| Network Load Balancer | Routes traffic from API Gateway to backend services securely     | Validated HTTPS traffic from API Gateway   | Sends requests to backend orchestration or VPC endpoints       |
| VPC Endpoints     | Private networking bridge to access Kafka, SQS, and internal services       | Requests from API Gateway and NLB | Secure internal access to Kafka topics, SQS queues    |
| Kafka Pub (A)    | Publishes events to Apache Kafka (Region A)       | Events from API Gateway   | Kafka messages to MSK topic  |
| Kafka Pub (B)    | Publishes events to Apache Kafka (Region B)       | Events from syncer or API Gateway | Kafka messages to MSK topic  |
| SQS Queue (A)    | Stores async requests for later processing (Region A)      | Messages from API Gateway or syncer | Pulled by Payment Orchestrator      |
| SQS Queue (B)    | Stores async requests for later processing (Region B)      | Messages from API Gateway or syncer | Pulled by Payment Orchestrator      |
| Syncer Lambda    | Syncs messages/events across regions (A ↔️ B)      | Kafka and SQS events from Region A or B    | Republishes events to opposite region's Kafka and SQS |

### 4.2.2 Payment Hub - Payment Orchestrator

The payment orchestrator is the core of the system, it is responsible for the payment processing and orchestration of the different components. It is composed by multiple components that are responsible for different tasks. This run in a cluster of ECS Fargate and are deployed using a CI/CD pipeline. The orchestrator is responsible for the payment processing, validation, authorization, settlement and risk assessment. It connects the platform to the needed external or internal systems to execute payments correctly and securely.

A modular containerized architecture running on ECS Fargate, with services for:

1. Real-time and async execution
2. Error handling and retries
4. Network selection for outbound traffic through NAT Gateways
5. Is deployed in a multiregion setup, with internal Route 53 DNS and regional failover support.

It provides an event-driven and scalable orchestration engine for high-volume payment processing.


![Payment Orchestrator](./assets/uml-render/04.02-ComponentPaymentOrch/04-component-payment-orchestrator.svg)

| Component/Actor | Description | Inputs | Outputs |
|:---:|:---:|:---:|:---:|
| Validation Service | Performs business rule validations       | Triggered by Async Executor<br>Incoming payment context         | Triggers Risk Assessment<br>Triggers Authorization<br>Triggers Network Selector             |
| Authorization Engine | Verifies if the payment is allowed to proceed         | Triggered by Validation Service or Executors<br>May request Identity context    | Triggers Payment Settlement<br>Triggers Network Selector      |
| Risk Assessment   | Calculates fraud and risk scores for transactions      | Triggered by Validation<br>Calls Risk Assessment Engine         | Returns risk scoring result to Validation       |
| Real-time Executor | Handles low-latency, synchronous payment execution     | Kafka / SQS message from API Gateway | Triggers Validation, Authorization and Settlement             |
| Async Executor    | Handles async settlement and long-running payments     | Kafka / SQS message from API Gateway | Triggers Validation, Authorization and Settlement             |
| Network Selector  | Selects which NAT path or provider to use | Triggered by Authorization / Validation           | Routes payment via selected NAT to external providers         |
| Error Handler     | Handles payment processing failures and retries        | Triggered on failure from orchestrator components | Logs incidents, retries logic, possible alerting |
| Payment Settler   | Performs the final step of sending a payment           | Triggered by Authorization<br>Receives context + provider info  | Writes result to Payment Ledger<br>Sends response to third parties            |
| Read Payment      | Handles read operations on payment data  | Called by API Gateway or external service         | Reads from Payment Ledger and returns current state           |
| ECS Fargate Services | Cluster hosting all orchestrator components          | Receives NLB-routed requests     | Runs containers, coordinates components         |
| ECS Cluster       | Orchestration layer for running tasks    | Runs Fargate task definitions    | Pulls container images from ECR   |
| ECR Images        | Container image registry for all services | Receives image builds            | Provides container images to ECS  |
| NAT Gateway       | Enables outbound access to external systems            | Used by Network Selector         | Sends payment instructions to Financial Institutions and 3rd Parties          |
| Route 53 (internal) | DNS resolution for orchestrator services | Called by API Gateway and 3rd Party Gateway       | Resolves ECS internal endpoints   |
| Risk Assessment Engine | External or centralized risk scoring engine        | Called by local Risk Assessment service           | Provides risk score and indicators |
| Identity Provider | Verifies JWT, user profiles, scopes, and mTLS identity | Called by Fargate components (Validation, Authz, Risk, etc.)    | Returns tokens, scopes, cryptographic trust     |
| Security Services | Handles encryption, key management, mTLS, and IAM      | Called by orchestrator containers   | Provides cryptographic operations and compliance functions    |
| Payment Ledger & Data Manager | Central state storage and transformation     | Written to by Payment Settler<br>Read by Read Payment           | Stores, retrieves, and anonymizes transactional data          |


### 4.2.3 Payment Hub - Payment Ledger & Data Manager

Payment Ledger & Data Manager is the core data platform responsible for securely storing, managing, and transforming all payment-related information. It handles:

1. Operational data (e.g., transactions, settlements) via fast, ACID-compliant databases like Aurora.
2. Non-transactional data (e.g., customer profiles, metadata) using scalable NoSQL storage like DynamoDB.
3. Low-latency access through ElastiCache for frequently accessed data.
4. Data lifecycle management, with archival tiers via S3 Glacier.
5. **Anonymization** and **reidentification** of personal data using Lambda and Athena CTAS queries.
6. Multiregion replication and internal DNS (Route 53) for service discovery.

It ensures compliance, data protection, and real-time availability of critical payment data across regions.

![Payment Ledger & Data Manager](./assets/uml-render/04.03-ComponentPaymentDB/04-component-payment-db.svg)

| Component/Actor | Description | Inputs | Outputs |
|:---:|:---:|:---:|:---:|
| Operational Payment DB (Aurora) | Stores secured transactional payment data (hot/cool tiers) | Payment transactions (structured JSON)   | Payment records (SQL rows), replicated data |
| Non-Transactional Store (DynamoDB) | Stores customer, merchant, and 3rd party profiles & metadata | User profiles, merchant info (JSON)    | Profile records, metadata entries        |
| Hot Cache (ElastiCache) | Caches frequently accessed payment/session data             | Payment state lookups (JSON keys)      | Cached payment/session records           |
| S3 Archive (Glacier)    | Long-term cold storage for TTL or historical datasets      | Archived JSON, Parquet or JSON files from Firehose       | Archived datasets (cold-tier files)      |
| S3 Hot Storage          | Stores recent anonymized non-PII datasets    | Anonymized JSON/Parquet documents      | Datasets for analysis/reporting         |
| Anonymization Lambda    | Lambda that anonymizes or reidentifies sensitive data      | Payment or profile JSON documents      | Anonymized data (JSON/Parquet) to S3 Hot Storage          |
| Data TTL Selector Lambda| Lambda that identifies old data to archive/delete          | Metadata tags, timestamps, TTL configurations           | Selected datasets for TTL stream         |
| Kinesis Stream          | Real-time streaming layer for TTL data       | TTL event metadata (JSON records)      | Event stream to Firehose   |
| Data Firehose           | Transformation + delivery to archive storage | JSON stream from Kinesis | JSON/Parquet files in S3 Archive          |
| Query Engine (Athena)   | Serverless query engine for anonymized & archived data     | SQL/CTAS queries, references to S3 objects | CTAS output, anonymized files, query results |
| Internal Route 53       | Private DNS resolution across VPCs & components            | DNS requests for internal data services   | Resolved Aurora, DynamoDB, Lambda endpoints |
| Identity Provider (IdP) | Provides token verification for data access/authentication | JWTs, access tokens      | Validated identities, claims             |
| Security Services       | Handles encryption, decryption, and KMS-based controls     | Encrypted values, KMS keys             | Decrypted/secured data     |
| Payment Orchestrator    | Writes transactions and reads data for payment flows       | Payment requests (JSON), query operations | Data read/write to Aurora, Dynamo, Redis |
| Risk Assessment Engine  | Writes risk evaluations or uses profile metadata          | Risk score input, user profile (JSON)  | Risk score output, profile lookups       |
| Analytics Engine        | Reads anonymized data for reporting          | Anonymized Parquet/JSON files from S3 Hot  | BI dashboards, metrics, insights         |

### 4.2.4 Payment Hub - Risk Assessment Engine

The Risk Assessment Engine evaluates transactions for potential fraud using a combination of rule-based logic and machine learning. It supports:

1. Real-time ingestion of transaction events via Amazon Kinesis.
2. Initial processing and workflow orchestration with AWS Lambda and Step Functions.
3. Scoring through Amazon Fraud Detector (rules + ML) and SageMaker (custom AI models).
4. Alerting via SNS for rejections or anomalies.
5. Historical and contextual data access from the Payment Ledger & Data Manager.
6. Model training pipeline using SageMaker Trainer with training data prepared by Lambda and stored in S3.

The engine is designed for multiregion deployment, integrates securely with the Identity Provider and Security Services, and ensures fast, accurate, and explainable risk decisions for every payment.

![Risk Assessment](./assets/uml-render/04.04-RiskManagement/04-component-risk-Assessment.svg)

| Component/Actor | Description | Inputs | Outputs |
|:---:|:---:|:---:|:---:|
| API Gateway            | Exposes internal REST endpoint for triggering risk evaluations or training | HTTPS requests with transaction to review (JSON)           | Invokes transaction processor         |
| Kinesis Stream         | Ingests real-time transaction events for risk evaluation and training            | Transaction events (JSON records) from Payment Orchestrator           | Delivers events to Lambda and Step Functions            |
| Transaction Processor (Lambda) | Consumes transaction events and starts the decision workflow      | Events from Kinesis stream | Triggers Step Functions with enriched event payloads    |
| Risk Decision Flow (Step Functions) | Coordinates rule and ML-based scoring, makes accept/reject decisions | Input JSON from Lambda    | Accept/reject result, SNS notifications   |
| Amazon Fraud Detector  | Performs rule-based and machine learning scoring       | Transaction data (JSON) from Step Function | Risk score or rule-based decision      |
| SageMaker Predictor    | Runs a custom AI model for fraud scoring | Transaction feature vector (JSON)      | Risk prediction score    |
| SageMaker Trainer      | Trains new ML models with historical labeled data      | Training dataset from S3  | Model artifact stored in S3            |
| Model Training Lambda  | Prepares and sends training data to S3   | Training samples from Kinesis stream or internal event triggers       | Writes processed datasets to S3        |
| S3 Training Hot Storage| Stores model training datasets        | Processed training files (CSV/Parquet) | Available for SageMaker training       |
| S3 Fraud Hot Storage   | Stores trained model artifacts and fraud metadata     | Model output from SageMaker Trainer    | Used by SageMaker Predictor            |
| SNS (Fraud Alert)      | Notifies other systems of fraud alerts or transaction rejections     | Reject event from Step Functions       | Email/SMS/Webhook notification         |
| Payment Orchestrator   | Sends transactions for evaluation and receives decision | JSON transaction (input) | Accept/reject response (JSON)          |
| Payment Ledger & Data Manager | Provides historical data for evaluation           | Lookup request from Step Functions     | Customer profiles, transaction context (from DynamoDB/Aurora)         |
| Route 53 Internal      | Resolves internal service addresses   | DNS queries for services  | DNS resolution for API, database, or Lambda endpoints   |
| Identity Provider (IdP)| Verifies identities and authorizations (if needed)    | Token validations (optional per service)  | Claims and permissions   |
| Security Services      | Manages encryption and key-based access controls       | Encrypted datasets or model output     | Decrypted or signed content            |

### 4.2.5 Integration Hub - Open Banking Gateway 

The Open Banking Gateway is the secure API entry point for all Open Banking interactions between the platform and financial institutions or aggregators. It:

1. Exposes public REST APIs via Amazon API Gateway and protects them using WAF and AWS Shield.
2. Supports OAuth2/OpenID Connect authentication with a dedicated authorization gateway.
3. Uses NLB + PrivateLink to securely route traffic to the Open Banking Services hosted within private VPCs.
4. Leverages Route 53 for internal and external DNS resolution.
5. Communicates with financial institutions over secure channels, including site-to-site VPN if needed.

![Open Banking Gateway](./assets/uml-render/04.05-OBGW/04-component-ob-api.svg)

| Component/Actor | Description | Inputs | Outputs |
|:---:|:---:|:---:|:---:|
| API Gateway  | Public-facing REST API gateway for Open Banking flows      | HTTPS API requests from financial institutions and integrators  | Routed requests to backend services via NLB        |
| Authentication API Gateway| Dedicated gateway to handle OAuth2/OpenID Connect token flow             | Authentication requests from clients | Access tokens / claims via Identity Provider       |
| Identity Provider (IdP)    | Validates tokens and user scopes           | JWT tokens from API Gateway or Auth Gateway       | Token validation results and user claims           |
| Web Application Firewall   | Filters malicious traffic    | Incoming HTTPS traffic from Route53 | Clean and secured traffic to API Gateway           |
| AWS Shield   | Protects APIs from DDoS attacks            | Potential volumetric or layer 3/4 attacks         | DDoS mitigation     |
| Network Load Balancer      | Routes validated internal API traffic to services          | HTTPS traffic from API Gateway   | Internal routing to VPC Endpoints |
| VPC Endpoints | Private network access to internal services (e.g., Open Banking Services)            | Traffic from NLB   | Routed requests to Open Banking Services           |
| Route 53     | Resolves internal and external DNS records   | DNS queries from Financial Institutions and internal services   | DNS records for API Gateways and internal endpoints |
| Open Banking Services      | Handles actual Open Banking payment initiation and account access flows  | HTTPS requests routed from API Gateway            | Payment initiation, confirmation responses, account data         |
| Security Services          | Manages encryption, secrets, and key access  | Key access requests from services   | Decryption, signing, secure secrets access         |
| Financial Institution      | External actor consuming or integrating via Open Banking APIs            | HTTPS requests, DNS resolution   | Open Banking communication over VPN or TLS        |

### 4.2.6 Integration Hub - Open Banking Services

The Open Banking Services component handles all internal business logic required to interact with financial institutions using Open Banking protocols. It is based on Microservices Architecture and it consists of:

1. The Account Service for balance inquiries and account data access.
2. The Payment Service for initiating and managing payments.
3. The Sync Service to coordinate and update external data from bank APIs.

Deployed on ECS Fargate, it securely connects to external institutions via VPN or secure channels, caches session/account data with ElastiCache, and enforces strong security via mTLS, IAM, and token validation from the Identity Provider.

It is fully integrated into the platform via the Open Banking Gateway and communicates bi-directionally with the Payment Orchestrator for operational requests and check the actual data storage, leveraging the data management to the Payment Hub.

![Open Banking Services](./assets/uml-render/04.06-OBServices/04-component-ob-services.svg)

| Component/Actor | Description | Inputs | Outputs |
|:---:|:---:|:---:|:---:|
| Account Service       | Manages account-related requests via Open Banking APIs            | HTTPS requests (e.g., account info, balances) from Gateway         | Account details, account access confirmations          |
| Payment Service       | Handles initiation and status of Open Banking payments            | Payment initiation requests from Payment Orchestrator | Payment execution, confirmation status   |
| Sync Service          | Synchronizes data with external banking systems     | Scheduled triggers or responses from external APIs   | Updated local data, sync status, external call results |
| ECS Fargate Services  | Hosts and runs containerized Open Banking services  | Routed traffic from NLB | Executes application containers       |
| ECS Cluster           | Manages Fargate service scheduling and scaling      | Container task definitions           | Deploys containers to Fargate         |
| ECR Images            | Stores container images for services  | CI/CD pushes, ECS image pulls       | Container artifacts     |
| Network Load Balancer | Routes HTTPS traffic from Gateway to backend services             | HTTPS requests from API Gateway or Gateway-to-VPC    | Forwards requests to ECS services     |
| Route 53 (Internal)   | DNS resolution for backend services   | Internal service discovery queries  | DNS names for NLB and services        |
| Hot Cache (ElastiCache Redis) | Caches frequently accessed data (e.g., session tokens, sync results) | Session/account data reads          | Quick-access key/value responses      |
| Identity Provider (IdP)| Authenticates and authorizes Open Banking requests | OAuth2 / mTLS token validations      | User claims, scopes     |
| Security Services     | Manages encryption, secrets, and access control     | Key requests from services           | Encrypted/decrypted content, secret access             |
| Open Banking Gateway  | Routes public Open Banking API requests to internal services      | HTTPS requests from external banks or TPPs           | Routed traffic to NLB   |
| Financial Institution | External system for Open Banking integration        | HTTPS calls from Sync Service       | Account/payment data, sync response  |
| Payment Orchestrator  | Core backend invoking Open Banking flows            | HTTPS requests to Account/Payment Service            | Responses confirming action success/failure            |

### 4.2.7 Integration Hub - 3rd Party Gateway

The 3rd Party Gateway exposes the platform’s public APIs to external integrators, allowing them to initiate payments, request refunds, and query transaction status. the behavior is similar as other Gateways

![3rd Party Gateway](./assets/uml-render/04.07-3RDGW/04-component-3rd-api.svg)


| Component/Actor | Description | Inputs | Outputs |
|:---:|:---:|:---:|:---:|
| API Gateway  | Public REST API interface for external integrators          | HTTPS requests from integrators     | Routed requests to backend services (e.g., Payment Orchestrator)    |
| Authentication API Gateway| Manages OAuth2/OpenID token acquisition and auth flows      | Token requests from clients         | Access tokens and user claims        |
| Identity Provider (IdP)    | Validates JWTs, scopes, and client identities | OAuth2 token requests or validations   | Claims, token metadata |
| WAF          | Filters malicious traffic at the edge      | HTTPS traffic from Route 53         | Cleaned and secured traffic          |
| AWS Shield   | Protects the gateway from DDoS attacks     | L3/L4 level malicious traffic       | DDoS mitigation or rate-limiting     |
| Route 53     | Handles DNS resolution for gateway endpoints | DNS queries from integrators        | Domain name to IP resolution         |
| CloudFront   | Delivers static content to integrators (documentation, SDKs, UI assets)  | HTTPS requests for UI or API specs  | CDN-optimized content delivery       |
| S3 Static Content          | Stores UI assets, SDKs, documentation, and public info     | Uploads from dev team or CI/CD pipeline | Static files served via CloudFront   |
| Payment Orchestrator       | Receives routed API requests for core payment workflows    | HTTPS API calls from Gateway        | Executes payment validations, status queries, etc.    |
| Integrator (External Actor)| External platform/system integrating with the payment solution           | HTTPS requests (auth, payment, refund, status)       | Receives tokens, responses, confirmations             |
| Security Services          | Provides encryption, IAM policies, and secret management   | Requests from backend components    | Encrypted data, IAM policies         |

### 4.2.8 Monitoring & Logging - Alert Engine

The **Alert & Logging Engine** is responsible for collecting, processing, and distributing alerts and operational logs across the platform. It ingests logs and metrics from services in the **Payment Hub**, **Integration Hub**, and external accounts using **AWS CloudWatch**, **VPC Flow Logs**, and **CloudTrail**.

Key functions include:

1. Real-time **log ingestion** and **alarm evaluation** with CloudWatch.
2. **Event-driven alerting** through **Lambda**, **SNS**, and **SES** for email notifications.
3. **Integration with Datadog** via **VPN** for unified monitoring and observability.
4. Storage of historical logs in **S3 Intelligent Tiering**.
5. The **Support Advisor** consumes logs and alerts to provide agents with contextual incident insights.
6. Ensures **compliance and visibility** for support and security teams across multiregion environments.

Also the system allow the notification of specific alerts to the different stakeholders such as Compliance Officers.

![Alert Engine](./assets/uml-render/04.08-AlertEngine/04-component-alert-engine.svg)


| Component/Actor | Description | Inputs | Outputs |
|:---:|:---:|:---:|:---:|
| AWS CloudWatch     | Centralized logging, metrics, and alarm engine            | Logs/metrics from services, VPC Flow Logs, CloudTrail        | Alarms, metrics, and trigger events |
| CloudWatch Logs (origin & other)| Collects log data from platform and other accounts        | Application/service logs      | Log records accessible to CloudWatch   |
| CloudWatch Events  | Event bus for reacting to alarm thresholds or system changes            | Log alarms or rule-based triggers | Event triggers for processors or EventBridge         |
| EventBridge        | Routes events from AWS services and applications          | Events from CloudWatch/Services  | Routed events (optional cross-account) |
| Lambda (Alert Processor)         | Handles triggered alarms and formats alert content        | Alarm events from CloudWatch  | Publishes messages to SNS           |
| Amazon SNS         | Notification system that routes alerts to subscribers     | Structured alerts from Lambda | Sends notifications to SES and external tools        |
| Amazon SES         | Sends email notifications to stakeholders   | Alert messages from SNS       | Emails to support/compliance        |
| Kinesis Data Stream | Stream layer for logs/metrics data       | Data from CloudWatch          | Forwards to Firehose  |
| Amazon Data Firehose             | Transforms and routes data to storage    | Data from Kinesis             | Persisted logs to S3  |
| S3 (Intelligent Tiering)         | Stores logs with auto-tiered storage for cost optimization | Transformed logs from Firehose   | Historical data for support and compliance           |
| S3 (Other Logging Store)         | Stores raw or secondary logs from external accounts       | Logs from CloudTrail, CloudWatch | Raw logs for forensic/analytics     |
| VPC Flow Logs      | Captures network traffic data            | VPC interfaces   | Logs to CloudWatch Logs             |
| AWS CloudTrail     | Tracks API activity across AWS services  | API calls, config changes     | Logs to S3 or CloudWatch            |
| Datadog (External Tool)          | Integrated APM/monitoring platform       | SNS alerts / CloudWatch logs  | Dashboards, monitors, alert triggers  |
| Support Advisor    | Reads logs/alerts and provides recommendations for support agents       | Log and alert data from S3    | Contextual data for incident handling  |
| Support Agent      | Consumes logs and alerts   | Log queries or email notifications             | Operational response   |
| Compliance Officer | Receives alert emails      | Alerts via SES  | Compliance actions    |
| Security Services  | Analyzes logs for potential security threats | CloudWatch logs/metrics       | Security decisions, encryption policies |

### 4.2.9 Monitoring & Logging - Support Advisor

The **Support Advisor** is virtual assistant designed to help support agents resolve incidents faster.

Agents connect securely via **VPN** and initiate a chat through **Amazon Lex**. The conversation triggers a **Lambda function**, which orchestrates a multi-step logic pipeline:

- **Amazon DynamoDB**: Stores historical support cases or contextual memory.
- **Amazon Kendra**: Searches internal documentation and FAQs, with fallback to the **Internet** when needed.
- **Amazon SageMaker**: Provides AI-generated natural language responses.
- **Amazon Bedrock**: Invoked when advanced code-based answers are required.
- **S3**: Supplies documents and knowledge base resources for Kendra.
- **Alert Engine**: Provides real-time incident context from logs and alerts.
- **Identity Provider & Security Services**: Ensure secure, authenticated access across all layers.

This architecture delivers a contextual, AI-powered assistant that accelerates problem resolution and improves the quality of support.

![Support Advisor](./assets/uml-render/04.09-SupportAdvisor/04-component-support-advisor.svg)


| Component/Actor | Description | Inputs | Outputs |
|:---:|:---:|:---:|:---:|
| Amazon Lex             | Conversational chatbot interface for support agents      | Chat messages from Support Agent (via VPN)      | Event to trigger Support Logic Lambda             |
| Support Logic Lambda   | Central orchestrator that handles queries and generates responses      | Input from Lex; Alerts; Authenticated requests  | Queries to Kendra, DynamoDB, SageMaker, Bedrock; final response |
| DynamoDB | NoSQL store with historical support context and interaction memory      | Query from Lambda | Historical insights and case data   |
| Amazon Kendra          | Intelligent search service for documentation and FAQs    | Query from Lambda | Relevant documents and articles  |
| S3 Document Storage    | Stores support documentation and FAQs accessed by Kendra | Fetch requests from Kendra      | Files/documents for responses    |
| Amazon SageMaker       | Generates natural language responses using AI/ML models  | Prompt from Lambda | AI-generated advice and guidance |
| Amazon Bedrock         | Used for generating code or technical answers (via generative AI)      | Invocation from Lambda          | Generated code or advanced response |
| Alert & Logging Engine | Supplies alert metadata and logging context for the current issue      | JSON request from Lambda        | Alert context (root cause, trace)   |
| Identity Provider      | Authenticates support agents and services  | Auth token request from Lambda  | Access claims and identity metadata |
| Security Services      | Ensures encryption and access control during all interactions          | Security request from Lambda    | Encrypted/decrypted data         |
| Internet | Used by Kendra when documentation is not available internally          | HTTP request from Kendra        | External search results          |
| Support Agent          | User who initiates chat and investigates incidents       | VPN-secured HTTPS chat input   | Response via Amazon Lex          |

### 4.2.10 Reporting Hub - Analytics Engine

The **Analytics Engine** processes and analyzes anonymized data stored in the Payment Hub. It uses a combination of AWS services to deliver structured insights and support internal and external reporting needs.

1. **AWS Glue** performs ETL and transformation, registering metadata in the **Glue Data Catalog**.
2. **Amazon EMR** can be used for large-scale distributed data processing when needed.
3. **Amazon Athena** enables direct SQL-based queries over the raw data in S3.
4. The transformed data is loaded into **Amazon Redshift**, a federated data warehouse.
5. **Amazon QuickSight** and **Amazon OpenSearch** are used for business intelligence dashboards and data exploration.
6. For external data sharing, **AWS Data Exchange** provides a controlled interface to share structured datasets with other BI tools.

![Analytics Engine](./assets/uml-render/04.10-AnalyticsEngine/04-component-analytics-engine.svg)

| Component/Actor | Description | Inputs | Outputs |
|:---:|:---:|:---:|:---:|
| S3 Anonymized Data    | Stores anonymized datasets generated by the Payment Hub       | Raw anonymized datasets      | Source for Glue and Athena  |
| AWS Glue | Performs ETL and transformation over S3 datasets | Data from S3 | Transformed data to Redshift, OpenSearch, QuickSight    |
| Glue Data Catalog     | Organizes dataset schemas and metadata for Glue and Athena    | Schema registration by Glue   | Metadata for querying and transformation  |
| Amazon EMR            | Distributed cluster for large-scale processing  | Transformed/intermediate datasets           | Optional batch processing outputs for storage or Redshift |
| Amazon Athena         | Ad hoc query engine over S3 data  | Queries and metadata          | On-demand query results  |
| Amazon Redshift       | Central federated warehouse for structured analytics          | Transformed data from Glue or EMR           | BI dashboards and data exports            |
| Amazon QuickSight     | Creates visual dashboards and analytics         | Data from Redshift or Glue    | BI dashboards            |
| Amazon OpenSearch     | Indexes structured data for exploration and visualization     | Data from Glue or Redshift    | Searchable, browsable data insights       |
| AWS Data Exchange     | Publishes analytics datasets externally for other BI tools    | Data from Redshift or Glue    | Exported data for third-party consumers   |

### 4.2.11 Administration Hub - Configuration Platform

The **Administration Hub** provides a centralized environment for platform administrators to manage, and operate the infrastructure and system

It is composed of standard AWS governance and management services, including:

- **Bastion Hosts** for secure shell access,
- **Control Tower** for multi-account governance,
- **Systems Manager** for operational control and scripting,
- **CloudFormation** for infrastructure as code, and
- **AWS Config** for continuous compliance monitoring.

The configuration platform acts as the intermediary between administrative actions and the broader system — applying changes to the Payment Hub, Monitoring, Reporting, Integration, and Security Hubs.

> [!INFO] 
> This representation is a **simplified view**. In a full production setup, some administrative operations may interact with other parts of the AWS environment beyond this hub, including CI/CD pipelines, Identity and Security services, and cross-region or cross-account operations.

![Configuration Platform](assets/uml-render/04.11-ConfigurationPlatform/04-component-administration-hub.svg)

| Component/Actor | Description | Inputs | Outputs |
|:---:|:---:|:---:|:---:|
| Bastion Host     | EC2 instance that enables secure access to internal resources.              | Admin session via SSH / SSM                 | Access to private infrastructure             |
| Control Tower    | Manages multi-account setup and landing zone governance.                   | Admin-defined account templates             | Account creation and guardrails              |
| Systems Manager  | Used for patching, automation, command execution and inventory.            | Admin requests, automation scripts          | Executed commands, patch status, logs        |
| CloudFormation   | Infrastructure as Code engine for repeatable and versioned deployments.    | Templates (YAML/JSON)                       | Cloud infrastructure resources               |
| AWS Config       | Monitors configuration compliance of resources.                            | AWS changes / events                        | Compliance evaluations, change logs          |
| Configuration Platform | Central coordination layer for updating configuration across hubs.    | Admin Console / APIs                        | Updates applied to other platform hubs       |

### 4.2.12 Security Hub - Security Services

The **Security Services Hub** provides centralized, automated, and scalable security tooling for the High-Volume Payment Processing Platform. It includes core AWS services such as KMS, Secrets Manager, Certificate Manager, GuardDuty, Macie, Inspector, and Payment Cryptography.

This hub handles key management, secret storage, encryption/decryption, certificate provisioning, traffic filtering, vulnerability scanning, and anomaly detection. It also integrates with other hubs such as the Payment Hub, Monitoring Hub, Reporting Hub, and Integration Hub to deliver proactive security and compliance.

Administrators use this hub to configure and monitor the platform’s security posture, enabling automated response and continuous auditing. It serves as the backbone for secure operations across the architecture.

One of the important things is the Crytography Service that is responsible for the encryption and decryption of sensitive data, such as payment information, customer data, and other confidential information. It uses AWS Payment Cryptography to provide a secure and compliant solution for handling sensitive data.

> [!INFO] 
> The following interactions represent examples of possible security integrations.
These can and should be extended to fulfill regulatory, compliance, and security best practices according to the operational and enterprise context.

![Security Services](assets/uml-render/04.13.SecServices/04-component-security-services.svg)

| Component/Actor | Description | Inputs | Outputs |
|:---:|:---:|:---:|:---:|
| AWS KMS | Key management for encryption/decryption of sensitive data       | Encryption requests from other services             | Encrypted/decrypted data |
| Secrets Manager       | Secure storage for tokens, credentials, and API keys             | Secrets and credentials | Retrieved secrets         |
| Certificate Manager   | TLS/SSL certificate lifecycle management           | Certificate requests | Certificates for secure communication     |
| Amazon GuardDuty      | Threat detection using anomaly detection and threat intel feeds  | Logs and traffic data   | Alerts and findings      |
| Security Hub          | Aggregates security findings from various services | Security alerts from services         | Prioritized and normalized security insights            |
| Amazon Macie          | Detects PII/sensitive data in S3 and other sources | Data in S3 and other storage          | Alerts and sensitive data classification reports        |
| Amazon Detective      | Investigates and correlates findings from GuardDuty | Security logs, GuardDuty alerts       | Investigation insights   |
| Amazon Inspector      | Scans workloads for vulnerabilities  | Container/EC2 configuration and metadata            | Vulnerability findings   |
| AWS Network Firewall  | Stateful traffic filtering across VPC boundaries   | Network traffic      | Allowed/blocked traffic  |
| AWS Payment Cryptography | Performs cryptographic operations for secure payment processing | Payment API requests | Signed or encrypted payment operations    |

### 4.2.13 Security Hub - Identity Provider (IdP)

The **Identity Provider** component enables secure, federated authentication for all internal and external actors of the payment platform.  
It uses **Amazon Cognito User Pools** to authenticate users via standards like **SAML**, **OIDC**, and social providers.  
Once authenticated, **Cognito Identity Pools** issue temporary AWS credentials through **IAM roles**, enabling fine-grained access to specific platform components such as the Payment API, Admin Console, or Analytics Engine.  

This approach ensures centralized identity management, supports role-based access control, and integrates seamlessly with AWS-native services.

![IdP](assets/uml-render/04.13.SecServices/04-component-security-services.svg)

| Component/Actor | Description | Inputs | Outputs |
|:---:|:---:|:---:|:---:|
| Amazon Cognito User Pools| Handles user authentication via SAML, OIDC, or social identity providers.  | Login credentials / federation tokens            | Authenticated identity and claims                                      |
| Amazon Cognito Identity Pools | Maps authenticated users to AWS IAM roles for service access.          | Cognito tokens from User Pools                   | Temporary AWS credentials                                              |
| AWS IAM                  | Manages policies and permissions for accessing AWS services.               | Cognito Identity Pool credentials                | Scoped access to APIs and services across the platform                 |
| Payment API Gateway      | API that verifies user identity and executes transactions.                 | IAM-scoped credentials                           | Response to payment/refund request                                     |
| Admin Console            | Admin interface requiring elevated IAM permissions.                        | IAM role tokens                                  | Access to configuration and deployment operations                      |
| Open Banking Gateway     | Service for external bank connections requiring secure access.             | IAM credentials                                  | Authorized access to banking APIs                                      |
| Analytics Platform       | Platform for querying and visualizing data.                               | IAM credentials                                  | Access to datasets and dashboards                                      |
| Support Advisor          | Conversational tool for support agents.                                    | IAM credentials                                  | Responses, documentation links, alert context                          |

# 5 Runtime View

![Runtime](assets/runtime.gif)



The runtime view describes concrete behavior and interactions of the system’s building blocks in form of scenarios in order to show that the use cases are covered by the runtime of the application, for the case of this project we will just represent a single simplified workflow.

## 5.1 Scenario 1 - Payment (Simplified)

| Actor      | Description                                               |
|------------|-----------------------------------------------------------|
| Customer   | Initiates the payment through the platform                |
| Payment Processor | External system that processes the payment               |

| Participant          | Description                                                                 |
|----------------------|-----------------------------------------------------------------------------|
| Payment API Gateway         | Entry point for external API requests and initial validation                |
| Payment Orchestrator | Coordinates the full payment lifecycle (risk check, processing, settlement) |
| Risk Assessment Engine         | Assesses fraud risk and transaction trust level                             |
| Payment Ledger & Data Manager       | Stores the transaction state and result                                     |
| Identity Provider | Authenticates and verifies the identity of the user |

Workflow: 

> [!NOTE] 
> Please remember that this is a simplified version

<div class="mermaid">
sequenceDiagram
    autonumber
    participant Customer
    participant API Gateway
    participant Identity Provider
    participant Payment Orchestrator
    participant Risk Engine
    participant Processor
    participant Security Services
    participant Payment Ledger & Data Manager 

    Customer->>API Gateway: Submit Payment Request
    API Gateway->>Identity Provider: Validate Token
    Identity Provider-->>API Gateway: Identity Verified
    API Gateway->>Payment Orchestrator: Forward Payment Request
    Payment Orchestrator->>Risk Engine: Execute Risk Assessment
    Risk Engine-->>Payment Orchestrator: Risk Cleared / Blocked
    alt Risk Cleared
        Payment Orchestrator->>Processor: Request Authorization & Execute Payment
        Payment Orchestrator->>Security Services: Cryptography & Tokenization
         alt Payment Approved
            Processor-->>Payment Orchestrator: Payment Approved
            Payment Orchestrator->>Payment Ledger & Data Manager : Record Transaction
            Payment Orchestrator->>Customer: Notify Customer (Success)
         else Payment Declined
            Processor-->>Payment Orchestrator: Payment Declined
            Payment Orchestrator->>Payment Ledger & Data Manager : Record Transaction
            Payment Orchestrator->>Customer: Notify Customer (Declined)
         end
    else Risk Blocked
        Payment Orchestrator->>Customer: Notify Customer (Rejected)
    end
</div>

1. **Customer** initiates a payment request through the **Payment API Gateway**.
2. The API Gateway forwards the request to the **Identity Provider** to validate the customer's token and identity.
3. Once the identity is confirmed, the API Gateway sends the request to the **Payment Orchestrator**.
4. The **Payment Orchestrator** begins the payment flow by calling the **Risk Engine** to perform fraud and risk assessment.
5. The **Risk Engine** evaluates the transaction using rule-based and ML-based models and returns a result (cleared or blocked).
6. If the transaction is cleared:
   - The Payment Orchestrator request encryption and sends the request to the **Payment Processor** to authorize and execute the payment.
   - **Customer** is notified of the payment status.
7. If the transaction is blocked:
   - The Payment Orchestrator notifies the **Customer** that the payment was rejected due to Risk Clearance issues.
8. In any case, the **Payment Orchestrator** records the transaction in the **Payment Ledger**.

# 6 Design Decisions


This Section aim to document all the architectural decisions taken

| Problem | Constraints | Alternatives | Decision |
| --- | --- | --- | --- |
| Whether to include a separate deployment view | The architecture already includes detailed building blocks and AWS components | Create a specific deployment diagram       | **Not included**, as the existing diagrams already show network boundaries and component distribution |
| Selecting a centralized monitoring solution   | Must be cloud-friendly, support multi-account/multi-region, and be cost-efficient | Datadog, AppDynamics, Instana              | **Datadog** chosen due to strong integration capabilities, industry adoption, and better cost/quality ratio |
| MultiRegion Vs MultiZone | Must ensure high availability and disaster recovery | MultiRegion, MultiZone                     | **MultiRegion** chosen for better disaster recovery and availability, despite higher costs  and to comply with regulatory contraints and customer need for critical (money) operations |

# 7 Technical Risks

| Risk/Debt | Description | Mitigation/Additional Info |
|:---:|:---:|:---:|
| Cross-region latency and consistency issues    | Multiregion replication of services (e.g., DynamoDB Global Tables, Aurora) may introduce delays or conflicts | Use eventual consistency where appropriate, implement conflict resolution strategies <br> this can be mitigated on low level specification      |
| Vendor lock-in with AWS                        | Strong reliance on AWS-specific services (e.g., Bedrock, Kendra, Control Tower) limits portability | Abstract interactions via internal adapters or façade services for future migration ease <br> Having this abstraction might imply cost increase but technically is possible  |
| Complexity of distributed troubleshooting      | Event-driven, multi-region microservices can create a complex debugging and root cause analysis system       | Invest in observability (e.g., distributed tracing, Datadog APM, correlation IDs) <br> Most of this has been included           |
| Real-time risk assessment bottlenecks          | Synchronous fraud/risk evaluations may add latency to payment flows                              | Cache low-risk profiles, pre-score transactions, optimize SageMaker/Fraud Detector models <br> some of this can be tem porary stored in the caches if do not involve PII data  |
| Security misconfigurations across accounts     | Multi-account architecture increases chances of IAM, KMS, or Secrets or other misconfigurations           | Use Control Tower, AWS Config, and periodic security audits via Inspector and Security Hub <br> Most of this components have been included in the solution |
| Overhead of managing AI/ML lifecycle           | Use of SageMaker and Bedrock for support and fraud detection requires continuous tuning/training | Automate model lifecycle management and monitor drift with SageMaker Pipelines <br> Model should be trained with the information from Support on false positives             |
| Limited observability during external failures | Failures in third-party providers or banks may propagate downstream with little visibility        | Use fallback timeouts, retries, circuit breakers, and vendor status health tracking<br> Some of this logic might be already implemented in the Error handling       |
| Cost overruns due to monitoring and data tools | Use of tools like Datadog, Redshift, EMR, and Bedrock can incur high cost if not properly scoped  | Apply usage quotas, alerts, and periodic cost reviews using AWS Budgets and Cost Explorer  |

# 8 Quality Requirements

<div align="center">
	<code><img src="assets/qaok.gif" alt="QAOK" title="Quality Requirements"/></code>
</div>


| ID      | Quality                | Fulfillment Justification |
|:---:|:---:|:---:|
| QG-001  | Security               | The system integrates AWS-native security services (e.g., KMS, Secrets Manager, Network Firewall, Payment Cryptography, GuardDuty) and follows best practices (encryption, multi-account setup, TLS, mTLS, token validation) to ensure compliance with GDPR, PSD2, and AML regulations. |
| QG-002  | Security               | End-to-end encryption is enforced through KMS and ACM. IAM roles and Cognito with multi-factor authentication ensure least privilege and Zero Trust access. Each subsystem applies its own authentication and authorization checks, isolating services appropriately. |
| QG-003  | Compatibility          | The use of standardized Open Banking APIs, dedicated Open Banking Services, and Data Exchange modules ensures smooth integration with financial institutions. VPNs, Route53, and NAT Gateways facilitate secure connectivity. |
| QG-004  | Compatibility          | The 3rd Party Gateway and API abstraction layers enable integration with external PSPs via REST APIs, Webhooks, or encrypted channels. Network segmentation and validation layers ensure secure and flexible interactions. |
| QG-005  | Maintainability        | The architecture includes centralized alerting, logging, and monitoring through AWS CloudWatch, Kinesis, SNS, SES, and integration with Datadog. The Support Advisor further enhances observability and operational response. |
| QG-006  | Transferability        | Jenkins integration and CI/CD pipelines are defined in the Administration Hub, using infrastructure-as-code (IaC) tools like AWS CloudFormation. The solution supports portability across Jenkins instances and CI/CD environments. |
| QG-007  | Performance Efficiency | Horizontal scaling is enabled through Fargate-based container orchestration, multiregion setup (e.g., DynamoDB Global Tables, Aurora Global), and asynchronous/event-driven architecture using SQS, Kafka (MSK), and Step Functions. |
| QG-008  | Performance Efficiency | The architecture minimizes latency using real-time processing paths, Kinesis for streaming, ElastiCache for caching, and isolated execution paths (sync vs async). Core flows are designed for high throughput under concurrent load. |
| QG-009  | Reliability            | The platform is deployed across multiple regions, with built-in failover via Route53, SQS, Kafka sync, and regional replicas of stateful services. Cloud-native services ensure automatic failover and load distribution. |
| QG-010  | Reliability            | Key services use retries, circuit breakers, event sourcing, and idempotency to maintain consistency during partial failures. Backup and audit components (e.g., S3 tiered storage, data replication) ensure recovery and traceability. |
| QG-011  | Usability              | Administrators and support agents are provided with intuitive tools such as the Admin Console and Support Advisor (Lex-based conversational support), while developers interact through well-documented APIs and self-service tools. |


# 9 Glossary

| Term/Abbreviation         | Description |
|---------------------------|-------------|
| **API Gateway**           | AWS-managed service for exposing RESTful APIs. Used across hubs to expose microservices securely. |
| **Aurora**                | High-performance relational database used for storing secure transactional payment data. |
| **AWS**                   | Amazon Web Services — the main cloud provider used for the solution. |
| **AWS Glue**              | ETL (Extract, Transform, Load) service used to prepare and transform anonymized data. |
| **AWS KMS**               | Key Management Service used for encryption and secure key handling. |
| **AWS Lambda**            | Serverless compute engine used for tasks like orchestration logic, data anonymization, and alert processing. |
| **AWS Payment Cryptography** | Service that manages cryptographic operations required for card payments and secure transactions. |
| **Athena**                | Serverless query service to run SQL over data stored in S3. Used for querying anonymized datasets. |
| **Bedrock**               | AWS service that enables generative AI and code-based response generation. Used in the Support Advisor. |
| **CloudFormation**        | Infrastructure-as-Code service for provisioning resources. Used by administrators. |
| **CloudWatch**            | Monitoring and logging service used for alerts, metrics, and events. Integrated with Datadog. |
| **Cognito**               | AWS service that handles user authentication and identity federation via OIDC, SAML, or social logins. |
| **Control Tower**         | Tool for managing multi-account AWS environments with governance and compliance controls. |
| **Customer**              | The end-user who initiates payments, views statuses, and receives notifications. |
| **Data Exchange**         | AWS service to securely share and subscribe to external datasets. Used for BI integrations. |
| **Datadog**               | Third-party monitoring platform integrated via VPN to provide centralized observability and APM. |
| **DynamoDB**              | NoSQL database used for storing non-transactional data such as profiles, sessions, and metadata. |
| **EC2 Bastion Host**      | Secure access point to private VPC resources used for manual administration. |
| **ElastiCache**           | In-memory cache used for frequently accessed or session-related data. |
| **EMR**                   | Managed cluster for large-scale distributed data processing. Used in the Analytics Engine. |
| **EventBridge**           | Event bus for routing and handling asynchronous communication between services. |
| **Financial Institution** | External bank or provider that integrates through Open Banking or traditional payment APIs. |
| **Fraud Detector**        | AWS-managed service to detect potential fraud using rules and ML models. |
| **Glue Data Catalog**     | Central schema and metadata repository for Glue and Athena. |
| **GuardDuty**             | Threat detection service analyzing logs and network behavior to detect anomalies. |
| **IAM**                   | AWS Identity and Access Management used for assigning fine-grained permissions and roles. |
| **Inspector**             | AWS vulnerability scanning service to find security flaws in workloads. |
| **Integrator**            | External system or user that connects via third-party API Gateway. |
| **Kendra**                | Intelligent search engine used by Support Advisor to index documentation and fetch insights. |
| **Kinesis**               | Real-time streaming service for ingesting payment events and logs. |
| **Lex**                   | Conversational bot interface for support agents in the Support Advisor. |
| **Macie**                 | Service for identifying sensitive data in S3 and alerting on possible leaks. |
| **Merchant**              | Actor that receives payments and initiates refunds. Has access to dashboards and insights. |
| **Network Firewall**      | AWS-managed firewall service used to control traffic flow into and within VPCs. |
| **Open Banking Gateway**  | Entry point for Open Banking APIs enabling integration with financial institutions. |
| **OpenSearch**            | Managed search and analytics engine used to explore and visualize structured data. |
| **Payment Hub**           | Core platform subsystem responsible for payment validation, orchestration, and settlement. |
| **QuickSight**            | AWS visualization and reporting tool used to display analytical insights. |
| **Redshift**              | AWS data warehouse used to store and analyze structured, transformed data. |
| **Reporting Hub**         | Subsystem responsible for exposing metrics, dashboards, and regulatory reports. |
| **Risk Assessment Engine**| Subsystem evaluating fraud and risk using ML, rules, and historical analysis. |
| **Route 53**              | DNS service used for internal resolution and traffic routing across regions. |
| **S3 (Simple Storage Service)** | Object storage service used for logs, anonymized datasets, documentation, and archival data. |
| **SageMaker**             | Managed ML service used in fraud detection and Support Advisor response generation. |
| **Secrets Manager**       | Stores secure credentials, API keys, and database passwords. |
| **Security Hub**          | Aggregates alerts and findings from multiple security services for centralized visibility. |
| **SES (Simple Email Service)** | AWS service to send email notifications (e.g. fraud alerts, system notifications). |
| **SNS (Simple Notification Service)** | Messaging service used for pub/sub of alerts and event-based messages. |
| **Step Functions**        | Serverless workflow engine coordinating risk decisions and other orchestrations. |
| **Support Advisor**       | Support tool leveraging AI and search to assist agents in handling alerts and issues. |
| **System Manager (SSM)**  | AWS service for managing resources, executing commands, and maintaining patch compliance. |
| **VPC**                   | Virtual Private Cloud — isolated network environment for components in each AWS account. |
| **VPN (Virtual Private Network)** | Secure network tunnel used for accessing AWS services from internal tools or external systems. |


