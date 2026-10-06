## What is Streams and Types of Streams ?

Snowflake streams is change data capture(CDC) machanism used to track changes in data at row level in snowflake.

There are three types:

i.   Standard (delta): tracks inserts, updates, and deletes. Updates show up as a DELETE + INSERT pair, flagged by METADATA$ISUPDATE. Works on tables, views, and directory tables.
ii.  Append-only: tracks inserts only and ignores updates and deletes. Cheaper, and good for ingestion-style pipelines. Works on tables, views, and directory tables.
iii. Insert-only: tracks inserts only, built for external tables (and Iceberg tables). It can't see deletes, since Snowflake doesn't own the underlying files.

Metadata columns on every stream:

METADATA$ACTION (INSERT/DELETE)
METADATA$ISUPDATE
METADATA$ROW_ID

----------------------------------------------------------------------------------------------------------------------------------------------------------

## what is Snowpipe and how it works?

   Snowpipe is Snowflake’s continuous data ingestion service. It is used to automatically load new files from cloud storage like Amazon S3 into a Snowflake table as soon as they arrive.
   
The flow is: S3 → Event Notification → Snowpipe → Snowflake staging table.

For example, when a new JSON or CSV file is uploaded to S3, an event notification triggers Snowpipe. Snowpipe uses a COPY INTO statement to load that file into the target Snowflake table. It tracks the files that have already been loaded, so the same file is not loaded again.”

What happens if Snowpipe fails?"
“First, I check the Snowpipe load history and error details to identify whether the issue is related to the file, permissions, stage, or target table. After fixing the issue, I reprocess the failed files and validate the record count and data quality.”

-----------------------------------------------------------------------------------------------------------------------------------------------------------

## What is task and how it works ?

A Snowflake Task is used to automate SQL statements or stored procedures. We can schedule a task to run at a specific time or at a regular interval, and we can also create task dependencies where one task runs after another.
For example, in my pipeline, after Snowpipe loads data from S3 into the staging table, a Task can pick up that data, perform transformations, and load it into the target table.
So basically, Snowpipe handles the ingestion, and Task handles the automated processing or transformation.”

--------------------------------------------------------------------------------------------------------------------------------------------------------------------
   
    
