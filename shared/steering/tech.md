# Tech

## Stack

- **Language**: Java 21
- **Framework**: Spring Boot, Spring Cloud Function 4.1.3
- **Build**: Maven 3.x (multi-module POM)
- **AWS SDK**: v2.29.51
- **Runtime**: AWS Lambda (Java 21 managed runtime)
- **IaC**: AWS CloudFormation (nested stacks)
- **AI/ML**: Amazon Bedrock (Nova Lite), Amazon Comprehend
- **Evaluation**: Python 3.x for model evaluation scripts

## Architecture

Event-driven serverless pipeline:

1. AnyCompany Java app uploads CSV to S3 input bucket (AWS SDK, IAM auth)
2. EventBridge detects upload, triggers Step Functions workflow
3. PII Redaction Lambda reads CSV, calls Comprehend `DetectPiiEntities`, permanently redacts PII, splits into batches, writes to S3 staging
4. Step Functions Inline Map invokes Categorization Lambda per batch in parallel
5. Each Categorization Lambda calls Bedrock Nova Lite, merges categories, writes result CSV to S3 output

Key services: S3, EventBridge, Step Functions, Lambda, Comprehend, Bedrock, VPC with private endpoints, KMS, CloudTrail.

## Database

- **Production platform**: PostgreSQL (on-premises, migrating to RDS)
- **AI pipeline**: No database — CSV in/out via S3

## Infrastructure

- **Cloud**: AWS
- **Networking**: VPC with private subnets, NAT Gateway, S3 Gateway Endpoint, Bedrock Runtime Interface Endpoint
- **Security**: KMS encryption at rest, TLS 1.2+ in transit, least-privilege IAM, VPC endpoint policies
- **Monitoring**: CloudWatch Logs/Alarms, SNS notifications, CloudTrail audit logging
- **CI/CD**: Maven build → S3 artifact upload → CloudFormation deploy (to be automated)

## Coding Standards

- **Package naming**: `com.anycompany.<module>` (e.g., `com.anycompany.categorization`)
- **Lambda handlers**: Spring Cloud Function `Function<Input, Output>` beans, not raw AWS handler interfaces
- **Logging**: SLF4J with structured fields (duration, row counts, error counts)
- **Error handling**: Log and rethrow for retryable errors, log and return empty for non-critical failures
- **AWS clients**: Created as Spring `@Bean`, use default credential/region resolution
- **CloudFormation**: Nested stacks via `main.yaml` entry point, parameterized, outputs for cross-stack references
- **IAM**: Least-privilege per Lambda function, scoped to specific S3 buckets and API actions
- **Naming**: kebab-case for stack names and resource names, PascalCase for CloudFormation logical IDs
