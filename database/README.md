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

# AWS RDS (Relational Database Service) & Aurora
- RDS stands for Relational Database Service
- It's a managed DB service for DB use SQL as a query language
- It allows you to create databases in the cloud that are managed by AWS:
	- Postgres
	- MySQL
	- MariaDB
	- Oracle
	- Microsoft SQL Server
	- Aurora (AWS Proprietary database)

## Advantages of RDS
- RDS is a managed service:
	- Automated provisioning, OS patching
	- Continuous backups and restore to specific timestamp (Point in Time Restore)
	- Monitoring dashboards
	- Read replicas for improved read performance
	- Multi AZ setup for DR (Disaster Recovery)
	- Maintenance windows for upgrades
	- Scaling capability (vertical and horizontal)
	- Storage backed by EBS (gp2 or io1)
- BUT you can't SSH into your instances

## RDS Solution Architecture
![](/assets/rds_solution_architect.png)

## Amazon Aurora
- Aurora is a proprietary technology from AWS (not open sourced)
- Postgres and MySQL are both supported as Aurora DB (that means your drivers will work as if Aurora was a Postgres or MySQL database)
- Aurora is "AWS cloud optimized" and claims 5x performance improvement over MySQL on RDS, over 3x the performance of Postgres on RDS
- Aurora storage automatically grows in increments of 10GB, up to 128TB
- Aurora costs more than RDS (20% more) - but is more efficient
- Not in the free tier

## RDS Deployment Options: Read Replica, Multi AZ
- Read replicas & Multi AZ can be setup for enhanced performance, availability and disaster recovery
![](/assets/rds_deployments_read_replica_multi_az.png)

### RDS Deployments: Multi-Region
![](/assets/rds_deployment_multi_region.png)
- RDS Multi AZ is for Disaster Recovery
	- Disaster Recovery (DR) in case of region issue
	- Local performance for global reads
	- Replication cost involved

## Amazon ElastiCache
![](/assets/elasticcache_solution_architecture_cache.png)
- The same way RDS is to get managed Relational Databases, ElastiCache is to get managed Redis or Memcached
- Caches are in-memory databases with really high performance, low latency
- Helps reduce load off of databases for read intensive workloads

- AWS takes care of OS maintenance / patching, optimizations, setup, configuration, monitoring, failure recovery and backups

# DynamoDB
- Fully Managed Highly available with replication across 3 AZ
- NoSQL Database - not a relational database
- Scales to massive workloads, distributed "serverless" database
- Millions of requests per seconds, trillions of row, 100s of TB of storage
- Fast and consistent in performance
- Single-digit millisecond latency - low latency retrieval
- Integrated with IAM for security, authorization and administration
- Low cost and auto scaling capabilities
- Standard & Infrequent Access (IA) Table Class.

## DynamoDB - type of data
![](/assets/dynamodb_key_value.png)
- DynamoDB is a key/value database

## DynamoDB Accelerator (DAX)
![](/assets/dynamodb_accelerator_dax.png)

## DynamoDB - Global Tables
![](/assets/dynamodb_global_tables.png)
- Make a DynamoDB table accessible with low latency from anywhere in multiple regions
- Active-Active replication (read / write to any AWS Region) - When you write to a region, the data is automatically replicated to other regions

# Redshift
- Redshift is Amazon's data warehousing solution
- Redshift is based on PostgresSQL, but it's not used for OLTP (Online Transaction Processing) but for OLAP (Online Analytical Processing) - analytics and data warehousing
- Load data once every hour, not every second
- 10x better performance than other data warehouses, scale to PBs (Petabytes) of data in the cloud
- Columnar storage with compressed blocks (instead of row-based)
- Massive parallel query execution (Massive Parallel Processing - MPP), highly available
- Pay as you go based on the instances you provision
- Has a SQL interface for performing queries
- BI tools such as AWS Quicksight, Tableau, etc. can connect to Redshift

# Amazon EMR (Elastic MapReduce)
- EMR helps creating Hadoop clusters (HDFS - Hadoop Distributed File System, Big Data) to analyze and process vast amount of data
- The clusters can be made of hundreds of EC2 instances
- Also supports Apache Spark, HBase, Presto, Flink, etc.
- EMR takes care of all the provisioning and configuration, automatically
- Auto-scaling and integrated with Spot instances
- Use cases: data processing, machine learning, big data, web indexing, etc.

# Amazon Athena
- Serverless query service to perform analytics directly on S3 files (using SQL language)
- Uses standard SQL language to query S3
- Supports CSV, JSON, ORCm Avro, and Parquet files (built on Presto)

- Pricing: $5.00 per TB of data scanned
- Use compressed of columnar data for cost-savings (less scan)

- Use cases: Business intelligence / analytics / reporting, analyze & query VPC (Virtual Private Cloud) flow logs, ELB (Elastic Load Balancing) logs, CloudTrail trails, cost and billing logs, etc.

- *Exam tip*: Athena is for ad-hoc querying data in S3, Redshift is for OLAP

# Amazon QuickSight
![](/assets/aws_quicksight.png)

# DocumentDB
![](/assets/documentdb.png)

# Amazon Timestream
![](/assets/amazon_timestream.png)

# Amazon Neptune
![](/assets/amazon_neptune.png)

# Amazon QLDB (Quantum Ledger Database) -> **Discontinued on 07/31/2025**
![](/assets/amazon_qldb.png)

## Amazon Managed Blockchain
![](/assets/amazon_managed_blockchain.png)

# AWS Glue
![](/assets/aws_glue.png)

# DMS (Database Migration Service)
![](/assets/amazon_dms.png)

# Databases & Analytics Summary in AWS
- Relational Databases - OLTP (Online Transaction Processing) - SQL
	- RDS (MySQL, Postgres, Oracle, SQL Server, Aurora)
	- Aurora Serverless (MySQL, Postgres)
- Differences between Multi-AZ, Read Replicas, Multi-Region

- In-memory databases / Caching - low latency
	- ElastiCache (Redis, Memcached)

- Key/Value Databases - fast and scalable - NoSQL
	- DynamoDB (serverless) and DAX (DynamoDB Accelerator, in-memory cache for DynamoDB)

- Warehouse - OLAP (Online Analytical Processing) - SQL
	- Redshift (Amazon's data warehousing solution)

- Hadoop / Big Data analytics
	- EMR (Elastic MapReduce)

- Querying data on S3
	- Athena (serverless SQL queries on S3)

- Business Intelligence
	- QuickSight (Business Intelligence - BI, dashboards, visualizations,...)

- "Aurora for MongoDB"
	- DocumentDB (JSON Document Database, NoSQL)

- Financial Ledger
	- QLDB (Quantum Ledger Database, immutable journal, cryptographically verifiable, central database) -> **Discontinued on 07/31/2025**

- Managed Blockchain
	- Managed Blockchain (Hyperledger Fabric and Ethereum frameworks)

- Managed ETL (Extract, Transform, Load) & Data Catalog
	- Glue (Managed ETL service) & Glue Data Catalog (central repository to store structural and operational metadata for all data assets)

- Migrate databases to AWS
	- DMS (Database Migration Service)

- Serverless Timeseries database
	- Amazon Timestream

- Graph Databases
	- Neptune (fully managed graph database service)
