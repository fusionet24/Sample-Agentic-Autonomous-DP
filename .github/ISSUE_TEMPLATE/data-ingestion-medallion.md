---
name: NYC Taxis Medallion Architecture Data Ingestion
about: Template for creating a simple bronze and silver layer medallion architecture for NYC Taxis dataset
title: 'Ingest NYC Taxis data to Medallion Architecture (Bronze & Silver)'
labels: ['data-ingestion', 'medallion', 'dlt-pipeline', 'enhancement']
assignees: ''
---

## 🎯 Objective
Create a simple medallion architecture with Bronze and Silver layers for the NYC Taxis dataset using Databricks Delta Live Tables (DLT).

## 📋 Task Description
Implement a two-layer medallion architecture to ingest and clean NYC Taxis data from the Databricks free dataset. The solution should demonstrate best practices for data ingestion and quality validation in a lakehouse architecture.

## 🏗️ Architecture Components

### Bronze Layer - Raw Data Ingestion
Create a DLT pipeline that ingests raw NYC Taxis data with minimal transformation.

**Requirements**:
- Ingest data from Databricks NYC Taxis sample dataset
- Preserve all original columns from source
- Store as Delta table with APPEND mode
- Partition data by pickup date (YYYY-MM-DD)
- Enable Change Data Feed for downstream processing
- Add metadata columns: `ingestion_timestamp`, `source_file`

**Expected Fields** (from source):
```
- pickup_datetime
- dropoff_datetime  
- pickup_location_id
- dropoff_location_id
- trip_distance
- fare_amount
- payment_type
- passenger_count
- tip_amount
- tolls_amount
- total_amount
```

### Silver Layer - Cleaned and Validated Data
Create a DLT pipeline that cleans, validates, and enriches the bronze data.

**Transformations Required**:
1. **Data Quality Checks**:
   - Remove records with null pickup/dropoff times
   - Filter invalid trip distances (distance <= 0 or distance > 100 miles)
   - Filter invalid fares (fare_amount <= 0)
   - Filter invalid passenger counts (passenger_count <= 0 or > 6)
   - Remove duplicate records based on key fields

2. **Data Enrichment**:
   - Calculate trip duration in minutes: `trip_duration_minutes`
   - Categorize trip distance: `trip_category` (short: <2mi, medium: 2-10mi, long: >10mi)
   - Add quality flag: `data_quality_score` (0-100 based on completeness)
   - Add processing timestamp: `processing_timestamp`

3. **Standardization**:
   - Ensure consistent datetime formats
   - Standardize column names (lowercase with underscores)
   - Cast numeric fields to appropriate types (decimal for currency)

**DLT Expectations** (Data Quality Rules):
```python
@dlt.expect("valid_fare", "fare_amount > 0")
@dlt.expect_or_drop("valid_distance", "trip_distance > 0 AND trip_distance < 100")
@dlt.expect_or_drop("valid_passengers", "passenger_count > 0 AND passenger_count <= 6")
@dlt.expect_or_drop("valid_timestamps", "pickup_datetime IS NOT NULL AND dropoff_datetime IS NOT NULL")
@dlt.expect("positive_duration", "trip_duration_minutes > 0")
```

## 📁 Deliverables

### 1. Pipeline Code
- [ ] `pipelines/bronze/nyc_taxis_bronze.py` - Bronze layer DLT pipeline
- [ ] `pipelines/silver/nyc_taxis_silver.py` - Silver layer DLT pipeline

### 2. Configuration Files
- [ ] `config/pipeline_config.json` - Pipeline configuration (database, storage paths)
- [ ] `config/expectations.yaml` - Data quality expectations documentation

### 3. Documentation
- [ ] Update `README.md` with pipeline setup instructions
- [ ] Add data dictionary in `docs/data-dictionary.md`
- [ ] Document transformation logic in pipeline code comments

### 4. Testing (Optional but Recommended)
- [ ] `tests/unit/test_transformations.py` - Unit tests for transformation functions
- [ ] `tests/integration/test_pipeline_e2e.py` - End-to-end pipeline test

## 🔧 Technical Specifications

### Pipeline Configuration
```json
{
  "pipeline_name": "nyc_taxis_medallion",
  "target_database": "nyc_taxis_db",
  "storage_location": "/mnt/delta/nyc_taxis/",
  "bronze_table": "bronze_trips",
  "silver_table": "silver_trips_cleaned"
}
```

