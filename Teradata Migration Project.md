## TeradataProject

In our project, Teradata is one of the main source systems. The data is generated or updated in Teradata during the business day, and based on the agreed batch schedule, the required data is picked up for downstream processing.

We use InfoWorks as the data-integration and workflow tool for ingestion. InfoWorks connects to Teradata, extracts the required data based on the configured jobs and load strategy, and loads it into Snowflake. For incremental loads, we identify new or updated records using a timestamp or key, and InfoWorks moves only those records.

In Snowflake, the data first lands in the L0 layer, our raw layer. We keep the source structure as close to the original as possible and apply no business transformations. We only do technical validations like load status, record counts, and data-type checks.

From L0 onwards, the transformations are built as dbt models in Snowflake. Once the InfoWorks load into L0 completes successfully, the dbt run is triggered [by the scheduled workflow / by the scheduler you use], first for L1 and then for L2. We use source() to read the L0 tables and ref() between models, so dbt handles lineage and run order.

In L1, we clean the data: duplicates removed with ROW_NUMBER, mandatory null checks, data-type conversion, trimming and standardization, filtering invalid records, and dataset-specific business rules. In L2, we combine the L1 datasets, apply the final business logic, and create business-ready dimensions and facts. For example, customer data from one source and transaction data from another are integrated in L2 into a business-level dataset. For large tables, we use incremental models with the merge strategy, so only new or changed rows are processed.

We also added dbt tests such as unique, not_null, and relationships, so data quality problems are caught when the models run.

After L2, we do final validation: record counts, duplicate checks, null checks, reconciliation against Teradata, and business-rule checks. The reporting team then consumes the L2 tables or views for Power BI.

The overall flow is: Teradata → InfoWorks → Snowflake L0 → dbt L1 → dbt L2 → Power BI."

Updated layer answers
L0: "InfoWorks extracts the data from Teradata and loads it into Snowflake L0. We preserve the source data with minimal transformation and do technical validations."
L1: "I build the dbt models for the cleansing and data-quality logic: duplicates, null checks, data-type conversion, standardization, and business rules. The goal is clean, consistent data."
L2: "L2 is the business-ready layer. I build dbt models that integrate the L1 datasets, apply final business logic, and prepare facts and dimensions for downstream use."

“Teradata is a relational database, so the data is stored in structured tables in rows and columns. We don't receive a JSON or CSV file inside Teradata. The source data is available as tables with defined columns and data types, and we extract the required records from those tables using SQL or the configured InfoWorks workflow.”
If he asks “Then how does it move to Snowflake?”:
“InfoWorks connects to the Teradata source, reads the required tables or records, and moves the data according to the configured workflow. The data is then loaded into the Snowflake L0 tables, where we preserve the source structure as much as possible.”
“A Teradata batch is a scheduled set of data-processing jobs that runs at a predefined time. It can extract or process the required data from Teradata and make the data ready for downstream processing. Once the batch is completed successfully, our InfoWorks workflow can start the next step of extracting that data and loading it into Snowflake.”

## 1. What was your responsibility?(Short ans)

   My main responsibilities were L0 ingestion, L1/L2 transformations, and migration validation. I worked on InfoWorks pipelines for Teradata-to-Snowflake    ingestion, built dbt models for the transformation layers, and performed source-to-target validation to make sure the migrated data was accurate.


## 2. What is your main responsibility in project ? (Berief Explaination)

In this project, we were migrating an enterprise data warehouse from Teradata to Snowflake. My main responsibilities were L0 ingestion, L1/L2 transformations, and data validation.

For ingestion, I worked on InfoWorks pipelines for 100+ tables, where raw Teradata data was loaded into the L0 layer in Snowflake. Some large-table loads were initially taking several hours, so I worked on optimizing them. For suitable tables, I changed full loads to incremental loads using a watermark column, used parallel reads based on key columns, and adjusted the Snowflake warehouse size. This brought the load time for those tables down to around 15 to 30 minutes.

For transformation, I built dbt models for the L1 and L2 layers. In L1, I handled cleansing such as data-type conversion, trimming, null handling, and deduplication using ROW_NUMBER(). In L2, I applied business rules, joins, and derived columns to create analytics-ready tables. I used source() and ref() for lineage, and incremental models with a merge strategy for large tables. I also added dbt tests such as unique, not_null, and relationships.

For validation, we followed a three-step approach. First, we compared row counts between Teradata and Snowflake. Second, we checked column-level metrics like null counts, distinct counts, sums, and minimum and maximum values. Third, for important tables, we compared records on the primary key to find missing or mismatched rows. I ran these checks for the tables I owned and investigated every mismatch.

For example, in one table the row counts matched but a string column did not, because Teradata CHAR columns carry trailing spaces. I added a TRIM in the L1 model and the comparison passed. Other mismatches came from timestamp precision and decimal rounding, and I fixed those in the mapping or transformation and reran the validation.

So overall, my responsibility was to move the data from Teradata to Snowflake, transform it through L1 and L2 using dbt, and make sure the migrated data was accurate and production-ready.

## 3.Difficluts u faced ?
In the Teradata-to-Snowflake migration, I owned the L0 ingestion pipelines in InfoWorks for 100+ tables. Some of the large tables were taking several hours to load, which was a risk for the batch window and for everything downstream."

