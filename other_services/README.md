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

# Table of Contents
- [Amazon WorkSpaces](#amazon-workspaces)
	- [Amazon WorkSpaces - Multiple Regions](#amazon-workspaces---multiple-regions)
- [Amazon AppStream 2.0](#amazon-appstream-20)
	- [Amazon AppStream 2.0 vs. WorkSpaces](#amazon-appstream-20-vs-workspaces)
- [AWS IoT Core](#aws-iot-core)
- [Amazon Elastic Transcoder](#amazon-elastic-transcoder)
- [AWS AppSync](#aws-appsync)
- [AWS Amplify](#aws-amplify)
- [AWS Application Composer (is now AWS Infrastructure Composer)](#aws-application-composer-is-now-aws-infrastructure-composer)
- [AWS Device Farm](#aws-device-farm)
- [AWS Backup](#aws-backup)
- [Disaster Recovery Strategies](#disaster-recovery-strategies)
- [AWS Elastic Disaster Recovery (DRS)](#aws-elastic-disaster-recovery-drs)
- [AWS DataSync](#aws-datasync)
- [Cloud Migration Strategies: The 7Rs](#cloud-migration-strategies-the-7rs)
- [AWS Application Discovery Service](#aws-application-discovery-service)
	- [AWS Application Migration Service (MGN)](#aws-application-migration-service-mgn)
- [AWS Migration Evaluator (formerly TSO Logic)](#aws-migration-evaluator-formerly-tso-logic)
- [AWS Migration Hub](#aws-migration-hub)
- [AWS Fault Injection Simulator (FIS)](#aws-fault-injection-simulator-fis)
- [Step Functions](#step-functions)
- [AWS Ground Station](#aws-ground-station)
- [AWS Pinpoint](#aws-pinpoint)

# Other AWS Services section
- Other services represent services that can't group into other sections.
- They are services reported by students as **sometimes, but rarely,** appearing in the exam.
- Each service is short and brief.
- *No summary section at the end of the section to keep things simple.*

# Amazon WorkSpaces
![](/assets/amazon_workspaces.png)

## Amazon WorkSpaces - Multiple Regions
![](/assets/amazon_workspaces_multiple_regions.png)

# Amazon AppStream 2.0
![](/assets/amazon_appstream_2_0.png)

## Amazon AppStream 2.0 vs. WorkSpaces
- **WorkSpaces**:
	- Fully managed VDI (Virtual Desktop Infrastructure) and desktop-as-a-service available on AWS
	- The users connect to the VDI and open native or WAM (WorkSpaces Application Manager) applications
	- WorkSpaces are On-Demand or always-on
- **AppStream 2.0**:
	- Stream a desktop application to a user's browser (no need to connect to a VDI)
	- Works with any device (that has web browser)
	- Allows to configure an instance type per application type (CPU, RAM, GPU)

# AWS IoT Core
![](/assets/aws_iot_core.png)

# Amazon Elastic Transcoder
![](/assets/amazon_elastic_transcoder.png)

# AWS AppSync
![](/assets/aws_appsync.png)

# AWS Amplify
![](/assets/aws_amplify.png)

# AWS Application Composer (is now AWS Infrastructure Composer)
- AWS Application Composer is now AWS Infrastructure Composer
  
![](/assets/aws_application_composer.png)

# AWS Device Farm
![](/assets/aws_device_farm.png)

# AWS Backup
![](/assets/aws_backup.png)
  
![](/assets/aws_backup_diagram.png)

# Disaster Recovery Strategies
![](/assets/disaster_recovery_strategies_1.png)
  
![](/assets/disaster_recovery_strategies_2.png)
  
![](/assets/typical_dr_setup_for_cloud_deployments.png)

# AWS Elastic Disaster Recovery (DRS)
- Used to be named "CloudEndure Disaster Recovery"
  
![](/assets/aws_elastic_disaster_recovery.png)

# AWS DataSync
![](/assets/aws_datasync.png)

![](/assets/aws_datasync_diagram.png)

# Cloud Migration Strategies: The 7Rs
![](/assets/cloud_migration_strategies.png)

- **Retire**:
	- Turn off things that you don't need (maybe as a result of Re-architecting)
	- Helps with reducing the surface areas of attacks (security)
	- Save costs, maybe up to 10-20% of your bill
	- Focus your attention to what resources that must be maintained
- **Retain**:
	- Do nothing for now (it's still a decision to make in a Cloud Migration)
	- Security, data compliance, performance, unresolved dependencies
	- No business value to migrate, mainframe or mid-range and non-x86 Unix apps
- **Relocate**:
	- Move app from on-premises to its Cloud version
	- Move EC2 instances to another VPC, AWS Account, or AWS Region
	- <u>Example</u>: transfer servers from VMWare Software-defined Data Center (SSDC) to AWS VMware Cloud on AWS
- **Rehost**:
	- Simple migration by re-hosting on AWS (application, database, data, ...)
	- Migrate machines (physical, virtual, another Cloud) to AWS Cloud
	- No optimization being done, applications is migrated as-is
	- Could save as much as 30% of your bill
	- <u>Example</u>: Migrate using AWS Migration Service
- **Replatform lift and reshape**:
	- <u>Example</u>: Migrate your data to Amazon RDS
	- <u>Example</u>: Migrate your application to AWS Elastic Beanstalk
	- Not changing the core architecture of the application, but leverage some of the Cloud optimizations
	- Save time and money by moving to a fully managed service or Serverless
- **Repurchase drop and shop**:
	- Moving to a difference product while moving to the Cloud
	- Often, moving to a SaaS model
	- Expensive in a short term, but can be beneficial in the long term
	- <u>Example</u>: CRM to Salesforce, HR to Workday, CMS to Drupal
- **Refactor / Re-architecture**:
	- Reimagining how the application is architected using Cloud-native features
	- Driven by the need of the business to add features and improve scalability, performance, security, and agility
	- Move from monolithic application to microservices
	- <u>Example</u>: move an application to Serverless architecture, use AWS S3

# AWS Application Discovery Service
![](/assets/aws_application_discovery_service.png)

## AWS Application Migration Service (MGN)
![](/assets/aws_application_migration_service.png)

*Note*: Why is it called "MGN" instead of "AMS"? Because "AMS" is already used by "Amazon Mechanical Turk" service, MGN is abbreviation for "Migration". [source](https://repost.aws/questions/QUt-HmDs2BT8GpAe0Sg5ZXvg/in-aws-mgn-n-for-what)

# AWS Migration Evaluator (formerly TSO Logic)
![](/assets/aws_migration_evaluator.png)

# AWS Migration Hub
![](/assets/aws_migration_hub.png)

# AWS Fault Injection Simulator (FIS)
![](/assets/aws_fault_injection_simulator.png)

# Step Functions
![](/assets/step_functions.png)

# AWS Ground Station
![](/assets/aws_ground_station.png)

# AWS Pinpoint
![](/assets/aws_pinpoint.png)
