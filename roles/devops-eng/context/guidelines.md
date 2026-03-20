# DevOps Engineer Guidelines

## CloudFormation Conventions

### Nested Stack Architecture

- `main.yaml` is the single deploy entry point — it references all child stacks via S3 template URLs
- Child stacks are organized by AWS resource domain:
  - `vpc-networking.yaml` — VPC, subnets, security groups, VPC endpoints
  - `storage-iam.yaml` — S3 buckets, SQS DLQ, IAM roles
  - `pipeline.yaml` — Lambda functions, Step Functions, EventBridge
  - `monitoring.yaml` — CloudWatch, SNS, CloudTrail
  - `bedrock-prompt.yaml` — Bedrock Prompt Management
- Use `Parameters` for all configurable values, `Outputs` for cross-stack references
- Upload child templates to S3 deployment bucket before deploying

### Naming

- **Stack names**: kebab-case (e.g., `ai-txn-categorizer`)
- **Logical IDs**: PascalCase (e.g., `InputBucket`, `PiiRedactionLambda`)
- **Template files**: kebab-case `.yaml`
- **Resource names**: kebab-case where AWS allows custom names

### Deployment Pipeline

```bash
# Build
mvn clean package -q -f src/pom.xml

# Upload artifacts to S3
aws s3 cp IaC/<template>.yaml s3://$DEPLOY_BUCKET/templates/
aws s3 cp src/<module>/target/<jar>.jar s3://$DEPLOY_BUCKET/

# Deploy
aws cloudformation deploy \
  --template-file IaC/main.yaml \
  --stack-name ai-txn-categorizer \
  --parameter-overrides TemplateBucket=$DEPLOY_BUCKET \
  --capabilities CAPABILITY_NAMED_IAM CAPABILITY_AUTO_EXPAND
```

## VPC & Networking

- Private subnets only for Lambda functions — no public subnet exposure
- S3 Gateway Endpoint (free) for S3 access
- Interface Endpoints for Bedrock Runtime and Comprehend
- NAT Gateway for outbound internet access (if needed)
- Security groups scoped to specific endpoint traffic
- VPC endpoint policies restricting access to specific resources

## Security — IAM

- Least-privilege per Lambda function — each gets its own IAM role
- Scope permissions to specific S3 buckets, KMS keys, and API actions
- Never use `*` for resources in production IAM policies
- Use `Condition` keys where possible (e.g., `aws:SourceArn`)

## Security — Encryption

- KMS customer-managed keys for S3 bucket encryption at rest
- TLS 1.2+ enforced for all in-transit communication
- S3 bucket policies deny non-HTTPS access (`aws:SecureTransport`)

## Monitoring & Alerting

- CloudWatch Log Groups for every Lambda function (30-day retention)
- CloudWatch Alarms for:
  - Lambda errors > 0 in 5-minute window
  - Step Functions execution failures
  - DLQ message count > 0
- SNS topic for alarm notifications
- CloudTrail enabled for all API audit logging

## Compliance

- **ISO 27001**: Encryption (KMS), access controls (IAM), audit logging (CloudTrail), key management
- **SOC2**: Least-privilege IAM, monitoring (CloudWatch), change management via IaC
- **GDPR**: PII permanently redacted before processing, single-region data residency, S3 lifecycle for right to erasure
