---
name: lambda-scaffolding
description: Scaffold a new AWS Lambda module with Spring Cloud Function bean, Maven pom.xml, application class, and CloudFormation resource definition following AnyCompany backend conventions.
---

## Steps

1. Ask for the Lambda function name (kebab-case) and its purpose
2. Create a new Maven module directory under `src/<lambda-name>/`
3. Generate:
   - `pom.xml` — inheriting from parent POM with shade plugin, Spring Cloud Function, and relevant AWS SDK dependencies
   - `src/main/java/com/anycompany/<module>/Application.java` — Spring Boot app with `Function` bean
   - `src/main/resources/application.yml` — Spring Cloud Function configuration
4. Add the new module to the parent `src/pom.xml` `<modules>` list
5. Generate a CloudFormation Lambda resource snippet for `IaC/pipeline.yaml` with least-privilege IAM
6. Present all generated files for review
