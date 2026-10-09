### Unified Data Platfrom

#### UDP project flow and Explination

         ERP Systems / REST APIs  → SNP Glue →  AWS S3 →  Snowpipe →  Snowflake Bronze →  Snapshots / Silver →  dbt Gold →  BI & Reporting

In my UDP project, we integrate data from multiple ERP systems and REST APIs into Snowflake for reporting and analytics.

First, ERP data is extracted using SNP Glue, while the orchestration team handles data extraction from REST APIs. The extracted data is landed in AWS S3, which acts as our staging area.

Once the files are available in S3, Snowpipe loads the data into Snowflake's Bronze layer, where we maintain the raw data.

Next, we process the data through the snapshot and Silver layers, where records are prepared for downstream transformations. This includes activities such as deduplication and standardization, depending on the processing requirements.

We then use dbt to transform the processed data into curated Gold-layer datasets. These models apply SQL transformations and business logic to make the data suitable for reporting and analytics.

One of the important components of our project is a metadata-driven utility that automates the creation of Snowpipes and snapshot objects for multiple tables. This reduces repetitive manual configuration and makes onboarding new tables easier.

Finally, we validate the data using SQL checks and data-quality tests to ensure the processed data is reliable for downstream reporting.
