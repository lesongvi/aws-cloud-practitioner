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

# CloudWatch
## Important Metrics
- EC2 instances: CPU Utilization, Status Check, Network (not RAM)
	- Default metrics every 5 minutes
	- Option for Detailed Monitoring ($$$): metrics every 1 minute
- EBS Volumes: Disk Read/Writes
- S3 Buckets: BucketSizeBytes, NumberOfObjects, AllRequests
- Billing: Total Estimated Charge (only in us-east-1, but it all your costs)
- Service Limits: how much you've been using a service API (ex: how many EC2 instances have you launched)
- Custom metrics: push your own metrics to CloudWatch (ex: RAM usage, failed logins, etc...)

## Amazon CloudWatch Alarms
- Alarms are used to trigger notifications for any metric
- Alarms actions...
	- Auto Scaling: increase or decrease EC2 instances "desired" count
	- EC2 Actions: stop, terminate, reboot or recover an EC2 instance
	- SNS notifications: send a notification into an SNS topic
- Various options (sampling, %, max, min, etc...)
- Can choose the period on which to evaluate an alarm
- Example: create a billing alarm on the CloudWatch Billing metric
- Alarm States: OK, INSUFFICIENT_DATA, ALARM

## CloudWatch Logs
- CloudWatch Logs can collect log from:
	- Elastic Beanstalk: collection of logs from application
	- ECS: collection from containers
	- AWS Lambda: collection from function logs
	- CloudTrail based on filter
	- CloudWatch log agents: on EC2 machines (for example, for web servers) or on-premise servers
	- Route53: Log DNS queries

- Enables real-time monitoring of logs for:
	- Troubleshooting
	- Performance analysis
	- Security analysis
- Adjustable CloudWatch Logs retention policy at the log group level (never expire, 30 days, etc...)

### CloudWatch Logs for EC2
![](/assets/cloudwatch_logs_for_ecs2.png)

## Amazon EventBridge (formerly CloudWatch Events)
![](/assets/amazon_eventbridge_formerly_cloudwatch_event.png)

### Amazon EventBridge Rules
![](/assets/amazon_eventbridge_rules.png)

### Amazon EventBridge Event Buses
![](/assets/amazon_eventbridge_event_bus.png)

# CloudTrail
- Provides governance, compliance and audit for your AWS account
- CloudTrail is enabled by default!
- Get an history of events/API calls made within your AWS account by:
	- Console
	- SDK
	- CLI
	- AWS Services
- Can put logs from CloudTrail into CloudWatch Logs or S3
- A trail can be applied to all regions (default) or a single region
- If a resource is deleted in AWS, look into CloudTrail first!

## CloudTrail diagram
![](/assets/cloudtrail_diagram.png)

# AWS X-Ray
## Why X-Ray?
- Debugging in production, the good old way:
	- Test locally
	- Add logs statements everywhere
	- Re-deploy in production
- Log formats differ across applicaions and log analysis is complex
- Debugging: one big monolith "easy", but distributed services (microservices) are hard
- No common views of your entire architecture

- Enter X-Ray!

## Visual analysis of your distributed application
![](/assets/aws_xray.png)

## AWS X-Ray advantages
- Troubleshooting performance (bottlenecks)
- Understand dependencies in a microservice architecture
- Pinpoint service issues
- Review request behavior
- Find errors and exceptions
- Are we meeting time SLA (Service Level Agreement)?
- Where I am throttled?
- Identify users that are impacted by these outages

# Amazon CodeGuru
![](/assets/aws_codeguru.png)

## Amazon CodeGuru Reviewer
![](/assets/aws_codeguru_reviewer.png)

## Amazon CodeGuru Profiler
![](/assets/amazon_codeguru_profiler.png)

# AWS Health Dashboard
![](/assets/aws_health_dashboard.png)

## AWS Health Dashboard - Your Account
![](/assets/aws_health_dashboard_your_account.png)

# Cloud Monitoring Summary
- CloudWatch:
	- Metrics: monitor the performance of AWS services and billing metrics
	- Alarms: automate notification, perform EC2 action, notify to SNS based on metrics
	- Logs: collect log files from EC2 instances, on-premise servers, AWS services (Lambda, etc...)
	- Events (or EventBridge): react to events in AWS, or trigger a rule on a schedule
- CloudTrail: audit API calls made in your AWS account
- CloudTrail Insights: automated analysis of your CloudTrail Events
- X-Ray: trace requests made through your distributed application
- AWS Health Dashboard: status of all AWS services across all regions
- AWS Account Health Dashboard: AWS events that might impact your AWS resources and services
- Amazon CodeGuru: automated code reviews and application performance recommendations