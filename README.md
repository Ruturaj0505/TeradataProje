# TeradataProject

“In our project, Teradata is one of the main source systems. The source data is generated or updated in Teradata during the business day, and based on the agreed batch schedule, the required data is picked up for our downstream processing.

We use InfoWorks as the data-integration and workflow orchestration tool. InfoWorks connects to the Teradata source, extracts the required data based on the configured jobs and load strategy, and transfers the data to the Snowflake environment.

For example, if it is an incremental load, we identify the records that are newly inserted or updated using the appropriate timestamp, key or change-based logic. InfoWorks then handles the extraction and movement of those records according to the scheduled workflow.

Once the data reaches Snowflake, we first load it into the L0 layer. L0 is our raw or landing layer. Here, we try to keep the source data as close to the original structure as possible. We don't apply major business transformations in L0. We mainly perform basic ingestion and technical validations such as file/load status, record counts and data-type checks.

From L0, the data moves to L1. L1 is where we actually start cleaning and transforming the data.

For example, in L1 we handle duplicate records, null validations for mandatory columns, data-type conversions, standardization of values, filtering invalid records, joins with other required source data, and deriving required columns. We also apply the business rules required for that particular dataset.

After the L1 transformation, the data moves to L2. L2 contains the more business-ready and integrated data. Here, we combine the required L1 datasets, apply the final business logic, create the required dimensions or facts, and prepare the data in a format that can directly be consumed by downstream applications.

For example, if customer information comes from one source and transaction information comes from another source, we can integrate those datasets in L2 and produce a business-level customer or transaction dataset.

Once L2 processing is completed, we perform final validation such as record counts, duplicate checks, null checks, reconciliation and business-rule validation.

The downstream team then consumes the L2 data. In our case, the reporting or analytics team can use these L2 tables or views for Power BI dashboards, reports or further analytical processing. So the downstream team does not need to work directly with the raw L0 data; they mainly consume the validated and business-ready L2 data.

So the overall flow is:

Teradata → InfoWorks → Snowflake L0 → L1 → L2 → Downstream/Power BI

What actaully each layer does ?

L0 — “What exactly happens?”
“L0 is the first landing layer. InfoWorks extracts the required data from Teradata and loads it into Snowflake L0. We preserve the source data with minimal transformation and perform technical validations.”
L1 — “What exactly do YOU do?”
“In L1, I work on the transformation and data-quality logic. I handle duplicates, mandatory null checks, data-type conversion, standardization, joins, filtering and business rules. The objective is to convert raw L0 data into clean and consistent data.”
L2 — “What exactly happens?”
“L2 is the business-ready layer. We integrate the required L1 datasets, apply final business logic and prepare fact or dimension datasets required by downstream consumers.”
Downstream — “What happens after L2?”
“The reporting and analytics team consumes L2 tables or views. They use that data for Power BI dashboards, reports and analytical requirements.”

“Teradata is a relational database, so the data is stored in structured tables in rows and columns. We don't receive a JSON or CSV file inside Teradata. The source data is available as tables with defined columns and data types, and we extract the required records from those tables using SQL or the configured InfoWorks workflow.”
If he asks “Then how does it move to Snowflake?”:
“InfoWorks connects to the Teradata source, reads the required tables or records, and moves the data according to the configured workflow. The data is then loaded into the Snowflake L0 tables, where we preserve the source structure as much as possible.”
“A Teradata batch is a scheduled set of data-processing jobs that runs at a predefined time. It can extract or process the required data from Teradata and make the data ready for downstream processing. Once the batch is completed successfully, our InfoWorks workflow can start the next step of extracting that data and loading it into Snowflake.”


------------------------------------------------------------------------------------------------------------------------------------------------------------------
1. What was your responsibility?(Short ans)

   My main responsibilities were L0 ingestion, L1/L2 transformations, and migration validation. I worked on InfoWorks pipelines for Teradata-to-Snowflake    ingestion, built dbt models for the transformation layers, and performed source-to-target validation to make sure the migrated data was accurate.


2. What is your main responsibility in project ? (Berief Explaination)

In this project, we were migrating an enterprise data warehouse from Teradata to Snowflake. My main responsibilities were L0 ingestion, L1/L2 transformations, and data validation.

For ingestion, I worked on InfoWorks pipelines for 100+ tables, where raw Teradata data was loaded into the L0 layer in Snowflake. Some large-table loads were initially taking several hours, so I worked on optimizing them. For suitable tables, I changed full loads to incremental loads using a watermark column, used parallel reads based on key columns, and adjusted the Snowflake warehouse size. This brought the load time for those tables down to around 15 to 30 minutes.

For transformation, I built dbt models for the L1 and L2 layers. In L1, I handled cleansing such as data-type conversion, trimming, null handling, and deduplication using ROW_NUMBER(). In L2, I applied business rules, joins, and derived columns to create analytics-ready tables. I used source() and ref() for lineage, and incremental models with a merge strategy for large tables. I also added dbt tests such as unique, not_null, and relationships.

For validation, we followed a three-step approach. First, we compared row counts between Teradata and Snowflake. Second, we checked column-level metrics like null counts, distinct counts, sums, and minimum and maximum values. Third, for important tables, we compared records on the primary key to find missing or mismatched rows. I ran these checks for the tables I owned and investigated every mismatch.

For example, in one table the row counts matched but a string column did not, because Teradata CHAR columns carry trailing spaces. I added a TRIM in the L1 model and the comparison passed. Other mismatches came from timestamp precision and decimal rounding, and I fixed those in the mapping or transformation and reran the validation.

So overall, my responsibility was to move the data from Teradata to Snowflake, transform it through L1 and L2 using dbt, and make sure the migrated data was accurate and production-ready.

-------------------------------------------------------------------------------------------------------------------------------------------------------------

3. How did you do the ingestion?

For ingestion, I worked on InfoWorks pipelines for 100+ tables, where raw Teradata data was loaded into the L0 layer in Snowflake. Some large-table loads were initially taking several hours, so I worked on optimizing them. For suitable tables, I changed full loads to incremental loads using a watermark column, used parallel reads based on key columns, and adjusted the Snowflake warehouse size. This brought the load time for those tables down to around 15 to 30 minutes.

------------------------------------------------------------------------------------------------------------------------------------------------------------

4. How did you do validation?

For validation, we followed a three-step approach. First, we compared row counts between Teradata and Snowflake. Second, we checked column-level metrics like null counts, distinct counts, sums, and minimum and maximum values. Third, for important tables, we compared records on the primary key to find missing or mismatched rows. I ran these checks for the tables I owned and investigated every mismatch.

For example, in one table the row counts matched but a string column did not, because Teradata CHAR columns carry trailing spaces. I added a TRIM in the L1 model and the comparison passed. Other mismatches came from timestamp precision and decimal rounding, and I fixed those in the mapping or transformation and reran the validation.

-------------------------------------------------------------------------------------------------------------------------------------------------
5. How did you choose the watermark column?

"I looked for a column that reliably changes whenever a row changes, usually a last_updated_ts or a monotonically increasing ID. I checked that it's indexed and not null in Teradata, and that updates always touch it."

--------------------------------------------------------------------------------------------------------------------------------------------------------------

6. How did you handle deletes and late-arriving records?

"A watermark only catches inserts and updates. For deletes, we either used a soft-delete flag from the source or ran a periodic full key comparison. For late-arriving records, we reloaded a small lookback window, for example the last few days, and merged on the primary key so nothing duplicated."








