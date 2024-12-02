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

# Elastic Compute Cloud (EC2)
- Elastic Compute Cloud (EC2) is a web service that provides resizable compute capacity in the cloud. It is designed to make web-scale cloud computing easier for developers.
- EC2 is one of the most popular of the AWS services.
- EC2 = Infrastructure as a Service (IaaS)
- It mainly consists in the capability of:
	- Renting virtual machines (EC2): EC2 provides scalable computing capacity in the Amazon Web Services (AWS) cloud. Using Amazon EC2 eliminates your need to invest in hardware up front, so you can develop and deploy applications faster.
	- Storing data on virtual drives (EBS): Amazon Elastic Block Store (Amazon EBS) provides persistent block storage volumes for use with Amazon EC2 instances in the AWS Cloud. Each Amazon EBS volume is automatically replicated within its Availability Zone to protect you from component failure, offering high availability and durability.
	- Distributing load across machines (ELB): Elastic Load Balancing automatically distributes incoming application traffic across multiple targets, such as Amazon EC2 instances, containers, IP addresses, and Lambda functions. It can handle the varying load of your application traffic in a single Availability Zone or across multiple Availability Zones. Elastic Load Balancing offers three types of load balancers that all feature the high availability, automatic scaling, and robust security necessary to make your applications fault tolerant.
	- Scaling the services using an auto-scaling group (ASG): Auto Scaling helps you maintain application availability and allows you to scale your Amazon EC2 capacity up or down automatically according to conditions you define. You can use Auto Scaling to help ensure that you are running your desired number of Amazon EC2 instances. Auto Scaling can also automatically increase the number of Amazon EC2 instances during demand spikes to maintain performance and decrease capacity during lulls to reduce costs. Auto Scaling is well suited both to applications that have stable demand patterns or that experience hourly, daily, or weekly variability in usage.
- Knowing EC2 is fundamental to understand how the Cloud works.

## AWS Budget Setup
- AWS Budget is a free service that allows you to set custom budgets that alert you when your costs or usage exceed (or are forecasted to exceed) your budgeted amount.
- You can access it from the AWS Management Console by searching for "Budgets" in the search bar.

## EC2 sizing and configuration options
- OS:
	- Linux
	- Windows
	- Mac
- How much CPU?
- How much RAM?
- How much Storage?
	- Network-attached (EBS)
	- Hardware (EC2 Instance Store)
- Network card: speed of the card, public IP address
- Firewall rules (security groups)
- Bootstrap script (configure at first launch): EC2 User Data

### EC2 User Data
- It is possible to bootstrap our instances using an EC2 User Data script.
- Bootstrapping means launching commands when a machine starts.
- That script is only run once at the instance first start.
- EC2 User Data is used to automate boot tasks such as:
	- Installing updates
	- Installing software
	- Downloading common files from the internet
	- Anything you can think of, really
- The EC2 User Data script runs with the root user.

### EC2 Instance Types
![](/assets/example_of_ec2_instance_types.png)
<small>*Note: t2.micro is the free tier eligible instance type (up to 750 hours per month)*</small>
- You can use different types of EC2 instances that are optimised for different use cases:
	- Optimised for compute
	- Optimised for memory
	- Optimised for storage
	- Optimised for accelerated computing
	- Optimised for graphics
