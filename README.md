# Snowflake SnowPro Core COF-C02 Certifification - Reviewer
Contains all the key concepts for Snowflake SnowPro Core COF-C02 Certification

# Contents
- [Performance Concepts](#performance-concepts)
- [Tasks](#tasks)
- [Cloning](#cloning)
- [Account Usage and Monitoring](#account-usage-and-monitoring)
- [Data Sharing](#data-sharing)
- [Data Loading and Unloading](#data-loading-and-unloading)

---
# Performance Concepts
### Choosing columns when defining Clustering Keys - [Source](https://docs.snowflake.com/en/user-guide/tables-clustering-keys)
- Columns frequently used in `WHERE` clause or other selective filter
- Columns that are used in `JOIN`
- `High Cardinality` = effective partition pruning
- `Low Cardinality` = effective grouping data into micro-partitions
  
### Reduce query queuing on a virtual warehouse - [Source](https://docs.snowflake.com/en/user-guide/performance-query-warehouse-queue#options-for-reducing-queues)
- Add more virtual warehouses to distribute the query load
- Switch from a standard to a multi-cluster virtual warehouse
- If using a multi-cluster warehouse, increase the max cluster size

### When to use Materialized Views - [Source](https://docs.snowflake.com/en/user-guide/views-materialized)
- Optimize queries for accessing subsets of data from external tables.
- Use result caching for frequent, similar complex queries on the same table

### Invalid data types for clustering keys - [Source](https://docs.snowflake.com/en/user-guide/tables-clustering-keys#defining-a-clustering-key-for-a-table)
- GEOGRAPHY
- VARIANT
- OBJECT
- ARRAY

---
# Tasks
### Serverless Task Maximum Compute Size - [Source](https://docs.snowflake.com/en/user-guide/tasks-intro#serverless-tasks)
- XX-LARGE or 2X-Large

---
# Cloning
### Named Internal Stages - [Source](https://docs.snowflake.com/en/user-guide/object-clone)
- `Named Internal Stages` cannot be cloned
- Any `Snowpipe` that points to a `Named Internal Stage` is not cloned

# Account Usage and Monitoring
### How can a suspended warehouse be resumed by a Resource Monitor - [Source](https://docs.snowflake.com/en/user-guide/resource-monitors)
- The monitor's credit quota is increased
- The credit threshold for the suspend action is increased
- The next interval for the resource monitor starts

---
## Data Sharing
### What data-sharing services does Snowflake provide - [Source](https://docs.snowflake.com/en/guides-overview-sharing)
- Direct Sharing
- Snowflake Marketplace
- Data Exchange

---
## Data Loading and Unloading
### When does load metadata for a table expire - [Source](https://docs.snowflake.com/en/user-guide/data-load-considerations-load#load-metadata)
- 64 Days













