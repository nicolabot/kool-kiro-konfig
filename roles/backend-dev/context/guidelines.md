# Backend Developer Guidelines

## Spring Cloud Function Lambda Pattern

Every Lambda is a Spring Boot application with a `Function<Input, Output>` bean:

```java
@SpringBootApplication
public class MyLambdaApplication {
    public static void main(String[] args) {
        SpringApplication.run(MyLambdaApplication.class, args);
    }

    @Bean
    public Function<Map<String, Object>, Map<String, Object>> myFunction(
            MyService myService) {
        return input -> myService.process(input);
    }
}
```

Do NOT use raw AWS Lambda handler interfaces (`RequestHandler`, `RequestStreamHandler`). Always use Spring Cloud Function beans.

## Maven Multi-Module Structure

- Each Lambda is a separate Maven module under `src/`
- Module name matches the Lambda function name in kebab-case (e.g., `pii-redaction-lambda`)
- Each module has its own `pom.xml` inheriting from the parent POM
- Use `maven-shade-plugin` to produce a shaded JAR for Lambda deployment
- Package naming: `com.anycompany.<module>` (e.g., `com.anycompany.pii`)

## AWS Client Configuration

Create AWS service clients as Spring `@Bean`s:

```java
@Bean
public S3Client s3Client() {
    return S3Client.create(); // uses default credential and region resolution
}
```

Never hardcode credentials or regions. Always rely on the Lambda execution environment defaults.

## Structured Logging

Use SLF4J with structured key-value fields:

```java
log.info("Processing complete", kv("duration_ms", duration), kv("row_count", rows), kv("error_count", errors));
```

Always log: duration, row/item counts, error counts, and S3 keys being processed.

## Error Handling

- **Retryable errors** (transient AWS failures, throttling): log the error and rethrow so Step Functions can retry
- **Non-critical failures** (single row parse error): log a warning and continue processing remaining rows
- **Fatal errors** (missing config, invalid input format): log error with context and throw immediately

## PII Redaction Pattern

When integrating with Amazon Comprehend for PII detection:

1. Call `DetectPiiEntities` with the text content
2. Iterate over returned entities in reverse offset order
3. Replace each entity span with `[REDACTED]`
4. The original PII must be irrecoverable — never store it separately

## Bedrock Integration Pattern

When calling Amazon Bedrock for categorization:

1. Use the Bedrock Runtime client (`BedrockRuntimeClient`)
2. Load the system prompt from Bedrock Prompt Management (not hardcoded)
3. Send transactions in batches (default: 200 rows per batch)
4. Parse the model response and merge categories back into the original CSV data
5. Handle model refusals or empty responses gracefully
