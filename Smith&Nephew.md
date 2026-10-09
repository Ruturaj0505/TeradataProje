## Smith & Nephew

The main objective was to ingest structured data from AWS S3 into Snowflake and transform it into clean, analytics-ready datasets for business reporting.

First, the data files were stored in AWS S3. I worked with Snowpipe to automate loading new files into Snowflake tables. I also worked with Snowflake objects such as schemas, tables, views, and stored procedures.

Next, I developed dbt models to transform the data using SQL and implement reusable business logic. I also worked with incremental models, which process new or changed records rather than rebuilding the entire dataset every time. This helped improve processing efficiency.

I was also involved in query performance optimization. I tuned SQL queries, reviewed query execution, and optimized warehouse settings to improve performance. Through these optimizations, we achieved approximately 20–30% improvement in query performance.

Finally, I performed SQL-based data validation to check record counts and ensure the transformed data was accurate and reliable. We processed millions of records to support business reporting and analytics.
