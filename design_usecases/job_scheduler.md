# Job Scheduler 

## Source

1. https://www.hellointerview.com/learn/system-design/problem-breakdowns/job-scheduler
2. https://medium.com/@mayilb77/design-a-distributed-job-scheduler-for-millions-of-tasks-in-daily-operations-4132dc6d645f
3. https://dropbox.tech/infrastructure/asynchronous-task-scheduling-at-dropbox

## Area to focus

1. DB design - query pattern and keys of DBs
2. Timely pick of a job by worker
3. Worker coordination
4. Priority tasks
5. Retry & job failure flow
6. Ensure at-least-once execution of jobs

## New Learning

1. SQS - Delayed Message Delivery
2. SQS - Visibility Timeout
3. DynamoDB - PK, SK, GSI and LSI
4. API design
5. DB Query pattern