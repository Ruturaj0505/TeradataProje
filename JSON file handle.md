
1. How did you handle JSON data in your project?”,

“When we receive JSON data, first we load the raw JSON into a Snowflake staging table. Snowflake supports semi-structured data using the VARIANT data type.
In the staging layer, we keep the raw JSON so that we don't lose the original structure. Then in the transformation layer, we use Snowflake functions like : and FLATTEN to extract the required fields and arrays.
For example, if the JSON contains customer details like customer ID, name and address, I extract those fields and transform them into relational columns. Then I apply validations and business transformations and load the required data into the target tables.”

--------------------------------------------------------------------------------------------------------------------------------------------------------------

 2. Actual L0,L1, and L2 does in Teradata project ?
 
"In my current project, Teradata is one of our main source systems. Business data is generated or updated in Teradata during the day, and based on the agreed batch schedule, we pick it up for downstream processing.

We use InfoWorks as our data integration and orchestration tool. It connects to Teradata, extracts data based on the configured jobs, and loads it into Snowflake. For incremental loads, we identify new and updated records using a timestamp or key-based logic and a stored watermark, and InfoWorks moves only those records.

In Snowflake, we follow a three-layer architecture.

L0 is the raw landing layer. The data is loaded as close to the source as possible, with only minimal transformation. We add audit columns like batch ID and load timestamp, and run technical checks such as record counts and data-type validation.

L1 is the cleansing and transformation layer, and this is where I mainly work. I remove duplicates using ROW_NUMBER on the business key, apply mandatory null checks, convert and standardize data types and values, filter invalid records, do the required joins, and apply business rules. The goal is to turn raw L0 data into clean, consistent data.

L2 is the business-ready layer. Here we integrate the required L1 datasets, apply the final business logic, and build the fact and dimension datasets. For example, if customer data comes from one source and transactions from another, we integrate them in L2 to produce a business-level customer or transaction dataset.

Before publishing L2, we run validations: record count reconciliation, duplicate checks, null checks, and business-rule checks.

Downstream, the reporting and analytics team consumes the L2 tables or views for Power BI dashboards and reports. They never need to touch the raw L0 data.

So the overall flow is: Teradata → InfoWorks → Snowflake L0 → L1 → L2 → Power BI.
---------------------------------------------------------------------------------------------------------------------------------------------------------
My role was writing and optimizing the Snowflake SQL for the L1 and L2 transformations, handling incremental load logic, validating data against the source, and fixing data-quality and performance issues.
