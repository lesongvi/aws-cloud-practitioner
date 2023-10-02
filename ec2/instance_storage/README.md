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

# EC2 Instance Storage
## EBS
- EBS stands for Elastic Block Store
- EBS uses Network Attached Storage (NAS) - think of it as a network drive, which means there might be a bit of latency
- EBS is a network drive you can attach to your instances while they run. It can be detached from an instance and attached to another instance quickly.
- EBS Volume allows your instance to persist data, even after termination. So you can restart your instance with the same data by attaching the EBS Volume to it
- They can only be mounted to one instance at a time (at the CCP - Certified Cloud Practitioner - level) - But with EBS Multi-Attach, you can mount the same EBS Volume to multiple instances at the same time (at the Solutions Architect - Associate level)
- They are locked to a specific Availability Zone (AZ). For example, an EBS Volume in `us-east-1a` cannot be attached to an instance in `us-east-1b`. To move a volume across, you first need to snapshot it
- Because of EBS is a network drive, you will need to have a provisioned capacity (size in GBs, and IOPS - Input Output Operations per Second). You will be billed for the provisioned capacity, not the used space. You can increase the capacity of the drive over time.
- EBS don't have to be attached to an instance.

<small>Note: Think of EBS as a USB stick that you can attach to your instance while it's running</small>
<small>Note: The free tier includes 30GB of EBS storage</small>

### EBS Snapshots
- Snapshots are backups of your EBS Volumes
- EBS Snapshots are a point in time copy of an EBS Volume
- Not necessary to detach volume to do snapshot, but recommended
- Can copy snapshots across AZ or Region

#### EBS Snapshots Features
- EBS Snapshots Archive:
	- Move a Snapshot to an "archive tier" that is 75% cheaper
	- Takes within 24 to 72 hours for restoring the archive
- Recycle Bin for EBS Snapshots:
	- Setup rules to retain deleted snapshots so you can recover them after an accidental deletion
	- Specify retention (from 1 day to 1 year)

## AMI - Amazon Machine Image
- AMI are a customization of an EC2 instance
	- You add your own software, configuration, operating system, monitoring
	- Faster boot / configuration time because all your software is pre-packaged
- AMI are built for a specific region (and can be copied across regions)
- You can launch EC2 instances from:
	- A Public AMI: AWS provided
	- Your own AMI: you make and maintain them yourself
	- An AWS Marketplace AMI: an AMI someone else made and published

### AMI Process (From an EC2 Instance)
- Start an EC2 Instance and customize it
- Stop the instance (for data integrity)
- Build an AMI - this will also create EBS snapshots (for root volume) - this is the fastest way to create an AMI
- Launch instances from other AMIs

## EC2 Image Builder
![](/assets/ec2_image_builder.png)

## EC2 Instance Store
- EBS Volumes are network drives with good but "limited" performance.
- If you need a high-performance hardware disk, use EC2 Instance Store.
- Better I/O performance
- EC2 Instance Store lose their storage if they're stopped (ephemeral storage) -> cannot be used as a durable long term storage
- Good for buffer, cache, scratch data, temporary content, etc...
- Risk of data loss if hardware fails (EC2 Instance Store drive)

### Local EC2 Instance Store IOPS (Input/Output Operations Per Second)
![Local EC2 Instance Store IOPS](/assets/ec2_instance_store.png)

## EFS - Elastic File System
![](/assets/efs.png)
- Managed NFS (Network File System) that can be mounted on 100s of EC2 instances at the same time.
- EFS only works with Linux EC2 instances in multi-AZ.
- Highly available, scalable, expensive (3x gp2), pay per use, no capacity planning.

### EBS vs. EFS
- EBS is only available in one AZ, EFS is available in multi-AZ
- EBS can only be attached to one instance, EFS can be attached to multiple instances
- EBS is locked at the instance level, EFS is locked at the file system level
- EBS supports a handful of instances, EFS supports all instances running Linux
- EBS is cheaper than EFS

### EFS Infrequent Access (EFS IA)
- EFS IA is a storage class that is cost-optimized for files not accessed every day
- EFS IA is a storage class that can save you up to 92% compared to the regular EFS price
- EFS will automatically move files to EFS IA based on the last time they were accessed
- Enable EFS-IA with a Lifecycle Policy
- Example: Move files that are not accessed for 7 days to EFS IA
- Trasparent to the applications accessing EFS.

## Shared Responsibility Model for EC2 Storage
![](/assets/shared_responsibility_model_ec2_storage.png)

## Amazon FSx
- Launch 3rd party high-performance file system on AWS
- Fully managed, Windows native, file system
	- FSx for Lustre (high performance computing, machine learning, video processing, financial modeling, etc...)
	- FSx for Windows File Server (Windows applications, SQL Server, SAP, home directories, etc...)
	- FSx for NetApp ONTAP (SAP, high performance computing, Oracle DB, etc...)

### FSx for Windows File Server
![](/assets/fsx_for_windows_file_server.png)
- A fully managed, highly reliable, and scalable Windows native file system
- Built on Windows Server
- Supports SMB protocol & Windows NTFS
- Integrated with Microsoft Active Directory.
- Can be accessed from on-premise infrastructure

### FSx for Lustre
![](/assets/amazon_fsx.png)
- A fully managed, high-performance, scalable file system for High Performance Computing (HPC)
- The name Lustre is a portmanteau word derived from Linux and cluster
- Use cases: Machine learning, high performance computing, video processing, financial modeling, electronic design automation, etc...
- Scales up to 100s GB/s, millions of IOPS, sub-ms latencies

## EC2 Instance Storage Summary
- EBS volumes:
	- Network drives attached to one EC2 instance at a time
	- Mapped to an AZ
	- Can use EBS Snapshots for backups or to create new EBS volumes
- AMI: create ready-to-use EC2 instances with our own customizations
- EC2 Image Builder: automatically build, test and distribute AMIs
- EC2 Instance Store:
	- High-performance hardware disk attached to your EC2 instance
	- Lose their storage if they're stopped (ephemeral storage)
- EFS: Managed NFS that can be attached to 100s of EC2 instances in a region in parallel
- EFS IA: cost-optimized storage class for infrequently accessed files
- FSx for Windows File Server: Network File System (NFS) compatible with Windows Servers
- FSx for Lustre: High-performance Computing Linux file system
