# kool-kiro-konfig

Shared Kiro CLI configuration for the AnyCompany team. This repo contains steering docs, role-specific agent configs, context files, and skills that give every team member a consistent, tailored AI dev environment.

## Available Roles

| Role | Folder | Description |
|------|--------|-------------|
| Frontend Developer | `roles/frontend-dev/` | UI components, design system, accessibility, state management |
| Backend Developer | `roles/backend-dev/` | Lambda functions, Spring Cloud Function, Comprehend/Bedrock integration |
| Mobile Developer (iOS) | `roles/mobile-dev/` | Swift/SwiftUI, Human Interface Guidelines, iOS patterns |
| DevOps Engineer | `roles/devops-eng/` | CloudFormation, VPC/networking, CI/CD, monitoring, security |

## How to Pull Config for Your Role

1. Open Kiro CLI in your project
2. Use the `@team-member-setup` workflow
3. Point it to this repo: `nicolabot/kool-kiro-konfig`
4. Select your role
5. The agent copies the shared steering docs + your role-specific config into your local `.kiro/` directory

## What You Get

- **Steering docs** (all roles): Product vision, tech stack, project structure standards
- **Agent config**: A tailored AI assistant with tools scoped to your role
- **Context files**: Role-specific guidelines the agent references
- **Skills**: Auto-activating workflows for common tasks (scaffolding, reviews, etc.)

## Updating Config

- **Sync**: Use `@sync` to pull the latest changes from this repo
- **Propose changes**: Use `@propose-changes` to push local improvements back via PR
