# Structure

## Directory Layout

```
src/                              # Java source code
├── pom.xml                       # Parent POM (multi-module)
├── pii-redaction-lambda/         # PII redaction Lambda module
│   ├── pom.xml
│   └── src/main/java/com/anycompany/pii/
└── categorization-lambda/        # Categorization Lambda module
    ├── pom.xml
    └── src/main/java/com/anycompany/categorization/

IaC/                              # CloudFormation templates
├── main.yaml                     # Parent nested stack (deploy entry point)
├── vpc-networking.yaml           # VPC, subnets, security groups, endpoints
├── bedrock-prompt.yaml           # Bedrock Prompt Management
├── storage-iam.yaml              # S3 buckets, SQS DLQ, IAM roles
├── pipeline.yaml                 # Lambda functions, Step Functions, EventBridge
├── state-machine-definition.json # Step Functions ASL definition
└── monitoring.yaml               # CloudWatch, SNS, CloudTrail

sample_data/
├── test/                         # Pipeline input CSVs (no categories)
└── validation/                   # Ground truth CSVs (with categories)

prompts/                          # AI prompt templates
└── categorization-system-prompt.txt
```

## Module Organization

Code is organized by Lambda function — each Lambda is a separate Maven module under `src/`. Each module is a self-contained Spring Boot application with its own `pom.xml`, shaded JAR output, and single `*Application.java` entry point.

IaC is organized by AWS resource domain — networking, storage/IAM, pipeline logic, and monitoring are separate CloudFormation templates composed via a parent nested stack.

## Naming Conventions

- **Java packages**: `com.anycompany.<module-name>` (e.g., `com.anycompany.pii`)
- **Java classes**: PascalCase, `*Application.java` for entry points
- **Maven modules**: kebab-case matching the Lambda function name
- **CloudFormation files**: kebab-case `.yaml`, logical IDs in PascalCase
- **S3 keys**: `customer/{customerId}/{date}/{filename}-{batchId}.csv`
- **Stack names**: kebab-case (e.g., `ai-txn-categorizer`)

## Key Files

- `src/pom.xml` — Parent POM defining Java 21, Spring Cloud Function, AWS SDK BOM, shade plugin
- `IaC/main.yaml` — Single deploy entry point, references all child stacks via S3 template URLs
- `.gitignore` — Excludes target/, .venv/, IDE files
