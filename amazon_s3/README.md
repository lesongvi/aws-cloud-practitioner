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

# Amazon S3
- Amazon S3 stands for Simple Storage Service
- Amazon S3 is one of the main building blocks of AWS
- It's advertised as "infinitely scaling" storage

- Many websites use S3 as a backbone (think of it as the hard drive of the cloud)
- Many AWS services use Amazon S3 as an integration as well

## Amazon S3 Use Cases
- Backup and Storage
- Disaster Recovery
- Archive
- Hybrid Cloud Storage
- Application Hosting
- Media Hosting
- Data Lakes & Big Data Analytics
- Software Delivery
- Static Website

- Example: Nasdaq stores 7 years of data into S3 Glacier, Sysco run analytics on its data and gain business insights, Netflix uses S3 to store movies, etc...

## Amazon S3 Buckets
- Amazon S3 allows people to store objects (files) in "buckets" (directories)
- Buckets must have a globally unique name (across all regions and accounts)
- Buckets are defined at the region level
- S3 looks like a global service but it's not: bucket are created in a region and never leave it (unless you copy objects to other regions)
- Naming convention:
	- No uppercase, no underscore
	- 3-63 characters long
	- Not an IP
	- Must start with lowercase letter or number

## Amazon S3 Objects
- Objects (files) have a Key
- The key is the FULL path:
	- `s3://my-bucket/my_file.txt`
	- `s3://my-bucket/my_folder1/another_folder/my_file.txt`
- The key is composed of **prefix** + **object name**
- There's no concept of "directories" within buckets - just keys with very long names that contain slashes (`/`) for visual representation of folders (although the UI will trick you to think there are directories)
- Object values are the content of the body:
	- Max Object Size is 5TB (5000GB)
	- If uploading more than 5GB, must use "multi-part upload"
- Metadata (list of text key/value pairs - system or user metadata)
- Tags (Unicode key/value pair - up to 10)
- Version ID (if versioning is enabled)

## Amazon S3 Security
- User based:
	- IAM Policies - which API calls should be allowed for a specific user from IAM console
- Resource Based:
	- Bucket Policies - bucket wide rules from S3 console - allows cross account
	- Object Access Control List (ACL) - finer grain (can be disabled)
	- Bucket Access Control List (ACL) - less common (can be disabled)

<small>*Note: an IAM principal can access an S3 object if the user IAM permissions AND the bucket permissions allow the API call*</small>

~[](/assets/s3_bucket_policies.png)

### Block all public access
- This setting is a bucket setting that can be enabled to prevent public access to the bucket and its objects to prevent data leaks
- It can be enabled at the account level to prevent accidental public access to new buckets
- It can be enabled at the bucket level to prevent accidental public access to existing buckets. So when enabled at the account level, it will still deny public access to new buckets

## Amazon S3 - Static Website Hosting
- S3 can host static websites and have them accessible on the www
- The website URL will be:
	- `<bucket-name>.s3-website-<AWS-region>.amazonaws.com`
	- `<bucket-name>.s3-website.<AWS-region>.amazonaws.com`
- If you get a 403 forbidden error, make sure the bucket policy allows public reads

## Amazon S3 - Versioning
![](/assets/s3_versoning.png)

## Amazon S3 - Replication (CRR & SRR)
- Must enable versioning in order to use replication
- Cross-Region Replication (CRR): is used to replicate objects across buckets in different AWS accounts and/or regions
- Same-Region Replication (SRR): is used to replicate objects across buckets in the same AWS account and/or region
- Buckets can be in different accounts
- Copying is asynchronous
- Must give proper IAM permissions to S3

- Use cases:
	- CRR: compliance, lower latency access, replication across accounts
	- SRR: log aggregation, live replication between production and test accounts

## Amazon S3 - S3 Storage Classes
- Amazon S3 Storage Classes are different tiers of storage available in Amazon S3, there several types of storage classes:
	- Amazon S3 Standard - General Purpose
	- Amazon S3 Standard - Infrequent Access (IA)
	- Amazon S3 One Zone - Infrequent Access
	- Amazon S3 Glacier Instant Retrieval
	- Amazon S3 Glacier Flexible Retrieval
	- Amazon S3 Glacier Deep Archive
	- Amazon S3 Intelligent Tiering

