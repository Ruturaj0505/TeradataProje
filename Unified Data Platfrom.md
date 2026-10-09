### Unified Data Platfrom

#### UDP project flow and Explination

         ERP Systems / REST APIs  → SNP Glue →  AWS S3 →  Snowpipe →  Snowflake Bronze →  Snapshots / Silver →  dbt Gold →  BI & Reporting

The main objective UDP is to integrate data from multiple ERP systems and REST APIs into Snowflake for reporting and analytics.

First, ERP data is extracted using SNP Glue, while the orchestration team handles data extraction from REST APIs. The extracted data is stored in AWS S3, which acts as our staging area.

Next, Snowpipe loads the files from S3 into Snowflake's Bronze layer, where we store the raw data with minimal transformations.

From Bronze, the data moves to the Silver layer, where we clean and standardize it. This includes handling duplicate records, converting data types, and applying data-quality rules. We also use snapshots to track data changes for the relevant tables.

After that, the data moves to the Gold layer, where we use dbt to apply business logic, join related tables, and create curated datasets for reporting and analytics.

One important component of our project is a metadata-driven utility that automates the creation of Snowpipes and snapshot objects for multiple tables. This reduces repetitive manual configuration and makes it easier to onboard new tables.

Finally, we perform SQL-based validations and data-quality checks to verify the data before it is used for downstream reporting.
