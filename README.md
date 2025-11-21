# Sample-Agentic-Autonomous-DP

## Overview
This repository demonstrates a **medallion architecture** design for Databricks Delta Live Tables (DLT) with autonomous GitHub Copilot agents. The project showcases best practices for building a lakehouse data pipeline using the NYC Taxis dataset.

## Purpose
- **Architecture Design**: High-level design for bronze and silver layer data pipelines
- **Autonomous Development**: Leverage GitHub Copilot agents to autonomously implement data engineering tasks
- **Best Practices**: Demonstrate data quality, testing, and documentation standards

## 📚 Documentation

### Core Documentation
- **[architecture.md](architecture.md)** - Detailed medallion architecture design with DLT patterns
- **[repo-structure.md](repo-structure.md)** - Repository organization and file structure guidelines
- **[agents.md](agents.md)** - How autonomous Copilot agents work and how to use them effectively

### Getting Started
1. Read [architecture.md](architecture.md) to understand the medallion pattern and DLT design
2. Review [repo-structure.md](repo-structure.md) to understand how the repository is organized
3. Check [agents.md](agents.md) to learn about autonomous agent workflows
4. Use the [GitHub Issue Template](.github/ISSUE_TEMPLATE/data-ingestion-medallion.md) to create data ingestion tasks

## 🎯 Project Goals

This repository serves as a:
- **Design Reference**: Architecture patterns for Databricks DLT medallion implementations
- **Agent Showcase**: Demonstration of autonomous agent capabilities for data engineering
- **Learning Resource**: Examples and templates for building data pipelines

## 🏗️ Medallion Architecture

The medallion architecture consists of:

- **Bronze Layer**: Raw data ingestion with minimal transformation
- **Silver Layer**: Cleaned, validated, and enriched data
- **Gold Layer**: Business-level aggregations (future enhancement)

See [architecture.md](architecture.md) for detailed design specifications.

## 🤖 Autonomous Agents

This project leverages GitHub Copilot agents to autonomously:
- Generate pipeline code following established patterns
- Create comprehensive documentation
- Write tests and validation logic
- Maintain code quality and consistency

Learn more in [agents.md](agents.md).

## 📋 Creating Data Ingestion Tasks

Use the GitHub issue template to create new data ingestion tasks:
1. Go to Issues → New Issue
2. Select "NYC Taxis Medallion Architecture Data Ingestion"
3. Fill in the required details
4. Submit the issue for autonomous agent processing

Template: [.github/ISSUE_TEMPLATE/data-ingestion-medallion.md](.github/ISSUE_TEMPLATE/data-ingestion-medallion.md)

## 🚀 Quick Start

### For Architects
1. Review the architecture documentation
2. Adapt the medallion pattern to your use case
3. Use the repository structure as a template

### For Developers
1. Understand the architecture and repository structure
2. Create issues using the templates
3. Let autonomous agents implement the tasks
4. Review and approve the generated code

### For Data Engineers
1. Study the DLT pipeline patterns
2. Understand data quality expectations
3. Learn about incremental processing patterns
4. Apply the patterns to your own datasets

## 🛠️ Technology Stack

- **Platform**: Databricks
- **Framework**: Delta Live Tables (DLT)
- **Language**: Python (PySpark)
- **Storage**: Delta Lake
- **Orchestration**: DLT Pipelines
- **Development**: GitHub Copilot Agents

## 📂 Repository Structure

```
Sample-Agentic-Autonomous-DP/
├── README.md                          # This file
├── architecture.md                    # Architecture documentation
├── repo-structure.md                  # Repository organization guide
├── agents.md                          # Autonomous agents guide
└── .github/
    └── ISSUE_TEMPLATE/
        └── data-ingestion-medallion.md  # Issue template
```

Future structure (when code is implemented):
```
├── pipelines/                         # DLT pipeline code
│   ├── bronze/                       # Bronze layer pipelines
│   └── silver/                       # Silver layer pipelines
├── config/                            # Configuration files
├── tests/                             # Test suites
└── docs/                              # Additional documentation
```

See [repo-structure.md](repo-structure.md) for complete structure details.

## 🤝 Contributing

1. Review the documentation to understand the architecture
2. Create issues using the provided templates
3. Let autonomous agents implement the work
4. Review pull requests from agents
5. Provide feedback and approve changes

## 📖 Learning Resources

- [Databricks Delta Live Tables Documentation](https://docs.databricks.com/delta-live-tables/index.html)
- [Medallion Architecture Guide](https://www.databricks.com/glossary/medallion-architecture)
- [Delta Lake Best Practices](https://docs.databricks.com/delta/best-practices.html)

## 📄 License

This is a sample/demo repository for educational and reference purposes.

## 🙋 Support

For questions or issues:
1. Review the documentation in this repository
2. Check existing GitHub issues
3. Create a new issue using the appropriate template