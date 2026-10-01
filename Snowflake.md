1. What is Streams and Types of Streams ?

Snowflake streams is change data capture(CDC) machanism used to track changes in data at row level in snowflake.

There are three types:

i.   Standard (delta): tracks inserts, updates, and deletes. Updates show up as a DELETE + INSERT pair, flagged by METADATA$ISUPDATE. Works on tables, views, and directory tables.
ii.  Append-only: tracks inserts only and ignores updates and deletes. Cheaper, and good for ingestion-style pipelines. Works on tables, views, and directory tables.
iii. Insert-only: tracks inserts only, built for external tables (and Iceberg tables). It can't see deletes, since Snowflake doesn't own the underlying files.

Metadata columns on every stream:

METADATA$ACTION (INSERT/DELETE)
METADATA$ISUPDATE
METADATA$ROW_ID

2. 