- Can move between storage classes manually or using S3 Lifecycle configuration

### Durability & Availability
- Durability:
	- High durability (99.999999999%, 11 x 9s) of objects across multiple AZ
	- If you store 10,000,000 objects with Amazon S3, you can on average expect to incur a loss of a single object once every 10,000 years
	- Same durability across all storage classes
- Availability:
	- Measures how readily available a service is
	- Varies depending on the storage class
	- Example: S3 standard has 99.99% availability (SLA) which means it's down 53 minutes per year

### S3 Standard - General Purpose
- 99.99% availability
- Used for frequently accessed data
- Low latency and high throughput
- Sustain 2 concurrent facility failures
- Use cases: big data analytics, mobile & gaming applications, content distribution

### S3 Standard - Infrequent Access (IA)
- For data that is accessed less frequently, but requires rapid access when needed
- Lower cost compared to S3 Standard, but charged a retrieval fee

#### Amazon S3 Standard-Infrequent Access (S3 Standard-IA)
	- 99.9% availability
	- Use cases: Disaster Recovery, backups
#### Amazon S3 One Zone-Infrequent Access (S3 One Zone-IA)
	- High durability (99.999999999%) in a single AZ; data can be lost if AZ is destroyed
	- 99.5% availability
	- Use cases: secondary backup copies of on-premise data, or storing data you can recreate

### Amazon S3 Glacier Storage Classes
- Low cost object storage meant for archiving/backup
- Pricing: price for storage per GB + retrieval cost

#### Amazon S3 Glacier Instant Retrieval
- Data is available within millisecond, but more expensive. Great for data accessed once a quarter (example: end of quarter financial report, every 3 months, ...)
- Minimum storage duration of 90 days

#### Amazon S3 Glacier Flexible Retrieval (formerly known as Amazon S3 Glacier)
- There is 3 retrieval options:
	- Expedited (1 to 5 minutes)
	- Standard (3 to 5 hours)
	- Bulk (5 to 12 hours) - free
- Minimum storage duration of 90 days

#### Amazon S3 Glacier Deep Archive - for long term storage
- There is 2 retrieval options:
	- Standard (12 hours)
	- Bulk (48 hours)
- Minimum storage duration of 180 days

