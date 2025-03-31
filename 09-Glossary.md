# 9 Glossary
<!-- TOC -->

- [9 Glossary](#9-glossary)
- [Solution Content](#solution-content)

<!-- /TOC -->

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