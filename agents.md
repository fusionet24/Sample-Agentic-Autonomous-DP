# Autonomous Copilot Agents

## Overview
This document explains how GitHub Copilot agents autonomously solve development tasks in this repository, enabling efficient and intelligent automation of data engineering workflows.

## What are Copilot Agents?

GitHub Copilot agents are AI-powered assistants that can:
- **Understand context**: Analyze repository structure, code, and documentation
- **Make decisions**: Determine the best approach to solve tasks
- **Write code**: Generate implementation code following best practices
- **Execute tasks**: Autonomously complete assigned work items
- **Collaborate**: Work alongside human developers in a pull request workflow

## Agent Capabilities

### 1. Code Generation
- Generate DLT pipeline code for bronze, silver, and gold layers
- Create data transformation logic with proper error handling
- Implement data quality expectations
- Write unit and integration tests

### 2. Documentation
- Create architecture documentation
- Generate API documentation
- Write data dictionaries and schema docs
- Update README files with accurate information

### 3. Configuration
- Generate pipeline configuration files
- Create cluster specifications
- Set up environment-specific configs
- Define data quality rules

### 4. Testing & Validation
- Write automated tests for pipelines
- Generate test data sets
- Validate data transformations
- Perform code reviews

## How Agents Work in This Repository

### Workflow Overview

```
┌──────────────────────────────────────────────────────────────┐
│  1. GitHub Issue Created                                     │
│     "Ingest NYC Taxis data to medallion architecture"       │
└────────────────────┬─────────────────────────────────────────┘
                     │
                     ▼
┌──────────────────────────────────────────────────────────────┐
│  2. Copilot Agent Activated                                  │
│     - Analyzes issue requirements                            │
│     - Reviews repository structure                           │
│     - Plans implementation approach                          │
└────────────────────┬─────────────────────────────────────────┘
                     │
                     ▼
┌──────────────────────────────────────────────────────────────┐
│  3. Agent Creates Feature Branch                             │
│     Branch: copilot/create-medallion-architecture           │
└────────────────────┬─────────────────────────────────────────┘
                     │
                     ▼
┌──────────────────────────────────────────────────────────────┐
│  4. Agent Implements Solution                                │
│     - Creates bronze layer pipeline                          │
│     - Creates silver layer pipeline                          │
│     - Adds data quality expectations                         │
│     - Generates configuration files                          │
│     - Writes tests                                           │
└────────────────────┬─────────────────────────────────────────┘
                     │
                     ▼
┌──────────────────────────────────────────────────────────────┐
│  5. Agent Validates Work                                     │
│     - Runs linters                                           │
│     - Executes tests                                         │
│     - Checks code quality                                    │
└────────────────────┬─────────────────────────────────────────┘
                     │
                     ▼
┌──────────────────────────────────────────────────────────────┐
│  6. Agent Creates Pull Request                               │
│     - Descriptive title and body                             │
│     - Links to original issue                                │
│     - Includes implementation details                        │
└────────────────────┬─────────────────────────────────────────┘
                     │
                     ▼
┌──────────────────────────────────────────────────────────────┐
│  7. Human Review & Merge                                     │
│     - Review agent's work                                    │
│     - Request changes if needed                              │
│     - Merge when approved                                    │
└──────────────────────────────────────────────────────────────┘
```

### Agent Task Types

#### Type 1: Data Ingestion Tasks
**Example Issue**: "Ingest NYC Taxis dataset to bronze layer"

**Agent Actions**:
1. Analyzes the data source (NYC Taxis dataset)
2. Creates bronze layer pipeline with:
   - Source connection configuration
   - Schema definition
   - Streaming or batch ingestion setup
   - Partitioning strategy
3. Adds configuration for the pipeline
4. Creates basic tests
5. Updates documentation

#### Type 2: Transformation Tasks
**Example Issue**: "Create silver layer with data quality rules"

**Agent Actions**:
1. Reviews bronze layer schema
2. Designs transformation logic:
   - Data cleaning rules
   - Validation logic
   - Type conversions
   - Enrichment calculations
3. Implements DLT expectations
4. Adds comprehensive tests
5. Documents transformations

#### Type 3: Documentation Tasks
**Example Issue**: "Document medallion architecture design"

**Agent Actions**:
1. Analyzes existing codebase
2. Creates architecture diagrams (ASCII/Mermaid)
3. Writes detailed documentation:
   - Architecture overview
   - Data flow diagrams
   - Component descriptions
4. Updates repository structure docs
5. Ensures consistency across docs

#### Type 4: Testing Tasks
**Example Issue**: "Add integration tests for pipeline"

**Agent Actions**:
1. Reviews pipeline implementation
2. Generates test scenarios
3. Creates test data fixtures
4. Implements test cases
5. Sets up test automation

## Agent Best Practices

### For Optimal Agent Performance

1. **Clear Issue Descriptions**
   - Use specific, actionable language
   - Include acceptance criteria
   - Reference relevant documentation
   - Provide context and examples

2. **Follow Repository Structure**
   - Agents work best with organized repos
   - Consistent naming conventions help
   - Clear separation of concerns
   - Well-documented existing code

3. **Incremental Tasks**
   - Break large features into smaller issues
   - One logical unit of work per issue
   - Clear dependencies between tasks

4. **Provide Context**
   - Link related issues and PRs
   - Include relevant documentation
   - Reference existing patterns to follow

### Issue Template Example

