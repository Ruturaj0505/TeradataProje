### 1. Your source contains 10 million records, but the target contains only 9.7 million after the pipeline completes. How would you identify where the 300K records were lost?
 10M source vs 9.7M target
First, I would compare the record counts at each stage—source, ingestion/L0, transformation/L1, and final target. Then I would use business keys to identify records present in the source but missing from the target. I’d check filters, joins, duplicate handling, rejected records, and incremental-load conditions. This helps me identify exactly at which stage the 300K records were lost.”

Key point: Don't just compare source and target. Check every layer.

## 2. A source column was previously an INTEGER, but suddenly starts receiving decimal values. How would you handle this without breaking the existing pipeline?
INTEGER suddenly receives decimal values
“First, I would check whether the decimal values are expected from the source or are bad data. If decimals are valid, I would update the target column to an appropriate numeric data type, such as NUMBER with precision and scale, after checking downstream dependencies. If the existing contract must remain INTEGER, I would isolate or validate those records rather than silently truncating the values.”

Important: Don't say “I will directly cast decimal to integer” because you can lose data.

## 3. A batch runs at midnight, but the source system operates in a different timezone. How would you ensure records are assigned to the correct business date?
Different timezone and midnight batch
“I would standardize the timestamps to a common timezone, usually UTC, and then apply the business timezone when deriving the business date. I would clearly define the cutoff—for example, what time represents the start and end of the business day—and use that consistently in the ingestion and transformation logic.”

Example:

UTC timestamp → convert to business timezone → derive business_date

## 4. The business asks you to reload data for the previous 6 months after a transformation logic change. How would you design the process without affecting the current production pipeline?
Reload previous 6 months after logic change
“I would not directly reload the six months into production. First, I would develop and test the changed transformation separately. Then I would process the historical data into a temporary or staging area, validate counts and business rules, and after approval perform a controlled backfill. Current incremental processing would continue separately so the historical reload doesn't interfere with the daily pipeline.”

Good interview phrase: controlled backfill

## 5. The source ingestion succeeds, but the transformation step fails because of invalid records. Would you stop the entire pipeline or isolate the bad records? Why?
 Ingestion succeeds but transformation has invalid records
“I would prefer to isolate the bad records rather than fail the entire pipeline, provided the business process allows it. I would store the rejected records in an error or quarantine table with the reason for rejection, while allowing valid records to continue. I would also monitor the rejected-record count and alert the team if it crosses an acceptable threshold.”

But add:

“If the invalid records can affect the correctness of the entire dataset, then I would stop the pipeline.”

This shows judgment rather than blindly choosing one approach.

## 6. A column that normally contains values suddenly starts receiving NULLs. How would you determine whether the issue is from the source, ingestion, or transformation layer?
Column suddenly becomes NULL
“I would trace the column from source to target. First I would check whether the source itself contains NULLs. If not, I would check the ingestion/L0 layer and then the transformation logic, especially joins, CASE statements, casts, and filters. I would compare a few affected records across each layer to identify exactly where the NULL was introduced.”

## 7. Your Snowflake objects work correctly in DEV but fail after deployment to QA/PROD. What would you check first?
DEV works but QA/PROD fails
“First, I would compare the environment-specific configurations. I would check database, schema, warehouse, roles and privileges, stages, integrations, variables, secrets, and object names. I would also check whether all dependent objects exist in QA or PROD. Then I would look at the deployment logs and error message to identify the exact failing dependency.”

Don't immediately say “code issue.” Environment differences are often the reason.

## 8. Another business team needs access to selected datasets, but you don't want to physically copy the data. How would you securely provide access?
 Another team needs selected datasets without copying data
“I would use Snowflake's secure data-sharing capabilities instead of physically copying the data. I can expose only the required tables or secure views and control access using roles and privileges. If needed, I can create secure views to expose only selected columns or rows.”

Strong answer: Secure Data Sharing + secure views + RBAC
## 9. A reporting dashboard uses several nested views and suddenly becomes slow. How would you identify which part of the SQL/view hierarchy is causing the performance issue?
 Nested views suddenly become slow
“First, I would identify the actual query being executed by the dashboard and check its Query Profile. I would look for expensive joins, large scans, filters applied late, unnecessary aggregations, and repeated view logic. Then I would trace the nested views to find the expensive part. Based on that, I could simplify the SQL, improve filtering, materialize a transformation if appropriate, or optimize the underlying tables.”

Snowflake-specific: mention Query Profile.

## 10. Pipeline B should start only after Pipeline A successfully completes, but Pipeline A sometimes finishes with partial data. How would you design the dependency and validation mechanism?
Pipeline B starts only after A, but A sometimes has partial data
“I wouldn't trigger Pipeline B only based on Pipeline A's technical success status. Pipeline A should first perform validation checks such as expected record count, duplicate checks, NULL checks, and reconciliation with the source. Only if those validations pass would it mark the pipeline as successfully completed and allow Pipeline B to start. If validation fails, Pipeline B should not run and the issue should be alerted.”

The key distinction is:

Pipeline A completed ≠ Pipeline A completed successfully with valid data.

These are very good scenario-based Snowflake/Data Engineering interview questions. For your interview, I would answer them in a practical sequence: identify → isolate → validate → fix → prevent recurrence.
