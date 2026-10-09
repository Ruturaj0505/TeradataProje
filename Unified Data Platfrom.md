### Unified Data Platfrom

UDP, the Unified Data Platform, was a project where we brought data from multiple ERP source systems into one platform on Snowflake, so the business could do cross-system reporting and analytics. I worked on it as a Data Engineer from December 2022 to March 2025.

The ERP data was extracted using SNP Glue into AWS storage, and for semi-structured data, the orchestration team fetched it from REST APIs. I built Snowpipes for multiple tables to load this data automatically into Snowflake, along with snapshots to track history.

We followed the Medallion architecture: bronze for raw data, silver for cleaned and standardized data, and gold for curated business models. The transformations were built with dbt.

One thing I'm proud of is the utility I created to build Snowpipes and snapshots from metadata. Instead of writing code for each table, onboarding a new table became a configuration entry, which saved effort and kept everything consistent.

I also improved pipeline performance and reliability through SQL optimization."

My role in one line

"I built the Snowpipes and snapshots, created the metadata-driven utility, developed the dbt models, and worked with the orchestration team on API ingestion."

### Version A: you used Streams and Tasks for snapshots

"Did you create Streams and Tasks?"
"Yes. For the snapshots, I created a Stream on the raw table to capture inserts and updates, and a Task to process those changes on a schedule."

"How did you create the snapshots?"
"Snowpipe loads new data into the raw table. A Stream on that table tracks the new and changed rows. A Task runs on a schedule, checks SYSTEM$STREAM_HAS_DATA, and runs a MERGE into the snapshot table. For a changed record, the MERGE closes the old row by setting an end date and inserts the new version with a start date and a current flag. That way we keep the full history of each record. The utility generated the Stream, Task, and snapshot table from metadata, so every table followed the same pattern."

Follow-ups to be ready for:

What if the Task fails? "It retries on the next schedule. The Stream keeps the offset until the changes are consumed, so no changes are lost."
Can a Stream go stale? "Yes, if its changes aren't consumed within the table's data retention period, so we monitored the Task runs."

### Version B: you did not use Streams and Tasks

"Did you create Streams and Tasks?"
"In UDP, I built the ingestion with Snowpipe and the snapshots with [dbt snapshots / a MERGE-based approach]. I haven't built Streams and Tasks in production, but I understand them: a Stream tracks changes on a table, and a Task runs SQL on a schedule, and together they give incremental processing."

"How did you create the snapshots?" Pick one:

dbt snapshots: "We used dbt snapshots with the timestamp or check strategy. dbt adds dbt_valid_from and dbt_valid_to columns, so each change creates a new version of the row and closes the old one. The utility generated the snapshot configuration from metadata."
MERGE-based: "A MERGE compares incoming records with the current version, closes the changed rows with an end date, and inserts the new versions."
