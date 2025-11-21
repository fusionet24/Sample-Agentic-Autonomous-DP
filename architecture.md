# Databricks DLT Medallion Architecture

## Overview
This document describes the high-level architecture for implementing a medallion architecture using Databricks Delta Live Tables (DLT) with the NYC Taxis dataset.

## Medallion Architecture Pattern

The medallion architecture is a data design pattern used to logically organize data in a lakehouse, with the goal of incrementally improving the structure and quality of data as it flows through each layer.

### Architecture Layers

```
┌─────────────────────────────────────────────────────────────────┐
│                        Data Sources                             │
│                  (NYC Taxis Dataset)                            │
└────────────────────────────┬────────────────────────────────────┘
                             │
                             ▼
┌─────────────────────────────────────────────────────────────────┐
│                      BRONZE LAYER                               │
│  - Raw data ingestion                                           │
│  - Append-only ingestion                                        │
│  - Preserves source data lineage                                │
│  - Minimal transformations                                      │
│  - Schema enforcement on read                                   │
└────────────────────────────┬────────────────────────────────────┘
                             │
                             ▼
┌─────────────────────────────────────────────────────────────────┐
│                      SILVER LAYER                               │
│  - Cleaned and validated data                                   │
│  - Deduplicated records                                         │
│  - Standardized formats                                         │
│  - Data quality rules applied                                   │
│  - Enriched with metadata                                       │
└────────────────────────────┬────────────────────────────────────┘
                             │
                             ▼
┌─────────────────────────────────────────────────────────────────┐
│                      GOLD LAYER (Future)                        │
│  - Business-level aggregations                                  │
│  - Feature engineering                                          │
│  - Optimized for analytics                                      │
└─────────────────────────────────────────────────────────────────┘
```

## NYC Taxis Dataset Architecture

### Bronze Layer: Raw Ingestion

**Purpose**: Ingest raw NYC Taxis data with minimal transformation

**Key Characteristics**:
- **Data Format**: Delta tables
- **Ingestion Pattern**: Streaming or batch from source
- **Schema**: Preserve original schema from source
- **Data Quality**: No validation at this stage
- **Partitioning**: By ingestion date/time for efficient queries

**Example Fields**:
```
- pickup_datetime
- dropoff_datetime
- pickup_location_id
- dropoff_location_id
- trip_distance
- fare_amount
- payment_type
- passenger_count
```

### Silver Layer: Cleaned and Validated

**Purpose**: Clean, validate, and enrich NYC Taxis data

**Key Transformations**:
1. **Data Quality Checks**:
   - Remove null values in critical fields
   - Filter invalid trip distances (< 0 or > reasonable limit)
   - Validate fare amounts (> 0)
   - Validate passenger counts (> 0 and <= 6)
   - Remove duplicate records

2. **Data Enrichment**:
   - Add ingestion timestamp
   - Calculate trip duration
   - Categorize trip distance ranges
   - Add data quality flags

3. **Standardization**:
   - Consistent datetime formats
   - Standardized column naming
   - Type casting and validation

**Data Quality Expectations** (DLT):
```python
@dlt.expect("valid_fare", "fare_amount > 0")
@dlt.expect_or_drop("valid_distance", "trip_distance > 0 AND trip_distance < 100")
@dlt.expect_or_drop("valid_passengers", "passenger_count > 0 AND passenger_count <= 6")
```

## Delta Live Tables (DLT) Framework

### Key Components

1. **DLT Pipeline**:
   - Declarative ETL framework
   - Automatic dependency management
   - Built-in data quality monitoring
   - Automatic error handling and recovery

2. **Expectations (Data Quality)**:
   - `@dlt.expect()`: Track violations but continue
   - `@dlt.expect_or_drop()`: Drop invalid records
   - `@dlt.expect_or_fail()`: Fail pipeline on violations

3. **Materialization Types**:
   - **Streaming Tables**: For continuous incremental processing
   - **Materialized Views**: For batch processing with complete refresh

### Pipeline Configuration

```yaml
Pipeline Name: nyc_taxis_medallion
Target Database: nyc_taxis_db
Storage Location: /mnt/delta/nyc_taxis/

Bronze Layer:
  - Table: bronze_trips
  - Type: Streaming Table
  - Source: Databricks NYC Taxis dataset
  
Silver Layer:
  - Table: silver_trips_cleaned
  - Type: Streaming Table
  - Source: bronze_trips
  - Quality Rules: Applied via expectations
```

## Data Flow

```
Source Data (NYC Taxis)
    │
    ├──> Bronze DLT Table (bronze_trips)
    │       - Raw ingestion
    │       - Change Data Feed enabled
    │       - Partitioned by date
    │
    └──> Silver DLT Table (silver_trips_cleaned)
            - Cleaned data
            - Quality expectations enforced
            - Ready for analytics
```

## Infrastructure Requirements

### Databricks Workspace
- **Cluster Configuration**: 
  - Runtime: DBR 13.x+ with Delta Live Tables support
  - Autoscaling: Min 1, Max 4 workers
  - Node Type: Standard_DS3_v2 or equivalent

### Storage
- **Delta Lake Storage**: Unity Catalog or DBFS
- **Checkpointing**: Enabled for streaming reliability
- **Data Retention**: 30 days default

### Security & Governance
- **Access Control**: Unity Catalog table ACLs
- **Data Lineage**: Automatic tracking via DLT
- **Audit Logging**: Enabled for compliance

## Monitoring & Observability

1. **DLT Event Logs**: Track pipeline execution metrics
2. **Data Quality Metrics**: Monitor expectation violations
3. **Pipeline Health**: Success/failure rates
4. **Data Freshness**: Track ingestion delays

## Scalability Considerations

- **Incremental Processing**: Use streaming tables for efficient processing
- **Partition Pruning**: Partition by date for query optimization
- **Z-Ordering**: On high-cardinality columns (pickup_location_id)
- **Autoscaling**: Cluster scales based on data volume

## Best Practices

1. **Bronze Layer**:
   - Keep raw data immutable
   - Use `APPEND` mode for ingestion
   - Enable Change Data Feed for downstream processing

2. **Silver Layer**:
   - Apply comprehensive data quality rules
   - Document all transformations
   - Use streaming for real-time processing
   - Maintain audit columns (ingestion_time, update_time)

3. **DLT Pipelines**:
   - Use meaningful table and expectation names
   - Implement incremental processing where possible
   - Monitor data quality metrics regularly
   - Set up alerts for pipeline failures

## Future Enhancements (Gold Layer)

When ready to extend to Gold layer:
- **Business Aggregations**: Daily/hourly trip summaries
- **Feature Engineering**: ML-ready features for demand prediction
- **Dimensional Models**: Star schema for BI tools
- **Real-time Analytics**: Materialized views for dashboards
