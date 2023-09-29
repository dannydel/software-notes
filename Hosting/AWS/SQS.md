# General
- Supports Deduplication


## Deduplication
- Receive message wait time: always set to 20 seconds so that you do not cause many calls in a short amount of time. It keeps the connection open for 20 seconds.
- You can only set the timeout in one place, SNS properties in AWS or on the message in the code. Not both.


