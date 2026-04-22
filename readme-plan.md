The user wants to auto-generate production-grade documentation for multiple repositories organized in a
workspace with an org/repo hierarchy. Two documentation files per repo:
- README.md — human-optimized onboarding document
- AGENTS.md — LLM/GenAI-optimized onboarding document

If a README.md already exists, it should be analyzed alongside the codebase, consolidated, reorganized
into a standard format, with outdated/contradictory info removed.

Open Questions (pending user response)

1. Actual path to the workspace on this system
2. Total number of repositories
3. Output location (inside each repo vs. separate output dir)
4. Whether org-level README files are needed
5. AGENTS.md preferred structure/sections

---
Proposed Approach (draft — subject to revision after exploration)

Phase 1: Workspace Discovery

For each repository:
- Recursively list all files/directories, excluding:
  - venv/, node_modules/, .git/, __pycache__/, *.pyc, *.pyo
  - dist/, build/, target/, out/, bin/, obj/
  - *.class, *.jar (Java compiled)
  - vendor/ (Go/PHP), bower_components/
  - .next/, .nuxt/ (JS frameworks)
  - Pods/ (iOS), .gradle/
  - *.egg-info/, .eggs/
- Detect language/framework from config files:
  - package.json → Node.js/TypeScript
  - requirements.txt / pyproject.toml / setup.py → Python
  - go.mod → Go
  - Cargo.toml → Rust
  - pom.xml / build.gradle → Java/Kotlin
  - *.csproj / *.sln → .NET/C#
  - Gemfile → Ruby
- Read existing README.md (if present)
- Read key config/entry-point files
- Check for CI/CD configs (.github/workflows/, .gitlab-ci.yml, etc.)

Phase 2: README.md Generation

Standard sections (production-grade):
# Project Name
> One-line description

## Overview
Brief description of what the project does, its purpose, and target users.

## Architecture
High-level architecture diagram or description, key components.

## Prerequisites
Required tools, runtimes, versions.

## Installation
Step-by-step setup instructions.

## Configuration
Environment variables, config files, required secrets.

## Usage
How to run the project (dev, prod), key commands.

## API / Endpoints (if applicable)
Key endpoints or interfaces.

## Project Structure
Annotated directory tree (excluding build artifacts).

## Testing
How to run tests, test structure.

## Deployment
How to deploy, CI/CD pipeline description.

## Contributing
Contribution guidelines, branch strategy, PR process.

## License

Existing README handling:
- Parse existing README for sections
- Retain: accurate descriptions, useful examples, architecture decisions
- Remove: outdated commands, contradictory versions, stale links
- Reorganize into the standard format above
- Supplement with newly discovered info from codebase analysis

Phase 3: AGENTS.md Generation

LLM-optimized sections:
# AGENTS.md — [Project Name]

## Purpose
Concise machine-readable description of the project's function.

## Repository Map
Key files and their roles (entry points, config, models, routes, tests).

## Tech Stack
Exact versions of runtime, framework, key dependencies.

## Architecture Patterns
Design patterns used (MVC, event-driven, layered, etc.).

## Development Commands
Exact commands for: install, run, test, lint, build, migrate.

## Environment Setup
Required env vars with descriptions and example values.

## Coding Conventions
Naming conventions, file organization rules, style guide refs.

## Testing
Test framework, how to run, where tests live, coverage targets.

## Key Abstractions
Core classes/modules/functions an agent should know.

## Agentic Task Guidance
- ✅ Tasks suitable for autonomous execution
- ⚠️ Tasks requiring human review
- ❌ Tasks an agent should NOT do autonomously

## External Dependencies & Integrations
Third-party services, APIs, databases used.

## Common Pitfalls
Known gotchas, non-obvious behaviors, footguns.

Phase 4: Output

- Write README.md and AGENTS.md into each repository root
- Optionally generate org-level README.md summarizing contained repos

Files to Create (per repo)

- <workspace>/<org>/<repo>/README.md
- <workspace>/<org>/<repo>/AGENTS.md

Implementation Strategy

Use an Explore agent per repository to gather codebase intelligence, then a Plan agent to draft content,
then execute writes for each file. Process repos sequentially or in parallel depending on count.

Verification

After generation:
1. Review each generated file manually for accuracy
2. Check that no build artifacts or sensitive data leaked into docs
3. Verify all code commands in README are syntactically correct
4. Confirm AGENTS.md has machine-parseable structure