```markdown
## Task Description
Create bronze layer DLT pipeline for NYC Taxis dataset ingestion

## Acceptance Criteria
- [ ] Pipeline ingests data from Databricks NYC Taxis dataset
- [ ] Data stored as Delta table in bronze layer
- [ ] Partitioned by pickup date
- [ ] Includes all original columns
- [ ] Configuration file created
- [ ] Basic tests included

## Technical Requirements
- Use Delta Live Tables streaming table
- Follow naming convention: nyc_taxis_bronze
- Store in path: /pipelines/bronze/
- Reference architecture.md for design patterns

## Related Documentation
- architecture.md
- repo-structure.md
```

## Agent Limitations

### What Agents CAN Do
✅ Generate code following patterns and examples
✅ Create documentation and diagrams
✅ Write tests based on specifications
✅ Refactor code for clarity
✅ Fix bugs with clear reproduction steps
✅ Update configurations

### What Agents CANNOT Do (or struggle with)
❌ Make business decisions without specifications
❌ Access external systems or databases directly
❌ Deploy to production environments
❌ Resolve complex merge conflicts
❌ Make subjective design choices
❌ Understand implicit requirements

## Collaboration Model

### Human-Agent Collaboration

**Humans**:
- Define requirements and acceptance criteria
- Make architectural decisions
- Review and approve agent work
- Handle complex edge cases
- Make business decisions
- Manage deployments

**Agents**:
- Implement defined requirements
- Generate boilerplate code
- Write tests and documentation
- Perform code refactoring
- Fix clear bugs
- Update configurations

### Review Checklist for Agent Work

When reviewing agent-generated code:

- [ ] **Correctness**: Does it meet the requirements?
- [ ] **Code Quality**: Is it readable and maintainable?
- [ ] **Testing**: Are tests comprehensive?
- [ ] **Security**: Are there any security concerns?
- [ ] **Performance**: Are there obvious performance issues?
- [ ] **Documentation**: Is it well-documented?
- [ ] **Best Practices**: Does it follow project conventions?

## Agent Configuration

### Setting Up Agents

1. **Repository Settings**
   - Enable GitHub Copilot for repository
   - Configure agent permissions
   - Set up branch protection rules

2. **Issue Templates**
   - Create templates for common tasks
   - Include required fields and context
   - Add checklists for acceptance criteria

3. **Documentation**
   - Keep architecture docs up to date
   - Document coding standards
   - Provide clear examples

### Agent Prompt Engineering

When creating issues for agents, use this structure:

```
[ACTION VERB] [COMPONENT] [DETAIL]

Examples:
- "Create bronze layer pipeline for NYC Taxis"
- "Add data quality expectations to silver layer"
- "Write integration tests for medallion pipeline"
- "Document data transformation logic"
```

## Monitoring Agent Performance

### Success Metrics

- **Task Completion Rate**: % of issues successfully resolved
- **Code Quality**: Linting scores, test coverage
- **Review Cycles**: Number of review iterations needed
- **Time to Completion**: Time from issue to merge

### Continuous Improvement

- Review agent-generated code regularly
- Update documentation with learnings
- Refine issue templates based on outcomes
- Provide feedback through PR reviews

## Example Agent Workflows

### Workflow 1: New Data Source Ingestion

```
Issue: "Ingest Chicago Crime dataset to bronze layer"

Agent Process:
1. Analyze NYC Taxis pipeline as reference
2. Create new bronze pipeline file
3. Configure source connection
4. Set up schema and partitioning
5. Add configuration entry
6. Create basic tests
7. Update documentation
8. Create PR with detailed description
```

### Workflow 2: Add Data Quality Rules

```
Issue: "Add validation rules for trip distance and fare"

Agent Process:
1. Review silver layer pipeline
2. Analyze data patterns
3. Add DLT expectations:
   - @dlt.expect_or_drop for invalid distances
   - @dlt.expect for fare anomalies
4. Update tests with new rules
5. Document expectations in config
6. Create PR with examples
```

### Workflow 3: Create Aggregation Layer

```
Issue: "Create gold layer with daily trip summaries"

Agent Process:
1. Review silver layer schema
2. Design aggregation logic
3. Create gold layer pipeline:
   - Group by date
   - Calculate metrics
   - Optimize for queries
4. Add tests with sample data
5. Update architecture documentation
6. Create PR with performance notes
```

## Advanced Features

### Custom Agent Instructions

For complex tasks, provide structured instructions in issue:

```yaml
agent_instructions:
  reference_files:
    - pipelines/bronze/nyc_taxis_bronze.py
    - config/expectations.yaml
  
  patterns_to_follow:
    - Use streaming tables
    - Apply expectation decorators
    - Include audit columns
  
  output_requirements:
    - Unit tests with 80%+ coverage
    - Configuration in separate file
    - Documentation with examples
```

### Agent Chaining

Break complex tasks into sequential issues:

1. Issue #1: Create bronze layer (Agent A)
2. Issue #2: Create silver layer (Agent B, depends on #1)
3. Issue #3: Add gold layer (Agent C, depends on #2)

Each agent builds on previous work, creating a pipeline of autonomous development.

## Conclusion

Autonomous Copilot agents enable rapid, consistent development of data pipelines by:
- Reducing manual coding effort
- Ensuring adherence to patterns and standards
- Maintaining documentation alongside code
- Accelerating time-to-value for new features

By following the guidelines in this document, teams can effectively leverage agents to build and maintain robust medallion architectures on Databricks.
