### 1. Introduction

My name is Ruturaj Shinde. I’m working as a Data Engineer at Persistent Systems, with almost four years of experience in data engineering.

My primary skills are Snowflake, SQL, dbt, and AWS.

Currently, I’m working on the UDP, or Unified Data Platform, project. It integrates data from multiple ERP systems and REST APIs into Snowflake for reporting and analytics. My responsibilities include Snowpipe ingestion, snapshots, automating ingestion processes using a metadata-driven utility, and building dbt transformation models following the Medallion architecture.

Previously, I worked on the Smith & Nephew project, where I loaded data from AWS S3 into Snowflake using Snowpipe, developed dbt models, and worked on SQL optimization to improve query performance.

### 2. What were your responsibilities?

My main responsibilities in the UDP project were developing Snowpipes to load data from S3 into Snowflake, implementing snapshots to track data changes, building a metadata-driven utility to automate these processes, developing dbt models for the Gold layer, and performing data validation and SQL optimization.

### 3. What is your main responsibility in the project? (brief explanation)

“UDP integrates data from multiple ERP systems and REST APIs into Snowflake. ERP data is extracted using SNP Glue, the orchestration team handles the REST APIs, and everything lands in S3.

For ingestion, I built Snowpipes for multiple tables to load data from S3 into the Bronze layer as raw data. I also built snapshots to track changes for the relevant tables.

I created a metadata-driven utility that generates Snowpipes and snapshots from configuration. Onboarding a new table became a config entry instead of custom code, which saved effort and kept everything consistent.

For the Silver layer, we clean and standardize the data: handling duplicates, converting data types, and applying quality rules. For the Gold layer, I built dbt models that apply business logic and joins to create curated datasets for reporting and analytics.

For validation, we ran SQL checks such as row count comparisons and data-quality tests before the data went to reporting. I also improved pipeline performance through SQL optimization.

Overall, my responsibility was to get data from the ERP systems into Snowflake reliably and turn it into accurate, analytics-ready datasets.”