### Amazon S3 Intelligent Tiering
- We need to create a Lifecycle policy to move objects between tiers
- Small monthly monitoring and auto-tiering fee
- Automatically moves objects between 2 access tiers based on changing access patterns (Example: If an object is uploaded to S3 and not accessed for 30 days, it will be moved to the infrequent access tier, if it's not accessed for another 30 days, it will be moved to the archive tier, ...)
- There are no retrieval charges in S3 Intelligent-Tiering

- **Frequent Access Tier** (automatic): default tier for objects
- **Infrequent Access Tier** (automatic): objects that haven't been accessed for at least 30 days are moved here
- **Archive Instant Access Tier** (manual): objects that haven't been accessed for at least 90 days are moved here
- **Archive Access Tier** (manual): configurable from 90 days to 700+ days
- **Deep Archive Access Tier** (manual): configurable from 180 days to 700+ days

### S3 Storage Classes Comparison
![](/assets/s3_storage_classes_comparison.png)
#### Example
![](/assets/s3_storage_classes_price_example.png)

## S3 Encryption
- There is 2 types of encryption for S3:
	- Server Side Encryption (SSE): S3 encrypts the object itself
	- Client Side Encryption (CSE): client encrypts the object before sending it to S3
- Server side encryption default is enabled

## Shared Responsibility Model for S3
![](/assets/shared_responsibility_model_s3.png)

## AWS Snow Family
- Highly secure, portable devices to collect and process data at the edge, and migrate data into and out of AWS
- Data migration can be done via Snowcone, Snowball Edge, Snowmobile
- Edge computing can be done via Snowcone, Snowball Edge

### Why use Snow Family?
![](/assets/why_need_aws_snow_family.png)

### How it works?
![](/assets/aws_snow_family_diagram.png)

### Snowball Edge (for data transfer)
![](/assets/aws_snowball_edge.png)

### AWS Snowcone & Snowcone SSD (for data transfer)
![AWS Snowcone](/assets/aws_snow_cone.png)

### AWS Snowmobile (for data transfer)
![](/assets/aws_snowmobile.png)

### AWS Snow Family for Data Migrations
![](/assets/aws_snow_family_for_data_migrations.png)

### Snow Family - Usage Process (for data transfer)
1. Request a Snowball device from the AWS Management Console
2. Install the Snowball client / AWS OpsHub on your servers
3. Connect the device to your servers, and copy files using the client
4. Ship back the device when you're done (goes to the right AWS facility)
5. Data is loaded into an S3 bucket
6. Device is completely wiped

### What is Edge Computing?
- Process data while it's being created on an edge location: A truck on the road, a ship at sea, a remote wind farm, a construction site, ...
- These locations may have:
	- Limited or no internet connectivity
	- Limited or no access to computing power
- We setup a Snowball Edge / Snowcone device on the edge location to do edge computing
- Use cases of Edge Computing:
	- Preprocess data (filtering, aggregation, ...)
	- Machine Learning at the edge
	- Transcoding media streams (video, audio, ...)
- Eventually (if need be) we can ship the device back to AWS (for transferring data for example)

### Snow Family - Edge Computing
- Snowcone & Snowcone SSD (smaller)
	- 2 CPUs, 4 GV of memory, wired or wireless access
	- USB-C power using a cord or the optional battery
- Snowball Edge - Compute Optimized
	- 104 vCPUs, 416 GiB of RAM
	- Optional GPU (useful for video processing, machine learning, ...)
	- 28 TB NVMe or 42TB HDD usable storage
	- Storage Clustering available (up to 16 nodes)
- Snowball Edge - Storage Optimized
	- Up to 40 vCPUs, 80 GiB of RAM, 80 TB storage
- All: Can run EC2 instances & Lambda functions (using AWS IoT Greengrass)
- Long-term deployment options: 1 and 3 years discounted pricing

### AWS OpsHub
![](/assets/aws_opshub.png)

### Hybrid Cloud Storage
- Hybrid Cloud Storage is a storage solution that uses both on-premise and cloud resources to store data
- AWS is pushing for "hybrid cloud":
	- Part of your infrastructure on-premise
	- Part of your infrastructure on the cloud
- This can be due to:
	- Long cloud migrations
	- Security requirements
	- Compliance requirements
	- IT strategy
- S3 is a proprietary storage technology (unlink EFS / NFS), so how can we integrate it with on-premise infrastructure? **AWS Storage Gateway**!

![](/assets/aws_storage_cloud_native_options.png)

![](/assets/aws_storage_gateway.png)

- From certified cloud practitioner perspective, the Storage Gateway is a way for you to bridge your on-premise infrastructure (your file servers, your backup servers, your tape drives, ...) with the AWS cloud

## Amazon S3 - Summary
- Buckets vs. Objects: global unique name, tied to a region
- S3 Security: IAM policies, S3 Bucket Policies (public / cross account), S3 Encryption (in transit / at rest)
- S3 Websites: static websites hosted on S3, public access, HTTP 403 error = bucket policy is blocking access
- S3 Versioning: stores all versions of an object (including all writes and even if you delete an object), great backup tool, once enabled, versioning cannot be disabled, integrates with lifecycle rules
- S3 Replication: must enable versioning, CRR across accounts / regions, SRR same account / region
- S3 Storage Classes: Standard, IA, OZ-IA, Intelligent, Glacier (Instant, Flexible, Deep Archive)
- Snow Family: import data onto S3 through physical devices, edge computing, data migration
- OpsHub: desktop application to manage Snow Family devices
- Storage Gateway: hybrid solution to extend on-premise storage to S3