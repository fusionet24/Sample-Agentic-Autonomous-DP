# Repository Structure

## Overview
This document outlines the recommended repository structure for organizing Databricks Delta Live Tables (DLT) medallion architecture code and documentation.

## Directory Layout

```
Sample-Agentic-Autonomous-DP/
│
├── README.md                          # Project overview and quick start
├── architecture.md                    # High-level architecture documentation
├── repo-structure.md                  # This file - repository organization
├── agents.md                          # Autonomous agents documentation
│
├── docs/                              # Additional documentation
│   ├── setup-guide.md                # Environment setup instructions
│   ├── deployment.md                 # Deployment procedures
│   └── data-dictionary.md            # Data schema documentation
│
├── pipelines/                         # DLT pipeline definitions
│   ├── bronze/                       # Bronze layer pipelines
│   │   └── nyc_taxis_bronze.py      # NYC Taxis bronze ingestion
│   │
│   ├── silver/                       # Silver layer pipelines
│   │   └── nyc_taxis_silver.py      # NYC Taxis silver transformations
│   │
│   └── gold/                         # Gold layer pipelines (future)
│       └── nyc_taxis_gold.py        # Business aggregations
│
├── config/                            # Configuration files
│   ├── pipeline_config.json          # DLT pipeline configurations
│   ├── cluster_config.json           # Cluster specifications
│   └── expectations.yaml             # Data quality expectations
│
├── notebooks/                         # Databricks notebooks
│   ├── exploration/                  # Data exploration notebooks
│   │   └── nyc_taxis_eda.ipynb      # Exploratory data analysis
│   │
│   └── validation/                   # Data validation notebooks
│       └── quality_checks.ipynb     # Manual quality checks
│
├── tests/                             # Test files
│   ├── unit/                         # Unit tests
│   │   └── test_transformations.py  # Test transformation logic
│   │
│   └── integration/                  # Integration tests
│       └── test_pipeline_e2e.py     # End-to-end pipeline tests
│
├── scripts/                           # Utility scripts
│   ├── deploy_pipeline.py            # Pipeline deployment automation
│   ├── create_tables.py              # Table creation utilities
│   └── monitoring.py                 # Monitoring and alerting
│
├── .github/                           # GitHub configuration
│   ├── workflows/                    # CI/CD workflows
│   │   └── deploy.yml               # Automated deployment
│   │
│   └── ISSUE_TEMPLATE/               # Issue templates
│       └── data-ingestion.md        # Template for data ingestion tasks
│
└── .gitignore                         # Git ignore rules
```

## Directory Descriptions

### Root Level Files

- **README.md**: Project overview, purpose, and quick start guide
- **architecture.md**: Detailed architecture documentation for the medallion pattern
- **repo-structure.md**: This document explaining the repository organization
- **agents.md**: Documentation on autonomous Copilot agents and their usage

### `/docs/` - Documentation

Contains comprehensive documentation beyond the root-level files:
- Setup guides for local and cloud environments
- Deployment procedures and best practices
- Data dictionaries and schema documentation
- API documentation if applicable

### `/pipelines/` - DLT Pipeline Code

The core of the repository containing Delta Live Tables pipeline definitions:

- **`/pipelines/bronze/`**: Raw data ingestion pipelines
  - Minimal transformations
  - Source system integrations
  - Schema preservation

- **`/pipelines/silver/`**: Cleaned and validated data pipelines
  - Data quality rules
  - Standardization logic
  - Enrichment transformations

- **`/pipelines/gold/`**: Business-level aggregation pipelines (future)
  - Aggregated views
  - Feature engineering
  - Analytics-ready datasets

**Pipeline File Naming Convention**: `{dataset}_{layer}.py`
Example: `nyc_taxis_bronze.py`, `nyc_taxis_silver.py`

### `/config/` - Configuration Files

Stores all configuration separate from code:

- **pipeline_config.json**: DLT pipeline settings (target database, storage location)
- **cluster_config.json**: Cluster specifications and autoscaling settings
- **expectations.yaml**: Centralized data quality expectations

