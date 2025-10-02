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
- Example: monthly trends
