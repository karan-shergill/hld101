# AWS SQS

See: [**AWS SQS Overview For Beginners**](https://youtu.be/CyYZ3adwboc)

Read: [**Amazon Simple Queue Service (SQS): A Comprehensive Tutorial**](https://www.datacamp.com/tutorial/amazon-sqs)

## SQS Visibility Timeout

1. The period during which a message becomes invisible to other consumers after one consumer receives it.
2. Prevents multiple consumers from processing the same message simultaneously.
3. Default visibility timeout is 30 seconds; it can be configured up to 12 hours.
4. If the consumer does not delete the message before the visibility timeout expires, the message becomes visible again for reprocessing.
5. Useful for ensuring at-least-once processing with failure recovery.
6. Consumers can extend visibility timeout using ChangeMessageVisibility if processing takes longer than expected.

## Delayed Message Delivery

1. Allows delaying a message so it only becomes visible to consumers after a specified delay period.
2. Delay can be set per-message (DelaySeconds) or at the queue level.
3. Maximum allowed delay is 15 minutes.
4. Useful for scheduled tasks, retry with backoff, and smoothing traffic spikes.
5. During the delay period, the message is stored in the queue but cannot be read by consumers.

## Standard vs FIFO Queues

1. Standard Queue
    1. Provides at-least-once delivery; duplicate messages may appear.
    2. Offers best-effort ordering; ordering is not guaranteed.
    3. Supports very high throughput (nearly unlimited transactions per second).
    4. Suitable for background tasks, notifications, log processing, and any workload where ordering does not matter.
2. FIFO Queue
    1. Guarantees exactly-once processing (no duplicates).

   ii. Ensures messages are processed in the exact order they are sent.

   iii. Throughput is limited to 300 messages per second (or 3,000 with batching).

   iv. Supports message grouping using MessageGroupId for parallel ordered streams.

   v. Suitable for financial transactions, order management, inventory updates, and any workflow where strict ordering matters.