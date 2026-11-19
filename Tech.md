1. What is DBT?
2. Why do we use DBT when we can perform transformations directly in Snowflake?
3. What are the main DBT objects?
4. What are DBT models?
5. What is the difference between dbt run and dbt compile?
6. What is dbt test?
7. What is a DBT snapshot?
8. How does DBT snapshot implement SCD Type 2?
9. What is a DBT incremental model?
10. What are incremental strategies in DBT?
11. What is the difference between snapshot and incremental models?
12. What are DBT materializations?
13. What is the difference between ref() and source()?
14. What are DBT macros?
15. What are DBT seeds?
16. What are generic tests in DBT?
17. How do you identify duplicate records using DBT?
18. How do you troubleshoot a failed DBT model?
19. What is dbt build?
20. What is the difference between staging, intermediate and mart models?
2. Snowflake interview questions
1. What is Snowflake?
2. Explain Snowflake architecture.
3. What is the difference between database and warehouse?
4. What are virtual warehouses?
5. What are warehouse sizes?
6. What is Auto Suspend?
7. What is Auto Resume?
8. How do you reduce Snowflake warehouse costs?
9. What is a Snowflake stage?
10. What is an external stage?
11. What is Snowpipe?
12. How does Snowpipe work?
13. What is COPY INTO?
14. What is the difference between Snowpipe and COPY INTO?
15. What is a file format?
16. What is semi-structured data?
17. What is FLATTEN in Snowflake?
18. What is masking policy?
19. How do you identify failed file loads?
20. What is COPY_HISTORY?
3. SQL interview questions
1. Find the highest-paid employee in each department.
2. Find the second-highest salary in each department.
3. Find duplicate records in a table.
4. Delete duplicate records while keeping the latest record.
5. Find missing records between source and target.
6. Find employees who earn more than their department's average salary.
7. Explain ROW_NUMBER(), RANK() and DENSE_RANK().
8. Difference between UNION and UNION ALL.
9. Difference between WHERE and HAVING.
10. Difference between INNER JOIN and LEFT JOIN.
11. What is a FULL OUTER JOIN?
12. What is a CTE?
13. Difference between CTE and subquery.
14. What is a window function?
15. How do you use GROUP BY and aggregate functions?
4. AWS S3 and data loading questions
1. What is the main use of an AWS S3 bucket?
2. Why do we use S3 as a staging layer?
3. How do you load data from S3 into Snowflake?
4. What is an IAM role?
5. What is a storage integration?
6. How do you handle duplicate files arriving in S3?
7. How do you identify whether the same file has already been loaded?
8. How do you load data from Python into Snowflake?
9. What is the difference between Snowflake Python Connector and Snowpark?
10. What is the role of file metadata in data ingestion?
5. Real-time pipeline scenario questions
These are particularly important because they came up in today's discussion.
1. Your ERP system sends the same customer file twice to S3. How will you identify and handle it?
2. How will you prevent duplicate records from loading into Snowflake?
3. Your daily pipeline fails. How will you troubleshoot it?
4. If you rerun a failed pipeline, how will you prevent duplicate records?
5. How will you identify which DBT model failed?
6. How will you recover a failed pipeline?
7. ERP contains 20,000 records, but Snowflake contains only 19,500. How will you identify the missing 500 records?
8. What if both source and target have 20,000 records but some values are incorrect?
9. How do you implement watermark management?
10. How do you identify records using timestamps?
11. What happens when late-arriving records come into the pipeline?
12. How do you validate source-to-target data?
13. How do you handle incremental data loading?
14. How do you investigate a pipeline failure?
6. Snowflake performance and cost optimization
1. Your Snowflake warehouse cost increases by 50%. How will you investigate?
2. How will you reduce warehouse costs?
3. How do you optimize slow-running SQL queries?
4. What is query profiling?
5. What is partition pruning?
6. How does warehouse sizing affect performance?
7. When would you use Auto Suspend and Auto Resume?
8. How do you identify expensive queries?
7. Project and HR questions
1. Explain your current project architecture.
2. Explain your day-to-day activities.
3. What is your role and responsibility?
4. Explain your ERP-to-Snowflake pipeline.
5. Explain Bronze, Silver and Gold layers.
6. What transformations do you perform in the Gold layer?
7. How do you validate data?
8. What challenges have you faced in your project?
9. What value addition will you bring to our organization?
10. Why should we hire you?
11. What are your strengths?
12. Are you comfortable learning new technologies?
13. Do you have any questions for us?
