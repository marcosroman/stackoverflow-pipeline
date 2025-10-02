# Stack Overflow Data Pipeline - Bronze/Silver/Gold

## Architecture

**Bronze Layer**: Raw API data  
**Silver Layer**: Clean and typed data  
**Gold Layer**: Aggregated metrics for analysis

## Layers

### Bronze (`bronze.questions`)
- Raw data from ~900 API pages fetched in parallel
- Complex fields converted to JSON strings
- Partitioned by `creation_period`

### Silver (`silver.questions`)
- Selected fields: `question_id`, `creation_timestamp`, `last_activity_timestamp`, `tags`, `answer_count`, `is_answered`, `view_count`
- Timestamps converted from Unix epoch
- Deduplicated data with correct types
- Partitioned by `creation_period`

### Gold (multiple tables)
- Business-specific aggregations
- Examples: monthly trends, tag statistics, top questions

## Ingestion Process
```python
# 1. Parallel fetch (10 workers, 900 pages)
fetch_and_write_parallel(
    pages=range(1, 901),
    table_name="bronze.questions",
    max_workers=10
)

# 2. Optimization
OPTIMIZE bronze.questions ZORDER BY (creation_period, question_id)

# 3. Transform to Silver
# Type conversion, cleaning, deduplication