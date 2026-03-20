---
name: cfn-template-scaffolding
description: Scaffold a new CloudFormation child template with parameters, resources, outputs, and integration into the AnyCompany main.yaml nested stack following team IaC conventions.
---

## Steps

1. Ask what AWS resources the new template will manage and its purpose
2. Create `IaC/<template-name>.yaml` with:
   - `AWSTemplateFormatVersion` and `Description`
   - `Parameters` for configurable values (bucket names, ARNs from other stacks)
   - `Resources` with PascalCase logical IDs and least-privilege IAM where applicable
   - `Outputs` for any values other stacks need to reference
3. Add the new child stack resource to `IaC/main.yaml` with S3 template URL and parameter mappings
4. If the template includes Lambda functions, add corresponding IAM roles scoped to specific resources
5. If monitoring is needed, add CloudWatch alarms and log groups
6. Present the generated template for review
