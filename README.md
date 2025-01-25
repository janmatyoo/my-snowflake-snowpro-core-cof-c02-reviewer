# Snowflake SnowPro Core COF-C02 Certifification - Reviewer
Contains all the key concepts for Snowflake SnowPro Core COF-C02 Certification

# Contents
- [Performance Concepts](#performance-concepts)
- [Tasks](#tasks)
- [Cloning](#cloning)
- [Account Usage and Monitoring](#account-usage-and-monitoring)
- [Data Sharing](#data-sharing)
- [Data Loading and Unloading](#data-loading-and-unloading)
- [Data Governance](#data-governance)



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

---
# Account Usage and Monitoring
### How can a suspended warehouse be resumed by a Resource Monitor - [Source](https://docs.snowflake.com/en/user-guide/resource-monitors)
- The monitor's credit quota is increased
- The credit threshold for the suspend action is increased
- The next interval for the resource monitor starts

---
# Data Sharing
### What data-sharing services does Snowflake provide - [Source](https://docs.snowflake.com/en/guides-overview-sharing)
- Direct Sharing
- Snowflake Marketplace | Listings
- Data Exchange

---
# Data Loading and Unloading
### When does load metadata for a table expire - [Source](https://docs.snowflake.com/en/user-guide/data-load-considerations-load#load-metadata)
- 64 Days

# Snowsight Query History expiration - [Source](https://docs.snowflake.com/en/user-guide/ui-snowsight-activity#query-history)
- 14 Days

---
# Data Governance
## Object Tagging
- Monitor and manage sensitive data for compliance, discovery, protection, and resource usage.
- **Editions**: Available in Enterprise Edition or higher.

### What are Tags?
- Schema-level objects storing key-value pairs.
- Tags are metadata that provide descriptive information about Snowflake objects.
- Tag value is always a string

### Maximum Unique Tags
- Snowflake limits the number of tags in an account to 10,000.
- A single table or view object: 50 unique tags.
- All columns combined in a single table or view: 50 unique tags.

### Objects You Can Tag
- **Tables**
- **Views**
- **Columns**
- **Warehouses**
- **File Formats**
- **Stages**
- **Tasks**
- **Functions**
- **Pipes**
- **Streams**
- **Shares**
- **Procedures**

### Use Cases for Tags
- **Compliance**: Track sensitive data for regulatory requirements.
- **Auditing**: Identify and review tagged objects for accountability.
- **Governance**: Classify and monitor resources for organizational policies.
- **Reporting**: Generate reports based on tagged data for better insights.

### Tag Propagation
- When a tag is applied to a parent object (e.g., a table), it propagates to child objects (e.g., columns).
Propagation can be disabled if needed.

### Tagging Policies
- Privileges Required: Users need the MANAGE TAGS privilege to create or modify tags.
- Access Control: Tags are stored in schemas and follow the schema’s access controls.

### Best Practices for Tags
- Use meaningful and consistent tag names.
- Regularly audit tags to ensure compliance with governance policies.

### Creating Tags
```sql
CREATE TAG sensitive_data;
```

### Assigning Tags
```sql
ALTER TABLE employees SET TAG sensitive_data = 'yes';
```

### Viewing Tags
- Use the TAG_REFERENCES view in the INFORMATION_SCHEMA to see where tags are applied:
```sql
SELECT * FROM information_schema.tag_references;
```

### Dropping Tag
```sql
DROP TAG sensitive_data;
```

---
## Tag-based Masking Policy
- Automatically enforce data protection by applying masking policies to tagged objects.
- **Editions**: Available in Enterprise Edition or higher.
- **Privileges**: Users must have the MANAGE TAGS privilege to modify tags and assign masking policies.

### Tag-based Masking Policies
- Combine object tagging with masking policies to secure sensitive data.
- Automatically apply masking policies based on tags.

### Application Rules
- The masking policy applies automatically when the tag is assigned to an object.
- Directly assigned masking policies override tag-based masking policies.

### Restrictions
- A tag supports only one masking policy per data type.
- System tags cannot have masking policies.
- Dropping a tag or masking policy is restricted if they are associated.
- Materialized views cannot use underlying objects protected by tag-based masking policies.

### Best Practices
- Define clear and consistent tags and policies for sensitive data.
- Regularly audit and update tags and masking policies to maintain compliance.
- Use specific roles and permissions to manage tags and policies securely.

### Notes
- Ensures automatic data masking when new objects are tagged, reducing manual effort.
- Supports regulatory compliance (e.g., GDPR, HIPAA) with consistent data protection.

### Create a Tag
```sql
CREATE TAG sensitive_data;
```

### Create a Masking Policy
- Define a policy to mask specific data types. Example:
```sql
CREATE MASKING POLICY mask_ssn 
AS (val STRING) RETURNS STRING ->
  CASE 
    WHEN CURRENT_ROLE() IN ('DATA_ANALYST') THEN val 
    ELSE 'XXXX-XX-XXXX' 
  END;
```

### Assign Masking Policy to Tag
- Link the masking policy to the tag:
```sql
ALTER TAG sensitive_data SET MASKING POLICY mask_ssn;
```

### Apply Tag to Object
- Assign the tag to the desired object or column:
```sql
ALTER TABLE employees SET TAG sensitive_data = 'SSN';
```

### Viewing Tag-based Policies
- Query the TAG_REFERENCES view in INFORMATION_SCHEMA to review tag assignments:
```sql
SELECT object_name, tag_name, tag_value, masking_policy
FROM information_schema.tag_references;
```





