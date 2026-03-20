# Product

## Vision

AnyCompany provides accounting SaaS solutions for small and medium businesses, helping them manage financial operations. The platform is expanding to AWS to overcome on-premises capacity limits and introduce AI-powered features, starting with automated transaction categorization.

## Goals

1. Migrate core SaaS platform from on-premises VMs to AWS
2. Launch AI-powered transaction categorization with >90% accuracy
3. Maintain ISO 27001, SOC2, and GDPR compliance throughout migration
4. Reduce manual transaction categorization effort by 80%
5. Support 5,000–20,000 file uploads per month at launch

## Target Users

- **SMB Accountants**: Use the SaaS platform daily to manage client finances. Need accurate, automated categorization to reduce manual work.
- **SMB Business Owners**: Review categorized transactions for financial reporting. Need reliable data they can trust.
- **AnyCompany DevOps**: Manage infrastructure and deployments. Transitioning from on-prem VM management to AWS serverless.

## Core Features

**AI Transaction Categorization**
- CSV file upload from Java Spring app via AWS SDK
- PII detection and permanent redaction (Amazon Comprehend)
- AI categorization into 13 product categories (Amazon Bedrock)
- Results stored as CSV in S3, partitioned by date

**Platform**
- Multi-tenant SaaS accounting platform
- Java Spring backend with PostgreSQL database
- Event-driven serverless pipeline on AWS

## Success Metrics

- Transaction categorization accuracy ≥79.5% (Nova Lite baseline), target ≥90%
- Pipeline processing time <5 minutes per file
- Zero PII leakage to AI models
- Monthly infrastructure cost ≤$200 for categorization pipeline
