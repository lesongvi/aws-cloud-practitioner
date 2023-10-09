<!--
 Copyright 2023 lesongvi
 
 Licensed under the Apache License, Version 2.0 (the "License");
 you may not use this file except in compliance with the License.
 You may obtain a copy of the License at
 
     http://www.apache.org/licenses/LICENSE-2.0
 
 Unless required by applicable law or agreed to in writing, software
 distributed under the License is distributed on an "AS IS" BASIS,
 WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
 See the License for the specific language governing permissions and
 limitations under the License.
-->

# Why Global Applications?
- Synchronous between applications can be problematic if there are sudden spikes in traffic
- What if you need to suddenly encode 1000 videos but your servers can only handle 100?

- In that case, it's better to *decouple* your applications and have them be asynchronous:
	- Using SQS (polling mechanism): queue model
	- Using SNS (push mechanism): pub/sub model
	- Using Kinesis (streaming mechanism): streaming model
- These services can scale independently from our application!

# Amazon Simple Queue Service (SQS)
![](/assets/amazon_sqs_whats_a_q.png)

## SQS - Standard Queue
- Oldest AWS offering (over 10 years old)
- Fully managed service (serverless offering), use to decouple applications
- Scales from 1 message per second to 10,000s per second
- Default retention of messages: 4 days, maximum of 14 days
- No limit to how many messages can be in the queue
- Messages are *deleted* after they're read by consumers
- Low latency (<10 ms on publish and receive)
- Consumers share the work to read messages & scale horizontally

## SQS to decouple between applications tiers
![](/assets/amazon_sqs_to_decouple_between_application_tiers.png)

## SQS - FIFO Queue
![](/assets/amazon_sqs_fifo_queue.png)

# Amazon Kinesis
![](/assets/amazon_kinesis.png)

## Amazon Kenesis - High Level
![](/assets/amazon_kenesis_high_level_overview.png)

- We can use Amazon Kinesis Streams to stream data to many different applications such as ClickStream analysis, Metrics Generation, Log Aggregation, etc...
- Then we can use Amazon Kinesis Analytics to analyze the data in real-time
- Then we can use Amazon Kinesis Firehose to load the data into S3, Redshift, ElasticSearch, etc...

# Amazon Simple Notification Service (SNS)
## Why SNS?
![](/assets/why_sns.png)

## Overview
![](/assets/amazon_sns.png)

# Amazon MQ
![](/assets/amazon_mq.png)

# Cloud Integration - Summary
- SQS:
	- Queue service in AWS
	- Multiple producers, messages are kept up to 14 days
	- Multiple consumers share the read and delete messages when done
	- Used to *decouple* applications in AWS
- SNS:
	- Notification service in AWS
	- Subscribe email addresses, phone numbers, AWS Lambda, SQS queues, HTTP endpoints, etc...
	- Multiple subscribers, send all messages to all subscribers
	- No message retention, push mechanism
- Kinesis: realtime data streaming, persistence and analytics
- Amazon MQ: managed message broker for ActiveMQ and RabbitMQ in the cloud (MQTT, AMQP.. protocols)