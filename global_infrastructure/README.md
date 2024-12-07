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
- [Why Global Applications?](#why-global-applications)
	- [Global AWS Infrastructure](#global-aws-infrastructure)
	- [Global Application in AWS](#global-application-in-aws)
- [Amazon Route 53](#amazon-route-53)
	- [Route 53 - Diagram for A Record](#route-53---diagram-for-a-record)
	- [Route 53 Routing Policies](#route-53-routing-policies)
		- [Weighted Routing Policy](#weighted-routing-policy)
- [AWS CloudFront - Content Delivery Network (CDN)](#aws-cloudfront---content-delivery-network-cdn)
	- [AWS CloudFront - Origins](#aws-cloudfront---origins)
	- [AWS CloudFront at a high level](#aws-cloudfront-at-a-high-level)
	- [AWS CloudFront - S3 as an Origin](#aws-cloudfront---s3-as-an-origin)
	- [AWS CloudFront vs. S3 Cross Region Replication (CRR)](#aws-cloudfront-vs-s3-cross-region-replication-crr)
- [S3 Transfer Acceleration](#s3-transfer-acceleration)
- [AWS Global Accelerator](#aws-global-accelerator)
	- [AWS Global Accelerator versus. AWS CloudFront](#aws-global-accelerator-versus-aws-cloudfront)
	- [Tools to test latency](#tools-to-test-latency)
- [AWS Outposts](#aws-outposts)
- [AWS WaveLength](#aws-wavelength)
- [AWS Local Zones](#aws-local-zones)
- [Global Applications Architecture](#global-applications-architecture)
- [Leveraging AWS Global Infrastructure summary](#leveraging-aws-global-infrastructure-summary)

# Why Global Applications?
- A global application is an application deployed in multiple geographies
- On AWS: this coud be Regions and/or Edge Locations
- Decreased Latency:
	- Latency is the time it takes for a network packet to reach a server
	- It takes time for a packet from Asia to reach a server in the US
	- Deploy your applications closer to your users to decrease latency, better user experience
- Disaster Recovery (DR):
	- If a AWS region goes down (earthquake, storms, power shutdown, politics, etc...)
	- You can fail-over to another region and have your application still be available
	- A DR plan is important to increase the availability of your application
- Attack protection: distributed global infrastructure is harder to attack

## Global AWS Infrastructure
![](/assets/global_aws_infrastructure.png)

## Global Application in AWS
![](/assets/global_aws_infrastructure.png)

# Amazon Route 53
- Route 53 is a Managed DNS (Domain Name System)
- DNS is a collection of rules and records which helps clients understand how to reach a server through URLs

- In AWS, the most common records are:
	- A: URL to IPv4
	- AAAA: URL to IPv6
	- CNAME: URL to URL
	- Alias: URL to AWS resource (ex: ELB, CloudFront, S3, ...)

## Route 53 - Diagram for A Record
![](/assets/route_53.png)

## Route 53 Routing Policies
### Weighted Routing Policy
- Control the % of the requests that go to specific endpoint
![](/assets/route_53_routing_policies.png)

- Latency Routing Policy
	- Redirect to the server that has the least latency close to us
	- Super helpful when latency is a key concern
- Failover Routing Policy
	- Active / Passive setup
	- Health checks are used to determine the health of the "primary" server
	- Route 53 will failover to the "secondary" server if the "primary" is unhealthy
![](/assets/route_53_routing_policies_2.png)

# AWS CloudFront - Content Delivery Network (CDN)
![](/assets/aws_cloudfront_overview.png)

## AWS CloudFront - Origins
- S3 Bucket
	- For distributing files and caching them at the edge
	- Enhanced security with CloudFront Origin Access Control (OAC)
	- OAC is replacing Origin Access Identity (OAI)
	- CloudFront can be used as an ingress (to upload files to S3)
- Custom Origin (HTTP)
	- Application Load Balancer
	- EC2 Instance
	- S3 Website (must first enable the bucket as a static S3 website)
	- Any HTTP backend you want

## AWS CloudFront at a high level
![](/assets/aws_cloudfron_at_a_high_level.png)

## AWS CloudFront - S3 as an Origin
![](/assets/aws_cloudfront_s3_as_an_origin.png)

## AWS CloudFront vs. S3 Cross Region Replication (CRR)
- CloudFront:
	- Global Edge Network
	- Files are cached for a TTL (maybe a day)
	- Great for static content that must be available everywhere

- S3 CRR:
	- Must be setup for each region you want replication to happen
	- Files are updated in near real-time
	- Read only
	- Great for dynamic content that needs to be available at low-latency in few regions

# S3 Transfer Acceleration
- Increase transfer speed by transferring file to an AWS edge location which will forward the data to the S3 bucket in the target region
![](/assets/s3_transfer_acceleration.png)
- Test the tool here: [https://s3-accelerate-speedtest.s3-accelerate.amazonaws.com/en/accelerate-speed-comparsion.html](https://s3-accelerate-speedtest.s3-accelerate.amazonaws.com/en/accelerate-speed-comparsion.html)

# AWS Global Accelerator
![](/assets/aws_global_acceleration.png)

![](/assets/aws_global_accelerator_versus_without_it.png)

## AWS Global Accelerator versus. AWS CloudFront
![](/assets/aws_global_accelerator_versus_cloudfront.png)

## Tools to test latency
- [https://speedtest.globalaccelerator.aws/](https://speedtest.globalaccelerator.aws/)

# AWS Outposts
![](/assets/aws_outposts.png)

# AWS WaveLength
![](/assets/aws_wavelength.png)

# AWS Local Zones
![](assets/aws_local_zone.png)

# Global Applications Architecture
![](/assets/global_applications_architecture.png)

![](/assets/global_applications_architecture_2.png)

# Leveraging AWS Global Infrastructure summary
- Global DNS: Route 53
	- Great to route users to the closest deployment with least latency
	- Great for disaster recovery (DR) strategies
- Global Content Delivery (CDN): CloudFront
	- Replicate part of your application to AWS Edge Locations - decrease latency
	- Cache common requests - improved user experience and decrease latency
- S3 Transfer Acceleration
	- Accelerate global uploads & downloads into Amazon S3
- AWS Global Accelerator
	- Improve global application availability and performance using the AWS global network
- AWS Outposts
	- Deploy Outposts Racks in your own Data Centers to extend AWS services
- AWS Wavelength
	- Brings AWS services to the edge of the 5G network
	- Ultra-low latency applications for mobile devices
- AWS Local Zones
	- Bring AWS resources (compute, database, storage, ...) closer to your end-users
	- Good for latency-sensitive applications