### Task:
"I had to bring the load time down without affecting data accuracy."

### Action:
"First, I looked at where the time was going. The large tables were doing a full load every time, and the read from Teradata was a single stream. So I made three   changes:

For tables with a reliable timestamp column and no hard deletes, I changed the full load to an incremental load using a watermark column.
I enabled parallel reads based on a key column, so several threads read different ranges of the table at once.
I adjusted the Snowflake warehouse size for the heavy loads.

After each change I validated the data against Teradata with row counts and column-level checks, to confirm that faster did not mean wrong."

### Result:
"The load time for those tables came down from several hours to around 15 to 30 minutes, and the validation still passed."

## 3. How did you do the ingestion?

For ingestion, I worked on InfoWorks pipelines for 100+ tables, where raw Teradata data was loaded into the L0 layer in Snowflake. Some large-table loads were initially taking several hours, so I worked on optimizing them. For suitable tables, I changed full loads to incremental loads using a watermark column, used parallel reads based on key columns, and adjusted the Snowflake warehouse size. This brought the load time for those tables down to around 15 to 30 minutes.

## 4. How did you do validation?

For validation, we followed a three-step approach. First, we compared row counts between Teradata and Snowflake. Second, we checked column-level metrics like null counts, distinct counts, sums, and minimum and maximum values. Third, for important tables, we compared records on the primary key to find missing or mismatched rows. I ran these checks for the tables I owned and investigated every mismatch.

For example, in one table the row counts matched but a string column did not, because Teradata CHAR columns carry trailing spaces. I added a TRIM in the L1 model and the comparison passed. Other mismatches came from timestamp precision and decimal rounding, and I fixed those in the mapping or transformation and reran the validation.

## 5. How did you choose the watermark column?

"I looked for a column that reliably changes whenever a row changes, usually a last_updated_ts or a monotonically increasing ID. I checked that it's indexed and not null in Teradata, and that updates always touch it."

## 6. How did you handle deletes and late-arriving records?
   --

"A watermark only catches inserts and updates. For deletes, we either used a soft-delete flag from the source or ran a periodic full key comparison. For late-arriving records, we reloaded a small lookback window, for example the last few days, and merged on the primary key so nothing duplicated."

## 7. Why did you use InfoWorks?
“InfoWorks was used as the ingestion and pipeline management tool. It connected to Teradata, extracted the required data, and loaded it into Snowflake based on the configured load strategy.”

## 8. “How exactly did you implement incremental loading using a watermark?”
“For tables where we had a reliable last-updated timestamp, we used that as the watermark. We stored the last successfully processed timestamp. During the next run, InfoWorks extracted only records where the source update timestamp was greater than the previous watermark. After the load completed successfully, the watermark was updated for the next run.”

If they ask: “Give me an example.”
“For example, if the previous successful watermark was 10:00 AM, the next run would pick records updated after 10:00 AM. So instead of reading the entire table, we processed only the new or updated records.”

Cross-question: “What if there is no reliable timestamp?”
“Then I wouldn't use a timestamp-based incremental strategy blindly. We would need another change-detection mechanism, such as CDC, a sequence column, or source-system change tracking.”

## 9. “How did you reduce the load time from hours to 15–30 minutes?”
“First, I identified that large tables were processing too much data through full loads. For suitable tables, I changed them to incremental loading using a watermark, so only new or changed records were extracted. We also divided large reads into parallel ranges using a suitable key column, and adjusted the Snowflake warehouse size based on the workload. These changes reduced the amount of data processed and improved parallelism, bringing the load time down to around 15–30 minutes.”

If they ask: “How did you know your optimization worked?”
“I compared the execution time and processed record volume before and after the changes. The same table that previously took several hours was completing in roughly 15–30 minutes after optimization.”

## 10. “How did you implement deduplication using ROW_NUMBER()?”
“We partitioned the records by the business or primary key and ordered them by the latest update or load timestamp in descending order. The latest record received ROW_NUMBER() = 1, and we retained only that record.”

Example:
WITH ranked AS (
    SELECT
        *,
        ROW_NUMBER() OVER (
            PARTITION BY customer_id
            ORDER BY updated_timestamp DESC
        ) AS rn
    FROM source_data
)
SELECT *
FROM ranked
WHERE rn = 1;

If they ask: “What if two records have the same timestamp?”
“Then timestamp alone isn't sufficient. We use another column as a tie-breaker, such as an ingestion timestamp, sequence number, or another reliable column.”

## 11. “Explain your three-step source-to-target validation.”
“First, I compared the row counts between Teradata and Snowflake. If they matched, I moved to column-level validation, where I compared things like null counts, distinct counts, sums, minimum and maximum values. Finally, for important tables, I performed a primary-key-based comparison to identify missing or mismatched records.”

If they ask: “Give me an actual mismatch example.”
“In one case, the row counts matched, but a string column was different. During investigation, I found that the Teradata column was CHAR, so it contained trailing spaces. I applied TRIM() in the L1 transformation and reran the validation, after which the comparison passed.”

If they ask: “What other mismatches did you see?”
“We also encountered timestamp precision and decimal precision or scale differences. I checked the source and target definitions, standardized the transformation, and reran the validation.”