- [EC2 Instance Types](https://aws.amazon.com/ec2/instance-types/)
#### Instance Type name convention
Example: m5.2xlarge
- m: instance class. M stands for main choice of general purpose.
- 5: generation of the instance class (5th generation, AWS keeps releasing new generations of instances)
- 2xlarge: size within the instance class (the bigger the size, the more powerful the instance is)

#### Instance Types - General Purpose
- Great for diversity of workloads such as web servers, code repositories, dev/test environments, microservices, and small databases.
- Balance between:
	- Compute
	- Memory
	- Networking
- All have T in their name (T stands for Turbo, it's a burstable performance instance).

<small>*Our t2.micro instance is a general purpose instance.*</small>
<small>*We can check the comparison of the different general purpose instances [here](https://ec2instances.info/)*</small>

#### Instance Types - Compute Optimised
- Great for compute-bound applications that benefit from high-performance processors.
	- Batch processing workloads
	- Media transcoding
	- High-performance web servers
	- High-performance computing (HPC)
	- Scientific modelling & machine learning
	- Dedicated gaming servers
- All have C in their name (C stands for Compute).

#### Instance Types - Memory Optimised
- Great for workloads that process large data sets in memory.
- Use cases:
	- High performance, relational & NoSQL databases
	- Distributed web scale cache stores
	- In-memory databases optimized for BI (business intelligence)
	- Applications performing real-time processing of big unstructured data
- All have R, but also X and Z in their name (R stands for RAM, X and Z are just different generations).

#### Instance Types - Storage Optimised
- Great for storage-intensive tasks that require high, sequential read and write access to large data sets on local storage.
- Use cases:
	- High frequency online transaction processing (OLTP) systems
	- Relational & NoSQL databases
	- Cache for in-memory databases (Redis)
	- Data warehousing applications
	- Distributed file systems
- All have D in their name (D stands for Density).

## Security Groups & Classic Ports
![](/assets/security_groups.png)
- Security groups are the fundamental of network security in AWS.
- They control how traffic is allowed into or out of our EC2 Machines.

- Security groups only contain allow rules.
- Security groups rules can reference by IP or by security group.

- They are associated to EC2 instances, and act as a firewall at the EC2 instance level.
- They regulate:
	- Access to ports
	- Authorised IP ranges - IPv4 and IPv6
	- Control of inbound network (from other to the instance)
	- Control of outbound network (from the instance to other)

### Good to know
- Can be attached to multiple instances
- Locked down to a region/VPC combination
- Does live "outside" the EC2 - if traffic is blocked the EC2 instance won't see it
- It's good to maintain one separate security group for SSH access
- If your application is not accessible (timeout), then it's a security group issue
- If your application gives a "connection refused" error, then it's an application error or it's not launched
- By default, all inbound traffic is blocked
- By default, all outbound traffic is authorised

### Referencing other security groups
![](/assets/referencing_other_security_groups_diagram.png)

### What classic ports should you know?
- 22: SSH (Secure Shell) - log into a EC2 Linux instance
- 21: FTP (File Transfer Protocol) - upload files into a file share
- 22: SFTP (Secure File Transfer Protocol) - upload files using SSH
- 80: HTTP - access unsecured websites
- 443: HTTPS - access secured websites
- 3389: RDP (Remote Desktop Protocol) - log into a EC2 Windows instance

## SSH
- Default Amazon Linux user: ec2-user
- You must set pem file permission to 400 (chmod 0400 mypem.pem)
- You must install pem to connect to the instance (ssh -i mypem.pem ec2-user@<instance_public_ip>)
- Never ever share your pem file or expose it to internet
- Never ever enter your Access Key ID and Secret Access Key in a EC2 instance, use EC2 Instance Roles instead

### EC2 Instance Connect
- Click on "Connect" button in the EC2 instance page

### EC2 Instance Roles
- IAM roles are a secure way to grant permissions to entities that you trust.
- To modify role for instance, go to EC2 instance page, click on "Actions" button, then click on "Security" and finally click on "Modify IAM Role". Then you input your pre-created role.

## EC2 Instance Purchasing Options
- On Demand: allows you to pay a fixed rate by the hour (or by the second) with no commitment.
- Reserved: provides you with a capacity reservation, and offer a significant discount on the hourly charge for an instance. 1 year or 3 years.
	- Standard Reserved Instances (up to 75% off on-demand)
	- Convertible Reserved Instances (up to 54% off on-demand) - can change the EC2 instance type
- Saving Plans: offer the same discounts as Reserved Instances, in exchange for a commitment to a consistent amount of compute usage (measured in $/hour) for a 1 or 3 year period.
- Spot Instances: enable you to bid whatever price you want for instance capacity, providing for even greater savings if your applications have flexible start and end times.
- Dedicated Hosts: physical EC2 server dedicated for your use. Dedicated Hosts can help you reduce costs by allowing you to use your existing server-bound software licenses.
- Dedicated Instances: which run on hardware dedicated to a single customer for additional isolation.
- Capacity Reservations: allow you to reserve capacity for your EC2 instances in a specific Availability Zone for any duration.

### EC2 Reserved Instances
![](/assets/ec2_reserved_instances.png)

### EC2 Saving Plans
![](/assets/ec2_savings_plan.png)

### EC2 Spot Instances
![](/assets/ec2_spot_instances.png)

### EC2 Dedicated Hosts
![](/assets/ec2_dedicated_hosts.png)

### EC2 Dedicated Instances
![](/assets/ec2_dedicated_instances_and_compare_to_ec2_dedicated_hosts.png)

### EC2 Capacity Reservations
![](/assets/ec2_capacity_reservations.png)

### Which EC2 purchasing option should you choose?
![](/assets/ec2_which_purchasing_option_is_right_for_me.png)

### Price comparison
![](/assets/ec2_price_comparison.png)

## Shared Responsibility Model for EC2
![](/assets/shared_responsibility_model_for_ec2.png)

## EC2 Summary
- EC2 Instance: AMI (Amazon Machine Image) + Instance Type (Hardware) + Instance Size (CPU, RAM, Storage, Network card) + Security Groups (Firewall) + EC2 User Data (Bootstrap script)
- Security Groups: Control how traffic is allowed into or out of our EC2 Machines
- EC2 User Data: Bootstrap script to run commands at first launch of an EC2 instance
- SSH: Secure Shell, used to connect to EC2 Linux instances
- EC2 Instance Connect: Connect to EC2 instances using a Java applet in the browser
- Purchasing Options: On Demand, Reserved, Spot, Dedicated Hosts, Dedicated Instances, Capacity Reservations
