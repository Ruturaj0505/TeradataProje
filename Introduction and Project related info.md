## Introduction

My name is Ruturaj Shinde. I’m currently working as a Lead Software Engineer at Persistent Systems, and I have around 4 years of experience working with data platforms and SQL-based data processing.

Currently, I’m working on a Teradata-to-Snowflake migration project, where I work mainly with SQL, Snowflake, and data validation. My responsibilities include source-to-target data validation, writing SQL transformations, handling joins and duplicates, and performing null and data-quality checks based on business requirements.

I also have experience working with Snowflake concepts such as Snowpipe, Tasks, Streams, data loading, and performance optimization, along with dbt-based transformation concepts.


## What was your responsibility?(Short ans)
My main responsibilities were L0 ingestion, L1/L2 transformations, and migration validation. I worked on InfoWorks pipelines for Teradata-to-Snowflake ingestion, built dbt models for the transformation layers, and performed source-to-target validation to make sure the migrated data was accurate.

## What is your main responsibility in project ? (Berief Explaination)
In this project, we were migrating an enterprise data warehouse from Teradata to Snowflake. My main responsibilities were L0 ingestion, L1/L2 transformations, and data validation.

For ingestion, I worked on InfoWorks pipelines for 100+ tables, where raw Teradata data was loaded into the L0 layer in Snowflake. Some large-table loads were initially taking several hours, so I worked on optimizing them. For suitable tables, I changed full loads to incremental loads using a watermark column, used parallel reads based on key columns, and adjusted the Snowflake warehouse size. This brought the load time for those tables down to around 15 to 30 minutes.

For transformation, I built dbt models for the L1 and L2 layers. In L1, I handled cleansing such as data-type conversion, trimming, null handling, and deduplication using ROW_NUMBER(). In L2, I applied business rules, joins, and derived columns to create analytics-ready tables. I used source() and ref() for lineage, and incremental models with a merge strategy for large tables. I also added dbt tests such as unique, not_null, and relationships.

For validation, we followed a three-step approach. First, we compared row counts between Teradata and Snowflake. Second, we checked column-level metrics like null counts, distinct counts, sums, and minimum and maximum values. Third, for important tables, we compared records on the primary key to find missing or mismatched rows. I ran these checks for the tables I owned and investigated every mismatch.

For example, in one table the row counts matched but a string column did not, because Teradata CHAR columns carry trailing spaces. I added a TRIM in the L1 model and the comparison passed. Other mismatches came from timestamp precision and decimal rounding, and I fixed those in the mapping or transformation and reran the validation.

So overall, my responsibility was to move the data from Teradata to Snowflake, transform it through L1 and L2 using dbt, and make sure the migrated data was accurate and production-ready.
