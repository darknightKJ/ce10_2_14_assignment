# ce10_2_14_assignment
2.14 Assignment 


1. Does SNS guarantee exactly-once delivery to subscribers?

    * Standard SNS topics provide at‑least‑once delivery, meaning messages may be delivered more than once, and duplicates are possible
    * SNS FIFO topics, when subscribed to SQS FIFO queues, can achieve exactly-once delivery, given proper deduplication attributes and absence of filtering
    * In practice, even with FIFO, network disruptions or publisher-side misconfiguration can lead to duplicates, so applications should still handle idempotency

2. What is the purpose of the Dead‑Letter Queue (DLQ)?
    a. A DLQ (commonly an SQS queue) is attached to an SNS subscription to store messages that failed delivery after retries due to client or server errors
  
    b. It provides mechanisms for:
   
       * Troubleshooting and reprocessing undelivered messages

       * Fault isolation, preventing retries from overwhelming the primary processing system

       * Monitoring DLQ activity via CloudWatch metrics (e.g., ApproximateNumberOfMessagesVisible) and alarms

4. How to enable email notifications when messages are added to the DLQ?
  * Create a CloudWatch alarm on the DLQ’s ApproximateNumberOfMessagesVisible metric (threshold ≥1).
  * Define an SNS topic as the alarm’s notification endpoint.
  * Subscribe an email endpoint to that SNS topic.
  * Upon a message entering the DLQ, the alarm triggers SNS, which sends the alert via email