### Cluster Configuration
```json
{
  "cluster_name": "dlt-nyc-taxis",
  "runtime_version": "13.3.x-scala2.12",
  "node_type": "Standard_DS3_v2",
  "autoscale": {
    "min_workers": 1,
    "max_workers": 4
  }
}
```

### Source Data Location
- Dataset: `samples.nyctaxi.trips` (Databricks sample dataset)
- Alternative: `/databricks-datasets/nyctaxi/` (DBFS path)

## ✅ Acceptance Criteria

### Bronze Layer
- [ ] Pipeline successfully ingests data from NYC Taxis source
- [ ] All source columns are preserved without transformation
- [ ] Data is stored as Delta table with proper partitioning
- [ ] Metadata columns are added (ingestion_timestamp, source_file)
- [ ] Pipeline runs in streaming mode (incremental ingestion)
- [ ] Pipeline can be executed successfully in Databricks

### Silver Layer
- [ ] Pipeline reads from bronze layer
- [ ] All data quality rules are implemented as DLT expectations
- [ ] Invalid records are dropped or flagged appropriately
- [ ] Enrichment calculations are correct (trip_duration, trip_category)
- [ ] Data quality metrics are tracked via DLT event log
- [ ] Pipeline runs in streaming mode with incremental processing
- [ ] Output schema is documented

### General Requirements
- [ ] Code follows Python PEP 8 style guidelines
- [ ] All functions have docstrings
- [ ] Configuration is externalized (not hardcoded)
- [ ] Pipeline dependencies are clearly defined
- [ ] Error handling is implemented
- [ ] Logging is included for debugging

## 📖 Reference Documentation
Please refer to the following documentation for guidance:
- [architecture.md](../architecture.md) - High-level architecture design
- [repo-structure.md](../repo-structure.md) - Repository organization
- [agents.md](../agents.md) - Autonomous agent workflows

## 🎓 Example Code Structure

### Bronze Layer Example
```python
import dlt
from pyspark.sql.functions import current_timestamp, input_file_name

@dlt.table(
    name="bronze_trips",
    comment="Raw NYC Taxis trip data - Bronze layer",
    table_properties={
        "quality": "bronze",
        "pipelines.autoOptimize.zOrderCols": "pickup_location_id"
    }
)
def bronze_trips():
    return (
        spark.readStream
        .format("delta")
        .load("samples.nyctaxi.trips")
        .withColumn("ingestion_timestamp", current_timestamp())
        .withColumn("source_file", input_file_name())
    )
```

### Silver Layer Example
```python
import dlt
from pyspark.sql.functions import col, when, unix_timestamp

@dlt.table(
    name="silver_trips_cleaned",
    comment="Cleaned and validated NYC Taxis trip data - Silver layer"
)
@dlt.expect("valid_fare", "fare_amount > 0")
@dlt.expect_or_drop("valid_distance", "trip_distance > 0 AND trip_distance < 100")
def silver_trips_cleaned():
    return (
        dlt.read_stream("bronze_trips")
        .withColumn(
            "trip_duration_minutes",
            (unix_timestamp("dropoff_datetime") - unix_timestamp("pickup_datetime")) / 60
        )
        .withColumn(
            "trip_category",
            when(col("trip_distance") < 2, "short")
            .when(col("trip_distance") <= 10, "medium")
            .otherwise("long")
        )
    )
```

## 🚀 Getting Started

1. Review the [architecture.md](../architecture.md) for design patterns
2. Set up Databricks workspace with DLT enabled
3. Create the bronze layer pipeline first
4. Test bronze layer ingestion
5. Create the silver layer pipeline
6. Validate end-to-end data flow
7. Document any decisions or deviations

## 📝 Additional Notes

- Keep the implementation simple and focused on demonstrating the medallion pattern
- Prioritize code clarity over optimization at this stage
- Ensure the solution can run on a free Databricks Community Edition (if applicable)
- This is a demo/POC - production features like schema evolution, SCD Type 2, etc. are out of scope

## 💡 Success Indicators

The implementation is successful when:
1. Bronze pipeline ingests data without errors
2. Silver pipeline applies quality rules and produces clean data
3. Data lineage is visible in DLT UI
4. Pipeline can be rerun idempotently
5. Documentation is clear and complete
6. Code is ready for demo purposes

---

**Related Issues**: #N/A
**Epic**: Medallion Architecture Implementation
**Priority**: High
**Estimated Effort**: 4-8 hours (for autonomous agent)
