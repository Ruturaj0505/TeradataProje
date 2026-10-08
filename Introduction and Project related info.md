## 1. Introduction
I’m Ruturaj Shinde, a Lead Software Engineer at Persistent Systems with around 4 years of experience working with data platforms and SQL-based data processing.

My work is mainly focused on SQL, Snowflake, and dbt, with experience in data transformation, performance optimization, and data validation to ensure the data is accurate and reliable.

Currently, I’m working on a Teradata-to-Snowflake migration project, where I work on ingestion, L1/L2 transformations, and source-to-target validation.

Previously, I worked on a Unified Data Platform on Snowflake, where I worked with Snowpipe, dbt, AWS, semi-structured data, and curated data models for analytics and reporting.


## 2. What was your responsibility?(Short ans)
My main responsibilities were L0 ingestion, L1/L2 transformations, and migration validation. I worked on InfoWorks pipelines for Teradata-to-Snowflake ingestion, built dbt models for the transformation layers, and performed source-to-target validation to make sure the migrated data was accurate.


## 3. What is your main responsibility in project ? (Brief Explanation)
In this project, we were migrating an enterprise data warehouse from Teradata to Snowflake. My main responsibilities were L0 ingestion, L1/L2 transformations, and data validation.

For ingestion, I worked on InfoWorks pipelines for 100+ tables, where raw Teradata data was loaded into the L0 layer in Snowflake. Some large-table loads were initially taking several hours, so I worked on optimizing them. For suitable tables, I changed full loads to incremental loads using a watermark column, used parallel reads based on key columns, and adjusted the Snowflake warehouse size. This brought the load time for those tables down to around 15 to 30 minutes.

For transformation, I built dbt models for the L1 and L2 layers. In L1, I handled cleansing such as data-type conversion, trimming, null handling, and deduplication using ROW_NUMBER(). In L2, I applied business rules, joins, and derived columns to create analytics-ready tables. I used source() and ref() for lineage, and incremental models with a merge strategy for large tables. I also added dbt tests such as unique, not_null, and relationships.

For validation, we followed a three-step approach. First, we compared row counts between Teradata and Snowflake. Second, we checked column-level metrics like null counts, distinct counts, sums, and minimum and maximum values. Third, for important tables, we compared records on the primary key to find missing or mismatched rows. I ran these checks for the tables I owned and investigated every mismatch.

For example, in one table the row counts matched but a string column did not, because Teradata CHAR columns carry trailing spaces. I added a TRIM in the L1 model and the comparison passed. Other mismatches came from timestamp precision and decimal rounding, and I fixed those in the mapping or transformation and reran the validation.

So overall, my responsibility was to move the data from Teradata to Snowflake, transform it through L1 and L2 using dbt, and make sure the migrated data was accurate and production-ready.

## 4. How did you validate data between Teradata and Snowflake?
"I validated the data at multiple levels. First, I compared the row counts between Teradata and Snowflake. Then I checked aggregates like SUM and COUNT on important columns. I also validated nulls, duplicates, data types, and date formats. For detailed validation, I used EXCEPT queries to identify records present in one system but missing in the other. If there was a mismatch, I analyzed the transformation logic and business rules."

If they ask: "Give me an example"
"For example, if Teradata had 1 million records and Snowflake had 999,500, I would first identify the missing 500 records using key-based comparison or EXCEPT, and then check whether they were filtered intentionally or missed during processing."

## 5. How do you handle duplicates?
"First, I identify duplicates using the business key with GROUP BY and HAVING COUNT greater than 1. Then I check whether they are actual duplicates or valid multiple records according to the business rule. If we need to retain only the latest record, I use ROW_NUMBER with PARTITION BY the business key and ORDER BY the latest timestamp, and keep only row number 1."

SELECT *
FROM (
    SELECT *,
           ROW_NUMBER() OVER (
               PARTITION BY id
               ORDER BY updated_ts DESC
           ) AS rn
    FROM table_name
)
WHERE rn = 1;

Cross-question: What can cause duplicates apart from source data?
"Joins can also create duplicates, especially when there is a one-to-many relationship. So I check the join keys and cardinality."

## 6.


