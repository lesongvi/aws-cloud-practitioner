<!--
 Copyright 2024 lesongvi
 
 Licensed under the Apache License, Version 2.0 (the "License");
 you may not use this file except in compliance with the License.
 You may obtain a copy of the License at
 
     https://www.apache.org/licenses/LICENSE-2.0
 
 Unless required by applicable law or agreed to in writing, software
 distributed under the License is distributed on an "AS IS" BASIS,
 WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
 See the License for the specific language governing permissions and
 limitations under the License.
-->

- One of the hardest topics in the exam... but also one of the most important ones.

# Table of Contents
- [AWS Organizations](#aws-organizations)
	- [Multi Account Strategies](#multi-account-strategies)
	- [Organizational Units (OUs) - Examples](#organizational-units-ous---examples)
		- [AWS Organization](#aws-organization)
		- [Service Control Policies (SCPs)](#service-control-policies-scps)
			- [SCP Example](#scp-example)
- [AWS Control Tower](#aws-control-tower)
- [AWS Resource Access Manager (AWS RAM)](#aws-resource-access-manager-aws-ram)
- [AWS Service Catalog](#aws-service-catalog)
- [Pricing Models of the Cloud](#pricing-models-of-the-cloud)
	- [AWS Pricing Models](#aws-pricing-models)
	- [Free services & free tier in AWS](#free-services--free-tier-in-aws)
	- [Computing Pricing - EC2](#computing-pricing---ec2)
		- [EC2 Pricing Models](#ec2-pricing-models)
	- [Computing Pricing - Lambda & ECS](#computing-pricing---lambda--ecs)
	- [Storage Pricing - S3](#storage-pricing---s3)
	- [Storage Pricing - EBS](#storage-pricing---ebs)
	- [Database Pricing - RDS](#database-pricing---rds)
	- [Content Delivery - CloudFront](#content-delivery---cloudfront)
	- [Networking Costs in AWS per GB - Simplified](#networking-costs-in-aws-per-gb---simplified)
- [Savings Plan Overview](#savings-plan-overview)
	- [EC2 Savings Plan](#ec2-savings-plan)
	- [Compute Savings Plan](#compute-savings-plan)
	- [Machine Learning Savings Plan](#machine-learning-savings-plan)
	- [Setting up & Estimating Savings Plan](#setting-up--estimating-savings-plan)
- [AWS Compute Optimizer](#aws-compute-optimizer)
- [Billing & Costing Tools (little bit of boring stuff)](#billing--costing-tools-little-bit-of-boring-stuff)
	- [AWS Pricing Calculator](#aws-pricing-calculator)
	- [Billing Dashboard, Cost Allocation Tags, Cost and Usage Report & Cost Explorer](#billing-dashboard-cost-allocation-tags-cost-and-usage-report--cost-explorer)
		- [Billing Dashboard](#billing-dashboard)
			- [AWS Free Tier Dashboard](#aws-free-tier-dashboard)
		- [Cost Allocation Tags](#cost-allocation-tags)
			- [Tagging and Resource Groups](#tagging-and-resource-groups)
		- [Cost and Usage Report](#cost-and-usage-report)
		- [Cost Explorer](#cost-explorer)
	- [Monitoring against costs plans](#monitoring-against-costs-plans)
		- [Billing Alarm in CloudWatch](#billing-alarm-in-cloudwatch)
		- [Budgets](#budgets)
- [AWS Cost Anomaly Detection](#aws-cost-anomaly-detection)
- [AWS Service Quotas](#aws-service-quotas)
- [AWS Trusted Advisor](#aws-trusted-advisor)
- [AWS Support Plans Pricing](#aws-support-plans-pricing)
	- [AWS Basic Support Plan](#aws-basic-support-plan)
	- [AWS Developer Support Plan](#aws-developer-support-plan)
	- [AWS Business Support Plan](#aws-business-support-plan)
	- [AWS Enterprise On-Ramp Support Plan (24/7)](#aws-enterprise-on-ramp-support-plan-247)
	- [AWS Enterprise Support Plan (24/7) - Not On-Ramp](#aws-enterprise-support-plan-247---not-on-ramp)
- [Account best practices summary](#account-best-practices-summary)
- [Billing & Costing Tools summary](#billing--costing-tools-summary)

# AWS Organizations
- Global service
- Allows you to manage **multiple AWS accounts**
- The main account is the **master account**
- Cost benefits:
	- **Consolidated billing** across all accounts - single payment method
	- Pricing benefits from **aggregated usage** (volume discounts for EC2, S3, etc.)
	- **Pooling of reserved EC2 instances** for optimal savings
- API is available to **automated AWS account creation**
- **Restrict account privileges using Service Control Policies (SCPs)**

## Multi Account Strategies
- Create account per **department**, per **cost center**, per **dev / test / prod**, based on **regulatory restrictions** (using **SCP**), for **better resource isolation** (ex: VPC), to have **separate per-account service limits**, isolate account for **logging and auditing** purposes, etc.
- Multi Account vs. One Account Multi VPC
- Use tagging standard for billing purposes
- Enable CloudTrail on all accounts, send logs to a central S3 bucket
- Send CloudWatch Logs to central logging account

## Organizational Units (OUs) - Examples
![OUs](/assets/organizational_unit_examples.png)  
- More detail, [https://aws.amazon.com/answers/account-management/aws-multi-account-billing-strategy/](https://aws.amazon.com/answers/account-management/aws-multi-account-billing-strategy/)

### AWS Organization
![AWS Organization](/assets/aws_organization_ou.png)  

### Service Control Policies (SCPs)
- Whitelist or blacklist IAM actions
- Applied at the **OU** or **Account** level
- Does not apply to the Master 
- SCP is applied to all the **Users and Roles** of the Account, including Root
- The SCP does not affect service-linked roles:
	- Service-linked roles enable other AWS services to integrate with AWS Organizations and can't be restricted by SCPs
- SCP must have an exploit Allow (does not allow anything by default)
- Use cases:
	- Restrict access to certain services (for example: can't use EMR)
	- Enforce PCI compliance by explicit disabling services

- *Note*: PCI = Payment Card Industry

#### SCP Example
![SCP Example](/assets/scp_examples.png)

## AWS Organization - Consolidated Billing
- When enabled, provides you with:
	- **Combined usage** - combine the usage across all AWS accounts in the AWS Organization **to share he volume pricing, Reserved Instances and Saving Plan discounts**
	- **One Bills** - one bill for all AWS Accounts in the AWS Organization
- The management account can turn off Reserved Instance discount sharing for any account in the AWS Organization, including self.
  
![](/assets/aws_organization_bill.png)

# AWS Control Tower
- Easy way to **set up and govern a secure and compliant <u>multi-account AWS environment</u>** based on best practices.
- Benefits:
	- Automate the set up of your environment in a few clicks.
	- Automate ongoing policy management using **guardrails**.
	- Detect policy violations and remediate them.
	- Monitor compliance through an interactive dashboard.
- AWS Control Tower runns on top of AWS Organizations:
	- It automatically sets up AWS Organizations to organize accounts and implement SCPs (Service Control Policies).

- *Note*: guardrails in AWS Control Tower are stuff like "You must enable MFA for the root account", "You must enable CloudTrail for all regions", etc... Governance rules that you can enable on your organization units (OUs) to enforce policies or detect violations.

# AWS Resource Access Manager (AWS RAM)
![](/assets/aws_resource_access_manager.png)

# AWS Service Catalog
- Users that are new to AWS have too many options, and may create stacks that are not compliant / in line with the rest of the organization.
- Some users just want a quick **self-service portal** to launch a set of **authorized products** pre-defined **by admins**
- Includes: Virtual machines, databases, storage options, etc...
- **Simple summary**: AWS Service Catalog allows you to create and manage catalogs of IT services that are approved for use on AWS. Helps organization members, especially new ones, to quickly deploy approved IT services without being overwhelmed by the AWS Console.

# Pricing Models of the Cloud
## AWS Pricing Models
- AWS has 4 pricing models:
	- **Pay as you go**: Pay for what you use, remain agile, responsive, meet scale demands.
	- **Save when you reserve**: Minimize risks, predictably manage budgets, comply with a long-term requirement.
		- Reservations are available for EC2 Reserved Instances, DynamoDB Reserved Capacity, ElastiCache Reserved Nodes, RDS Reserved Instances, Redshift Reserved Nodes.
	- **Pay less by using more**: Volume-based discounts, tiered pricing, data transfer out pricing.
	- **Pay less as AWS grows**: AWS lowers prices over time, pass the savings back to customers.

## Free services & free tier in AWS
- IAM
- VPC
- Consolidated Billing
- Elastic Beanstalk
- CloudFormation
- ASG
- Free tier: [https://aws.amazon.com/free/](https://aws.amazon.com/free/)
	- EC2 t2.micro instance (750 hours per month) for 12 months
	- S3, EBS, ELB, AWS Data Transfer, etc...
  
*Note*: With Elastic Beanstalk, CloudFormation and ASG, you pay for the underlying resources that these services create (EC2 instances, RDS instances, etc...). The services themselves are free.

## Computing Pricing - EC2
- Only charged for what you use
- Number of instances
- Instance configuration:
	- Physical capacity
	- Region
	- OS and software
	- Instance type
	- Instance size
- ELB running time and amount of data processed
- Detailed monitoring

### EC2 Pricing Models
- On-Demand Instances:
	- Minimum for 60s
	- Pay-per-second (Linux/Windows) or per hour (other)
- Reserved instances:
	- Up to 75% discount compared to On-Demand on hourly rate
	- 1 year or 3 year term
	- All upfront, partial upfront, no upfront
- Spot Instances:
	- Up to 90% discount compared to On-Demand on hourly rate
	- Bid for unused EC2 capacity
- Dedicated Hosts:
	- On-Demand
	- Reservation for 1 year or 3 years commitment
- Savings Plan as an alternative to save on sustained usage

## Computing Pricing - Lambda & ECS
- Lambda:
	- Pay per call
	- Pay per duration
- ECS:
	- EC2 Launch Type Model: No additional fees, you pay for AWS resources stored and created in your application
- Fargate:
	- Fargate Launch Type Model: Pay for the vCPU and memory resources allocated to your applications in your containers

## Storage Pricing - S3
- Storage class: S3 Standard, S3 Infrequent Access, S3 One Zone-IA, S3 Intelligent Tiering, S3 Glacier and S3 Glacier Deep Archive
- Number and size of objects: Price can be tiered (based on volume)
- Number and type of requests
- Data transfer OUT of the S3 region -> Data transfer IN is free
- S3 Transfer Acceleration: Faster uploads to S3, additional fees apply
- Lifecycle transitions: Moving objects between storage classes
  
- Similar service: [EFS](/ec2/instance_storage/README.md#efs---elastic-file-system) (pay-per-use, has infrequent access & lifecycle rules)

## Storage Pricing - EBS
- Volume type (based on performance characteristics)
- Storage volume in **Gigabytes** per month **provisioned**
- IOPS (I/O operations per second):
	- General Purpose SSD: Included in the price
	- Provisioned IOPS SSD: Charged per provisioned IOPS
	- Magnetic: Number of requests
- Snapshots:
	- Added data cost per GB per month
- Data transfer:
	- Outbound data transfer are tiered for volume discounts
	- Inbound data transfer is free

## Database Pricing - RDS
- Per-hour billing
- Database characteristics:
	- Engine
	- Size
	- Memory class
- Purchase type:
	- On-Demand
	- Reserved instances (1 year or 3 years) with required upfront payment
- Backup storage: There is no additional charge for backup storage up to 100% of your total database storage for a region.
- Additional storage: Charged per GB per month
- Number of input and output requests per month
- Deployment types (storage and I/O are variable):
	- Single AZ
	- Multi-AZ
- Data transfer:
	- Outbound data transfer are tiered for volume discounts
	- Inbound data transfer is free

## Content Delivery - CloudFront
- Pricing is different across different geographic regions
- Aggregated for each edge location, then applied to your bill
- Data transfer OUT (volume discounts) and data transfer IN is always free
- Number of HTTP/HTTPS requests

![CloudFront Pricing based on regions](/assets/cloud_front_pricing_table.png)

## Networking Costs in AWS per GB - Simplified
- This is a simplified version of the AWS data transfer costs per GB, the actual costs are more complex and depend on the region and the amount of data transferred.

![](/assets/networking_costs_simplified.png)

- Recomendation: Always use private IP addresses when communicating between AWS EC2 instances.

# Savings Plan Overview
- Commit a certain $ amount per hour for 1 or 3 years
- Easiest way to setup long-term commitment on AWS

## EC2 Savings Plan
- Up to 72% discount compared to On-Demand
- **Commit to usage of individual instance families in a region** (eg. C5 or M5 instances in US West)
- Regardless of AZ, size (m5.large, m5.xlarge, etc... within the family), OS (linux/windows) or tenancy
- All upfront, partial upfront, no upfront

## Compute Savings Plan
- Up to 66% discount compared to On-Demand
- Regardless of **Family**, **Region**, size, OS, tenancy, **compute option**
- Compute options: EC2, Fargate, Lambda

## Machine Learning Savings Plan
- SageMaker,...

## Setting up & Estimating Savings Plan
- Setup from the AWS Cost Explorer console
- Estimate pricing at [https://aws.amazon.com/savingsplans/pricing/](https://aws.amazon.com/savingsplans/pricing/)

# AWS Compute Optimizer
![AWS Compute Optimizer](/assets/aws_compute_optimizer_dashboard.png)
- **Reduce costs** and **improve performance** by recommending optimal AWS resources for your workloads
- Helps you choose optimal configurations and right-sizes your workloads (over/under-provisioned)
- Uses **machine learning** to analyze your **resources' utilization** and their **ultilization CloudWatch metrics**
- Supported resources:
	- **EC2 instances**
	- **Auto Scaling groups**
	- ECS tasks
	- **EBS volumes**
	- **Lambda functions**
	- RDS databases
- Lower your costs by **up to 25%** by using the recommendations
- Recommendations themselves can be exported to S3

# Billing & Costing Tools (little bit of boring stuff)
- **Estimating costs in the cloud**:
	- [Pricing calculator](#aws-pricing-calculator)
- **Tracking costs in the cloud**:
	- [Billing Dashboard](#billing-dashboard)
	- [Cost Allocation Tags](#cost-allocation-tags)
	- [Cost and Usage Report](#cost-and-usage-report)
	- [Cost Explorer](#cost-explorer)
- **Monitoring against costs plans**:
	- Billing Alarm
	- Budgets

## AWS Pricing Calculator
- Available at [https://calculator.aws/](https://calculator.aws/)
- Estimate the cost for your solution architecture

![](/assets/aws_calculator_summary.png)

## Billing Dashboard, Cost Allocation Tags, Cost and Usage Report & Cost Explorer
### Billing Dashboard
![](/assets/aws_billing_dashboard.png)

#### AWS Free Tier Dashboard
![](/assets/aws_free_tier_dashboard.png)

### Cost Allocation Tags
![](/assets/cost_allocation_tags.png)

#### Tagging and Resource Groups
- **Tags** are used for organizing resources:
	- EC2: instances, images, load balancers, security groups, etc...
	- RDS, VPC resources, Route 53, IAM users, etc...
	- Resources created by CloudFormation are all tagged the same way
- Free naming, common tags are:
	- **Environment**: dev, test, prod
	- **Role**: web server, db server, etc...
	- **Owner**: name of the owner
	- **Project**: name of the project
	- **Name**: name of the resource
- Tags can be used to create **Resource Groups**:
	- Create, maintain, and view a collection of resources that share common tags
	- Manage these tags using Tag Editor

### Cost and Usage Report
- Dive deeper into your AWS costs and usage
- The AWS Cost and Usage Report contains **the most comprehensive set of AWS cost and usage data available**, including additional metadata about AWS services, pricing, and reservations **(e.g., Amazon EC2 Reserved Instances (RIs))**.
- The AWS Cost and Usage Report lists AWS usage for each service category used by an account and its IAM users in hourly or daily line items, as well as any tags that you have activated for cost allocation purposes.
- Can be integrated with Athena, QuickSight or Redshift for further analysis

![](/assets/cost_and_usage_report.png)

### Cost Explorer
- Visualize, understand, and manage your AWS costs and usage over time
- Create custom reports and analyze your costs and usage data
- Analyze your data at a high level: total costs and usage across all accounts
- Or monthly, hourly, resource level granularity
- Choose an optimal **Savings Plan** based on your usage (to lower prices on your bill)
- **Forecast usage up to 12 months based on previous usage (historical data)**

## Monitoring costs in the cloud - Billing Alarm & Budgets
### Billing Alarm in CloudWatch
![](/assets/cloudwatch_billing_alarm.png)

### Budgets
- Create a budget and **send alarms when costs exceed the budget**
- 4 types of budgets:
	- **Cost budget**: Monitor your costs
	- **Usage budget**: Monitor your usage
	- **Reservation budget**: Monitor your reservation usage
	- **Savings plan budget**: Monitor your savings plan usage
- For Reserved Instances (RI)
	- Track utilization of your RIs
	- Support EC2, ElastiCache, RDS, Redshift, etc...
- Up to 5 SNS notifications per budget
- Can filter by: Service, Linked Account, Tag, Purchase Option, Instance Type, Region, Availability Zone, API Operation, etc...
- Same options as AWS Cost Explorer
- 2 budgets are free, then $0.02 per budget per day

# AWS Cost Anomaly Detection
![](/assets/aws_cost_anomaly_detection.png)

# AWS Service Quotas
![](/assets/aws_service_quotas.png)

# AWS Trusted Advisor
![](/assets/aws_trusted_advisor.png)

# AWS Support Plans Pricing
![](/assets/aws_support_plans_pricing.png)

*Note*: You are expected to know the different between the AWS Support Plans (Basic, Developer, Business, Enterprise) and what they offer.

## AWS Basic Support Plan
- **Customer Service & Communities**: 24/7 access to customer service, documentation, whitepapers, and support forums
- **AWS Trusted Advisor**: Access to the 7 core Trusted Advisor checks and guidance to provision your resources following best practices to increase performance and improve security
- **AWS Personal Health Dashboard**: A personalized view of the health of AWS services, and alerts when your resources are impacted

## AWS Developer Support Plan
- All features of the Basic Support Plan, plus:
	- **Business Hours Email Access** to Cloud Support Associates
	- Unlimited cases / unlimited contacts
	- <u>Case severity / response time</u>:
		- General guidance: <u>within 24 hours</u>
		- System impaired: <u>within 12 hours</u>

## AWS Business Support Plan
- Intended to be used if you have **production workloads**
- **Trusted Advisor** - Full set of checks + API access
- **24x7 phone, email, and chat access** to Cloud Support Engineers
- Unlimited cases / unlimited contacts
- Access to Infrastructure Event Management **for additional fee**.
- <u>Case severity / response time</u>:
	- General guidance: <u>within 24 hours</u>
	- System impaired: <u>within 12 hours</u>
	- **Production system impaired: <u>within 4 hours</u>**
	- **Production system down: <u>within 1 hour</u>**

## AWS Enterprise On-Ramp Support Plan (24/7)
- Intended to be used if you have **production or critical workloads**
- All features of the Business Support Plan, plus:
	- Access to a pool of **Technical Account Managers (TAMs)**
	- **Concierge Support Team** (for billing and account best practices)
	- **Infrastructure Event Management, Well-Architected Reviews, and Operations Reviews**
	- <u>Case severity / response time</u>:
		- General guidance: <u>within 24 hours</u>
		- System impaired: <u>within 12 hours</u>
		- Production system impaired: <u>within 4 hours</u>
		- Production system down: <u>within 1 hour</u>
		- **Business-critical system down: <u>within 30 minutes</u>**

## AWS Enterprise Support Plan (24/7) - Not On-Ramp
- Intended to be used if you have **mission-critical workloads**
- All features of the Business Support Plan, plus:
	- Access to a **designated** Technical Account Manager (TAM)
	- **Concierge Support Team** (for billing and account best practices)
	- **Infrastructure Event Management, Well-Architected Reviews, and Operations Reviews**
	- Access to **AWS Incident Detection and Response** (for additional fee)
	- <u>Case severity / response time</u>:
		- General guidance: <u>within 24 hours</u>
		- System impaired: <u>within 12 hours</u>
		- Production system impaired: <u>within 4 hours</u>
		- Production system down: <u>within 1 hour</u>
		- **Business-critical system down: <u>within 15 minutes</u>**

# Account best practices summary
- Operate multiple accounts using **AWS Organizations**
- Use **SCP** (Service Control Policies) to restrict account privileges
- Easily setup multiple accounts with best-practices using **AWS Control Tower**
- **Use Tags & Cost Allocation Tags** for easy management and billing
- **IAM guidelines**: MFA, least-privilege, password policies, password rotation
- **Config** to record all resources configurations & compliance all the time
- **CloudFormation** to deploy stacks across accounts and regions
- **Trusted Advisor** to get insights, Support Plans adapted to your needs
- Send Service Logs and Access Logs to **S3 or CloudWatch Logs**
- **CloudTrail** to record API calls made within your account
- **Incase your account is compromised**: Change the root password, delete and rotate all passwords / keys, contact the AWS Support
- Allows user the create pre-defined stacks defined by the admin using **AWS Service Catalog**

# Billing & Costing Tools summary
- **Compute Optimizer**: Recommends resources' configurations to reduce cost
- **Pricing Calculator**: Cost of services on AWS
- **Billing Dashboard**: High-level overview + free tier dashboard
- **Cost Allocation Tags**: Tags resources to create detailed reports
- **Cost and Usage Report**: Most comprehensive billing dataset
- **Cost Explorer**: View current usage (detailed) and forecast usage
- **Billing Alarm**: In us-east-1 - track overall and per-service billing
- **Budgets**: More advanced - track usage, costs, RI, and get alerts,...
- **Saving Plans**: Easy way to save based on long-term usage of AWS
- **Cost Anomaly Detection**: Detect unusual spends using Machine Learning
- **Service Quotas**: Notify you when you're close to service quotas thresholds
