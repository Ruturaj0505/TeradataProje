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