**Benefits**:
- Environment-specific configurations (dev, staging, prod)
- Easy configuration updates without code changes
- Version control for configuration changes

### `/notebooks/` - Databricks Notebooks

Interactive notebooks for exploration and analysis:

- **`/notebooks/exploration/`**: 
  - Data profiling and statistics
  - Schema exploration
  - Sample data queries
  
- **`/notebooks/validation/`**:
  - Manual data quality checks
  - Pipeline output validation
  - Ad-hoc analysis

### `/tests/` - Test Suites

Automated testing for data pipelines:

- **`/tests/unit/`**: Unit tests for individual functions
  - Transformation logic tests
  - Data validation rules
  - Helper function tests

- **`/tests/integration/`**: Integration and end-to-end tests
  - Full pipeline execution tests
  - Data flow validation
  - Performance benchmarks

**Testing Framework**: pytest or unittest for Python-based tests

### `/scripts/` - Utility Scripts

Automation and operational scripts:

- **deploy_pipeline.py**: Automate DLT pipeline deployment
- **create_tables.py**: Initialize databases and tables
- **monitoring.py**: Pipeline monitoring and alerting setup
- **backup.py**: Data backup utilities

### `/.github/` - GitHub Configuration

GitHub-specific configurations:

- **`/workflows/`**: GitHub Actions for CI/CD
  - Automated testing on PR
  - Automated deployment to environments
  - Scheduled pipeline runs

- **`/ISSUE_TEMPLATE/`**: Issue templates
  - Standardized data ingestion requests
  - Bug report templates
  - Feature request templates

## File Naming Conventions

### Python Files
- Use snake_case: `nyc_taxis_bronze.py`
- Descriptive names indicating layer and dataset
- Test files prefixed with `test_`: `test_transformations.py`

### Configuration Files
- Use lowercase with underscores: `pipeline_config.json`
- Include file type in name: `cluster_config.json`

### Notebooks
- Use snake_case with descriptive names: `nyc_taxis_eda.ipynb`
- Include purpose in name: `quality_checks.ipynb`

## Version Control Best Practices

### What to Commit
- All pipeline code (`.py` files)
- Configuration files (`.json`, `.yaml`)
- Documentation files (`.md`)
- Test files
- Infrastructure as Code (IaC) files

### What NOT to Commit (Add to .gitignore)
- Credentials and secrets (`.env` files)
- Local IDE settings (`.vscode/`, `.idea/`)
- Python cache (`__pycache__/`, `*.pyc`)
- Databricks workspace exports (`.dbc` files)
- Large data files
- Temporary files (`*.tmp`, `*.log`)

## Branching Strategy

### Branch Types

- **`main`**: Production-ready code only
- **`develop`**: Integration branch for features
- **`feature/*`**: Individual feature branches
- **`hotfix/*`**: Emergency production fixes
- **`copilot/*`**: Autonomous agent work branches

### Example Branch Names
```
feature/add-trip-duration-calculation
feature/add-location-enrichment
hotfix/fix-date-parsing-error
copilot/create-medallion-architecture
```

## Environment Organization

### Directory Structure per Environment
```
config/
├── dev/
│   ├── pipeline_config.json
│   └── cluster_config.json
├── staging/
│   ├── pipeline_config.json
│   └── cluster_config.json
└── prod/
    ├── pipeline_config.json
    └── cluster_config.json
```

## Maintenance

### Regular Updates
- Keep documentation in sync with code changes
- Update data dictionaries when schema changes
- Review and update configuration files per environment
- Archive obsolete pipelines to `/archived/` directory

### Code Review Process
1. Feature branches created from `develop`
2. Pull requests reviewed by at least one team member
3. Automated tests must pass
4. Merge to `develop` for testing
5. Promote to `main` for production deployment

## Getting Started

For new developers:
1. Read the `README.md` for project overview
2. Review `architecture.md` to understand the design
3. Check `docs/setup-guide.md` for environment setup
4. Explore `/pipelines/` to see implementation examples
5. Review `agents.md` to understand autonomous agent workflows
