# 7 Technical Risks
<!-- TOC -->

- [7 Technical Risks](#7-technical-risks)
- [Solution Content](#solution-content)

<!-- /TOC -->

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