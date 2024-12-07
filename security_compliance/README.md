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

# Table of Contents
- [Talking again about AWS Shared Responsibility Model](#talking-again-about-aws-shared-responsibility-model)
	- [Example, for RDS](#example-for-rds)
	- [Example, for S3](#example-for-s3)
	- [Shared Responsibility Model diagram](#shared-responsibility-model-diagram)
- [Protect your server against DDoS attacks](#protect-your-server-against-ddos-attacks)
	- [Sample Reference Architecture for DDoS Protection](#sample-reference-architecture-for-ddos-protection)
	- [AWS Shield](#aws-shield)
	- [AWS WAF](#aws-waf)
	- [AWS Network Firewall (VPC)](#aws-network-firewall-vpc)
- [Penetration Testing on AWS Cloud](#penetration-testing-on-aws-cloud)
	- [What is Penetration Testing?](#what-is-penetration-testing)
	- [AWS Penetration Testing Policy](#aws-penetration-testing-policy)
		- [Prohibited Testing](#prohibited-testing)
- [Encryption with KMS & CloudHSM](#encryption-with-kms--cloudhsm)
	- [Data at rest vs. Data in transit](#data-at-rest-vs-data-in-transit)
	- [AWS KMS (Key Management Service)](#aws-kms-key-management-service)
	- [CloudHSM (Hardware Security Module)](#cloudhsm-hardware-security-module)
		- [CloudHSM diagram](#cloudhsm-diagram)
	- [Types of Customer Master Keys (CMK)](#types-of-customer-master-keys-cmk)
- [AWS Certificate Manager (ACM)](#aws-certificate-manager-acm)
- [Secrets Manager overview](#secrets-manager-overview)
- [Artifact overview (not really a service)](#artifact-overview-not-really-a-service)
- [Amazon GuardDuty](#amazon-guardduty)
- [Amazon Inspector](#amazon-inspector)
	- [What does Amazon Inspector evaluate?](#what-does-amazon-inspector-evaluate)
- [AWS Config](#aws-config)
	- [AWS Config Resource](#aws-config-resource)
- [Amazon Macie](#amazon-macie)
- [AWS Security Hub](#aws-security-hub)
- [Amazon Detective](#amazon-detective)
- [AWS Abuse](#aws-abuse)
- [Root user privileges](#root-user-privileges)
- [IAM Access Analyzer](#iam-access-analyzer)
- [Security & Compliance - Summary](#security--compliance---summary)


# Talking again about AWS Shared Responsibility Model
- AWS responsibility - Security of the Cloud
	- Protecting infrastructure (hardware, software, facilities, and networking) that runs AWS services
	- Managed services like S3, DynamoDB, RDS, etc...
- Customer responsibility - Security in the Cloud
	- For EC2 instace, customer is responsible for management of the guest OS (including security patches and updates), firewall & network configuration, IAM
	- Encrypting application data
- Shared controls:
	- Patch management, configuration management, awareness & training

## Example, for RDS
- AWS responsibility:
	- Manage the underlying EC2 instance, disable SSH access
	- Automated DB patching
	- Automated OS patching
	- Audit the underlying instance and disks & guarantee it functions
- Your responsibility:
	- Check the ports / IP / security group inbound rules in DB's security group
	- In-database user creation and permissions
	- Creating a database with or without public access
	- Ensure parameter groups or DB is configured to only allow SSL connections
	- Database encryption setting

## Example, for S3
- AWS responsibility:
	- Guarantee you get unlimited storage
	- Guarantee you get encryption
	- Ensure separation of your data from other customers
	- Ensure AWS employees have no access to your data
- Your responsibility:
	- Bucket configuration
	- Bucket policy / public setting
	- IAM user and role
	- Enabling encryption

## Shared Responsibility Model diagram
![](/assets/shared_responsibility_model_diagram_compliance.png)

# Protect your server against DDoS attacks
- AWS Shield Standard: protect against DDoS attack for your website and application, for all customer at no additional charge
- AWS Shield Advanced: 24/7 premium DDoS protection
- AWS WAF: Filter specific requests based on rules (ex: allow only GET requests to my website)
- CloudFront and Route 53:
	- Availability protection using global edge network
	- Combined with AWS Shield, provides attack mitigation at the edge of the AWS network

- Be ready to scale - leverage AWS Auto Scaling

## Sample Reference Architecture for DDoS Protection
![](/assets/sample_reference_architecture_for_ddos_protection.png)

## AWS Shield
- AWS Shield Standard:
	- Free service that is activated for every AWS customer
	- Provides protection from attacks such as SYN/UDP Floods, Reflection attacks and other layer 3/layer 4 attacks
- AWS Shield Advanced:
	- Optional DDoS mitigation service ($3,000 per month per organization)
	- Protect against more sophisticated attacks on Amazon EC2, Elastic Load Balancing (ELB), Amazon CloudFront, AWS Global Accelerator, and Amazon Route 53
	- 24/7 access to AWS DDoS Response Team (DRT)
	- Protect against higher fees during usage spikes

## AWS WAF
- Protects your web applications from common web exploits (Layer 7)
- Layer 7 is HTTP (vs. Layer 4 is TCP)
- Deploy on Application Load Balancer (ALB), Amazon CloudFront, API Gateway

- Define Web ACL (Web Access Control List) which contains rules:
	- Rules can include IP addresses, HTTP headers, HTTP body, or URI strings
	- Protects from common attack - SQL injection, cross-site scripting, etc...
	- Size constraints, geo-match (block countries)
	- Rate-based rules (ex: block IP address that exceed 100 requests per 5 minutes)

## AWS Network Firewall (VPC)
![](/assets/aws_network_firewall.png)

# Penetration Testing on AWS Cloud
## What is Penetration Testing?
- Penetration testing (pen testing) is a security exercise where a cyber-security expert tries to find and exploit vulnerabilities in a computer system.

## AWS Penetration Testing Policy
- AWS customers are welcome to carry out security assessments or penetration tests against their AWS infrastructure without prior approval for **8 services**:
	- [Amazon EC2](/ec2/README.md) instances, [NAT Gateways](/vpc_networking/README.md#internet-gateway-igw--nat-gateway), and [Elastic Load Balancers](/elb_asg/README.md)
	- [Amazon RDS](/database/README.md#aws-rds-relational-database-service--aurora)
	- [Amazon CloudFront](/global_infrastructure/README.md#aws-cloudfront---content-delivery-network-cdn)
	- [Amazon Aurora](/database//README.md#amazon-aurora)
	- [Amazon API Gateway](/other_compute_services/README.md#amazon-api-gateway)
	- [AWS Lambda](/other_compute_services/README.md#lambda) and Lambda Edge functions (read about Lambda Edge functions below)
	- [Amazon Lightsail](/other_compute_services/README.md#amazon-lightsail-simple-cloud-servers---shouldnt-be-used-in-your-career-as-a-aws-developer) resources
	- [Amazon Elastic Beanstalk](/deployments/README.md#aws-elastic-beanstalk) environments
- *Note*: You must remember this list of services, it's important for the exam.
- *Note 2*: This list can be updated by AWS at any time, but you will not be tested on the updated list.
- *Note about Lamda Edge functions*: This is the same as Lambda but for CloudFront. It allows you to run Lambda functions at AWS edge location (CloudFront) that closest to your users.

### Prohibited Testing
- DNS zone walking via Amazon Route 53 Hosted Zones
- Denial of Service (DoS), Distributed Denial of Service (DDoS), Simulated DoS, Simulated DDoS
- Port flooding
- Protocol flooding
- Request flooding (login request flooding, API request flooding)

*For any other simulated events, contact: aws-security-simulated-event@amazon.com*
*Read more: [https://aws.amazon.com/security/penetration-testing/](https://aws.amazon.com/security/penetration-testing/)*

# Encryption with KMS & CloudHSM
## Data at rest vs. Data in transit
![](/assets/data_at_rest_and_data_in_transit.png)
> It's at rest because it's not moving.

## AWS KMS (Key Management Service)
- Anytime you hear "encryption" for an AWS service, it's most likely KMS
- KMS = AWS manages the encryption keys for us
- Encryption Opt-in:
	- EBS volumes: encrypt volumes
	- S3 buckets: Server-side encryption of objects
	- Redshift: encryption of data
	- RDS database: encryption of data
	- EFS drives: encryption of data
- Encryption Automatically enabled:
	- CloudTrail Logs
	- S3 Glacier
	- Storage Gateway
  
*Note*: SSE = Server Side Encryption

## CloudHSM (Hardware Security Module)
![](/assets/cloudhsm.png)

### CloudHSM diagram
![](/assets/cloudhsm_diagram.png)

## Types of Customer Master Keys (CMK)
- Customer Managed CMK:
	- Created, managed and used by the customer, can enable or disable
	- Possibility of rotation policy (new key generated every year, old key preserved)
	- Possibility to bring-your-own-key (BYOK)
- AWS managed CMK:
	- Created, managed and used on your behalf by AWS
	- Used by AWS services (AWS/S3, AWS/EBS, AWS/Redshift, etc...)
- AWS owned CMK:
	- Collection of CMKs that an AWS service owns and manages to use in multiple accounts
	- AWS can use those to protect resources in your account (but you can't view the keys)
- CloudHSM Keys (custom keystone)
	- Key generated from your own CloudHSM hardware device
	- Cryptographic operations are performed within the CloudHSM cluster

# AWS Certificate Manager (ACM)
![](/assets/acm.png)

# Secrets Manager overview
- Newer service, meant for storing secrets
- Capability to force rotation of secrets every X days
- Automate generation of secrets on rotation (uses Lambda)
- Integration with Amazon RDS (MySQL, PostgreSQL, Aurora)
- Secrets are encrypted using KMS

- Mostly meant for RDS integration

# Artifact overview (not really a service)
- Portal that provides customers with on-demand access to AWS compliance documentation and AWS agreements
- Artifact Reports - Allows you to download AWS security and compliance documents from third-party auditors, like AWS ISO certifications, Payment Card Industry (PCI), and System and Organization Controls (SOC) reports
- Artifact Agreements - Allows you to review, accept, and track the status of AWS agreements such as the Business Associate Addendum (BAA) or the Health Insurance Portability and Accountability Act (HIPAA) for an individual account or in your organization.

- Can be used to support internal audit or compliance.

# Amazon GuardDuty
![](/assets/amazon_guardduty.png)
- Intelligent threat discovery to protect your AWS account
- Uses machine learning algorithm, anomaly detection, 3rd party data
- One click to enable (30 day trial), no need to intall software or agents
- Input data includes:
	- CloudTrail Event Logs - unusual API calls, unauthorized deployments
		- CloudTrail Management Events - create VPC subnet, create trail,...
		- CloudTrail S3 Data Events - get object list, list objects, delete object,...
	- VPC Flow Logs - unusual internal traffic patterns, unusual IP addresses
	- DNS Logs - compromised EC2 instances sending encoded data within DNS queries
	- Optional Features - EKS Audit Logs, RDS & Aurora, EBS, Lambda, S3 Data Event,...
- Can setup EventBridge rules to be notified in case of findings
- EventBridge rules can target AWS Lambda or SNS
- Can protect against CryptoCurrency attacks (has a dedicated "finding" for it)

# Amazon Inspector
![](/assets/amazon_inspector.png)

## What does Amazon Inspector evaluate?
- Remember: only for EC2 instances, Container Images & Lambda functions

- Continuous scanning of the infrastructure, only when needed

- Package vulnerabilities (EC2, ECR & Lambda) - database of CVE
- Network reachability (EC2)

- A risk score is associated with all vulnerabilities for prioritization
  
*Note*: CVE = Common Vulnerabilities and Exposures

# AWS Config
- Helps with auditing and recording compliance of your AWS resources
- Helps record configuration and changes over time
- Possibility of storing the configuration data into S3 (Analyzed by Athena)
- Questions that can be solved by AWS Config:
	- Is there unrestricted SSH access to my security groups?
	- Do my buckets have any public access?
	- How has my ALB configuration changed over time?
- You can receive alerts (SNS notifications) for any changes
- AWS Config is a per-region service
- Can be aggregated across regions and accounts

## AWS Config Resource
![](/assets/aws_config_resource.png)

# Amazon Macie
![](/assets/amazon_macie.png)

# AWS Security Hub
![](/assets/aws_security_hub.png)
- Central security tool to manage security across several AWS accounts and automate security checks
- Integrated dashboards showing current security and compliance status to quickly take actions
- Automatically aggregate alerts in predefined or personal findings formats from various AWS services & AWS partner tools:
	- Config
	- GuardDuty
	- Inspector
	- Macie
	- IAM Access Analyzer
	- AWS Systems Manager
	- AWS Firewall Manager
	- AWS Health
	- AWS Partner Network Solutions
- Must first enable the AWS Config Service

# Amazon Detective
- GuardDuty, Macie, and Security Hub are used to identify potential security issues, or findings
- Sometimes security findings require deeper analysis to isolate the root cause and take action - it's a complex process
- Amazon Detective analyzes, investigate, and quickly identifies the root cause of security issuees or suspicious activities (using ML and graphs)
- Automatically collects and processes events from VPC Flow Logs, CloudTrail, GuardDuty, and create a unified view
- Produces visualizetions with details and context to get to the root cause

# AWS Abuse
![](/assets/aws_abuse.png)

# Root user privileges
- Root user = Account Owner (created when the account is created)
- Has complete access to all AWS services and resources
- Lock away your AWS account root user access keys!
- Do not use the root account for everyday tasks, even administrative ones
- Actions that can be performed only by the root user:
	- Change account settings (account name, email address, root user password, root user access keys)
	- View certain tax invoices
	- Close your AWS account
	- Restore IAM user permissions
	- Change or cancel your AWS Support plan
	- Register as a seller in the Reserved Instance Marketplace
	- Configure an Amazon S3 bucket to enable MFA
	- Edit or delete an Amazon S3 bucket policy that includes an invalid VPC ID or VPC endpoint ID
	- Sign up for GovCloud (US)

# IAM Access Analyzer
![](/assets/iam_access_analyzer.png)

# Security & Compliance - Summary
- Shared Responsibility on AWS
- Shield: Automatic DDoS protection + 24/7 support for advanced
- WAF: Firewall to filter incoming requests based on rules
- KMS: Encryption keys managed by AWS
- CloudHSM: Hardware encryption, we manage encryption keys
- AWS Certificate Manager: provision, manage, and deploy SSL/TLS certificates
- Artifact: Get access to compliance reports such as PCI, ISO, etc...
- GuardDuty: Find malicious behavior with VPC, DNS & CloudTrail Logs
- Inspector: Find software vulnerabilities in EC2, ECR images, and Lambda functions
- Network Firewall: Protect VPC against network attacks
- Config: Track config changes and compliance against rules
- Macie: Find sensitive data (ex: PII data) in Amazon S3 buckets
- CloudTrail: Track API calls made by users within account
- AWS Security Hub: gather security findings from multiple AWS accounts
- Amazon Detective: Find the root cause of security issues or suspicious activities
- AWS Abuse: Report AWS resources used for abusive or illegal purposes
- Root user privileges:
	- Change account settings
	- Close your AWS account
	- Change or cancel your AWS Support plan
	- Register as a seller in the Reserved Instance Marketplace
- IAM Access Analyzer: Identify resources are shared externally